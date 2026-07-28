# 制約と互換性

## 追従対象

- 追従対象側で、対応するVRCFury SPS Socketが有効になっている必要があります。
- 位置・ヨー方向・ピッチ方向へ追従します。
- Socket軸回りのロール回転には追従しません。

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

## サポート範囲

一般購入者向けに案内できるのは、標準PrefabとProductAdapterによる基本導入までです。

既存商品の左右持ち替え・ワールド固定を残すためのAnimator改変は、商品ごとに構造が異なるため個別対応となります。対応商品を制作・販売する開発者は、対象商品の作者が定める利用規約とサポート範囲も確認してください。
