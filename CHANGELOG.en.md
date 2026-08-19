# Changelog

[日本語](CHANGELOG.md) | English

## v1.2.0 - 2026-08-20

- Added `SPSTracker_ProductAdapterMenu.prefab`, combining `Item Visible`, `Tracking`, and `World Fixed` in one submenu
- Scoped and automatically renamed internal parameters per menu-enabled ProductAdapter so multiple products can be controlled independently
- Kept the menu-free `SPSTracker_ProductAdapter.prefab` free of added Expression Menu controls and Expression Parameters
- Added World Fixed as a separate internal driver isolated from the legacy tracking Animator
- Documented the NDMF build stop when ProductAdapter Setup Assistant is not completed before Play / Build
- Removed `SPSTracker_TrackingMenuItem.prefab`; an in-place upgrade from v1.1 does not automatically delete the old Prefab, so it must be removed and replaced manually
- Made Roll Deformation safely skip invalid or unresolved socket frames without changing positions, normals, or tangents
- Clarified that SPSTracker and dependent ProductAdapters target Windows PC only

## v1.1.0 - 2026-08-12

- Documented ProductAdapter Setup Assistant initialization, Held / Tracked pose editing, and validation
- Added documentation for the optional SPS Tracker Roll Deformation feature
- Documented VRCFury SPS Plug coexistence, independent resolvers, and lilToon compatibility
- Documented Japanese/English ProductAdapter Inspector controls and validation for duplicate configuration and dependency versions
- Updated `SPSTracker_TrackingMenuItem.prefab` to control tracking permission through `TrackingEnabled` instead of `Activate`
- Corrected the additional cost of ProductAdapter and its optional menu item
- Documented the VRCFury 1.1403.0 minimum requirement for Roll Deformation
- Documented the lilToon 1.3.7 minimum requirement for independent Roll Deformation and its version warning
- Documented that missing or outdated Roll Deformation dependencies omit only the affected feature instead of blocking the core avatar build
- Documented automatic tracking-range adjustment based on `HeldRange` and progressive recovery-range expansion after tracking loss
- Documented tracking stabilization that compensates for effective Gain differences caused by the detection range

## v1.0.0 - 2026-07-27

- Created the initial developer documentation for SPSTracker v1.0.0
- Documented the public Anchors, parameters, ProductAdapter, and limitations
- Added English documentation
