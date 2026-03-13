# Feature Checklist

- どの層に機能を置くかを先に決めたか
- interface と実装の両方を更新したか
- constructor または DI container の配線を更新したか
- router, handler, controller, service 登録を確認したか
- request と response の変換責務が薄いままか
- transaction が必要な処理を usecase 側でまとめたか
- migration や seed が必要なら追加したか
- config や env の追加項目を整理したか
- `go test` または Makefile の検証手順を回したか
- README や運用手順の差分が必要か確認したか