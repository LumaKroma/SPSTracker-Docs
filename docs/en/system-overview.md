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

SPSTracker uses its normal six-Receiver configuration. If each compatible product includes its own tracking mechanism and adds a similar set of Contacts, the total number of Contacts on the avatar increases. Overlapping Contacts and additional processing load may reduce detection accuracy, tracking stability, or real-time responsiveness.

Custom implementations are not prohibited, but compatible products are encouraged to use the public Anchors and `TrackingStart` so that one SPSTracker can be shared. If the public API does not provide a state value or switching feature required by your project, submit a [feature request](../../README.en.md#feature-requests).

## Tracking Behavior

When SPSTracker detects an enabled SPS Socket, the core system tracks the Socket's position, yaw, and pitch. The optional [SPS Tracker Roll Deformation](roll-deformation.md) feature in ProductAdapter can add roll around the Socket axis to selected Renderers.

- Configure the compatible item's local `+Z` direction as its forward direction while tracking.
- The held-state detection range can be tuned with `HeldRange` inside the FX Animator. After tracking starts, the detection range is adjusted automatically so it does not become smaller than the held range.
- SPSTracker compensates for effective Gain differences caused by the detection range, keeping the tracking response relatively consistent as the range changes.
- If the Socket is lost, SPSTracker holds the last position and rotation for approximately 3 seconds. During that period, it expands the recovery range progressively to its maximum over approximately 1 second.
- If the Socket is detected again during the grace period, tracking continues. After holding for up to approximately 0.25 seconds, the detection range returns to normal over approximately 1 second. Otherwise, the item returns to its held position after approximately 3 seconds. These are typical timings and can vary with the avatar, Socket, and network conditions.

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

## Composition and Cost

SPSTracker uses its normal six-Receiver configuration. The ProductAdapter Menu Variant adds three synchronized Bool parameters (3 bits) per product, with Modular Avatar remapping final parameter names per instance.

Final Animator Layers, Constraints, Contacts, Expressions Parameter usage, and rendering cost vary with the target avatar, dependency packages, NDMF output, and optional features. Fixed performance values are not part of the public compatibility contract; inspect the Build result after installation.
