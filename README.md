# SPSTracker Developer Documentation

日本語 | [English](README.en.md)

SPSTrackerを使用したアイテムや対応ギミックを制作する方向けの技術資料です。

SPSTracker本体はこのリポジトリに含まれません。利用者は[BOOTH商品ページ](https://lumakroma.booth.pm/items/8623609)から別途導入してください。

## このリポジトリの対象

- 任意のモデルをSPSTrackerへ組み込む
- 既存のModular Avatar対応商品をProductAdapterへ接続する
- 公開Anchorと状態パラメータを使用して対応アイテムを制作する
- ProductAdapter関連ファイルをMIT Licenseの条件で商品へ同梱する

> [!IMPORTANT]
> SPSTrackerは、1つのアバターに配置した1つのSPSTrackerを複数の対応アイテムで共有し、使用するアイテムを切り替える構成を想定しています。アイテムごとに追従システムを重複して組み込む構成は推奨しません。詳しくは[複数アイテムで共有する設計](docs/system-overview.md#複数アイテムで共有する設計)を参照してください。

購入者向けの導入方法は、[画像付き導入手順書](https://docs.google.com/document/d/1uJOBLdxwRoDBxQUnmw-w6q-cMbQ8GVmhe8odOjaHqkc/edit?tab=t.0#heading=h.24aznar30yba)を参照してください。

## ドキュメント

- [ドキュメント索引](docs/README.md)
- [システム概要](docs/system-overview.md)
- [公開API・パラメータ](docs/public-api.md)
- [ProductAdapter](docs/product-adapter.md)
- [対応アイテム制作ガイド](docs/integration-guide.md)
- [制約と互換性](docs/limitations.md)
- [トラブルシューティング](docs/troubleshooting.md)

## 対応バージョン

本資料の初版はSPSTracker `v1.0.0`を基準としています。

- Unity 2022.3.22f1
- VRChat SDK - Avatars
- Modular Avatar
- VRChat Constraints

個別に動作確認したパッケージバージョンは、購入者向け導入手順書を参照してください。

## 機能リクエスト

作りたい商品やギミックのために公開API、切り替え方法、状態値などの追加機能が必要な場合は、一度LumaKromaへご相談ください。

複数の制作者が利用できる要望は、SPSTracker本体や公開APIへの追加を検討します。要望は[GitHub Issues](https://github.com/LumaKroma/SPSTracker-Docs/issues)または[BOOTHの問い合わせ先](https://lumakroma.booth.pm/)へお寄せください。

## ライセンス

ライセンスの適用範囲は[LICENSE.md](LICENSE.md)を参照してください。

- このリポジトリのドキュメントと、販売商品の収録物は同じライセンスではありません。
- ProductAdapterフォルダ内でMIT Licenseを明記したファイルだけが、MIT Licenseによる改変・同梱・再配布の対象です。
- SPSTracker本体およびLumaToysはMIT Licenseの対象外です。
- 第三者著作物には各権利者のライセンスが適用されます。

第三者著作物については[THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)も確認してください。

## 関連リンク

- [BOOTH商品ページ](https://lumakroma.booth.pm/items/8623609)
- [画像付き導入手順書](https://docs.google.com/document/d/1uJOBLdxwRoDBxQUnmw-w6q-cMbQ8GVmhe8odOjaHqkc/edit?tab=t.0#heading=h.24aznar30yba)
- [問い合わせ先（BOOTH）](https://lumakroma.booth.pm/)
