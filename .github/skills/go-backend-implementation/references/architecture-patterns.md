# Go バックエンド設計パターン

## Task-Controller に近い構成

- `cmd/app` に起動処理を集める
- `cmd/config` で env を読む
- `internal/usecase` が gRPC サービス実装を持つ
- `internal/infra` が repository 実装を持つ
- `transaction` を usecase 側から明示的に扱う
- migration が独立して存在する
- testcontainers で DB テストを回す余地がある

この型では、`main -> usecase -> infra` の配線が比較的直線的です。新機能では interface と constructor の更新漏れが起きやすいので、配線を最後まで確認します。

## hal-cinema backend に近い構成

- `internal/adapter/controller` で transport 層を切る
- `internal/adapter/gateway` や `repository` で外部依存を切る
- `internal/usecase/interactor` に業務ロジックを寄せる
- `internal/container` で DI を組み立てる
- router, middleware, health check, metrics などが独立している
- Makefile と Docker を前提にローカル開発を回す

この型では、controller や router にロジックを混ぜないこと、DI 定義の更新漏れを防ぐことが重要です。

## 共通ルール

- handler や controller は薄くする
- DB 変更は migration とセットで扱う
- config 読み込みを散在させない
- repository は永続化責務に閉じる
- usecase か interactor が transaction 境界を持つ
- entrypoint は wiring に徹し、業務ロジックを持たない
