# ai-rules

自己AIエージェント支援

## 追加したカスタム定義

このリポジトリには、Go バックエンド向けの custom agent と skills を追加しています。

- `.github/agents/golang-backend.agent.md`
- `.github/skills/go-backend-implementation/`
- `.github/skills/go-backend-review/`

## 想定している設計スタイル

以下の過去リポジトリに見られる構成を参考にしています。

- `cmd/` を薄いエントリポイントにする
- `internal/` 配下で責務を分離する
- `usecase` / `interactor` に業務ロジックを置く
- `infra` / `adapter` / `gateway` で外部 I/O を吸収する
- `repository` と `transaction` の境界を明示する
- `Makefile` / migration / config 読み込みを整備する

## 使い分け

- agent: Go の API / gRPC / Echo / DB まわりの実装や整理を一括で任せたいとき
- implementation skill: 新機能追加、責務整理、配線追加、テスト追加を進めたいとき
- review skill: レビュー、バグ調査、配線漏れ、transaction 漏れ、migration 漏れを点検したいとき
