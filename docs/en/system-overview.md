# System Overview

[日本語](../system-overview.md) | English

SPSTracker is a VRChat avatar gimmick that makes an item on your avatar track another player's VRCFury SPS Socket.

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

When SPSTracker detects an enabled SPS Socket, it tracks the Socket's position, yaw, and pitch. It does not track roll around the Socket axis.

- Configure the compatible item's local `+Z` direction as its forward direction while tracking.
- If the Socket is lost, SPSTracker holds the last position and rotation for approximately 3 seconds while attempting to detect it again.
- If the Socket is not detected again within approximately 3 seconds, the item returns to its held position.

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

The basic configuration of SPSTracker v1.0.0 has the following estimated cost:

| Item | Estimate |
| --- | ---: |
| Contact Receiver | 6 (7 with Nearest Lock enabled) |
| VRChat Constraint | 11 |
| Animator Layer | 6 |
| Expressions Parameter | 4 parameters, 11 bits |

Each ProductAdapter adds one VRC Parent Constraint and one Animator Layer. It does not add Contacts, Expressions Parameters, or rendered polygons.
