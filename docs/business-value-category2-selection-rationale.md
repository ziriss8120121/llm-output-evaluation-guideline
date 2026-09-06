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

現時点では、ビジネス価値の区分2を以下の5区分とする。

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

---

## 3. 今回の評価スコープ

### 3.1 対象

LLM機能が生成した**出力内容そのものから、機能の目的・要件・利用価値を満たしているか確認できる品質**を評価対象とする。

例えば以下を対象とする。

- そのLLM機能で期待する目的を達成できる出力になっているか
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

判断基準は以下とする。

| 問い | 配置先 |
|---|---|
| 出力による損失・不利益・誤認・権利侵害等を防げているか | **事業リスク** |
| 出力が目的・要件・利用価値をどの程度満たしているか | **ビジネス価値** |
| 両方に見える評価軸 | 原則どちらか一方に寄せ、二重評価しない |

特に、一般的なLLM評価で頻出する **Correctness / Accuracy / Faithfulness / Groundedness** は、本来は出力品質として重要な指標である。

しかし、本ガイドラインでは、入力・参照情報・確認可能な事実との不整合、根拠にない生成、利用者の誤認につながる出力は、固定済みの事業リスク「**誤情報・誤誘導**」で評価する。

そのため、これらは重要性が低いから除外するのではなく、**二重評価を避けるためビジネス価値側から意図的に除外する**。

---

## 5. 参照した主要文献・フレームワーク

### 5.1 ISO/IEC 25010:2023

