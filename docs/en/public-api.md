# Public API and Parameters

English | [日本語](../public-api.md)

Treat only the Anchors and parameters documented here as the public API for compatible items. Undocumented hierarchy, Animators, Animations, Constraints, and ProductAdapter internal parameters are not compatibility contracts.

## Transform API

### HeldAnchor

```text
SPSTracker/API/HeldAnchor
```

The holding position used while tracking is inactive. Use it for an item's held or standby state.

### TrackedAnchor

```text
SPSTracker/API/TrackedAnchor
```

The final position and rotation after Socket tracking and Offset. Use it for an item's tracked state.

Do not rename or move these Anchors in a normal installation. An independent Modular Avatar Prefab should connect to them at build time through MA Bone Proxy or an equivalent method.

## SPSTracker Menu Parameters

All SPSTracker parameters use this prefix:

```text
LumaKroma/ST/
```

| Parameter | Type | Default | Purpose |
| --- | --- | --- | --- |
| `LumaKroma/ST/Activate` | Bool | OFF | Enables Socket detection and tracking |
| `LumaKroma/ST/ShowGizmo` | Bool | ON | Toggles the local arrow and detection range while `Activate` is on |
| `LumaKroma/ST/Offset` | Float | 0.5 | Adjusts about -10 to +10 cm along the Socket axis; 0.5 is neutral |

Use the parameters already defined by SPSTracker. Do not register duplicate Expression Parameters with the same names.

## Tracking State

```text
LumaKroma/ST/TrackingStart
```

This is a read-only Float available to compatible items. Treat it as 0 or 1 for state decisions.

- `0`: tracking is not established
- `1`: tracking is active or in the tracking-loss grace period

Do not write to it externally or add it as a synchronized Expression Parameter.

## Internal Animator Adjustment

`LumaKroma/ST/HeldRange` is an internal FX Animator Float that adjusts the detection range while held. It is not registered in MA Parameters or the menu. It is not a public integration API; add-ons must not read or control it.

## ProductAdapter Internal Parameters

ProductAdapter v1.2 authors the following internal parameter names:

| Authored Parameter | Type | Default | Purpose |
| --- | --- | ---: | --- |
| `LumaKroma/ST/ProductAdapter/ItemVisible` | Bool | OFF | Product visibility |
| `LumaKroma/ST/TrackingEnabled` | Bool | ON | Permission to switch to Tracked Pose |
| `LumaKroma/ST/ProductAdapter/WorldFixed` | Bool | OFF | World-freezes ProductAdapter's Constraint |

These are ProductAdapter implementation details. In the Menu Variant, Modular Avatar automatically renames the final parameters per instance.

- Do not depend on the final built names from an external Animator.
- Do not manually add duplicate Expression Parameters.
- Keep the ProductAdapter `Menu` hierarchy inside its instance.
- Do not rename the internal parameters.

The Base Prefab keeps `TrackingEnabled` and `WorldFixed` as internal Animator values but adds no Expression Parameter or menu. Only the Menu Variant adds three synchronized Bool parameters.

## Recommended Connection

Compatible items switch between Held and Tracked using `TrackingStart`.

| State | Connection |
| --- | --- |
| `TrackingStart = 0` | `HeldAnchor` |
| `TrackingStart = 1` | `TrackedAnchor` |

If users need to disable tracking per product, define a parameter inside the add-on's own remapped scope, as the ProductAdapter Menu Variant does. Do not reference a ProductAdapter instance's final internal name from outside it.

See the included `SPSTracker_ProductAdapter.prefab` for an implementation example.
