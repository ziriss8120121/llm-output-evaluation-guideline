# ビジネス価値：区分2の網羅性（MECE）検討

## 1. 目的

LLM出力評価における **区分1「ビジネス価値」配下の区分2** について、網羅性を高め、可能な限りMECEに近い分類を設計するための調査結果を整理する。

本検討の目的は、区分2を最終確定することではなく、既存のソフトウェア品質モデル、生成AI評価サービス、LLM評価フレームワーク等を比較し、以下を明確にすることである。

- ビジネス価値配下で考慮すべき主要な出力品質・利用価値の領域
- 区分2候補に不足・重複がないか
- 固定済みの「事業リスク」と二重評価にならないか
- プロンプトUTで評価可能なものと、別の評価領域を混在させていないか
- 区分2をカタログ化する際の根拠

---

## 2. 調査方針

単一のフレームワークだけで区分2を決定せず、以下の3系統を横断して確認する。

1. **ソフトウェア・AIシステムの品質を広く整理した標準・品質モデル**
2. **生成AI / LLMの出力品質を実際に評価するサービス・フレームワーク**
3. **ユースケース固有の評価設計を支援するLLM評価手法**

主な参照文献・フレームワークは以下とする。

| 文献・フレームワーク | 主な役割 |
|---|---|
| ISO/IEC 25010:2023 | ソフトウェア品質、特に機能適合性に関する上位概念の確認 |
| ISO/IEC 25059:2023 | AIシステム品質としての抜け漏れ確認 |
| Amazon Bedrock Model Evaluation | 生成AI出力品質の主要評価軸の抽出 |
| Microsoft Foundry Evaluators | タスク達成・完全性・関連性・表現品質等のクロスチェック |
| Google Vertex AI Evaluation | 指示遵守・表現品質等の補完 |
| DeepEval | LLMアプリケーション評価軸の補完 |
| Ragas | 関連性・忠実性・正確性等の概念分離の確認 |
| OpenAI Evals / Graders | ユースケース固有の評価基準設計の考え方の確認 |

---

## 3. 最も参考になる文献

### 3.1 ISO/IEC 25010:2023

今回の目的に対して、上位概念の整理に最も参考になるのは **ISO/IEC 25010:2023** である。

ISO/IEC 25010では、ソフトウェア・ICT製品の品質特性を整理しており、特に今回の「LLM出力が期待する価値を提供できているか」という観点に近いものとして **Functional Suitability（機能適合性）** がある。

Functional Suitability配下では、以下の3つが区別される。

- **Functional Completeness**：必要な機能・タスクをどの程度カバーしているか
- **Functional Correctness**：必要な精度で正しい結果を提供できるか
- **Functional Appropriateness**：指定されたタスク・目的の達成をどの程度促進できるか

この分類から、少なくとも以下の概念を別々に検討する必要があることが分かる。

- 必要なものが揃っているか
- 正しいか
- 目的に対して適切か

### 3.2 ISOの分類設計から得られる重要な示唆

「役に立つか」「目的を達成できるか」だけを一つの包括的な区分にすると、正確性・完全性・適切性等の下位要因と重複しやすい。

そのため、区分2を設計する際は、

> **出力の価値・品質のどの側面を評価しているか**

という分類軸を揃え、上位概念と下位要因を同一階層に混在させない必要がある。

また、本ガイドラインでは固定済みの区分1「事業リスク」で誤情報・誤誘導を評価するため、ISO上のFunctional Correctnessをそのままビジネス価値の区分2として採用すると二重評価になる可能性がある。

---

## 4. Amazon Bedrock Model Evaluation

Amazon Bedrockでは、生成AI出力の評価指標として、代表的に以下が扱われている。

- Correctness
- Completeness
- Faithfulness
- Helpfulness
- Logical Coherence
- Relevance
- Following Instructions
- Professional Style and Tone
- Harmfulness
- Stereotyping
- Refusal

今回の区分2設計に対して、特に重要な候補は以下である。

- **Completeness**
- **Helpfulness**
- **Relevance**
- **Following Instructions**
- **Logical Coherence**
- **Professional Style and Tone**

一方、Correctness / Faithfulnessは、固定済みの事業リスク「誤情報・誤誘導」との重複可能性があるため、後工程で配置先を整理する必要がある。

また、Harmfulness / Stereotyping等の安全性指標は、ビジネス価値ではなく固定済みの事業リスク側で扱う。

---

