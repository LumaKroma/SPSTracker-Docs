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
- Move the Socket Front inside the blue detection range shown by `Show Gizmo`.
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

After tracking is lost, SPSTracker expands the detection range to its maximum over approximately 1 second while attempting to reacquire the Socket. If reacquisition succeeds, tracking continues and the range returns gradually to its normal value.

- Rapid Socket movement
- Different avatar scales
- Many overlapping Contacts
- Network conditions or frame rate
- Detection range reduction caused by Nearest Lock
- The Socket was disabled on the other avatar

## The Product Does Not Move with ProductAdapter

- Assign Product Root in `ProductAdapter Setup Assistant`, then run `Initialize / Repair Setup`.
- When using the Menu Variant, turn `Tracking` ON.
- The Base Prefab switches automatically when `TrackingStart` is active. The Menu Variant requires both active `TrackingStart` and `Tracking` ON.
- Confirm that no other Constraint, Animator, or PhysBone controls the same Transform.
- Confirm that `Held API Anchor (DO NOT EDIT)` and `Tracked API Anchor (DO NOT EDIT)` have not been edited.
- Resolve warnings and errors shown by Setup Assistant validation.

## Play / Build Stops with ProductAdapter

- Click `Complete Setup and Return to Held` in Setup Assistant.
- Confirm that neither `Editing Held pose` nor `Editing Tracked pose` remains displayed.
- Repair the Product Root, Tracking Driver, and Held / Tracked Pose references.

## Tracking Controls Are Duplicated After Upgrading from v1.1

UnityPackage does not automatically delete the retired `SPSTracker_TrackingMenuItem.prefab`. Remove the old Prefab from both the Hierarchy and Project, then replace the ProductAdapter with `SPSTracker_ProductAdapterMenu.prefab` when controls are needed.

## Roll Tracking Does Not Work

- Confirm that VRCFury is version 1.1403.0 or newer.
- For independent operation, confirm that lilToon is version 1.3.7 or newer. If a dependency is missing or too old, a warning is shown and only the affected Roll Deformation is omitted from the build.
- Confirm that `SPS Tracker Roll Deformation` is enabled and the target Renderer is listed in Target Renderers.
- For automatic setup, enable `Auto Configure Roll` and run Initialize or Complete Setup.
- For independent operation, confirm that the source Material uses a standard lilToon shader.
- Check the Unity Console for Roll Deformation build errors.

## Roll Has the Wrong Orientation

- Confirm that Roll Axis `+Z` points toward the Socket and `+Y` defines roll up.
- Automatic setup uses the Tracked guide `+Z` and world `+Y` as its reference.
- Socket Up is not standardized across avatars, so correct it with Runtime Offset.
- If the same Renderer uses an SPS Plug, check the Plug axis instead of the Roll Deformation axis.

## Roll Deformation Changes the Appearance

Independent operation officially supports standard lilToon shaders from lilToon 1.3.7 or newer. A custom shader or shader derived from lilToon may not preserve its original rendering. If lilToon itself is missing or too old, Roll Deformation components that include independent operation are omitted from the build.

- Confirm that the source Material uses a standard lilToon shader.
- Edit the source Material, not the generated build Material.
- Normals rotate with the mesh, so changes in world-direction-dependent lighting or MatCap effects may be expected.

## Hand Switching or World Lock Does Not Work

The standard ProductAdapter controls the product from Held Pose even while tracking is inactive. A conflict occurs if an existing feature controls the same Transform.

- If the existing feature is not required: disable the product's original position control and adjust Held Pose.
- If the existing feature must be preserved: modify the product-specific Animator so the controlling system switches according to `LumaKroma/ST/TrackingStart`.
- When using the Menu Variant's `World Fixed`: only ProductAdapter's `Tracking Driver` is frozen. Do not let another World Lock system control the same Transform simultaneously.

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
