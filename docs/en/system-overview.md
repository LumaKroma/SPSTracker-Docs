# System Overview

[日本語](../system-overview.md) | English

SPSTracker is a VRChat avatar gimmick that makes an item on your avatar track another player's VRCFury SPS Socket.

SPSTracker and ProductAdapter target VRChat on Windows PC. They cannot be used on Android / Quest avatars.

## Designing for Multiple Items to Share One Tracker

As a general rule, only one SPSTracker should be placed on an avatar. Its tracking result is designed to be shared by multiple compatible items. Each item references the same `HeldAnchor`, `TrackedAnchor`, and `TrackingStart`, while the product's display or selection menu determines which item is in use.

This design has the following goals:

- Avoid duplicating Contact Receivers for every item
- Reduce the load from Contacts processed in real time by VRChat
- Conserve Expressions Parameter memory and parameter slots
- Reduce duplicated Constraints, Animator Layers, and menu structures
- Provide consistent tracking state and controls across compatible products

SPSTracker uses 6 Contact Receivers in its basic configuration and 7 when Nearest Lock is enabled. If each compatible product includes its own tracking mechanism and adds a similar set of Contacts, the total number of Contacts on the avatar increases. Overlapping Contacts and additional processing load may reduce detection accuracy, tracking stability, or real-time responsiveness.

Custom implementations are not prohibited, but compatible products are encouraged to use the public Anchors and `TrackingStart` so that one SPSTracker can be shared. If the public API does not provide a state value or switching feature required by your project, submit a [feature request](../../README.en.md#feature-requests).

## Tracking Behavior

When SPSTracker detects an enabled SPS Socket, the core system tracks the Socket's position, yaw, and pitch. The optional [SPS Tracker Roll Deformation](roll-deformation.md) feature in ProductAdapter can add roll around the Socket axis to selected Renderers.

- Configure the compatible item's local `+Z` direction as its forward direction while tracking.
- The held-state detection range can be tuned with `HeldRange` inside the FX Animator. After tracking starts, the detection range is adjusted automatically so it does not become smaller than the held range.
- SPSTracker compensates for effective Gain differences caused by the detection range, keeping the tracking response relatively consistent as the range changes.
- If the Socket is lost, SPSTracker holds the last position and rotation for approximately 3 seconds. During that period, it expands the recovery range progressively to its maximum over approximately 1 second.
- If the Socket is detected again during the grace period, tracking continues and the detection range returns gradually to its normal value. Otherwise, the item returns to its held position after approximately 3 seconds.

Compatible items can read the tracking state from `LumaKroma/ST/TrackingStart`. See [Public API and Parameters](public-api.md) for details.

## Public Anchors

SPSTracker exposes the following Transforms for connecting items:

```text
SPSTracker/API/HeldAnchor
SPSTracker/API/TrackedAnchor
```

- `HeldAnchor`: Held position when tracking is inactive
- `TrackedAnchor`: Final tracked position and rotation after Offset is applied

See [Public API and Parameters](public-api.md) for details.

## Nearest Lock

Nearest Lock narrows the detection range around the current target after tracking begins, reducing target conflicts when multiple Sockets are within the same detection range.

It does not obtain a unique identifier for the target Socket and therefore cannot guarantee that a specific Socket will always be selected. Tracking may also be lost more easily when the Socket moves significantly, such as during large controller movements.

## Performance Estimates

The basic configuration of SPSTracker v1.2.0 has the following estimated cost:

| Item | Estimate |
| --- | ---: |
| Contact Receiver | 6 (7 with Nearest Lock enabled) |
| VRChat Constraint | 11 |
| Animator Layer | 6 |
| Expressions Parameter | 4 parameters, 11 bits |

Each menu-free ProductAdapter adds one VRC Parent Constraint and two included FX Layers. It adds no Contacts, Expression Parameters, or rendered polygons.

The Menu Variant adds three synchronized Bool Parameters (3 bits). `Item Visible`, `Tracking`, and `World Fixed` are independent per ProductAdapter because Modular Avatar automatically renames each instance's final parameters.

After NDMF, Modular Avatar also adds helper layers for MMD compatibility and Object Toggle processing. In a blank-avatar comparison using Unity 2022.3.22f1 and Modular Avatar 1.18.1, the Base ProductAdapter added `+4` FX Layers and the Menu Variant added `+7`. These are reference measurements that include generated helper layers and can vary with the avatar and Modular Avatar version.

Independent Roll Deformation generates one resolver MeshRenderer with 3 vertices and 1 triangle per component. It also generates two FX Animator Layers and four internal Animator Parameters shared by all independent Roll Deformation components on the avatar. These internal parameters are not registered in Expressions Parameters. Runtime Offset adds one FX Animator Layer for each unique parameter name. When Roll Deformation targets the same Renderer as an SPS Plug, it shares the Plug resolver and does not generate an independent resolver for that Renderer.
