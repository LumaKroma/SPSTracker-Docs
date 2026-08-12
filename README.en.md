# SPSTracker Developer Documentation

[日本語](README.md) | English

Technical documentation for creators who want to build compatible items and gimmicks using SPSTracker.

SPSTracker itself is not included in this repository. Users must obtain it separately from the [BOOTH product page](https://lumakroma.booth.pm/items/8623609).

## Scope of This Repository

- Add a custom model to SPSTracker
- Connect an existing Modular Avatar-compatible product through ProductAdapter
- Create compatible items using the public Anchors and tracking-state parameter
- Add roll tracking around the Socket axis through ProductAdapter
- Include ProductAdapter-related files with a product under the terms of the MIT License

> [!IMPORTANT]
> SPSTracker is designed so that one SPSTracker on an avatar can be shared by multiple compatible items, with the active item selected through the product's display or selection menu. Adding a separate tracking system for every item is not recommended. See [Designing for Multiple Items to Share One Tracker](docs/en/system-overview.md#designing-for-multiple-items-to-share-one-tracker) for details.

For end-user installation instructions, see the [illustrated installation guide (Japanese)](https://docs.google.com/document/d/1uJOBLdxwRoDBxQUnmw-w6q-cMbQ8GVmhe8odOjaHqkc/edit?tab=t.0#heading=h.24aznar30yba).

## Documentation

- [Documentation Index](docs/en/README.md)
- [System Overview](docs/en/system-overview.md)
- [Public API and Parameters](docs/en/public-api.md)
- [ProductAdapter](docs/en/product-adapter.md)
- [Roll Deformation](docs/en/roll-deformation.md)
- [Compatible Item Integration Guide](docs/en/integration-guide.md)
- [Limitations and Compatibility](docs/en/limitations.md)
- [Troubleshooting](docs/en/troubleshooting.md)

## Supported Versions

This documentation is based on SPSTracker `v1.1.0`.

- Unity 2022.3.22f1
- VRChat SDK - Avatars
- Modular Avatar
- VRChat Constraints
- VRCFury 1.1403.0 or newer (when using Roll Deformation)
- lilToon 1.3.7 or newer (for independent Roll Deformation)

See the end-user installation guide for the package versions that have been tested individually.

## Feature Requests

If you need an additional public API, switching method, state value, or other feature for a product or gimmick you want to create, please contact LumaKroma.

Requests that could benefit multiple creators may be considered for inclusion in SPSTracker or its public API. Submit requests through [GitHub Issues](https://github.com/LumaKroma/SPSTracker-Docs/issues) or the [BOOTH contact page](https://lumakroma.booth.pm/).

## License

See [LICENSE.en.md](LICENSE.en.md) for an English translation of the license information.

- The documentation in this repository and the files included with the commercial product are not covered by the same license.
- Only files in the ProductAdapter folder that explicitly state the MIT License may be modified, included with a product, and redistributed under the MIT License.
- SPSTracker itself and LumaToys are not covered by the MIT License.
- Third-party works remain subject to their respective licenses.

Also review [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) for third-party works.

## Related Links

- [BOOTH product page](https://lumakroma.booth.pm/items/8623609)
- [Illustrated installation guide (Japanese)](https://docs.google.com/document/d/1uJOBLdxwRoDBxQUnmw-w6q-cMbQ8GVmhe8odOjaHqkc/edit?tab=t.0#heading=h.24aznar30yba)
- [Contact LumaKroma on BOOTH](https://lumakroma.booth.pm/)
