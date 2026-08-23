# トラブルシューティング

日本語 | [English](en/troubleshooting.md)

## SPSTrackerメニューが表示されない

- Modular Avatarが導入されているか確認します。
- `SPSTracker.prefab`がArmature内ではなく、アバタールート直下にあるか確認します。
- Unity Consoleの赤いエラーを解消します。
- アバター変更後に再アップロードしたか確認します。

## Socketへ近づけても追従しない

- SPSTrackerメニューの`Activate`をONにします。
- 相手側で対応するSPS Socketが有効か確認します。
- `Show Gizmo`で表示される青い検知範囲の内側へ、Socket Frontを近づけます。
- Socket Frontへ近づけて再確認します。
- アバターをリロードして確認します。
- `Show Gizmo`をONにして検知範囲と向きを確認します。

## アイテムの向きが合わない

アイテムのローカル`+Z`方向が追従時の正面になっているか確認します。

- 直接追加したモデル: `VisibleRoot`以下のモデルを回転
- ProductAdapter: `Tracked Pose (EDIT - FRONT +Z)`を回転

`Offset`は軸方向の位置調整であり、向きの修正には使用しません。

## 追従中に手元へ戻る

Socketを約3秒間再検出できなかった可能性があります。

追従ロスト後は、約1秒かけて検知範囲を最大まで広げながら再検出を試みます。再検出した場合は、短時間の保持後、約1秒かけて通常値へ戻ります。これらは通常挙動の目安であり、環境により変わります。

- Socketの急な移動
- アバタースケールの違い
- 多数のContactの重なり
- 通信状態またはフレームレート
- 相手側でSocketが無効化された

## ProductAdapterで商品が動かない

- `ProductAdapter Setup Assistant`で商品ルートを指定し、`初期設定・参照を修復`を実行します。
- Menu Variantを使用している場合は`Tracking`をONにします。
- Base Prefabは`TrackingStart`成立時に自動で切り替わります。Menu Variantは`TrackingStart`成立かつ`Tracking` ONのときだけ切り替わります。
- 同じTransformを別のConstraint、Animator、PhysBoneが制御していないか確認します。
- `Held API Anchor (DO NOT EDIT)`と`Tracked API Anchor (DO NOT EDIT)`を編集していないか確認します。
- Setup Assistantの検証に表示される警告とエラーを解消します。

## ProductAdapterを含むPlay / Buildが停止する

- Setup Assistantで`設定を完了してHeldへ戻す`を実行します。
- `Held編集中`または`Tracked編集中`の表示が残っていないことを確認します。
- 商品ルート、Tracking Driver、Held / Tracked Poseの参照を修復します。

## v1.1から更新後にTrackingメニューが重複する

UnityPackageは廃止済みの`SPSTracker_TrackingMenuItem.prefab`を自動削除しません。HierarchyとProjectから旧Prefabを削除し、必要に応じて`SPSTracker_ProductAdapterMenu.prefab`へ置き換えてください。

## Roll追従が動かない

- VRCFuryが1.1403.0以降か確認します。
- 独立動作ではlilToonが1.3.7以降か確認します。依存が未導入または古い場合は警告が表示され、対象のRoll Deformationだけがビルドから除外されます。
- `SPS Tracker Roll Deformation`が有効で、Target Renderersに対象Rendererが含まれているか確認します。
- 自動設定の場合は`Rollを自動設定`をONにして、`初期設定・参照を修復`または完了操作を実行します。
- 独立動作では、対象Materialが標準lilToonシェーダーを使用しているか確認します。
- Unity ConsoleにRoll Deformationのビルドエラーがないか確認します。

## Rollの向きがずれる

- Roll Axisの`+Z`がSocket方向、`+Y`がロールの上方向か確認します。
- 自動設定ではTrackedガイドの`+Z`とワールド`+Y`が基準です。
- Socket Upはアバター間で統一されていないため、Runtime Offsetで補正します。
- 同じRendererにSPS Plugがある場合は、Roll DeformationではなくPlug側の軸設定を確認します。

## Roll Deformationで見た目が変わる

独立動作で正式対応するのはlilToon 1.3.7以降の標準lilToonシェーダーです。独自シェーダーやlilToon派生シェーダーでは、元の描画を維持できない場合があります。lilToon自体が未導入または古い場合は、独立動作を含むRoll Deformationがビルドから除外されます。

- 元Materialが標準lilToonか確認します。
- ビルド後に生成されたMaterialではなく、元Material側の設定を修正します。
- ロールによって法線も回転するため、ワールド方向へ依存するライティングやMatCapの変化は正常な場合があります。

## 左右持ち替え・ワールド固定が動作しない

ProductAdapterの標準構成は、追従していない間もHeld Poseから商品を制御します。既存機能と同じTransformを制御している場合は競合します。

- 既存機能を使わない場合: 商品側の位置制御を無効化し、Held Poseを調整します。
- 既存機能を残す場合: `LumaKroma/ST/TrackingStart`に応じて制御元を切り替える商品固有のAnimator改変が必要です。
- Menu Variantの`World Fixed`を使う場合: ProductAdapterの`Tracking Driver`だけが固定されます。商品側の別ワールド固定と同時に同じTransformを制御しないでください。

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
