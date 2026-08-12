# 公開API・パラメータ

日本語 | [English](en/public-api.md)

このページに記載されたAnchorとパラメータだけを、対応アイテムから利用する公開APIとして扱ってください。記載されていない階層、パラメータ、Animator、Animation、Constraintなどは互換性の保証対象ではなく、バージョン更新によって変更される場合があります。

## Transform API

### HeldAnchor

```text
SPSTracker/API/HeldAnchor
```

追従していないときの保持位置です。対応アイテムの手持ち状態や待機状態の接続先として使用します。

### TrackedAnchor

```text
SPSTracker/API/TrackedAnchor
```

Socket追従とOffset適用後の位置・回転です。対応アイテムの追従状態の接続先として使用します。

通常の導入では、両Anchorの名前や階層を変更しないでください。独立したModular Avatar Prefabから参照する場合は、MA Bone Proxyなどを使用してビルド時に接続します。

## メニューパラメータ

すべてのSPSTrackerパラメータは次のPrefixを使用します。

```text
LumaKroma/ST/
```

| パラメータ | 型 | 初期値 | 用途 |
| --- | --- | --- | --- |
| `LumaKroma/ST/Activate` | Bool | OFF | Socket検出と追従を切り替える |
| `LumaKroma/ST/ShowGizmo` | Bool | ON | `Activate`がONの間、ローカル表示の矢印と検知範囲を切り替える |
| `LumaKroma/ST/Offset` | Float | 0.5 | Socket軸方向へ約-10～+10cm調整する（0.5で補正なし） |
| `LumaKroma/ST/NearestMode` | Bool | OFF | Nearest Lockを切り替える |

アドオンからこれらを操作する場合は、SPSTrackerが定義した既存パラメータを使用してください。同名パラメータをExpressions Parametersへ重複登録しないでください。

## Animator内の調整値

`LumaKroma/ST/HeldRange`は、保持中の検知範囲を調整するFX Animator内のFloat値です。MA Parametersやメニューには登録されていません。追従中の検知範囲とGainはこの値と追従状態に応じて自動調整されるため、公開パラメータとしての`TrackingRange`や`TrackingGain`はありません。

`HeldRange`は利用者がAnimatorから調整するための値であり、対応商品から依存する公開APIではありません。アドオンからは参照・操作しないでください。

## ProductAdapterの任意パラメータ

```text
LumaKroma/ST/TrackingEnabled
```

ProductAdapterがTracked Poseへ切り替わることを許可するBool値です。

- `0`: `TrackingStart`がONでもHeld Poseを維持する
- `1`: `TrackingStart`がONになったときTracked Poseへ切り替える

ProductAdapter内の初期値はONです。`SPSTracker_TrackingMenuItem.prefab`を使用した場合のみ、同名の同期Bool ParameterがExpressions Parametersへ追加されます。複数のProductAdapterで同じ値を共有し、各商品へ同名Parameterを重複登録しないでください。

## 追従状態

```text
LumaKroma/ST/TrackingStart
```

対応アイテムから参照できる、読み取り専用のFloat値です。状態判定では0または1として扱います。

- `0`: 追従未成立
- `1`: 追従成立、または追従ロスト待機中

この値は外部から書き換えず、Expressions Parameterとして追加・同期しないでください。

## 推奨する接続方式

対応アイテムは、`TrackingStart`と任意の`TrackingEnabled`に応じて接続先を切り替える構成を推奨します。

| 状態 | 接続先 |
| --- | --- |
| `TrackingStart = 0` | `HeldAnchor` |
| `TrackingStart = 1`かつ`TrackingEnabled = 1` | `TrackedAnchor` |
| `TrackingEnabled = 0` | `HeldAnchor` |

切り替え例として、商品に同梱された`SPSTracker_ProductAdapter.prefab`を参照できます。
