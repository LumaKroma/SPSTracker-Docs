# ProductAdapter

[日本語](../product-adapter.md) | English

ProductAdapter is a Prefab for adding SPSTracker tracking support to an existing Modular Avatar-compatible product.

## Included Prefabs

### SPSTracker_ProductAdapter.prefab

Includes a VRC Parent Constraint, an Animator that switches between the held and tracked states, and a top-level `Tracking` menu.

### SPSTracker_TrackingMenuItem.prefab

Adds only the `Tracking` control to an existing product submenu.

`SPSTracker_ProductAdapter.prefab` already includes the same control, so you do not need to add both menu items.

## Basic Setup

1. Add SPSTracker and the target product to the avatar by following their normal installation instructions.
2. Place `SPSTracker_ProductAdapter.prefab` directly under the avatar root.
3. In the VRC Parent Constraint on `Tracking Driver`, assign the root that can move the entire product to `Target Transform`.
4. Adjust `Held Pose (EDIT)` to set the position and rotation used while tracking is inactive.
5. Set the Held-side Weight to 0 and the Tracked-side Weight to 1, then adjust `Tracked Pose (EDIT - FRONT +Z)`.
6. Align the product's tracking origin with the base of the cyan guide and its forward direction with local `+Z`.
7. After adjustment, restore the Held-side Weight to 1 and the Tracked-side Weight to 0.

Do not edit the following reference Transforms:

```text
Held API Anchor (DO NOT EDIT)
Tracked API Anchor (DO NOT EDIT)
```

Use each child `Pose (EDIT)` Transform for position and rotation adjustments.

## Behavior

ProductAdapter automatically switches between Held Pose and Tracked Pose according to `LumaKroma/ST/TrackingStart`. If the Socket is lost, it keeps Tracked Pose for approximately 3 seconds. If the Socket is not detected again, it returns to Held Pose.

## Using Multiple Items

Multiple ProductAdapters can share the same tracking state and public Anchors provided by one SPSTracker. You do not need to add another SPSTracker for each item.

If multiple Targets are visible at the same time, they all track the same Socket simultaneously. Normally, use each product's Object Toggle or selection menu to choose which item is visible and active.

ProductAdapter does not add Contact Receivers or Expressions Parameters, so compatible items can be added while continuing to share the same SPSTracker.

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

## Including ProductAdapter with a Product

Only files in the `ProductAdapter` folder included with the product are covered by the MIT License.

- Keep the copyright notice and full MIT License text.
- A modified ProductAdapter may be included with a product.
- SPSTracker itself and LumaToys may not be included or redistributed.
- Clearly state in the product description that users must install SPSTracker separately.

Always review the `LICENSE.md` included with the actual files you intend to redistribute.
