# Changelog

日本語 | [English](CHANGELOG.en.md)

## v1.1.0 - 2026-08-12

- ProductAdapter Setup Assistantによる初期設定、Held / Tracked姿勢編集、検証を文書化
- 任意のRoll追従機能「SPS Tracker Roll Deformation」の資料を追加
- VRCFury SPS Plugとの併用、独立Resolver、lilToon対応範囲を文書化
- ProductAdapter関連Inspectorの日英表示切り替えと、設定重複・依存バージョンの検証を文書化
- `SPSTracker_TrackingMenuItem.prefab`を`Activate`ではなく`TrackingEnabled`で追従許可を切り替える仕様へ更新
- ProductAdapter本体と任意メニュー項目の追加コストを訂正
- Roll Deformationで必要なVRCFury 1.1403.0以降を明記
- 独立Roll Deformationで必要なlilToon 1.3.7以降と、バージョン不足時の警告を明記
- Roll Deformationの依存が未導入または必要バージョン未満でも、本体ビルドを止めず対象機能だけを除外する仕様を文書化
- `HeldRange`に応じた追従範囲の自動調整、追従ロスト時の段階的な再検知範囲拡大を文書化
- 検知範囲による実効Gain差を補正する追従安定化を文書化

## v1.0.0 - 2026-07-27

- SPSTracker v1.0.0向け開発者ドキュメントの初期構成を作成
- 公開Anchor、パラメータ、ProductAdapter、制約事項を文書化
- 英語版ドキュメントを追加
