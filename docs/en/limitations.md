# Limitations and Compatibility

[日本語](../limitations.md) | English

## Tracking Target

- A compatible VRCFury SPS Socket must be enabled on the target avatar.
- SPSTracker follows position, yaw, and pitch.
- The SPSTracker core does not follow roll around the Socket axis. The optional ProductAdapter [Roll Deformation](roll-deformation.md) feature is required.

## Multiple Sockets

When multiple Sockets are within the same detection range, SPSTracker cannot guarantee that every axis will always select the same Socket.

Nearest Lock narrows the detection range after tracking begins to reduce the chance of switching to another Socket. It does not uniquely identify a Socket, so another Socket may still be detected in the following situations:

- Multiple Sockets are extremely close together
- Many Contacts overlap within the same area
- The Socket or controller moves rapidly

## Factors That Affect Tracking Accuracy

- Avatar scale
- Network conditions
- Frame rate
- Socket configuration
- Overlapping Contacts
- Reduced detection range caused by Nearest Lock

Operation is not guaranteed on every avatar, Socket, or World.

## ProductAdapter Conflicts

ProductAdapter controls the position and rotation of its Target Transform with a VRC Parent Constraint. If any of the following also controls the same Transform, the result may include offsets, shaking, unintended movement, or loss of an existing feature:

- Another Parent Constraint or Position Constraint
- An Animator that changes the Transform
- Hand switching or World Lock
- Some PhysBone configurations

If a conflict occurs, add a dedicated movable root or switch the controlling system according to the tracking state.

## Roll Deformation

- Requires VRCFury 1.1403.0 or newer. If it is missing or too old, Roll Deformation is omitted and the remaining avatar build continues.
- Independent operation officially supports standard lilToon shaders from lilToon 1.3.7 or newer. If lilToon is missing or too old, Roll Deformation components that include independent operation are omitted.
- The lilToon minimum-version requirement does not apply to Roll Deformation components that exclusively share SPS Plug resolvers.
- The independent Roll resolver and SPSTracker core resolve Sockets separately, so they may select different Sockets when several are close together.
- Socket Up has no universal convention across avatars. Some Sockets require Runtime Offset correction.
- It rigidly rotates Renderer vertices in the shader and does not rotate Transforms, Colliders, PhysBones, or Constraints.
- A Renderer cannot be controlled by more than one Roll Deformation component.

See [Roll Deformation](roll-deformation.md) for details.

## Support Scope

General end-user support covers basic installation with the standard Prefab and ProductAdapter.

Animator modifications required to preserve an existing product's hand-switching or World Lock features must be handled individually because every product has a different structure. Developers who create or sell compatible products should also review the original product author's terms of use and support scope.
