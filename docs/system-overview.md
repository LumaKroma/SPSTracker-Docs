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

SPSTrackerは、有効なSPS Socketを検出すると、位置・ヨー方向・ピッチ方向へ追従します。Socket軸回りのロール回転には追従しません。

- 対応アイテムは、ローカル`+Z`方向を追従時の正面として設定します。
- Socketの検出を失った場合は、最後の位置と回転で約3秒待機し、再検出を試みます。
- 約3秒以内に再検出できなかった場合は、保持位置へ戻ります。

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

SPSTracker v1.0.0の基本構成は次のとおりです。

| 項目 | 目安 |
| --- | ---: |
| Contact Receiver | 6（Nearest Lock有効時は7） |
| VRChat Constraint | 11 |
| Animator Layer | 6 |
| Expressions Parameter | 4個・11 bit |

ProductAdapter 1個につき、VRC Parent ConstraintとAnimator Layerが各1個追加されます。Contact、Expressions Parameter、描画ポリゴンは追加されません。
