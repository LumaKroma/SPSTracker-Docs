# システム概要

日本語 | [English](en/system-overview.md)

SPSTrackerは、アバター上のアイテムを他プレイヤーのVRCFury SPS Socketへ追従させるVRChatアバター向けギミックです。

SPSTrackerとProductAdapterはWindows PC版VRChat向けです。Android / Quest版アバターでは使用できません。

## 複数アイテムで共有する設計

SPSTrackerは、原則として1つのアバターに1つだけ配置し、その追従結果を複数の対応アイテムから共有する設計です。各アイテムは同じ`HeldAnchor`、`TrackedAnchor`、`TrackingStart`を参照し、商品側の表示・選択メニューで使用するアイテムを切り替えます。

この構成には次の目的があります。

- Contact Receiverをアイテムごとに重複させない
- VRChat上でリアルタイムに処理されるContactの負荷を抑える
- Expressions Parameterの容量とパラメータ数を節約する
- Constraint、Animator Layer、メニュー構成の重複を減らす
- 複数の対応商品で追従状態と操作方法を共通化する

SPSTrackerは通常の6 Receiver構成です。対応商品が追従機構を独自に内包し、アイテムごとに同様のContactを追加すると、アバター内のContact数が増加します。多数のContactの重なりや処理負荷によって、検出精度、追従の安定性、リアルタイムでの動作に問題が出る可能性があります。

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

## 構成と負荷

SPSTrackerは通常の6 Receiver構成です。ProductAdapter Menu Variantは商品ごとに同期Bool 3個（3 bit）を追加し、Modular Avatarが最終Parameter名をインスタンスごとにリマップします。

最終的なAnimator Layer数、Constraint数、Contact数、Expressions Parameter使用量、描画負荷は、導入先アバター、依存パッケージ、NDMFの生成結果、使用する任意機能で変わります。固定の性能値は公開互換契約に含めません。導入後のBuild結果を確認してください。
