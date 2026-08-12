# 制約と互換性

日本語 | [English](en/limitations.md)

## 追従対象

- 追従対象側で、対応するVRCFury SPS Socketが有効になっている必要があります。
- 位置・ヨー方向・ピッチ方向へ追従します。
- SPSTracker本体だけではSocket軸回りのロール回転へ追従しません。ProductAdapterの任意機能である[Roll Deformation](roll-deformation.md)が必要です。

## 複数Socket

同じ検知範囲に複数のSocketがある場合、各軸が常に同じSocketを選択することを保証できません。

Nearest Lockは追従成立後の検知範囲を狭め、別Socketへ移りにくくするための補助機能です。Socketを一意に識別する機能ではないため、次の状況では別Socketを検出する可能性があります。

- 複数Socketが非常に近い
- 多数のContactが同じ範囲へ重なっている
- Socketまたはコントローラーが急激に移動する

## 追従精度へ影響する要素

- アバタースケール
- 通信状態
- フレームレート
- Socketの設定
- Contactの重なり
- Nearest Lockによる検知範囲の縮小

すべてのアバター、Socket、Worldでの動作を保証するものではありません。

## ProductAdapterの競合

ProductAdapterはTarget Transformの位置と回転をVRC Parent Constraintで制御します。同じTransformを次の仕組みが制御すると、位置ずれ、振動、意図しない移動、既存機能の停止が発生する可能性があります。

- 別のParent ConstraintまたはPosition Constraint
- Transformを変更するAnimator
- 左右持ち替えやワールド固定
- 一部のPhysBone構成

競合する場合は、専用の可動ルートを追加するか、追従状態に応じて制御元を切り替えてください。

## Roll Deformation

- VRCFury 1.1403.0以降が必要です。未導入または古い場合はRoll Deformationをビルド対象から除外し、残りのアバタービルドを続行します。
- 独立動作で正式対応するシェーダーはlilToon 1.3.7以降の標準lilToonです。未導入または古い場合は、独立動作を含むRoll Deformationをビルド対象から除外します。
- SPS Plug共有だけで動作するRoll DeformationにはlilToonの最低バージョン要件を適用しません。
- 独立Roll ResolverとSPSTracker本体はSocketを個別に解決するため、複数Socketが近接すると異なるSocketを選ぶ可能性があります。
- Socket Upにはアバター間の共通規格がありません。SocketによってはRuntime Offsetによる補正が必要です。
- Rendererの頂点をシェーダーで剛体回転させるため、Transform、Collider、PhysBone、Constraintは回転しません。
- 同じRendererを複数のRoll Deformationコンポーネントから制御できません。

詳しくは[Roll Deformation](roll-deformation.md)を参照してください。

## サポート範囲

一般購入者向けに案内できるのは、標準PrefabとProductAdapterによる基本導入までです。

既存商品の左右持ち替え・ワールド固定を残すためのAnimator改変は、商品ごとに構造が異なるため個別対応となります。対応商品を制作・販売する開発者は、対象商品の作者が定める利用規約とサポート範囲も確認してください。
