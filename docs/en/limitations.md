# Limitations and Compatibility

[日本語](../limitations.md) | English

## Tracking Target

- A compatible VRCFury SPS Socket must be enabled on the target avatar.
- SPSTracker follows position, yaw, and pitch.
- It does not follow roll around the Socket axis.

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

## Support Scope

General end-user support covers basic installation with the standard Prefab and ProductAdapter.

Animator modifications required to preserve an existing product's hand-switching or World Lock features must be handled individually because every product has a different structure. Developers who create or sell compatible products should also review the original product author's terms of use and support scope.
