---
name: go-backend-review
description: 'Review or debug a Go backend change. Use for golang code review, regression hunting, repository and usecase boundary checks, DI wiring, gRPC or Echo routes, migration completeness, config mistakes, and test gaps in Task-Controller-style or hal-cinema-style backends.'
argument-hint: 'Describe the PR, bug, or area to review in the Go backend'
user-invocable: true
---

# Go Backend Review

## When to Use

- Go バックエンドのレビューをするとき
- 変更がレイヤ境界を壊していないか確認したいとき
- route, controller, usecase, repository の配線漏れを見たいとき
- migration, config, Docker, Makefile まで含めた変更漏れを洗いたいとき
- gRPC や Echo の挙動不整合、DB 更新漏れ、transaction の境界ミスを疑うとき

## Review Reference

レビュー前に [review-checklist.md](./references/review-checklist.md) を確認します。

## Procedure

1. 変更差分から、どのリクエスト経路または起動経路に影響するかを特定する。
2. router または service registration から、controller, usecase, repository まで到達できるかを確認する。
3. 追加された分岐やバリデーションが transport 層に寄りすぎていないかを見る。
4. DB 更新を伴う変更では、transaction, migration, no rows handling, rollback 前提の抜けを確認する。
5. config や env の追加がある場合、デフォルト値、読み込み位置、ローカル起動手順の破綻がないか確認する。
6. テストが不足している場合は、どのケースが未検証かを具体的に指摘する。

## Focus Areas

- レイヤ責務の崩れ
- DI や constructor 配線漏れ
- route 登録漏れ
- transaction のスコープ不整合
- migration とコードの不一致
- エラー処理の粒度不足
- integration test 不足

## Output Style

- 問題は重大度順に並べる
- 可能なら root cause を書く
- 単なる好みではなく、挙動リスクを中心に指摘する
- 問題がなければ、その旨と残るテストギャップだけを短く述べる