## 5. Microsoft Foundry Evaluators

Microsoft Foundryでは、生成AI・AI Agent向けの評価として、代表的に以下が扱われている。

- Customer Satisfaction
- Task Completion
- Coherence
- Groundedness
- Response Completeness
- Fluency
- Relevance

この分類からは、以下の候補を補完できる。

- **Task Completion**：タスク・目的達成
- **Response Completeness**：必要な内容の充足
- **Relevance**：要求との関連性
- **Coherence / Fluency**：論理性・自然さ等の表現品質

Groundednessは、入力・参照情報との整合という性質を持つため、固定済みの事業リスク「誤情報・誤誘導」との役割分担を後工程で整理する必要がある。

Customer Satisfactionは総合的な利用価値に関する指標であるが、複数の品質要因を包含し得るため、区分2としてそのまま横並びに置くかは慎重に検討する必要がある。

---

## 6. Google Vertex AI Evaluation

Google Vertex AIでは、生成AI出力の評価指標として、以下のような概念が扱われている。

- Coherence
- Fluency
- Instruction Following
- Groundedness
- Verbosity
- Text Quality

今回の設計では、特に以下の補完に有効である。

- **Instruction Following**：明示された条件・形式・制約への適合
- **Coherence / Fluency**：論理性・自然さ
- **Verbosity**：冗長性・簡潔性
- **Text Quality**：文章としての総合的な品質

これらは、区分2で細分化するか、より上位の「表現品質」に統合して下位の観点で扱うかを後工程で検討する。

---

## 7. DeepEval / Ragas / OpenAI Evals

### 7.1 DeepEval

DeepEvalでは、以下のような評価指標が扱われている。

- Task Completion
- Answer Relevancy
- Faithfulness
- Conversation Completeness
- Contextual Relevancy
- Contextual Recall
- Contextual Precision

Task Completionは「タスク達成」という品質概念の参考になる一方、現在のDeepEvalのTask Completion Metricは主にAgent / trajectory評価で利用されるため、本ガイドラインでは概念上のクロスチェックとして参照する。

Contextual Recall / Precision等はRetrieverやRAG検索品質に近く、最終的なLLM出力評価とは分離する必要がある。

### 7.2 Ragas

Ragasでは、Response Relevancy、Faithfulness、Answer Correctness等が扱われている。

特に、

- 関連しているか
- 根拠に忠実か
- 正しいか

を別概念として扱っている点が、区分候補の意味を分離する際に参考になる。

### 7.3 OpenAI Evals / Graders

OpenAI Evals / Gradersは、すべてのユースケースに固定された品質分類を適用するというより、評価対象ごとにTesting CriteriaやRubricを定義する考え方を取る。

この点から、区分2では案件横断で使える共通の品質領域までを整理し、具体的なユースケース固有価値は下位の観点・評価項目・評価基準で具体化する設計が適している。

---

## 8. 固定済み「事業リスク」との役割分担

事業リスクの区分2は、現段階で以下の5区分に固定する。

1. **公平性・差別**
2. **有害・危険コンテンツ**
3. **誤情報・誤誘導**
4. **プライバシー・機密情報**
5. **権利侵害**

ビジネス価値側では、これらと同じ内容を二重に評価しないことを前提とする。

特に外部フレームワークで頻出する以下は、ビジネス価値の候補として一度洗い出すが、配置先を後工程で検討する。

- Correctness / Accuracy
- Faithfulness
- Groundedness
- Harmfulness
- Stereotyping / Unfairness
- Privacy

判断の基本方針は以下とする。

```text
損失・不利益・誤認・権利侵害等を防ぐための評価
→ 事業リスク

目的・要件・利用価値をどの程度満たすかの評価
→ ビジネス価値
```

---

## 9. MECE化に向けた設計方針

区分2は、以下の問いに統一するのが望ましい。

> **出力の価値・品質のどの側面を評価するのか**

この問いに揃えることで、以下の混在を避ける。

### 区分2として検討するもの

- タスク・目的への適合
- 必要な内容の充足
- 要求との関連性
- 明示された指示への適合
- 論理性・自然さ・読みやすさ等の表現品質
- 正確性・根拠への忠実性（ただし事業リスクとの重複を要確認）

### 区分2とは別の評価領域として扱うもの

- 本番KPI・事業成果
- 実利用上の効果測定
- 業務効率・コスト
- モデル・システム自体のロバスト性・可用性
- Retriever / RAG検索品質
- システムセキュリティ

