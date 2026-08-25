# ProductAdapter

日本語 | [English](en/product-adapter.md)

ProductAdapterは、既存のModular Avatar対応商品へSPSTrackerの追従機能を追加するPrefabです。SPSTracker本体と同様、Windows PC版VRChat向けです。

## 収録Prefab

同じ商品には、用途に合うどちらか一方だけを配置します。

### `SPSTracker_ProductAdapter.prefab`

メニューなしの基本構成です。

- VRC Parent Constraint 1個
- Held / Trackedを切り替える追従Animator
- World Fixed用の独立した内部Animator
- ProductAdapter Setup Assistant
- Expression Menu / Expression Parameterの追加なし

### `SPSTracker_ProductAdapterMenu.prefab`

基本構成を継承したPrefab Variantです。`Product Adapter` SubMenuに次の項目を追加します。

| 項目 | 既定値 | 動作 |
| --- | --- | --- |
| `Item Visible` | OFF | 指定した商品ルートの表示を切り替える |
| `Tracking` | ON | Tracked Poseへの切り替えを許可する |
| `World Fixed` | OFF | `Tracking Driver`のVRC Parent Constraintをワールド固定する |

3項目はすべて同期ON・保存OFFです。内部ParameterはModular AvatarがProductAdapterインスタンスごとに自動リネームするため、複数商品を独立して操作できます。

## 基本設定

1. SPSTrackerと対象商品を通常の手順でアバターへ導入します。
2. 用途に合うProductAdapter Prefabを1つ、アバタールート直下へ配置します。
3. Prefabルートの`ProductAdapter Setup Assistant`で、商品全体を動かせるTransformを`商品ルート`へ指定します。
4. `初期設定・参照を修復`を実行します。
5. `Held姿勢を編集`で、追従していないときの位置と回転を調整します。
6. `Held姿勢をTrackedへコピー`を押してから`Tracked姿勢を編集`を開きます。
7. 商品の追従原点を水色ガイドの根元へ、正面をローカル`+Z`へ合わせます。
8. 必要なら`Rollを自動設定`をONにします。詳しくは[Roll Deformation](roll-deformation.md)を参照してください。
9. `設定を完了してHeldへ戻す`を実行し、Inspectorのエラーがないことを確認します。

> [!IMPORTANT]
> `Held編集中`または`Tracked編集中`のままPlay / Buildすると、NDMFは設定未完了としてビルドを停止します。必ず`設定を完了してHeldへ戻す`を実行してください。

次の基準Transformは直接編集しないでください。

```text
Held API Anchor (DO NOT EDIT)
Tracked API Anchor (DO NOT EDIT)
```

## メニュー付きVariantの設定

`Menu/Product Adapter/Item Visible`のMA Object Toggleへ、表示を切り替える商品ルートを指定します。

既存メニューへ追加する場合は、`Menu/Product Adapter`のMA Menu Installerで`Install Target Menu`を指定します。未指定ならAvatarのルートMenuへ`Product Adapter` SubMenuとして追加されます。

メニュー表示名、アイコン、MA Object Toggle対象、Install Target Menuは変更できます。`Menu`階層はProductAdapterの子に残し、内部Parameter名は変更しないでください。階層を外へ移動したりParameter名を変更したりすると、インスタンスごとの自動リネーム契約から外れます。

## 動作

ProductAdapterはSPSTrackerの読み取り専用状態`LumaKroma/ST/TrackingStart`を使用します。

- Base Prefab: `TrackingEnabled`のAnimator既定値はONで、Socket追従成立時に自動でTracked Poseへ切り替わります。
- Menu Variant: `Tracking`がONで、かつ`TrackingStart`が成立したときだけTracked Poseへ切り替わります。
- `World Fixed`: ProductAdapterの`Tracking Driver`を現在のワールド位置・回転へ固定します。商品側の別ConstraintやAnimatorが同じTransformを制御している場合は競合する可能性があります。

Socketを失った後も約3秒はTracked Poseを維持します。待機中に再検出できた場合は追従を続け、できなかった場合はHeld Poseへ戻ります。

## 複数アイテムで使用する

複数のProductAdapterは、同じSPSTrackerの`HeldAnchor`、`TrackedAnchor`、`TrackingStart`を共有します。アイテムごとにSPSTrackerを追加する必要はありません。

Menu Variantの3つの内部Parameterはインスタンスごとに分離されます。複数のTargetを同時に表示すると全Targetが同じSocketへ追従するため、通常は`Item Visible`などで使用中の商品だけを表示してください。

## v1.1からの上書き更新

v1.1のunitypackageへv1.2を上書き導入しても、UnityPackageは廃止済みの`SPSTracker_TrackingMenuItem.prefab`を自動削除しません。

1. アバターをバックアップします。
2. Hierarchy上の旧`SPSTracker_TrackingMenuItem`を削除します。
3. Project内に残った旧Prefabも削除します。
4. メニューが必要な商品は`SPSTracker_ProductAdapterMenu.prefab`へ置き換え、Setup Assistantを再設定します。
5. メニュー不要ならBase Prefabを使用します。

旧Tracking Menu Itemと新Menu Variantを同時に使用しないでください。

## Target Transformと既存機能

商品のMeshだけでなく、追従させる子オブジェクトをすべて含む可動ルートを指定してください。同じTransformを別のConstraint、Animator、PhysBoneが直接制御すると、位置ずれや振動が発生する可能性があります。

Menu VariantのWorld FixedはProductAdapter自身のConstraintだけを固定します。既存商品の左右持ち替え、装着位置切り替え、Pickup Animationなどを自動的に統合するものではありません。必要に応じて専用の可動ルートを追加してください。

## 追加コスト

安定した商品側の構成値は次のとおりです。

| 構成 | Contact | Expressions Parameter | 描画 | VRC Parent Constraint | 収録FX Layer |
| --- | ---: | ---: | ---: | ---: | ---: |
| Base Prefab | 0 | 0 | 0 | 1 | 2 |
| Menu Variant | 0 | Bool 3個・3 bit | 0 | 1 | Base 2 + MA Object Toggle生成 |

NDMF後の最終Animatorには、Modular AvatarがMMD互換やObject Toggle用の補助Layerを追加します。Unity 2022.3.22f1 / Modular Avatar 1.18.1の空Avatar比較では、BaseがFX Layer `+4`、Menu Variantが`+7`でした。この実測値は補助Layerを含み、Modular AvatarのバージョンやAvatar構成で変化するため公開API上の固定値ではありません。

Roll Deformationの追加コストは[Roll Deformation](roll-deformation.md)を参照してください。

## 商品への同梱・再配布

商品に収録された`ProductAdapter`フォルダ内でLumaKromaが権利を持つファイルだけがMIT Licenseの対象です。

- 著作権表示とMIT License全文を残してください。
- 改変したProductAdapterを商品へ同梱できます。
- SPSTracker本体およびLumaToysを同梱・再配布することはできません。
- 利用者がWindows PC版SPSTrackerを別途導入する必要があることを商品説明へ明記してください。

実際に同梱するファイルへ付属する`LICENSE.md`を必ず確認してください。
