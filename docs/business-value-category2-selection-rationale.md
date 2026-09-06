# ビジネス価値：区分2の選定根拠

## 1. 目的

本ドキュメントでは、LLMを用いた機能実装における**出力評価**について、区分1「ビジネス価値」配下の区分2をどのような根拠で設定したかを整理する。

想定する利用例は以下のようなLLM機能である。

```text
求人票
  ↓
LLM
  ↓
スカウト文面自動生成
```

このほか、求人要約、検索条件生成、レコメンド理由生成等、他のLLM機能実装にも横展開することを想定する。

本検討では、既存のソフトウェア品質モデル、生成AI評価サービス、LLM評価フレームワーク等から**出力品質・利用価値に関する評価軸を広く抽出**した上で、今回の「プロンプトUTにおけるLLM出力評価」というスコープと、固定済みの区分1「事業リスク」との重複有無に照らして取捨選択する。

目的は以下の3点である。

- **網羅性の向上**：担当者の思いつきだけで評価観点を決めず、既存フレームワークを母集団として検討する
- **説明可能性の向上**：なぜその区分を採用し、なぜ他の評価軸を除外・統合したかを説明できるようにする
- **二重評価の防止**：固定済みの「事業リスク」で評価する内容を「ビジネス価値」で重複して評価しない

---

## 2. 結論

ビジネス価値の区分2は以下の5区分とする。

1. **目的達成性**
2. **完全性**
3. **関連性**
4. **指示遵守**
5. **表現品質**

この5区分は、主要な外部評価フレームワークで繰り返し登場する出力品質軸を母集団とし、以下の条件で整理した結果である。

- プロンプトUTでLLMの**出力内容から評価可能**であること
- 案件横断で利用できる抽象度であること
- 固定済みの「事業リスク」と**二重評価にならない**こと
- 区分2を細分化しすぎず、案件固有の詳細は「観点シート」で展開できること

なお、これは「ビジネス価値に関係するすべての指標」を網羅した分類ではない。

> **LLM機能のプロンプトUTにおいて、出力そのものが期待する価値を提供できているかを評価するために必要な共通区分を抽出した結果**

である。

MECE性の詳細検証は、[ビジネス価値：区分2の網羅性（MECE）検討](business-value-category2-coverage.md)を参照する。

---

## 3. 今回の評価スコープ

### 3.1 対象

LLM機能が生成した**出力内容そのものから、機能の目的・要件・利用価値を満たしているか確認できる品質**を評価対象とする。

例えば以下を対象とする。

- そのLLM機能固有の目的・期待価値に適合した出力になっているか
- 必要な情報・要素が不足なく含まれているか
- 要求に関係する内容に適切に焦点を当てているか
- プロンプトで指定した条件・形式・制約等を守っているか
- 論理性、自然さ、読みやすさ、簡潔さ、スタイル・トーン等が用途に適しているか

### 3.2 対象外

以下はビジネス上重要ではあるが、本ガイドラインにおける**プロンプトUTのLLM出力評価とは別の評価領域**として扱う。

#### 本番KPI・事業成果

例：

- スカウト返信率
- 応募率・成約率
- 売上・利益
- CVR
- 継続率
- ユーザー満足度の実測値

これらはLLM出力の品質から影響を受ける可能性はあるが、プロンプトUTだけでは直接評価できないため、本番KPI・効果検証として別途扱う。

#### 業務効率・コスト

例：

- 文面作成時間の削減率
- APIレイテンシ
- 推論コスト
- トークンコスト
- 評価工数

本検討では「信頼性」「妥当性」にスコープを絞っており、効率性は別テーマとして扱う。

#### システム・モデル品質

例：

- モデル自体のロバスト性
- 可用性
- 性能劣化への耐性
- レスポンスタイム
- AIシステムの説明可能性
- モデルそのものの能力評価

これらはシステム・モデル評価として扱い、個別出力の価値評価とは分離する。

#### RAGの検索・取得品質そのもの

例：

