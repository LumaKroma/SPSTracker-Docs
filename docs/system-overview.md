# システム概要

日本語 | [English](en/system-overview.md)

SPSTrackerは、アバター上のアイテムを他プレイヤーのVRCFury SPS Socketへ追従させるVRChatアバター向けギミックです。

## 複数アイテムで共有する設計

SPSTrackerは、原則として1つのアバターに1つだけ配置し、その追従結果を複数の対応アイテムから共有する設計です。各アイテムは同じ`HeldAnchor`、`TrackedAnchor`、`TrackingStart`を参照し、商品側の表示・選択メニューで使用するアイテムを切り替えます。

この構成には次の目的があります。

- Contact Receiverをアイテムごとに重複させない
- VRChat上でリアルタイムに処理されるContactの負荷を抑える
- Expressions Parameterの容量とパラメータ数を節約する
- Constraint、Animator Layer、メニュー構成の重複を減らす
- 複数の対応商品で追従状態と操作方法を共通化する

SPSTrackerは基本構成だけで6個、Nearest Lock使用時には7個のContact Receiverを使用します。対応商品が追従機構を独自に内包し、アイテムごとに同様のContactを追加すると、アバター内のContact数が増加します。多数のContactの重なりや処理負荷によって、検出精度、追従の安定性、リアルタイムでの動作に問題が出る可能性があります。

独自実装を禁止するものではありませんが、対応商品では公開Anchorと`TrackingStart`を利用し、1つのSPSTrackerを共有する構成を推奨します。制作上必要な状態値や切り替え機能が公開APIにない場合は、[機能リクエスト](../README.md#機能リクエスト)をお寄せください。

## 追従仕様

SPSTracker本体は、有効なSPS Socketを検出すると、位置・ヨー方向・ピッチ方向へ追従します。Socket軸回りのロール回転は、ProductAdapterの任意機能である[SPS Tracker Roll Deformation](roll-deformation.md)からRendererへ追加できます。

- 対応アイテムは、ローカル`+Z`方向を追従時の正面として設定します。
- 保持中の検知範囲はFX Animator内の`HeldRange`で調整できます。追従成立後の検知範囲は、保持中より小さくならないよう自動調整されます。
- 検知範囲による実効Gainの差は自動補正され、範囲を変更しても追従感が大きく変わりにくい構成です。
- Socketの検出を失った場合は、最後の位置と回転で約3秒待機します。その間、約1秒かけて再検知範囲を最大まで広げます。
- 待機中に再検出した場合は追従を継続し、検知範囲を徐々に通常値へ戻します。約3秒以内に再検出できなかった場合は保持位置へ戻ります。

対応アイテムは、追従状態を`LumaKroma/ST/TrackingStart`から読み取れます。詳しくは[公開API・パラメータ](public-api.md)を参照してください。

## 公開Anchor

SPSTrackerはアイテム接続用に、次のTransformを公開します。

```text
SPSTracker/API/HeldAnchor
SPSTracker/API/TrackedAnchor
```

- `HeldAnchor`: 追従していないときの保持位置
- `TrackedAnchor`: Offset適用後の最終追従位置と回転

詳細は[公開API・パラメータ](public-api.md)を参照してください。

## Nearest Lock

Nearest Lockは、同じ検知範囲に複数のSocketがある場合に、追従成立後の検知範囲を対象付近へ絞るオプションです。

対象Socketを識別するIDを取得する機能ではないため、目的のSocketを常に保証するものではありません。また、コントローラー移動などでSocketが大きく動く場合は追従が外れやすくなることがあります。

## パフォーマンス目安

SPSTracker v1.1.0の基本構成は次のとおりです。

| 項目 | 目安 |
| --- | ---: |
| Contact Receiver | 6（Nearest Lock有効時は7） |
| VRChat Constraint | 11 |
| Animator Layer | 6 |
| Expressions Parameter | 4個・11 bit |

ProductAdapter 1個につき、VRC Parent ConstraintとAnimator Layerが各1個追加されます。ProductAdapter本体はContact、Expressions Parameter、描画ポリゴンを追加しません。

任意の`SPSTracker_TrackingMenuItem.prefab`を使用すると、同期Bool Parameterが1個（1 bit）追加されます。このパラメータは複数のProductAdapterから共有できます。

独立動作のRoll Deformationは、コンポーネントごとに3 vertices / 1 triangleのResolver用MeshRendererを1個生成します。また、独立動作するRoll Deformation全体で共有するFX Animator Layerを2個と、Expressions Parametersへ登録されない内部Animator Parameterを4個生成します。Runtime Offsetを使用する場合は、異なるParameter名ごとにFX Animator Layerが1個追加されます。SPS Plugと同じRendererへ適用する場合は、Plug側のResolverを共有するため独立Resolverは生成されません。
