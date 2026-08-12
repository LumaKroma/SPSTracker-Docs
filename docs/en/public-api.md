# Public API and Parameters

[日本語](../public-api.md) | English

Only the Anchors and parameters documented on this page should be treated as the public API for compatible items. Undocumented hierarchy objects, parameters, Animators, Animations, and Constraints are not covered by compatibility guarantees and may change between versions.

## Transform API

### HeldAnchor

```text
SPSTracker/API/HeldAnchor
```

The held position used while tracking is inactive. Use it as the connection point for the compatible item's held or standby state.

### TrackedAnchor

```text
SPSTracker/API/TrackedAnchor
```

The position and rotation after Socket tracking and Offset have been applied. Use it as the connection point for the compatible item's tracking state.

For normal installation, do not rename or move either Anchor. When referencing the Anchors from an independent Modular Avatar Prefab, use MA Bone Proxy or a similar component to connect them at build time.

## Menu Parameters

All SPSTracker parameters use the following prefix:

```text
LumaKroma/ST/
```

| Parameter | Type | Default | Purpose |
| --- | --- | --- | --- |
| `LumaKroma/ST/Activate` | Bool | OFF | Enables or disables Socket detection and tracking |
| `LumaKroma/ST/ShowGizmo` | Bool | ON | Shows or hides the local-only arrow and detection range while `Activate` is ON |
| `LumaKroma/ST/Offset` | Float | 0.5 | Adjusts the position along the Socket axis by approximately -10 to +10 cm (`0.5` applies no offset) |
| `LumaKroma/ST/NearestMode` | Bool | OFF | Enables or disables Nearest Lock |

If an add-on controls these parameters, use the existing parameters defined by SPSTracker. Do not register duplicate parameters with the same names in Expressions Parameters.

## Animator Tuning Value

`LumaKroma/ST/HeldRange` is a Float value inside the FX Animator that adjusts the detection range while the item is held. It is not registered in MA Parameters or exposed in the menu. The tracking range and Gain are adjusted automatically from this value and the tracking state, so there are no public `TrackingRange` or `TrackingGain` parameters.

`HeldRange` is intended for users who need to tune the Animator. It is not a public API for compatible products, and add-ons should not read or control it.

## Optional ProductAdapter Parameter

```text
LumaKroma/ST/TrackingEnabled
```

A Bool value that permits ProductAdapter to switch to Tracked Pose.

- `0`: Remain at Held Pose even when `TrackingStart` is ON
- `1`: Switch to Tracked Pose when `TrackingStart` becomes ON

Its default value inside ProductAdapter is ON. A synchronized Bool Parameter with this name is added to Expressions Parameters only when `SPSTracker_TrackingMenuItem.prefab` is used. Multiple ProductAdapters share the same value. Do not register a duplicate copy for every product.

## Tracking State

```text
LumaKroma/ST/TrackingStart
```

A read-only Float value that compatible items can reference. Treat it as `0` or `1` when evaluating the tracking state.

- `0`: Not tracking
- `1`: Tracking, or within the tracking-loss grace period

Do not write to this value externally or add and synchronize it as an Expressions Parameter.

## Recommended Connection

Compatible items should switch their connection point according to `TrackingStart` and the optional `TrackingEnabled` control.

| State | Connection Point |
| --- | --- |
| `TrackingStart = 0` | `HeldAnchor` |
| `TrackingStart = 1` and `TrackingEnabled = 1` | `TrackedAnchor` |
| `TrackingEnabled = 0` | `HeldAnchor` |

See the `SPSTracker_ProductAdapter.prefab` included with the product for an example implementation.
