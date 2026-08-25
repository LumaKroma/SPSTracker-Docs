# ProductAdapter

English | [日本語](../product-adapter.md)

ProductAdapter adds SPSTracker tracking to an existing Modular Avatar-compatible product. Like SPSTracker itself, it targets VRChat on Windows PC.

## Included Prefabs

Place only one Prefab that matches the product's use case.

### `SPSTracker_ProductAdapter.prefab`

The menu-free base configuration includes:

- one VRC Parent Constraint
- the legacy Held / Tracked tracking Animator
- a separate internal World Fixed Animator
- ProductAdapter Setup Assistant
- no added Expression Menu control or Expression Parameter

### `SPSTracker_ProductAdapterMenu.prefab`

This is a Prefab Variant of the base. Its `Product Adapter` submenu contains:

| Control | Default | Behavior |
| --- | --- | --- |
| `Item Visible` | OFF | Toggles the assigned product root |
| `Tracking` | ON | Allows switching to Tracked Pose |
| `World Fixed` | OFF | Freezes the `Tracking Driver` VRC Parent Constraint in world space |

All three controls are synced and not saved. Modular Avatar automatically renames their internal parameters per ProductAdapter instance, so multiple products can be controlled independently.

## Basic Setup

1. Install SPSTracker and the product normally.
2. Place one appropriate ProductAdapter Prefab directly under the avatar root.
3. In `ProductAdapter Setup Assistant`, assign a Transform that moves the complete product to `Product Root`.
4. Click `Initialize / Repair Setup`.
5. Use `Edit Held Pose` to set the non-tracked position and rotation.
6. Click `Copy Held to Tracked`, then open `Edit Tracked Pose`.
7. Place the tracking origin at the cyan guide root and face the product toward local `+Z`.
8. Enable `Auto Configure Roll` when needed. See [Roll Deformation](roll-deformation.md).
9. Click `Complete Setup and Return to Held`, then resolve every Inspector error.

> [!IMPORTANT]
> If Play / Build starts while the assistant still reports `Editing Held pose` or `Editing Tracked pose`, NDMF stops the build as an incomplete ProductAdapter setup. Always complete the setup and return to Held first.

Do not directly edit these reference Transforms:

```text
Held API Anchor (DO NOT EDIT)
Tracked API Anchor (DO NOT EDIT)
```

## Configuring the Menu Variant

Assign the product root that should be shown or hidden to the MA Object Toggle at `Menu/Product Adapter/Item Visible`.

To install into an existing menu, set `Install Target Menu` on the MA Menu Installer at `Menu/Product Adapter`. Leave it empty to install a `Product Adapter` submenu into the avatar root menu.

Menu labels, icons, the MA Object Toggle target, and Install Target Menu may be changed. Keep the `Menu` hierarchy under its ProductAdapter and do not rename internal parameters. Moving the hierarchy outside the instance or renaming parameters removes them from the per-instance remap contract.

## Behavior

ProductAdapter reads the public, read-only SPSTracker state `LumaKroma/ST/TrackingStart`.

- Base Prefab: the Animator default for `TrackingEnabled` is ON, so it switches automatically when tracking starts.
- Menu Variant: it switches to Tracked Pose only while `Tracking` is ON and `TrackingStart` is active.
- `World Fixed`: freezes the ProductAdapter `Tracking Driver` at its current world position and rotation. It can conflict with another Constraint or Animator that controls the same Transform.

After losing the Socket, ProductAdapter keeps Tracked Pose for about three seconds. It continues tracking if the Socket is found again and otherwise returns to Held Pose.

## Multiple Items

Multiple ProductAdapters share one SPSTracker's `HeldAnchor`, `TrackedAnchor`, and `TrackingStart`. Do not add a full SPSTracker per item.

The Menu Variant's three internal parameters are isolated per instance. When multiple targets are visible, all of them follow the same Socket, so normally use `Item Visible` or the product's own selection controls to show only the active product.

## In-place Upgrade from v1.1

Importing v1.2 over the v1.1 unitypackage does not make UnityPackage delete the retired `SPSTracker_TrackingMenuItem.prefab`.

1. Back up the avatar.
2. Remove old `SPSTracker_TrackingMenuItem` instances from the Hierarchy.
3. Delete the old Prefab that remains in the Project.
4. For products that need controls, replace the ProductAdapter with `SPSTracker_ProductAdapterMenu.prefab` and configure Setup Assistant again.
5. Use the Base Prefab when no menu is needed.

Do not use the old Tracking Menu Item together with the new Menu Variant.

## Target Transform and Existing Features

Assign a movable root that contains the Mesh and every child object that should track. Another Constraint, Animator, or PhysBone controlling the same Transform can cause offsets or vibration.

The Menu Variant's World Fixed control freezes only ProductAdapter's own Constraint. It does not automatically integrate a product's hand switching, attachment switching, Pickup animations, or other position controls. Add a dedicated movable root when necessary.

## Added Cost

The stable product-authored cost is:

| Configuration | Contacts | Expression Parameters | Rendering | VRC Parent Constraints | Included FX Layers |
| --- | ---: | ---: | ---: | ---: | ---: |
| Base Prefab | 0 | 0 | 0 | 1 | 2 |
| Menu Variant | 0 | 3 Bool / 3 bits | 0 | 1 | Base 2 + MA Object Toggle generation |

After NDMF, Modular Avatar adds helper layers for MMD compatibility and Object Toggle processing. In a blank-avatar comparison using Unity 2022.3.22f1 and Modular Avatar 1.18.1, the Base added `+4` FX Layers and the Menu Variant added `+7`. These measured totals include generated helper layers, can vary with Modular Avatar and avatar configuration, and are not a fixed public API contract.

See [Roll Deformation](roll-deformation.md) for its additional cost.

## Bundling and Redistribution

Only files in the product's `ProductAdapter` folder for which LumaKroma holds the rights are covered by the MIT License.

- Keep the copyright notice and full MIT License text.
- A modified ProductAdapter may be bundled with a product.
- The SPSTracker core and LumaToys may not be bundled or redistributed.
- State in the product description that the user must separately install SPSTracker for Windows PC.

Always check the `LICENSE.md` included with the actual files being distributed.
