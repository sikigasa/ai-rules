---
name: go-backend-implementation
description: 'Implement or refactor a Go backend feature in a layered architecture. Use for golang API, gRPC, Echo, repository, usecase, interactor, DI, config, migration, Docker, or test work inspired by Task-Controller and hal-cinema backend patterns.'
argument-hint: 'Describe the feature, bugfix, or refactor target in the Go backend'
user-invocable: true
---

# Go Backend Implementation

## When to Use

- Go バックエンドに新機能を追加するとき
- handler, controller, router, usecase, interactor, repository の責務を整理するとき
- gRPC や Echo のエンドポイントを追加するとき
- DB 操作を追加し、migration やテストまで揃えたいとき
- Task-Controller 系の軽量レイヤ構成か、hal-cinema backend 系の adapter と DI 構成に寄せたいとき

## Reference Patterns

先に [architecture-patterns.md](./references/architecture-patterns.md) を確認し、今回のリポジトリがどちらの構成に近いかを見極めます。

実装抜けを防ぐために [feature-checklist.md](./assets/feature-checklist.md) を使います。

## Procedure

1. 現在の package 構成を確認し、変更対象の責務がどの層に属するかを決める。
2. エントリポイントから処理の流れを追い、router, handler, controller, usecase, repository のどこまで触る必要があるか整理する。
3. 既存が constructor injection なら、その流儀を崩さず interface と実装を増やす。
4. DB 変更がある場合は、コード変更だけで終わらせず migration, seed, config, local 起動手順への影響も確認する。
5. handler や controller には変換と transport の責務だけを残し、業務条件分岐は usecase または interactor に寄せる。
6. repository や gateway は永続化・外部 API 呼び出しに限定し、HTTP や gRPC の詳細を持ち込まない。
7. 変更に応じて unit test または DB を使う integration test を追加し、可能なら `go test` や Makefile の対象コマンドで検証する。

## Implementation Heuristics

- `cmd/` は薄く保つ
- `internal/domain` や `internal/entities` はフレームワーク依存を避ける
- transaction は usecase 層でまとめる
- config は専用 package 経由で読む
- 既存が `dig` などの DI container を使う場合だけ container 定義を更新する
- 既存が明示的 constructor 配線なら、main や wiring 側だけを最小変更する

## Expected Outcome

- 機能追加に必要な層がすべてつながっている
- package の責務が悪化していない
- migration や設定差分が放置されていない
- テストか検証手順が最低限そろっている