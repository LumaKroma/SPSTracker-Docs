# ProductAdapter

ProductAdapterは、既存のModular Avatar対応商品へSPSTrackerの追従機能を追加するためのPrefabです。

## 収録Prefab

### SPSTracker_ProductAdapter.prefab

VRC Parent Constraint、追従状態を切り替えるAnimator、トップレベルの`Tracking`メニューを含む構成です。

### SPSTracker_TrackingMenuItem.prefab

既存商品のサブメニューへ`Tracking`操作項目だけを追加するPrefabです。

`SPSTracker_ProductAdapter.prefab`には同じ操作項目が含まれるため、両方のメニュー項目を同時に追加する必要はありません。

## 基本設定

1. SPSTrackerと対象商品を通常の手順でアバターへ導入します。
2. `SPSTracker_ProductAdapter.prefab`をアバタールート直下へ配置します。
3. `Tracking Driver`のVRC Parent Constraintで、`Target Transform`へ商品全体を動かせるルートを指定します。
4. `Held Pose (EDIT)`で追従していないときの位置と回転を調整します。
5. Held側のWeightを0、Tracked側を1にして、`Tracked Pose (EDIT - FRONT +Z)`を調整します。
6. 商品の追従原点を水色のガイドの根元へ、正面をローカル`+Z`方向へ合わせます。
7. 調整後はHeld側のWeightを1、Tracked側を0へ戻します。

次の基準Transformは編集しないでください。

```text
Held API Anchor (DO NOT EDIT)
Tracked API Anchor (DO NOT EDIT)
```

位置調整には、それぞれの子にある`Pose (EDIT)`を使用します。

## 動作

ProductAdapterは`LumaKroma/ST/TrackingStart`に応じて、Held PoseとTracked Poseを自動的に切り替えます。Socketの検出を失った後も約3秒間はTracked Poseを維持し、再検出できなかった場合はHeld Poseへ戻ります。

## 複数アイテムで使用する

複数のProductAdapterは、1つのSPSTrackerが提供する同じ追従状態と公開Anchorを共有できます。アイテムごとにSPSTrackerを追加する必要はありません。

複数のTargetが同時に表示されている場合は、すべてが同じSocketへ同時に追従します。通常は各商品のObject Toggleや選択メニューを使用し、表示・使用するアイテムを切り替えてください。

ProductAdapterはContact ReceiverとExpressions Parameterを追加しないため、SPSTracker本体を共有したまま対応アイテムを増やせます。

## Target Transformの選び方

商品のMeshだけではなく、追従させる子オブジェクトをすべて含む可動ルートを指定してください。

同じTransformを別のConstraint、Animator、PhysBoneが直接制御している場合は競合する可能性があります。必要に応じて、商品の可動部分をまとめる専用ルートGameObjectを作成してください。

## 左右持ち替え・ワールド固定との併用

標準のProductAdapterは、追従していない間もHeld Poseを基準としてTarget Transformを制御します。そのため、同じTransformを制御する次の機能は自動的には引き継がれません。

- 左右持ち替え
- ワールド固定
- 装着位置の切り替え
- Pickup位置を変更するAnimation
- 商品独自のParent ConstraintまたはPosition Constraint

既存機能を残す場合は、追従していない間は商品側、`TrackingStart = 1`の間だけProductAdapter側から制御する商品固有のAnimator構成が必要です。Prefabを配置するだけでは対応できません。

## 追加コスト

ProductAdapter 1個あたりの目安です。

- Contact Receiver: 追加なし
- Expressions Parameter: 追加なし
- 描画ポリゴン: 追加なし
- VRC Parent Constraint: 1
- Animator Layer: 1

## 商品への同梱・再配布

商品に収録された`ProductAdapter`フォルダ内のファイルだけがMIT Licenseの対象です。

- 著作権表示とMIT License全文を残してください。
- 改変したProductAdapterを商品へ同梱できます。
- SPSTracker本体およびLumaToysを同梱・再配布することはできません。
- 利用者が別途SPSTrackerを導入する必要があることを商品説明へ明記してください。

実際に同梱するファイルへ付属する`LICENSE.md`を必ず確認してください。
