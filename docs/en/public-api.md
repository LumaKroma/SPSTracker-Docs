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
| `LumaKroma/ST/ShowGizmo` | Bool | ON | Shows or hides the local-only gizmo |
| `LumaKroma/ST/Offset` | Float | 0.5 | Adjusts the position along the Socket axis by approximately 0–5 cm |
| `LumaKroma/ST/NearestMode` | Bool | OFF | Enables or disables Nearest Lock |

If an add-on controls these parameters, use the existing parameters defined by SPSTracker. Do not register duplicate parameters with the same names in Expressions Parameters.

## Tracking State

```text
LumaKroma/ST/TrackingStart
```

A read-only Bool value that compatible items can reference.

- `0`: Not tracking
- `1`: Tracking, or within the tracking-loss grace period

Do not write to this value externally or add and synchronize it as an Expressions Parameter.

## Recommended Connection

Compatible items should switch their connection point according to `TrackingStart`.

| State | Connection Point |
| --- | --- |
| Not tracking | `HeldAnchor` |
| Tracking, or within the tracking-loss grace period | `TrackedAnchor` |

See the `SPSTracker_ProductAdapter.prefab` included with the product for an example implementation.
