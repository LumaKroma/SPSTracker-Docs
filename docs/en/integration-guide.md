# Compatible Item Integration Guide

[日本語](../integration-guide.md) | English

Choose an integration method based on the item's structure and how it will be distributed.

## Basic Policy: Share One SPSTracker

Even when multiple compatible items are installed, only one SPSTracker should generally be placed on the avatar. Connect every item to the same public Anchors and `TrackingStart`, then use display or selection menus to choose the active item.

If every item includes its own tracking system equivalent to SPSTracker, Contacts, Constraints, Animator Layers, and Expressions Parameters are duplicated. SPSTracker uses a relatively large number of Contacts, so adding multiple tracking systems to the same avatar may reduce the stability of real-time detection and tracking.

If the existing public API does not provide a switch or state value required by your product, submit a [feature request](../../README.en.md#feature-requests) before building a separate tracking mechanism into the item.

## Choosing an Integration Method

| Method | Best For | Notes |
| --- | --- | --- |
| Place directly under `VisibleRoot` | Simple personal-use models | Not suitable for products with existing gimmicks |
| ProductAdapter | Existing Modular Avatar-compatible products | Uses Setup Assistant; may conflict with existing position controls |
| Custom add-on using the public API | Dedicated compatibility Prefabs for sale or distribution | Requires Animator and Constraint design |

## Adding a Custom Model Directly

Place a personal FBX or Prefab in the following hierarchy:

```text
SPSTracker
└─ WorldFixed
   └─ Object
      └─ VisibleRoot
         ├─ Gizmo
         └─ YourItem
```

1. Place the model under `VisibleRoot`, not under `Gizmo`.
2. Align the model's local `+Z` direction with its forward direction while tracking.
3. Align the tracking origin with the base of the Gizmo arrow.
4. If you enabled `VisibleRoot` and `LocalUI` for adjustment, set them back to inactive before uploading the avatar.

Products with existing Animators, Constraints, or PhysBones may stop functioning correctly when placed directly in this hierarchy. Use ProductAdapter or a custom add-on instead.

## Using ProductAdapter

See [ProductAdapter](product-adapter.md) when adding tracking support to an existing Modular Avatar-compatible product.

In particular, if the product supports hand switching, World Lock, or multiple attachment positions, design the setup so that multiple systems do not control the same Target Transform at the same time.

If roll around the Socket axis is required, configure the optional [Roll Deformation](roll-deformation.md) through ProductAdapter Setup Assistant.

## Creating a Custom Add-on

An independent add-on Prefab retrieves SPSTracker's public Anchors and connects them to a VRC Parent Constraint on the item side.

The following conceptual structure is recommended:

```text
YourAddon
├─ SPSTracker Anchors
│  ├─ Held API Anchor
│  │  └─ Held Pose
│  └─ Tracked API Anchor
│     └─ Tracked Pose (+Z Front)
├─ Tracking Driver
└─ Item Root
```

- Use MA Bone Proxy or a similar component to connect the Held side to `SPSTracker/API/HeldAnchor`.
- Connect the Tracked side to `SPSTracker/API/TrackedAnchor`.
- Connect Held and Tracked to a VRC Parent Constraint whose Target is Item Root.
- Switch between the Held and Tracked connection points according to `LumaKroma/ST/TrackingStart`.
- If users should be able to disable tracking per product, define a Bool inside the add-on's own MA parameter-remap scope, as the ProductAdapter Menu Variant does. Do not depend externally on a ProductAdapter instance's final internal parameter name.
- Do not reference SPSTracker elements that are not documented in the [public API](public-api.md).
- Multiple add-ons should continue to share the public API of the same SPSTracker.

See [Public API and Parameters](public-api.md) for the exact API names.

## Pre-Distribution Checklist

- [ ] SPSTracker itself is not included with the product
- [ ] SPSTracker or an equivalent tracking mechanism is not duplicated for every item
- [ ] The product description states that users must install SPSTracker separately
- [ ] Users can switch which item is displayed and active when multiple items are installed
- [ ] The item's local `+Z` direction faces forward while tracking
- [ ] The item's position has been checked in both Held and Tracked states
- [ ] The item returns to Held after the tracking-loss grace period expires
- [ ] Operation has been checked with Nearest Lock both ON and OFF
- [ ] Multiple Constraints do not control the same Target Transform
- [ ] A per-product Tracking menu is inside an MA remap scope and does not share its parameter with another instance
- [ ] ProductAdapter's `Menu` hierarchy and internal parameter names remain unchanged
- [ ] Setup Assistant was completed and returned to Held before Play / Build
- [ ] The product targets VRChat on Windows PC
- [ ] When Roll Deformation is used, the supported shader, Roll Axis, and Runtime Offset have been checked
- [ ] No Renderer is assigned to more than one Roll Deformation component
- [ ] The applicable license is included with redistributed files
- [ ] Drag-and-drop installation has been tested in a new avatar project
