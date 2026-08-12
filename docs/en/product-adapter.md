# ProductAdapter

[日本語](../product-adapter.md) | English

ProductAdapter is a Prefab for adding SPSTracker tracking support to an existing Modular Avatar-compatible product.

## Included Prefabs

### SPSTracker_ProductAdapter.prefab

Includes a VRC Parent Constraint, an Animator that switches between Held and Tracked states, and ProductAdapter Setup Assistant. It does not include a menu control.

### SPSTracker_TrackingMenuItem.prefab

Adds only the `Tracking` control to an existing product submenu.

When OFF, the product remains at Held Pose. When ON, automatic tracking is allowed when `TrackingStart` is detected. Multiple ProductAdapters share the same `LumaKroma/ST/TrackingEnabled` value.

## Basic Setup

1. Add SPSTracker and the target product to the avatar by following their normal installation instructions.
2. Place `SPSTracker_ProductAdapter.prefab` directly under the avatar root.
3. In `ProductAdapter Setup Assistant` on the Prefab root, assign a Transform that moves the complete product to `Product Root`.
4. Run `Initialize / Repair Setup`.
5. Click `Edit Held Pose` and adjust the position and rotation used while tracking is inactive.
6. Click `Copy Held Pose to Tracked`, then click `Edit Tracked Pose`.
7. Align the product's tracking origin with the base of the cyan guide and its forward direction with local `+Z`.
8. If needed, enable `Auto Configure Roll`. See [Roll Deformation](roll-deformation.md) for details.
9. Run `Complete Setup and Return to Held`, then resolve any errors reported by Inspector validation.

Setup Assistant uses the system language by default. Select Japanese or English from `Language` at the top of the Inspector.

Do not edit the following reference Transforms:

```text
Held API Anchor (DO NOT EDIT)
Tracked API Anchor (DO NOT EDIT)
```

Normally, select each `Pose (EDIT)` Transform through the Setup Assistant buttons. You do not need to edit Constraint Source weights directly.

## Behavior

ProductAdapter switches from Held Pose to Tracked Pose only while both `LumaKroma/ST/TrackingStart` and `LumaKroma/ST/TrackingEnabled` are ON. If the Socket is lost, it keeps Tracked Pose for approximately 3 seconds while SPSTracker expands the recovery range over approximately 1 second. Tracking continues if the Socket is detected again; otherwise, ProductAdapter returns to Held Pose.

The Animator default for `TrackingEnabled` is ON. A product that does not need a menu control can omit `SPSTracker_TrackingMenuItem.prefab` and continue to track automatically.

## Using Multiple Items

Multiple ProductAdapters can share the same tracking state and public Anchors provided by one SPSTracker. You do not need to add another SPSTracker for each item.

If multiple Targets are visible at the same time, they all track the same Socket simultaneously. Normally, use each product's Object Toggle or selection menu to choose which item is visible and active.

ProductAdapter itself does not add Contact Receivers or Expressions Parameters, so compatible items can be added while continuing to share the same SPSTracker. The optional Tracking Menu Item adds one shared synchronized Bool Parameter.

## Choosing the Target Transform

Assign a movable root that contains every child object that should follow the tracking movement, not just the product's Mesh.

Conflicts may occur if another Constraint, Animator, or PhysBone directly controls the same Transform. If necessary, create a dedicated root GameObject that groups the movable parts of the product.

## Combining with Hand Switching or World Lock

The standard ProductAdapter continues to control the Target Transform from Held Pose while tracking is inactive. As a result, the following features are not preserved automatically when they control the same Transform:

- Switching between left and right hands
- World Lock
- Switching between attachment positions
- Animations that change the pickup position
- Product-specific Parent Constraints or Position Constraints

To preserve these features, a product-specific Animator setup is required: the product controls the Transform while tracking is inactive, and ProductAdapter controls it only while `TrackingStart = 1`. Placing the Prefab alone is not sufficient.

## Additional Cost

Estimated cost per ProductAdapter:

- Contact Receiver: none
- Expressions Parameter: none
- Rendered polygons: none
- VRC Parent Constraint: 1
- Animator Layer: 1

The optional `SPSTracker_TrackingMenuItem.prefab` adds one synchronized Bool Parameter (1 bit). See [Roll Deformation](roll-deformation.md) for its additional cost.

## Including ProductAdapter with a Product

Only files in the `ProductAdapter` folder included with the product are covered by the MIT License.

- Keep the copyright notice and full MIT License text.
- A modified ProductAdapter may be included with a product.
- SPSTracker itself and LumaToys may not be included or redistributed.
- Clearly state in the product description that users must install SPSTracker separately.

Always review the `LICENSE.md` included with the actual files you intend to redistribute.
