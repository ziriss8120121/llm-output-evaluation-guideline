# LLM Output Evaluation Guideline

LLM出力評価プロセスの標準化を検討するためのリポジトリです。

## 目的

本検討では、LLM出力評価の以下2点にスコープを絞ります。

- **信頼性**: 評価者が変わっても評価結果が大きく変わらないこと
- **妥当性**: 案件の目的・要件に対して適切な観点・基準で評価できること

Python等によるLLM出力生成・評価の自動化など、**効率性を目的とした取り組みは本検討のスコープ外**とします。

## 評価体系の考え方

評価体系は以下の階層で整理します。

```text
区分1
  ↓
区分2（完了条件）
  ↓
観点
  ↓
観点の説明
  ↓
5段階の評価基準
  ↓
合格基準
```

現時点で、区分1は以下の2区分を全案件共通とします。

- **事業リスク**: LLM出力によって事業・ユーザー・第三者等に問題や損失を生じさせないか
- **ビジネス価値**: LLM出力によって期待する事業・業務・ユーザー価値を実現できるか

区分2以降は、共通化できるものは共通化し、案件依存性が高いものは候補カタログとして管理した上で、案件ごとに対象 / 対象外を判断する方針です。

## Documents

### 事業リスク

1. [区分2の網羅性（MECE）検討](docs/business-risk-category2-coverage.md)
2. [区分2の選定根拠](docs/business-risk-category2-selection-rationale.md)
3. [評価を通して目指すべき状態](docs/business-risk-target-states.md)

### ビジネス価値

1. [区分2の網羅性（MECE）検討](docs/business-value-category2-coverage.md)
2. [区分2の選定根拠](docs/business-value-category2-selection-rationale.md)
3. [評価を通して目指すべき状態](docs/business-value-target-states.md)

## Templates

事業リスク・ビジネス価値を対象としたプロンプトUT用FMTを用意しています。

1. [【FMT】完了条件_プロンプトUT](templates/【FMT】完了条件_プロンプトUT.md)
2. [【FMT】観点シート_プロンプトUT](templates/【FMT】観点シート_プロンプトUT.md)
3. [【FMT】仕様書_プロンプトUT](templates/【FMT】仕様書_プロンプトUT.md)

3つのFMTは、`完了条件No → 観点No` で前後工程を突合し、評価目的から観点・評価基準・合格基準までトレーサビリティを確保する前提で利用します。