# Review Checklist

## Layering

- controller や handler に業務ロジックが入り込んでいないか
- repository が transport 型や HTTP 文脈を知っていないか
- usecase または interactor が外部 I/O の細部を持ちすぎていないか

## Wiring

- constructor 呼び出しがすべて更新されているか
- `dig` などの container 登録漏れがないか
- router や gRPC service registration の追加漏れがないか

## Persistence

- migration がコード変更に追随しているか
- transaction が write 単位で適切に張られているか
- `sql.ErrNoRows` や更新件数 0 件の扱いが妥当か
- read と write が別 transaction になって破綻していないか

## Config and Runtime

- env 名、default 値、読み込み順が妥当か
- Docker, Makefile, local run 手順が壊れていないか
- health check や metrics のような運用導線に影響がないか

## Tests

- 正常系だけでなく異常系があるか
- DB 依存があるなら integration test の必要性を見落としていないか
- 既存テスト戦略に沿っているか