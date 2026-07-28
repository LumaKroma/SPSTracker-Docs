# Troubleshooting

[日本語](../troubleshooting.md) | English

## The SPSTracker Menu Does Not Appear

- Confirm that Modular Avatar is installed.
- Confirm that `SPSTracker.prefab` is directly under the avatar root, not inside the Armature.
- Resolve any red errors in the Unity Console.
- Confirm that the avatar was uploaded again after making the changes.

## The Item Does Not Track a Nearby Socket

- Turn `Activate` ON in the SPSTracker menu.
- Confirm that a compatible SPS Socket is enabled on the other avatar.
- Move the item within approximately 10 cm of the Socket Front.
- Turn `Nearest Lock` OFF temporarily and test again.
- Reload the avatar and test again.
- Turn `Show Gizmo` ON and check the detection range and direction.

## The Item Faces the Wrong Direction

Confirm that the item's local `+Z` direction is configured as its forward direction while tracking.

- Model added directly: rotate the model under `VisibleRoot`
- ProductAdapter: rotate `Tracked Pose (EDIT - FRONT +Z)`

`Offset` adjusts position along the axis and should not be used to correct orientation.

## The Item Returns to the Hand While Tracking

SPSTracker may have been unable to detect the Socket again for approximately 3 seconds.

- Rapid Socket movement
- Different avatar scales
- Many overlapping Contacts
- Network conditions or frame rate
- Detection range reduction caused by Nearest Lock
- The Socket was disabled on the other avatar

## The Product Does Not Move with ProductAdapter

- Confirm that `Target Transform` is assigned on the `Tracking Driver`.
- Assign a root Transform that can move the entire product.
- Confirm that no other Constraint, Animator, or PhysBone controls the same Transform.
- Confirm that `Held API Anchor (DO NOT EDIT)` and `Tracked API Anchor (DO NOT EDIT)` have not been edited.
- After adjustment, confirm that the Held-side Weight is 1 and the Tracked-side Weight is 0.

## Hand Switching or World Lock Does Not Work

The standard ProductAdapter controls the product from Held Pose even while tracking is inactive. A conflict occurs if an existing feature controls the same Transform.

- If the existing feature is not required: disable the product's original position control and adjust Held Pose.
- If the existing feature must be preserved: modify the product-specific Animator so the controlling system switches according to `LumaKroma/ST/TrackingStart`.

## Information to Include When Requesting Support

- Unity version
- VRChat SDK version
- Modular Avatar version
- VRCFury and lilToon versions, if used
- Screenshot of the complete Hierarchy
- Unity Console error messages
- Steps required to reproduce the problem
- Versions of SPSTracker and the compatible product

Contact: https://lumakroma.booth.pm/
