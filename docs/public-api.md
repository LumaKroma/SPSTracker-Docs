# 公開API・パラメータ

日本語 | [English](en/public-api.md)

このページに記載されたAnchorとパラメータだけを、対応アイテムから利用する公開APIとして扱ってください。記載されていない階層、Animator、Animation、Constraint、ProductAdapter内部Parameterは互換性保証の対象外です。

## Transform API

### HeldAnchor

```text
SPSTracker/API/HeldAnchor
```

追従していないときの保持位置です。対応アイテムの手持ち状態や待機状態へ使用します。

### TrackedAnchor

```text
SPSTracker/API/TrackedAnchor
```

Socket追従とOffset適用後の位置・回転です。対応アイテムの追従状態へ使用します。

通常の導入では両Anchorの名前や階層を変更しないでください。独立したModular Avatar PrefabからはMA Bone Proxyなどでビルド時に接続します。

## SPSTrackerメニューパラメータ

すべてのSPSTrackerパラメータは次のPrefixを使用します。

```text
LumaKroma/ST/
```

| パラメータ | 型 | 初期値 | 用途 |
| --- | --- | --- | --- |
| `LumaKroma/ST/Activate` | Bool | OFF | Socket検出と追従を切り替える |
| `LumaKroma/ST/ShowGizmo` | Bool | ON | `Activate`中のローカル矢印と検知範囲を切り替える |
| `LumaKroma/ST/Offset` | Float | 0.5 | Socket軸方向へ約-10～+10cm調整する（0.5で補正なし） |

アドオンから操作する場合はSPSTrackerが定義した既存Parameterを使用し、同名ParameterをExpressions Parametersへ重複登録しないでください。

## 追従状態

```text
LumaKroma/ST/TrackingStart
```

対応アイテムから参照できる読み取り専用Floatです。状態判定では0または1として扱います。

- `0`: 追従未成立
- `1`: 追従成立、または追従ロスト待機中

外部から書き換えず、Expressions Parameterとして追加・同期しないでください。

## Animator内の調整値

`LumaKroma/ST/HeldRange`は保持中の検知範囲を調整するFX Animator内のFloat値です。MA Parametersやメニューには登録されません。対応商品から依存する公開APIではないため、アドオンから参照・操作しないでください。

## ProductAdapter内部Parameter

v1.2のProductAdapterは、次の作成時Parameter名を内部で使用します。

| 作成時Parameter | 型 | 既定値 | 用途 |
| --- | --- | ---: | --- |
| `LumaKroma/ST/ProductAdapter/ItemVisible` | Bool | OFF | 商品表示 |
| `LumaKroma/ST/TrackingEnabled` | Bool | ON | Tracked Poseへの切り替え許可 |
| `LumaKroma/ST/ProductAdapter/WorldFixed` | Bool | OFF | ProductAdapter Constraintのワールド固定 |

これらはProductAdapterの実装詳細です。Menu VariantではModular Avatarがインスタンスごとに最終Parameter名を自動リネームします。

- 最終ビルド後の名前へ外部Animatorから依存しないでください。
- 同名Parameterを手動でExpressions Parametersへ追加しないでください。
- ProductAdapterの`Menu`階層をインスタンス外へ移動しないでください。
- 内部Parameter名を変更しないでください。

Base Prefabは`TrackingEnabled`と`WorldFixed`をAnimator内部値として持ちますが、Expression Parameterやメニューへ登録しません。Menu Variantだけが3つの同期Boolを追加します。

## 推奨する接続方式

対応アイテムは`TrackingStart`でHeld / Trackedを切り替えます。

| 状態 | 接続先 |
| --- | --- |
| `TrackingStart = 0` | `HeldAnchor` |
| `TrackingStart = 1` | `TrackedAnchor` |

利用者が商品ごとに追従を禁止できるようにする場合は、ProductAdapter Menu Variantのようにアドオン自身のスコープ内Parameterを定義してください。ProductAdapter内部Parameterの最終名を外部から参照しないでください。

実装例は商品に同梱された`SPSTracker_ProductAdapter.prefab`を参照できます。
