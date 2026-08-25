# Roll Deformation

[日本語](../roll-deformation.md) | English

SPS Tracker Roll Deformation reads the Socket Up direction from the SPS2 resolver and rigidly rotates ProductAdapter Renderers around the Socket forward axis. It adds roll tracking to the position, yaw, and pitch tracking provided by the SPSTracker core.

Unlike an SPS Plug, which bends a mesh according to vertex depth, Roll Deformation rotates the position, normal, and tangent of every affected vertex by the same angle.

## Requirements

- Unity 2022.3.22f1
- NDMF / Modular Avatar
- VRCFury 1.1403.0 or newer
- lilToon 1.3.7 or newer (the supported shader for independent operation)

If VRCFury is missing or older than 1.1403.0, a warning is shown and Roll Deformation components are removed from the build copy of the avatar. If lilToon is missing or older than 1.3.7, only Roll Deformation components that include independent operation are omitted in the same way. Components that exclusively share SPS Plug resolvers are not subject to the lilToon requirement.

The remaining avatar build, including the SPSTracker core, continues after omission. The original components in the scene or Prefab are not deleted, so updating the dependency re-enables them on the next build without reconfiguration. If a version cannot be evaluated, the build continues with a warning.

## Operating Modes

### Independent Resolver

When a target Renderer is not used by a VRCFury SPS Plug, Roll Deformation generates an SPS2 resolver at build time and reads the Socket Forward and Up directions directly.

- Uses the Roll Axis configured on Roll Deformation.
- Uses Resolver Length, Resolver Radius, and the self/other Socket permissions.
- Officially supports standard lilToon shader variants.
- Does not modify the source Material asset. A Roll-compatible Material is generated during the avatar build.

Custom shaders and shaders derived from lilToon are unsupported because their original appearance may not be preserved.

### With an SPS Plug

When the same Renderer is used by a VRCFury SPS Plug, Roll Deformation uses the Plug's axis and resolver settings. Roll is applied through VRCFury's `SPS_MODIFY_BAKE` hook.

- Roll Axis, Resolver Length, Resolver Radius, and Socket permissions on Roll Deformation are not used for that Renderer.
- Target Renderers and Runtime Offset remain active.
- Test the specific shader and SPS Plug combination in VRChat before distribution.

## Automatic Setup with Setup Assistant

Automatic setup is recommended for ProductAdapter.

1. Assign Product Root in `ProductAdapter Setup Assistant`.
2. Adjust the Held and Tracked poses.
3. Enable `Auto Configure Roll`.
4. Run `Initialize / Repair Setup` or `Complete Setup and Return to Held`.

Automatic setup uses the Tracked guide position as the roll pivot, local `+Z` as the Socket direction, and world `+Y` as roll up. MeshRenderers and SkinnedMeshRenderers under Product Root are collected automatically.

Manual Roll Deformation controls are disabled while automatic setup is enabled. Disable `Auto Configure Roll` when individual adjustment is required.

## Manual Setup

The main fields on `SPS Tracker Roll Deformation` are:

| Field | Purpose |
| --- | --- |
| Runtime Offset Parameter | Optional Float Parameter for adjusting roll at runtime |
| Runtime Offset Default | Initial Runtime Offset value; `0.5` applies no correction |
| Roll Axis | Position is the rigid roll pivot, `+Z` faces the Socket, and `+Y` defines roll up |
| Resolver Length | Maximum Socket acquisition distance; default `0.3 m` |
| Resolver Radius | Virtual radius supplied to Radius Offset Sockets; default `0.03 m` |
| Target Renderers | Renderers rotated rigidly by the same angle |
| Allow Self Sockets | Allows Sockets owned by the local avatar |
| Allow Other Sockets | Allows Sockets owned by other players |

If the same Renderer is assigned to more than one Roll Deformation component, the avatar build reports an error and does not process that Renderer.

## Runtime Offset

Socket Up does not have a universal orientation convention across avatars, so some Sockets may require correction. When Runtime Offset Parameter is assigned, it maps to the following range:

| Float Value | Correction |
| ---: | ---: |
| `0` | `-180°` |
| `0.5` | `0°` |
| `1` | `+180°` |

Roll Deformation generates the Animator Parameter and Blend Tree but does not add it to Expressions Parameters or a menu. To control it in VRChat, add the same Float Parameter to MA Parameters and a Radial Puppet.

## Limitations

- Roll is a Renderer-level shader deformation. It does not rotate Transforms, Colliders, PhysBones, or Constraints.
- The Roll resolver and SPSTracker core resolve Sockets independently. When multiple Sockets are close together, position tracking and roll tracking may select different Sockets.
- Socket Up is stable during runtime but may not have been configured to an intentional orientation by the Socket creator.
- Normals rotate with the mesh, so lighting and MatCap effects that depend on world direction may look different after roll is applied.
- If the Socket frame position or axes are zero, NaN, Infinity, degenerate, or otherwise unresolved, that frame is safely skipped without changing vertex positions, normals, or tangents. This does not repair invalid setup; check the Resolver and Roll Axis if it persists.

See [Troubleshooting](troubleshooting.md) for symptom-based checks.
