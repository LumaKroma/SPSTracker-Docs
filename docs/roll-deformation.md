# Roll Deformation

日本語 | [English](en/roll-deformation.md)

SPS Tracker Roll Deformationは、SPS2 ResolverからSocketのUp方向を取得し、ProductAdapterで接続したRendererをSocketの前方軸回りへ剛体回転させる任意機能です。SPSTracker本体が担当する位置・ヨー・ピッチ追従へ、ロール追従を追加します。

メッシュを奥行きごとに曲げるSPS Plugとは異なり、対象Rendererの頂点・法線・接線を同じ角度で回転します。

## 必要環境

- Unity 2022.3.22f1
- NDMF / Modular Avatar
- VRCFury 1.1403.0以降
- lilToon 1.3.7以降（独立動作で正式対応するシェーダー）

VRCFuryが未導入または1.1403.0未満の場合は警告を表示し、ビルド用アバターからRoll Deformationコンポーネントを除外します。lilToonが未導入または1.3.7未満の場合は、独立動作を含むRoll Deformationだけを同様に除外します。SPS Plug共有だけで動作するコンポーネントはlilToon要件の対象外です。

除外後もSPSTracker本体を含む残りのアバタービルドは続行します。シーンやPrefab上の元コンポーネントは削除されないため、依存を更新すれば再設定せず次回ビルドから有効になります。バージョンを判定できない場合は警告を表示してビルドを続行します。

## 動作モード

### 独立Resolver

対象RendererがVRCFury SPS Plugから使用されていない場合、Roll DeformationはSPS2 Resolverをビルド時に生成し、SocketのForwardとUpを直接取得します。

- Roll Axisの設定を使用します。
- 検出距離、検出半径、自分・他人のSocket許可設定を使用します。
- 標準lilToonシェーダーバリエーションを正式対応範囲とします。
- 元のMaterialアセットは変更せず、ビルド時にRoll対応Materialを生成します。

独自シェーダーやlilToon派生シェーダーは、表示を維持できない可能性があるため対応対象外です。

### SPS Plug併用

同じRendererをVRCFury SPS Plugが使用している場合は、Plug側の軸とResolver設定を使用し、VRCFuryの`SPS_MODIFY_BAKE`からRoll処理を適用します。

- Roll Deformation側のRoll Axis、検出距離、検出半径、Socket許可設定は、そのRendererには使用されません。
- Target RenderersとRuntime Offsetの設定は引き続き適用されます。
- シェーダーとSPS Plugの組み合わせは配布前に実機で確認してください。

## Setup Assistantで自動設定する

ProductAdapterでは自動設定を推奨します。

1. `ProductAdapter Setup Assistant`でProduct Rootを指定します。
2. Held姿勢とTracked姿勢を調整します。
3. `Rollを自動設定`をONにします。
4. `初期設定・参照を修復`または`設定を完了してHeldへ戻す`を実行します。

自動設定では、Trackedガイドの位置を回転中心、ローカル`+Z`をSocket方向、ワールド`+Y`をロールの上方向としてRoll Axisを作成します。Product Root以下のMeshRendererとSkinnedMeshRendererも自動収集します。

自動設定中はRoll Deformationの手動設定が無効になります。個別調整が必要な場合は`Rollを自動設定`をOFFにしてください。

## 手動設定

`SPS Tracker Roll Deformation`コンポーネントの主な項目です。

| 項目 | 用途 |
| --- | --- |
| Runtime Offset Parameter | 実行中にロール補正を変更する任意のFloat Parameter |
| Runtime Offset Default | Runtime Offsetの初期値。`0.5`で補正なし |
| Roll Axis | 位置が剛体回転の中心、`+Z`がSocket方向、`+Y`が上方向 |
| Resolver Length | Socketを取得する最大距離。初期値`0.3 m` |
| Resolver Radius | Radius Offset Socketへ渡す仮想半径。初期値`0.03 m` |
| Target Renderers | 同じ角度で剛体回転させるRenderer |
| Allow Self Sockets | 自分のアバターのSocketを許可する |
| Allow Other Sockets | 他プレイヤーのSocketを許可する |

同じRendererを複数のRoll Deformationコンポーネントへ登録すると、ビルド時にエラーとなり、そのRendererは処理されません。

## Runtime Offset

Socket Upにはアバター間で共通する向きの規格がないため、Socketによって補正が必要になる場合があります。Runtime Offset Parameterを指定すると、次の範囲で補正できます。

| Float値 | 補正角度 |
| ---: | ---: |
| `0` | `-180°` |
| `0.5` | `0°` |
| `1` | `+180°` |

Roll DeformationはAnimator ParameterとBlend Treeを生成しますが、Expressions Parametersやメニューには自動登録しません。VRChat上で操作する場合は、同じFloat ParameterをMA ParametersとRadial Puppetへ追加してください。

## 制約

- RollはRenderer単位のシェーダー変形です。Transform、Collider、PhysBone、Constraintの向きは変更しません。
- Roll ResolverとSPSTracker本体はSocketを個別に解決します。複数のSocketが近接している場合、位置追従とRoll追従が異なるSocketを選ぶ可能性があります。
- Socket Upはランタイム中は安定しますが、Socket制作者が意図した向きへ設定しているとは限りません。
- 剛体回転後は法線も回転するため、ワールド方向へ依存するライティングやMatCapの見え方は変化する場合があります。
- Socket frameの位置または軸がゼロ、NaN、Infinity、退化状態などで解決できない場合、そのframeは頂点・法線・接線を変更せず安全にスキップします。設定不備を自動修復する機能ではないため、継続して発生する場合はResolverとRoll Axisを確認してください。

症状別の確認項目は[トラブルシューティング](troubleshooting.md)を参照してください。
