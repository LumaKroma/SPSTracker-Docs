# トラブルシューティング

## SPSTrackerメニューが表示されない

- Modular Avatarが導入されているか確認します。
- `SPSTracker.prefab`がArmature内ではなく、アバタールート直下にあるか確認します。
- Unity Consoleの赤いエラーを解消します。
- アバター変更後に再アップロードしたか確認します。

## Socketへ近づけても追従しない

- SPSTrackerメニューの`Activate`をONにします。
- 相手側で対応するSPS Socketが有効か確認します。
- アイテムをSocket Frontから約10cm以内へ近づけます。
- `Nearest Lock`を一度OFFにして確認します。
- アバターをリロードして確認します。
- `Show Gizmo`をONにして検知範囲と向きを確認します。

## アイテムの向きが合わない

アイテムのローカル`+Z`方向が追従時の正面になっているか確認します。

- 直接追加したモデル: `VisibleRoot`以下のモデルを回転
- ProductAdapter: `Tracked Pose (EDIT - FRONT +Z)`を回転

`Offset`は軸方向の位置調整であり、向きの修正には使用しません。

## 追従中に手元へ戻る

Socketを約3秒間再検出できなかった可能性があります。

- Socketの急な移動
- アバタースケールの違い
- 多数のContactの重なり
- 通信状態またはフレームレート
- Nearest Lockによる検知範囲の縮小
- 相手側でSocketが無効化された

## ProductAdapterで商品が動かない

- `Tracking Driver`の`Target Transform`が設定されているか確認します。
- 商品全体を動かせるルートTransformを指定します。
- 同じTransformを別のConstraint、Animator、PhysBoneが制御していないか確認します。
- `Held API Anchor (DO NOT EDIT)`と`Tracked API Anchor (DO NOT EDIT)`を編集していないか確認します。
- 調整後にHeld側のWeightを1、Tracked側を0へ戻したか確認します。

## 左右持ち替え・ワールド固定が動作しない

ProductAdapterの標準構成は、追従していない間もHeld Poseから商品を制御します。既存機能と同じTransformを制御している場合は競合します。

- 既存機能を使わない場合: 商品側の位置制御を無効化し、Held Poseを調整します。
- 既存機能を残す場合: `LumaKroma/ST/TrackingStart`に応じて制御元を切り替える商品固有のAnimator改変が必要です。

## 問い合わせ時に用意する情報

- Unityのバージョン
- VRChat SDKのバージョン
- Modular Avatarのバージョン
- VRCFuryおよびlilToonのバージョン（使用している場合）
- Hierarchy全体のスクリーンショット
- Unity Consoleのエラー内容
- 問題が発生するまでの操作手順
- 使用しているSPSTrackerと対応商品のバージョン

問い合わせ先: https://lumakroma.booth.pm/
