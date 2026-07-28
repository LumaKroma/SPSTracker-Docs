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
| `LumaKroma/ST/ShowGizmo` | Bool | ON | ローカル表示のギズモを切り替える |
| `LumaKroma/ST/Offset` | Float | 0.5 | Socket軸方向へ約0～5cm調整する |
| `LumaKroma/ST/NearestMode` | Bool | OFF | Nearest Lockを切り替える |

アドオンからこれらを操作する場合は、SPSTrackerが定義した既存パラメータを使用してください。同名パラメータをExpressions Parametersへ重複登録しないでください。

## 追従状態

```text
LumaKroma/ST/TrackingStart
```

対応アイテムから参照できる、読み取り専用のBool値です。

- `0`: 追従未成立
- `1`: 追従成立、または追従ロスト待機中

この値は外部から書き換えず、Expressions Parameterとして追加・同期しないでください。

## 推奨する接続方式

対応アイテムは、`TrackingStart`に応じて接続先を切り替える構成を推奨します。

| 状態 | 接続先 |
| --- | --- |
| 追従未成立 | `HeldAnchor` |
| 追従成立、または追従ロスト待機中 | `TrackedAnchor` |

切り替え例として、商品に同梱された`SPSTracker_ProductAdapter.prefab`を参照できます。