- Context Precision
- Context Recall
- 検索結果のランキング品質
- Retrieverの性能

取得されたコンテキストを前提として生成された**最終出力**は本ガイドラインで評価できるが、Retrieverや検索処理そのものは別評価とする。

---

## 4. 事業リスクとの重複排除方針

事業リスクの区分2は現段階で以下の5区分に固定する。

1. **公平性・差別**
2. **有害・危険コンテンツ**
3. **誤情報・誤誘導**
4. **プライバシー・機密情報**
5. **権利侵害**

ビジネス価値側では、これらと**同じ内容を二重に評価しない**ことを原則とする。

| 問い | 配置先 |
|---|---|
| 出力による損失・不利益・誤認・権利侵害等を防げているか | **事業リスク** |
| 出力が目的・要件・利用価値をどの程度満たしているか | **ビジネス価値** |
| 両方に見える評価軸 | 原則どちらか一方に寄せ、二重評価しない |

特に、一般的なLLM評価で頻出する **Correctness / Accuracy / Faithfulness / Groundedness** は、本来は重要な出力品質指標である。

しかし、本ガイドラインでは、入力・参照情報・確認可能な事実との不整合、根拠にない生成、利用者の誤認につながる出力を、固定済みの事業リスク「**誤情報・誤誘導**」で評価する。

そのため、これらは重要性が低いから除外するのではなく、**二重評価を避けるためビジネス価値側から意図的に除外する**。

---

## 5. 参照した主要文献・フレームワーク

### 5.1 ISO/IEC 25010:2023

