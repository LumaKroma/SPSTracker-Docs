# 対応アイテム制作ガイド

日本語 | [English](en/integration-guide.md)

対応方法は、アイテムの構造と配布方法に合わせて選択します。

## 基本方針: 1つのSPSTrackerを共有する

複数の対応アイテムを導入する場合も、SPSTracker本体は原則として1つだけ配置します。各アイテムを同じ公開Anchorと`TrackingStart`へ接続し、表示・選択メニューによって使用するアイテムを切り替えてください。

アイテムごとにSPSTracker相当の追従機能を内蔵すると、Contact、Constraint、Animator Layer、Expressions Parameterが重複します。特にSPSTrackerはContactを多めに使用するため、複数の追従機能を同じアバターへ追加すると、リアルタイムの検出や追従が不安定になる可能性があります。

既存の公開APIだけでは実現できない切り替えや状態取得が必要な場合は、独自の追従機構を組み込む前に[機能リクエスト](../README.md#機能リクエスト)をご利用ください。

## 方法の選択

| 方法 | 向いている用途 | 注意点 |
| --- | --- | --- |
| `VisibleRoot`へ直接配置 | 自分用の単純なモデル | 既存ギミックを持つ商品には不向き |
| ProductAdapter | 既存のModular Avatar対応商品 | Setup Assistantを使用。既存の位置制御と競合する場合がある |
| 公開APIを使った独自アドオン | 販売・配布する専用対応Prefab | AnimatorとConstraintの設計が必要 |

## 任意のモデルを直接追加する

自分用のFBXやPrefabを次の階層へ配置します。

```text
SPSTracker
└─ WorldFixed
   └─ Object
      └─ VisibleRoot
         ├─ Gizmo
         └─ YourItem
```

1. モデルは`Gizmo`の子ではなく、`VisibleRoot`の子へ配置します。
2. モデルのローカル`+Z`方向を追従時の正面へ合わせます。
3. Gizmoの矢印の根元へ追従原点を合わせます。
4. 調整確認のため有効化した`VisibleRoot`と`LocalUI`は、アップロード前に非アクティブへ戻します。

既存のAnimator、Constraint、PhysBoneを持つ商品は、直接配置によって動作しなくなる場合があります。その場合はProductAdapterまたは独自アドオンを使用してください。

## ProductAdapterを使用する

既存のModular Avatar対応商品へ追加する場合は、[ProductAdapter](product-adapter.md)を参照してください。

特に、商品が左右持ち替え・ワールド固定・複数箇所への装着機能を持つ場合は、Target Transformを複数の仕組みが同時に制御しないように設計してください。

Socket軸回りのロール追従が必要な場合は、ProductAdapter Setup Assistantから任意の[Roll Deformation](roll-deformation.md)を設定できます。

## 独自アドオンを制作する

独立したアドオンPrefabは、SPSTrackerの公開Anchorを取得して、アイテム側のVRC Parent Constraintへ接続します。

推奨する概念構成は次のとおりです。

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

- MA Bone Proxyなどを使い、Held側を`SPSTracker/API/HeldAnchor`へ接続します。
- Tracked側を`SPSTracker/API/TrackedAnchor`へ接続します。
- Item RootをTargetとするVRC Parent Constraintへ、HeldとTrackedの2つを接続します。
- `LumaKroma/ST/TrackingStart`に応じて、HeldとTrackedの接続先を切り替えます。
- 利用者が追従を一時的に禁止できるようにする場合は、`LumaKroma/ST/TrackingEnabled`も切り替え条件へ追加します。
- [公開API](public-api.md)に記載されていないSPSTrackerの要素は参照しません。
- 複数のアドオンを導入する場合も、同じSPSTrackerの公開APIを共有します。

詳細なAPI名は[公開API・パラメータ](public-api.md)を参照してください。

## 配布前チェックリスト

- [ ] SPSTracker本体を商品へ含めていない
- [ ] SPSTracker本体や同等の追従機構をアイテムごとに重複配置していない
- [ ] 利用者がSPSTrackerを別途導入する必要があると明記した
- [ ] 複数アイテムを導入した場合に表示・選択を切り替えられる
- [ ] アイテムのローカル`+Z`が追従時の正面になっている
- [ ] Held状態とTracked状態の両方で位置を確認した
- [ ] Trackingロスト待機からHeld状態へ戻ることを確認した
- [ ] Nearest LockのON/OFFで動作を確認した
- [ ] 同じTarget Transformを複数のConstraintが制御していない
- [ ] Trackingメニューを付ける場合、`LumaKroma/ST/TrackingEnabled`を重複登録していない
- [ ] Roll Deformationを使用する場合、対応シェーダー、Roll Axis、Runtime Offsetを確認した
- [ ] Roll対象のRendererを複数のRoll Deformationへ重複登録していない
- [ ] 再配布するファイルのライセンスを同梱した
- [ ] 新規アバタープロジェクトへのD&D導入を確認した
