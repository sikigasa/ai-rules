---
name: go-backend-review
description: 'Go バックエンド変更のレビューやデバッグに使う。golang code review、回帰調査、repository と usecase の境界確認、DI 配線、gRPC や Echo の route、migration の完了性、config ミス、test の抜けを確認する。Task-Controller と hal-cinema backend の構成向け。'
argument-hint: 'レビューしたい PR、バグ、調査したい Go バックエンド領域を記述する'
user-invocable: true
---

# Go バックエンドレビュー

## 使う場面

- Go バックエンドのレビューをするとき
- 変更がレイヤ境界を壊していないか確認したいとき
- route, controller, usecase, repository の配線漏れを見たいとき
- migration, config, Docker, Makefile まで含めた変更漏れを洗いたいとき
- gRPC や Echo の挙動不整合、DB 更新漏れ、transaction の境界ミスを疑うとき

## 参照資料

レビュー前に [review-checklist.md](./references/review-checklist.md) を確認します。

## 手順

1. 変更差分から、どのリクエスト経路または起動経路に影響するかを特定する。
2. router または service registration から、controller, usecase, repository まで到達できるかを確認する。
3. 追加された分岐やバリデーションが transport 層に寄りすぎていないかを見る。
4. DB 更新を伴う変更では、transaction, migration, no rows handling, rollback 前提の抜けを確認する。
5. config や env の追加がある場合、デフォルト値、読み込み位置、ローカル起動手順の破綻がないか確認する。
6. テストが不足している場合は、どのケースが未検証かを具体的に指摘する。

## 注目観点

- レイヤ責務の崩れ
- DI や constructor 配線漏れ
- route 登録漏れ
- transaction のスコープ不整合
- migration とコードの不一致
- エラー処理の粒度不足
- integration test 不足

## 出力方針

- 問題は重大度順に並べる
- 可能なら root cause を書く
- 単なる好みではなく、挙動リスクを中心に指摘する
- 問題がなければ、その旨と残るテストギャップだけを短く述べる