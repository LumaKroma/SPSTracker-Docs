# ProductAdapter

日本語 | [English](en/product-adapter.md)

ProductAdapterは、既存のModular Avatar対応商品へSPSTrackerの追従機能を追加するためのPrefabです。

## 収録Prefab

### SPSTracker_ProductAdapter.prefab

VRC Parent Constraint、Held / Tracked状態を切り替えるAnimator、ProductAdapter Setup Assistantを含む構成です。メニュー項目は含みません。

### SPSTracker_TrackingMenuItem.prefab

既存商品のサブメニューへ`Tracking`操作項目だけを追加するPrefabです。

OFFでは商品をHeld Poseへ固定し、ONでは`TrackingStart`検出時の自動追従を許可します。複数配置したProductAdapterは同じ`LumaKroma/ST/TrackingEnabled`を共有します。

## 基本設定

1. SPSTrackerと対象商品を通常の手順でアバターへ導入します。
2. `SPSTracker_ProductAdapter.prefab`をアバタールート直下へ配置します。
3. Prefabルートの`ProductAdapter Setup Assistant`で、商品全体を動かせるTransformを`商品ルート`へ指定します。
4. `初期設定・参照を修復`を実行します。
5. `Held姿勢を編集`を押し、追従していないときの位置と回転を調整します。
6. `Held姿勢をTrackedへコピー`を押してから`Tracked姿勢を編集`を押します。
7. 商品の追従原点を水色のガイドの根元へ、正面をローカル`+Z`方向へ合わせます。
8. 必要なら`Rollを自動設定`をONにします。詳しくは[Roll Deformation](roll-deformation.md)を参照してください。
9. `設定を完了してHeldへ戻す`を実行し、Inspectorの検証にエラーがないことを確認します。

Setup Assistantの表示言語はシステム言語が初期値です。Inspector上部の`表示言語`から日本語または英語へ切り替えられます。

次の基準Transformは編集しないでください。

```text
Held API Anchor (DO NOT EDIT)
Tracked API Anchor (DO NOT EDIT)
```

通常はSetup Assistantの編集ボタンから、それぞれの`Pose (EDIT)`を選択してください。Constraint SourceのWeightを直接変更する必要はありません。

## 動作

ProductAdapterは`LumaKroma/ST/TrackingStart`と`LumaKroma/ST/TrackingEnabled`が両方ONのとき、Held PoseからTracked Poseへ切り替えます。Socketの検出を失った後も約3秒間はTracked Poseを維持し、その間はSPSTrackerが約1秒かけて再検知範囲を広げます。再検出できた場合は追従を継続し、できなかった場合はHeld Poseへ戻ります。

`TrackingEnabled`のAnimator初期値はONです。メニュー操作が不要な商品では`SPSTracker_TrackingMenuItem.prefab`を追加せず、そのまま自動追従を使用できます。

## 複数アイテムで使用する

複数のProductAdapterは、1つのSPSTrackerが提供する同じ追従状態と公開Anchorを共有できます。アイテムごとにSPSTrackerを追加する必要はありません。

複数のTargetが同時に表示されている場合は、すべてが同じSocketへ同時に追従します。通常は各商品のObject Toggleや選択メニューを使用し、表示・使用するアイテムを切り替えてください。

ProductAdapter本体はContact ReceiverとExpressions Parameterを追加しないため、SPSTracker本体を共有したまま対応アイテムを増やせます。Tracking Menu Itemを使用する場合は、共有の同期Bool Parameterが1個追加されます。

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

任意の`SPSTracker_TrackingMenuItem.prefab`を使用する場合は、同期Bool Parameterが1個（1 bit）追加されます。Roll Deformationの追加コストは[Roll Deformation](roll-deformation.md)を参照してください。

## 商品への同梱・再配布

商品に収録された`ProductAdapter`フォルダ内のファイルだけがMIT Licenseの対象です。

- 著作権表示とMIT License全文を残してください。
- 改変したProductAdapterを商品へ同梱できます。
- SPSTracker本体およびLumaToysを同梱・再配布することはできません。
- 利用者が別途SPSTrackerを導入する必要があることを商品説明へ明記してください。

実際に同梱するファイルへ付属する`LICENSE.md`を必ず確認してください。