[ISO/IEC 25010:2023 - Product quality model](https://www.iso.org/standard/78176.html)

ICT・ソフトウェア製品の品質を、要求定義、テスト目的、品質管理基準、受入基準等に利用できる品質モデルとして整理している。

本検討では、LLM固有の評価軸だけに依存せず、ソフトウェア品質として「期待する機能・目的に適合しているか」という上位概念を確認するために参照した。

特に Functional Suitability に関係する考え方である、以下を候補整理の参考とした。

- Functional Completeness
- Functional Correctness
- Functional Appropriateness

### 5.2 ISO/IEC 25059:2023

[ISO/IEC 25059:2023 - Quality model for AI systems](https://www.iso.org/obp/ui#iso:std:iso-iec:25059:ed-1:v1:en)

AIシステム向けの品質モデルであり、Functional Correctness、Robustness、User Controllability等のAI固有・AIで重要度が増す品質概念を整理している。

本検討では、AI品質としての抜け漏れ確認に用いる。

### 5.3 Amazon Bedrock Model Evaluation

[Amazon Bedrock - Use metrics to understand model performance](https://docs.aws.amazon.com/bedrock/latest/userguide/model-evaluation-metrics.html)

LLM-as-a-Judgeによる組み込み評価指標として、以下を提供している。

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

本検討では、生成AIの出力品質に関する候補を広く抽出する主要な母集団として使用した。

### 5.4 Microsoft Foundry Evaluators

[Microsoft Foundry - Run evaluations](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/evaluate-generative-ai-app)

生成AI・AI Agent向けの品質評価として、以下を提供している。

- Customer Satisfaction
- Task Completion
- Coherence
- Groundedness
- Response Completeness
- Fluency
- Relevance

加えて、Violence、Self-harm、Hate/Unfairness等のSafety Evaluatorを品質評価とは分離している。

本検討では、Task CompletionやResponse Completeness等の価値側候補と、安全性リスク側との切り分け確認に利用した。

### 5.5 Google Vertex AI Evaluation

[Google Cloud - View and interpret evaluation results](https://cloud.google.com/vertex-ai/generative-ai/docs/models/eval-python-sdk/view-evaluation)

生成AI出力を評価する指標として、Coherence、Fluency、Instruction Following、Text Quality等を扱っている。

本検討では、指示遵守や表現品質を独立した品質軸として扱う妥当性の確認に利用した。

### 5.6 DeepEval

[DeepEval - Evaluation Metrics](https://deepeval.com/docs/metrics-introduction)

LLMアプリケーション向けに、以下のような評価指標を提供している。

- Task Completion
- Answer Relevancy
- Faithfulness
- Conversation Completeness
- Contextual Relevancy
- Contextual Recall
- Contextual Precision

DeepEvalでは、汎用指標を過剰に並べるより、少数の汎用指標とユースケース固有指標を組み合わせる考え方も示されている。

本検討では、区分2を過度に細分化せず、下位の「観点」で案件固有化する方針の参考とした。

### 5.7 Ragas

[Ragas - Response Relevancy](https://docs.ragas.io/en/latest/concepts/metrics/available_metrics/answer_relevance/)

RAG評価を中心に、Response Relevancy、Faithfulness、Answer Correctness等を扱う。

Response Relevancyでは、ユーザー入力に直接・適切に応答しているかを評価し、事実の正確性とは別に扱っている。

本検討では、「関連性」と「正確性」を別概念として整理する際の参考とした。

### 5.8 OpenAI Evals / Graders

[OpenAI Evals](https://platform.openai.com/docs/api-reference/evals)

[OpenAI Graders](https://platform.openai.com/docs/api-reference/graders)

OpenAI Evalsでは、すべてのユースケースに一律の品質分類を適用するのではなく、評価対象ごとに Testing Criteria を定義し、String Check、Text Similarity、Model Grader等を組み合わせて評価できる。

本検討では、共通区分を上位に定義しつつ、案件固有の「観点」「評価項目」「評価基準」に落とす設計の参考とした。

---

## 6. 外部フレームワークからの候補抽出

各フレームワークで登場する評価軸を、意味の近いものごとに正規化すると、主に以下の候補群となる。

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
| Task Completion | **採用 / 統合** | 「目的達成性」へ統合。LLM機能固有の目的を実現できたかを扱う |
| Helpfulness | **採用 / 統合** | 「目的達成性」へ統合。単独区分にすると完全性・関連性・指示遵守等を包含しすぎるため |
| Functional Appropriateness | **採用 / 統合** | 「目的達成性」の根拠として利用 |
| Completeness / Response Completeness | **採用** | 「完全性」。必要な情報・要素が不足していないかを扱う |
| Relevance / Answer Relevancy | **採用** | 「関連性」。要求と関係のない情報に逸れていないかを扱う |
| Following Instructions / Instruction Following | **採用** | 「指示遵守」。プロンプトで指定した条件・形式・制約等を扱う |
| Logical Coherence / Coherence | **採用 / 統合** | 「表現品質」の下位観点として扱う |
| Fluency | **採用 / 統合** | 「表現品質」の下位観点として扱う |
| Professional Style and Tone | **採用 / 統合** | 「表現品質」の下位観点として扱う |
| Conciseness / Verbosity | **採用 / 統合** | 「表現品質」の下位観点として扱う |
| Correctness / Accuracy | **ビジネス価値から除外** | 固定済み事業リスク「誤情報・誤誘導」で評価するため。二重評価防止 |
| Functional Correctness | **ビジネス価値から除外** | 上記と同様。一般的には重要な品質軸だが、本ガイドラインでは事業リスク側へ寄せる |
| Faithfulness | **ビジネス価値から除外** | 入力・参照情報にない内容の生成は「誤情報・誤誘導」で評価するため |
| Groundedness | **ビジネス価値から除外** | 根拠との整合は「誤情報・誤誘導」で評価するため |
| Harmfulness / Violence / Self-harm等 | **除外** | 固定済み事業リスク「有害・危険コンテンツ」で評価 |
| Stereotyping / Hate / Unfairness | **除外** | 固定済み事業リスク「公平性・差別」で評価 |
| Privacy関連 | **除外** | 固定済み事業リスク「プライバシー・機密情報」で評価 |
| Customer Satisfaction | **スコープ外** | 実ユーザーの反応を必要とするため、本番・利用評価として扱う |
| Robustness | **スコープ外 / 別軸** | 1件の出力品質ではなく、条件変化に対して品質を維持できるかというテスト設計・モデル品質の論点 |
| Context Precision / Context Recall | **スコープ外** | Retriever・RAG検索品質の評価。最終出力評価とは分離する |
| Refusal | **独立区分にはしない** | 必要な回答を不当に拒否した場合は「目的達成性」または「指示遵守」で評価可能。安全上の適切な拒否は事業リスク側の設計と合わせて判断する |
| Efficiency / Cost / Latency | **スコープ外** | 本検討では効率性を対象外としている |

---

## 8. 最終区分2の選定理由

### 8.1 目的達成性

**そのLLM機能で期待する目的・価値を実現できる出力になっているか**を扱う。

外部フレームワークの以下を主に統合する。

- Task Completion
- Helpfulness
- Functional Appropriateness

この区分を設ける理由は、完全性・関連性・表現品質等だけでは、**その機能を何のために作ったのか**というユースケース固有の価値が評価から抜けるためである。

例：

- スカウト文面生成：候補者に求人の魅力を伝え、興味喚起につながる文面になっているか
- 求人要約：ユーザーが求人の要点を短時間で把握できるか
- 検索条件生成：ユーザーの検索意図を検索条件として表現できているか

### 8.2 完全性

**目的・要求を満たすために必要な情報・要素が不足なく含まれているか**を扱う。

主な根拠は以下である。

- Completeness
- Response Completeness
- Functional Completeness

「誤った情報があるか」は事業リスク「誤情報・誤誘導」で扱い、本区分では**必要なものが欠けていないか**に限定する。

### 8.3 関連性

**目的・要求に関係する内容に適切に焦点を当てた出力になっているか**を扱う。

主な根拠は以下である。

- Relevance
- Answer Relevancy
- Response Relevancy

完全性との境界は以下とする。

```text
完全性
→ 必要なものが足りているか

関連性
→ 不要なものに逸れていないか
```

### 8.4 指示遵守

**プロンプトで明示された条件・形式・制約等を守った出力になっているか**を扱う。

主な根拠は以下である。

- Following Instructions
- Instruction Following

例：

- 300文字以内
- 敬体で記載する
- 指定フォーマットで出力する
- 箇条書きを使用しない
- 特定の要素を含める / 含めない

内容自体が有用でも、業務要件として指定した制約を守れていなければ、期待する価値を満たしていないため独立区分とする。

### 8.5 表現品質

**成果物として、用途に応じた論理性・自然さ・読みやすさ・簡潔さ・スタイル・トーン等を備えた出力になっているか**を扱う。

外部フレームワークでは以下のように細分化されている。

- Logical Coherence / Coherence
- Fluency
- Professional Style and Tone
- Text Quality
- Conciseness / Verbosity

これらを区分2として個別に並べると粒度が細かくなりすぎるため、区分2では「表現品質」に統合し、案件ごとの「観点シート」で必要なものを選択・具体化する。

---

## 9. 区分2同士の境界

区分2同士の重複を抑えるため、以下の問いで切り分ける。

| 区分2 | 主な問い |
|---|---|
| **目的達成性** | そもそも、このLLM機能でやりたいことを実現できる出力か |
| **完全性** | 必要な情報・要素が不足していないか |
| **関連性** | 目的・要求に関係する内容に焦点を当てているか |
| **指示遵守** | 明示された条件・形式・制約を守っているか |
| **表現品質** | 成果物として論理的・自然・読みやすく、用途に適した表現か |

なお、区分2は評価項目そのものではない。

案件では、対象と判断したDone定義から「観点」を作成し、その後「評価項目」「評価基準」「合格基準」へ展開する。

```text
Done定義
  ↓
観点
  ↓
評価項目
  ↓
評価基準
  ↓
合格基準
```

---

## 10. 採用しなかった評価軸に関する重要な補足

### 10.1 Correctnessを採用しない理由

Correctness / Accuracyは、ISO、AWS、Ragas等で広く利用される重要な品質軸である。

しかし、本ガイドラインでは、例えば以下をすでに事業リスク「誤情報・誤誘導」で評価する。

- 求人票に存在しない年収を事実として生成する
- 入力情報と異なる勤務地を生成する
- 参照情報と矛盾する内容を生成する
- 利用者が誤認するような内容を生成する

これをビジネス価値の「正確性」として再度評価すると、同一出力に対する評価が二重化する。

そのため、**一般的な品質論として正確性を否定するものではなく、本ガイドライン固有の役割分担として事業リスク側に寄せる**。

### 10.2 Faithfulness / Groundednessを採用しない理由

Faithfulness / GroundednessもRAGや参照情報を利用するLLMでは重要である。

しかし、以下は事業リスク「誤情報・誤誘導」で扱える。

- 入力・参照情報に存在しない内容を生成する
- 参照情報と矛盾する主張を生成する
- 根拠がない内容を事実として提示する

よって、二重評価防止のためビジネス価値側の区分2には置かない。

### 10.3 Helpfulnessを独立区分にしない理由

Helpfulnessは「役に立つか」という意味で重要だが、AWSの定義でも指示遵守、一貫性、暗黙的なニーズへの対応等、複数の要素を包含する。

そのため、「完全性」「関連性」「指示遵守」「表現品質」と横並びにすると包含関係が大きくなる。

本ガイドラインでは、Helpfulnessの上位的な意味を「目的達成性」に統合し、具体的な品質要因は他区分・下位観点で扱う。

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

- Microsoft Foundry, Run evaluations from the Microsoft Foundry portal  
  https://learn.microsoft.com/en-us/azure/ai-studio/how-to/evaluate-generative-ai-app

- Google Cloud, View and interpret evaluation results  
  https://cloud.google.com/vertex-ai/generative-ai/docs/models/eval-python-sdk/view-evaluation

- DeepEval, Introduction to LLM Evaluation Metrics  
  https://deepeval.com/docs/metrics-introduction

- DeepEval, Answer Relevancy  
  https://deepeval.com/docs/metrics-answer-relevancy

- Ragas, Response Relevancy  
  https://docs.ragas.io/en/latest/concepts/metrics/available_metrics/answer_relevance/

- OpenAI, Evals API  
  https://platform.openai.com/docs/api-reference/evals

- OpenAI, Graders API  
  https://platform.openai.com/docs/api-reference/graders