これにより、「出力品質」と「その結果として得られる事業成果」や「システム自体の品質」を同一階層に混在させない。

---

## 10. 区分2候補（暫定）

文献を横断して整理すると、現段階では以下を区分2候補として検討する価値がある。

| 区分2候補 | 主な内容 | 主な根拠 |
|---|---|---|
| 目的・タスク適合 | LLM機能固有の目的、期待するタスク・価値への適合 | ISO / Microsoft / AWS / DeepEval |
| 完全性 | 必要な情報・要素・論点の不足がないこと | ISO / AWS / Microsoft |
| 正確性 | 入力・参照情報・事実に対して正しいこと | ISO / AWS / Ragas |
| 関連性 | 要求や目的に関係する内容へ焦点を当てること | AWS / Microsoft / DeepEval / Ragas |
| 指示遵守 | 明示された条件・形式・制約への適合 | AWS / Google |
| 根拠への忠実性 | 入力・参照コンテキストに忠実であること | AWS / Microsoft / DeepEval / Ragas |
| 論理的一貫性 | 内容・論理の流れに矛盾や破綻がないこと | AWS / Microsoft / Google |
| 自然さ・流暢性 | 文法・語彙・文章として自然であること | Microsoft / Google |
| スタイル・トーン | 用途に適した文章スタイル・トーンであること | AWS / Google |
| 簡潔性・冗長性 | 不要な繰り返しや過度な冗長さがないこと | Google等 |
| ユーザー満足 | 利用者にとって総合的に満足できる出力であること | Microsoft |
| 拒否の適切性 | 必要な回答を不当に拒否せず、安全上必要な場合に適切に拒否すること | AWS等 |
| 堅牢性 | 条件変化に対して品質を維持できること | ISO/IEC 25059等 |
| RAG検索品質 | 必要なコンテキストの取得・ランキング品質 | DeepEval / Ragas等 |
| 安全性 | 有害性、差別、プライバシー等 | AWS / Microsoft等 |

> NOTE: 上記は確定版ではない。次工程で、同義・類似概念の統合、固定済み「事業リスク」との重複排除、プロンプトUTのスコープ適合性、区分同士の重複・粒度を検証する。

---

## 11. 次に実施すること

区分2を確定する前に、以下のクロスマッピングを行う。

```text
ISO/IEC 25010・25059
        ×
AWS Bedrock
        ×
Microsoft Foundry
        ×
Google Vertex AI
        ×
DeepEval / Ragas / OpenAI Evals
        ×
固定済み「事業リスク」
        ×
区分2候補
```

その上で、以下を実施する。

1. 同義・類似する評価軸を統合する
2. 上位概念と下位要因が同一階層に混在していないか確認する
3. 固定済み「事業リスク」と重複するものをビジネス価値側から除外する
4. 「プロンプトUTにおけるLLM出力評価」のスコープ外を除外する
5. 区分2同士の重複・粒度を確認する
6. 実案件を複数当てて不足・重複を確認する

この検討結果を踏まえて、別ドキュメント「ビジネス価値：区分2の選定根拠」で、最終的に採用する区分2と、統合・除外した候補、その判断理由を整理する。

---

## 12. 参考文献

- ISO/IEC 25010:2023, Systems and software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — Product quality model  
  https://www.iso.org/standard/78176.html

- ISO/IEC 25059:2023, Software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — Quality model for AI systems  
  https://www.iso.org/obp/ui#iso:std:iso-iec:25059:ed-1:v1:en

- Amazon Bedrock, Use metrics to understand model performance  
  https://docs.aws.amazon.com/bedrock/latest/userguide/model-evaluation-metrics.html

- Microsoft Foundry, Run evaluations  
  https://learn.microsoft.com/en-us/azure/ai-studio/how-to/evaluate-generative-ai-app

- Google Cloud, Vertex AI Generative AI evaluation  
  https://cloud.google.com/vertex-ai/generative-ai/docs/models/evaluation-overview

- DeepEval, Introduction to LLM Evaluation Metrics  
  https://deepeval.com/docs/metrics-introduction

- Ragas, Response Relevancy  
  https://docs.ragas.io/en/latest/concepts/metrics/available_metrics/answer_relevance/

- OpenAI, Evals API  
  https://platform.openai.com/docs/api-reference/evals

- OpenAI, Graders API  
  https://platform.openai.com/docs/api-reference/graders