[ISO/IEC 25010:2023 - Product quality model](https://www.iso.org/standard/78176.html)

特に Functional Suitability に関係する以下を候補整理の参考とした。

- Functional Completeness
- Functional Correctness
- Functional Appropriateness

### 5.2 ISO/IEC 25059:2023

[ISO/IEC 25059:2023 - Quality model for AI systems](https://www.iso.org/obp/ui#iso:std:iso-iec:25059:ed-1:v1:en)

Functional Correctness、Robustness等を含むAI品質モデルとして、抜け漏れ確認に利用した。

### 5.3 Amazon Bedrock Model Evaluation

[Amazon Bedrock - Use metrics to understand model performance](https://docs.aws.amazon.com/bedrock/latest/userguide/model-evaluation-metrics.html)

主に以下を候補母集団として参照した。

- Correctness
- Completeness
- Faithfulness
- Helpfulness
- Logical coherence
- Relevance
- Following instructions
- Professional style and tone
- Harmfulness
- Stereotyping
- Refusal

### 5.4 Microsoft Foundry Evaluators

[Microsoft Foundry - Evaluate generative AI applications](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/evaluate-generative-ai-app)

主に以下を確認した。

- Customer Satisfaction
- Task Completion
- Coherence
- Groundedness
- Response Completeness
- Fluency
- Relevance

Safety Evaluatorが品質評価とは別に扱われていることも、事業リスクとの切り分け確認に利用した。

### 5.5 Google Vertex AI Evaluation

[Google Cloud - View and interpret evaluation results](https://cloud.google.com/vertex-ai/generative-ai/docs/models/eval-python-sdk/view-evaluation)

Coherence、Fluency、Instruction Following、Text Quality等を確認した。

### 5.6 DeepEval

[DeepEval - Evaluation Metrics](https://deepeval.com/docs/metrics-introduction)

Answer Relevancy、Faithfulness、Conversation Completeness、Contextual Relevancy等を確認した。

Task Completionについては「タスク達成」という品質概念のクロスチェックとして参照する。ただし、DeepEvalのTask Completion実装はAgentのtrace / trajectoryを前提とするため、本ガイドラインの単一プロンプトUTへ直接適用するものではない。

### 5.7 Ragas

[Ragas - Response Relevancy](https://docs.ragas.io/en/latest/concepts/metrics/available_metrics/answer_relevance/)

Response Relevancy、Faithfulness、Answer Correctness等を確認し、「関連性」と「正確性」を別概念として整理する際の参考とした。

### 5.8 OpenAI Evals / Graders

[OpenAI Evals](https://platform.openai.com/docs/api-reference/evals)

[OpenAI Graders](https://platform.openai.com/docs/api-reference/graders)

評価対象ごとにTesting CriteriaやRubricを定義する考え方を、共通区分から案件固有の観点・評価項目・評価基準へ展開する設計の参考とした。

---

## 6. 外部フレームワークからの候補抽出

| 候補概念 | 代表的な外部表現 | 主な参照元 |
|---|---|---|
| 目的・タスク達成 | Task Completion / Helpfulness / Functional Appropriateness | Microsoft / AWS / DeepEval / ISO |
| 正確性 | Correctness / Accuracy / Functional Correctness | AWS / ISO / Ragas |
| 完全性 | Completeness / Response Completeness / Functional Completeness | AWS / Microsoft / ISO |
| 関連性 | Relevance / Answer Relevancy / Response Relevancy | AWS / Microsoft / DeepEval / Ragas |
| 指示遵守 | Following Instructions / Instruction Following | AWS / Google |
| 根拠への忠実性 | Faithfulness / Groundedness | AWS / Microsoft / DeepEval / Ragas |
| 論理的一貫性 | Logical Coherence / Coherence | AWS / Microsoft / Google |
| 自然さ・流暢性 | Fluency | Microsoft / Google |
| スタイル・トーン | Professional Style and Tone / Text Quality | AWS / Google |
| 簡潔性・冗長性 | Conciseness / Verbosity | Google等 |
| ユーザー満足 | Customer Satisfaction | Microsoft |
| 堅牢性 | Robustness | ISO/IEC 25059等 |
| 安全性 | Harmfulness / Stereotyping / Violence / Hate etc. | AWS / Microsoft |
| RAG検索品質 | Context Precision / Context Recall / Contextual Relevancy | DeepEval等 |

---

## 7. 候補の取捨選択

候補母集団に対し、以下の4観点で採用・統合・除外を判断した。

1. **プロンプトUTで出力から評価できるか**
2. **案件横断で共通化できるか**
3. **事業リスクと重複しないか**
4. **他候補との包含・重複が大きすぎないか**

| 候補 | 今回の扱い | 理由 / 対応先 |
|---|---|---|
| Task Completion | **採用 / 統合** | 「目的達成性」へ統合。タスク達成という概念を参照する |
| Helpfulness | **採用 / 統合** | 独立区分にすると完全性・関連性・指示遵守・表現品質等を包含しすぎるため、固有価値を「目的達成性」へ寄せる |
| Functional Appropriateness | **採用 / 統合** | 「目的達成性」の主要な根拠として利用 |
| Completeness / Response Completeness | **採用** | 「完全性」。必要な内容・情報・論点が不足していないかを扱う |
| Relevance / Answer Relevancy | **採用** | 「関連性」。目的・要求と無関係な内容へ逸れていないかを扱う |
| Following Instructions / Instruction Following | **採用** | 「指示遵守」。明示された形式・条件・制約等を扱う |
| Logical Coherence / Coherence | **採用 / 統合** | 「表現品質」の下位観点として扱う |
| Fluency | **採用 / 統合** | 「表現品質」の下位観点として扱う |
| Professional Style and Tone | **採用 / 統合** | 「表現品質」の下位観点として扱う |
| Conciseness / Verbosity | **採用 / 統合** | 「表現品質」の下位観点として扱う |
| Correctness / Accuracy | **ビジネス価値から除外** | 固定済み事業リスク「誤情報・誤誘導」で評価するため |
| Functional Correctness | **ビジネス価値から除外** | 上記と同様 |
| Faithfulness / Groundedness | **ビジネス価値から除外** | 入力・参照情報・根拠との不整合は「誤情報・誤誘導」で評価するため |
| Harmfulness / Violence / Self-harm等 | **除外** | 固定済み事業リスク「有害・危険コンテンツ」で評価 |
| Stereotyping / Hate / Unfairness | **除外** | 固定済み事業リスク「公平性・差別」で評価 |
| Privacy関連 | **除外** | 固定済み事業リスク「プライバシー・機密情報」で評価 |
| Customer Satisfaction | **独立区分にはしない** | MicrosoftのEvaluatorは実ユーザーアンケートを必須とせず応答・会話から満足度を推定するが、目的達成性・関連性・表現品質等を包含するholisticな指標であり、共通区分2としては上位すぎるため |
| Robustness | **スコープ外 / 別軸** | 条件変化に対して品質を維持できるかというテスト設計・モデル品質の論点 |
| Context Precision / Context Recall | **スコープ外** | Retriever・RAG検索品質の評価。最終出力評価とは分離する |
| Refusal | **独立区分にはしない** | 不当な拒否は目的達成性または指示遵守、安全上必要な拒否は事業リスク側との関係で判断する |
| Efficiency / Cost / Latency | **スコープ外** | 本検討では効率性を対象外としている |

---

## 8. 最終区分2の選定理由

### 8.1 目的達成性

**他の共通品質区分では捉えきれない、そのLLM機能固有の目的・期待価値に適合した出力になっているか**を扱う。

主な根拠は以下である。

- Functional Appropriateness
- Task Completionという品質概念
- Helpfulnessのうちユースケース固有価値に関する部分

目的達成性を総合品質として広く定義すると、完全性・関連性・指示遵守・表現品質を包含してしまうため、本ガイドラインでは**ユースケース固有価値に限定**する。

例：

- スカウト文面生成：候補者に合わせた訴求になっているか、求人の魅力が伝わるか
- 求人要約：原文を読まなくても求人の特徴を短時間で把握できるか
- 検索条件生成：ユーザーの検索意図を検索条件として表現できているか

### 8.2 完全性

**目的・要求を満たすために必要な内容・情報・論点が不足なく含まれているか**を扱う。

主な根拠は以下である。

- Completeness
- Response Completeness
- Functional Completeness

「誤った情報があるか」は事業リスク「誤情報・誤誘導」で扱い、本区分では**必要なものが欠けていないか**に限定する。

また、形式・文字数等の明示的な制約は「指示遵守」で扱う。

### 8.3 関連性

**目的・要求に関係する内容に適切に焦点を当てた出力になっているか**を扱う。

主な根拠は以下である。

- Relevance
- Answer Relevancy
- Response Relevancy

境界は以下とする。

```text
完全性
→ 必要なものが足りているか

関連性
→ 不要・無関係な内容に逸れていないか

表現品質
→ 関連する内容をどう表現しているか
```

### 8.4 指示遵守

**プロンプトで明示された形式・条件・制約・出力方法等を守った出力になっているか**を扱う。

主な根拠は以下である。

- Following Instructions
- Instruction Following

例：

- 300文字以内
- 敬体で記載する
- 指定フォーマットで出力する
- 箇条書きを使用しない

明示された条件への違反は「指示遵守」を優先し、明示されていないが用途上不適切な表現は「表現品質」で扱う。

### 8.5 表現品質

**関連する内容を、成果物として論理的・自然・簡潔・読みやすく、用途に適したスタイル・トーンで表現できているか**を扱う。

外部フレームワークでは以下のように細分化される。

- Logical Coherence / Coherence
- Fluency
- Professional Style and Tone
- Text Quality
- Conciseness / Verbosity

これらは区分2では「表現品質」に統合し、案件ごとの観点シートで必要なものを選択・具体化する。

---

## 9. 区分2同士の境界

| 区分2 | 主な問い | 主に扱わないもの |
|---|---|---|
| **目的達成性** | 他の共通品質では捉えきれない、機能固有の目的・期待価値を実現できているか | 他4区分で評価できる一般品質 |
| **完全性** | 必要な内容・情報・論点が不足していないか | 情報の正誤、形式・文字数等の制約 |
| **関連性** | 目的・要求と関係する内容に焦点を当てているか | 文章そのものの読みやすさ・自然さ |
| **指示遵守** | 明示された形式・条件・制約を守っているか | 明示されていない暗黙的な文章品質 |
| **表現品質** | 関連する内容を論理的・自然・簡潔・用途に適した形で表現できているか | 明示的な指示違反、内容の誤り |

二重評価を防ぐため、案件での観点設計では以下を共通ルールとする。

> **同一の問題を複数区分で二重評価しない。複数区分に該当し得る場合は、その問題を最も直接的・具体的に表す区分へ寄せる。**

---

## 10. 採用しなかった評価軸に関する補足

### 10.1 Correctness / Accuracy

Correctness / Accuracyは重要な品質軸だが、本ガイドラインでは、入力情報と異なる内容、参照情報との矛盾、利用者が誤認する内容等を事業リスク「誤情報・誤誘導」で評価する。

そのため、一般的な品質論として正確性を否定するものではなく、**本ガイドライン固有の役割分担として事業リスク側に寄せる**。

### 10.2 Faithfulness / Groundedness

入力・参照情報に存在しない内容、参照情報との矛盾、根拠がない内容を事実として提示するケースは、事業リスク「誤情報・誤誘導」で扱う。

よって二重評価防止のため、ビジネス価値側の区分2には置かない。

### 10.3 Helpfulness

Helpfulnessは「役に立つか」という包括的な概念であり、完全性、関連性、指示遵守、表現品質等を包含し得る。

そのため独立区分にはせず、ユースケース固有価値に関する部分を「目的達成性」へ統合し、一般品質は他区分で扱う。

### 10.4 Customer Satisfaction

Customer Satisfactionは利用者が感じる総合成果を捉えるholisticな概念である。Microsoft FoundryのEvaluatorは実ユーザーアンケートを必須とせず、応答・会話から満足度を推定する。

一方、目的達成性、関連性、表現品質等と包含関係が大きく、共通区分2としては粒度が上位すぎるため独立区分にはしない。

---

## 11. 今後の運用

各案件では、Done定義シート上で本5区分を確認し、案件に対して**対象 / 対象外**を判断する。

対象外とする場合も削除せず、判断理由を記載する。

```text
ビジネス価値 区分2
↓
案件に対して対象 / 対象外を判断
↓
判断理由を記録
↓
対象となったDone定義から観点を作成
↓
評価項目・評価基準・合格基準を作成
```

また、実案件を複数運用する中で、特定の下位観点が案件横断で繰り返し利用されることが確認できた場合は、標準候補カタログへの昇格を検討する。

---

## 12. 参考文献

- ISO/IEC 25010:2023, Systems and software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — Product quality model  
  https://www.iso.org/standard/78176.html

- ISO/IEC 25059:2023, Software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — Quality model for AI systems  
  https://www.iso.org/obp/ui#iso:std:iso-iec:25059:ed-1:v1:en

- Amazon Bedrock, Use metrics to understand model performance  
  https://docs.aws.amazon.com/bedrock/latest/userguide/model-evaluation-metrics.html

- Microsoft Foundry, Evaluate generative AI applications  
  https://learn.microsoft.com/en-us/azure/ai-studio/how-to/evaluate-generative-ai-app

- Google Cloud, View and interpret evaluation results  
  https://cloud.google.com/vertex-ai/generative-ai/docs/models/eval-python-sdk/view-evaluation

- DeepEval, Introduction to LLM Evaluation Metrics  
  https://deepeval.com/docs/metrics-introduction

- DeepEval, Task Completion  
  https://deepeval.com/docs/metrics-task-completion

- Ragas, Response Relevancy  
  https://docs.ragas.io/en/latest/concepts/metrics/available_metrics/answer_relevance/

- OpenAI, Evals API  
  https://platform.openai.com/docs/api-reference/evals

- OpenAI, Graders API  
  https://platform.openai.com/docs/api-reference/graders
