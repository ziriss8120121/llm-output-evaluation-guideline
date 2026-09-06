# ビジネス価値：区分2の網羅性（MECE）検討

## 1. 目的

本ドキュメントでは、区分1「ビジネス価値」配下に設定した以下の5つの区分2について、**プロンプトUTにおけるLLM出力評価として十分な網羅性があるか、かつ区分間の重複を抑えられているか**を検証する。

1. **目的達成性**
2. **完全性**
3. **関連性**
4. **指示遵守**
5. **表現品質**

本検討でいうMECEは、数学的に完全な排他・網羅を保証するものではなく、以下を目指す。

- 主要なLLM出力品質の評価軸に大きな抜けがないこと
- 同一の問題を複数区分で二重評価しにくいこと
- 固定済みの区分1「事業リスク」と重複しないこと
- プロンプトUTで評価できない品質を無理に区分2へ含めないこと
- 案件固有の詳細を下位の「観点」で展開できる粒度であること

---

## 2. 結論

現時点では、ビジネス価値の区分2は以下の5区分を**維持する**。

| 区分2 | 判定 | MECE検討後の役割 |
|---|---|---|
| **目的達成性** | 維持 | 他の共通品質区分では捉えきれない、LLM機能固有の目的・期待価値への適合を扱う |
| **完全性** | 維持 | 必要な内容・情報・論点が不足していないかを扱う |
| **関連性** | 維持 | 目的・要求と無関係な内容へ逸れていないかを扱う |
| **指示遵守** | 維持 | プロンプトで明示された形式・条件・制約等への適合を扱う |
| **表現品質** | 維持 | 論理性、自然さ、読みやすさ、簡潔さ、用途に応じたスタイル・トーン等を扱う |

外部フレームワークの主要な出力品質軸は、上記5区分、固定済みの「事業リスク」、または本ガイドラインのスコープ外領域のいずれかへ概ね配置できる。

一方、区分名だけでは重複し得るため、**区分間の境界ルールを標準化することが必要**である。

---

## 3. 検証方法

以下の4観点でMECE性を検証する。

### 3.1 外部評価軸へのカバレッジ

主要なソフトウェア品質モデル、生成AI評価サービス、LLM評価フレームワークで扱われる評価軸を洗い出し、現在の5区分等へ配置できるか確認する。

### 3.2 区分2同士の重複

同一の具体例が複数の区分2へ該当しないかを確認し、重複する場合は境界ルールを定める。

### 3.3 事業リスクとの重複

固定済みの事業リスク5区分と同一内容を二重に評価しないことを確認する。

固定済みの事業リスクは以下とする。

1. **公平性・差別**
2. **有害・危険コンテンツ**
3. **誤情報・誤誘導**
4. **プライバシー・機密情報**
5. **権利侵害**

### 3.4 プロンプトUTのスコープ適合

本番KPI、業務効率、システム品質、Retriever性能等、最終的なLLM出力内容だけでは評価できないものを区分2へ混在させていないか確認する。

---

## 4. 参照した主要文献・フレームワーク

本検討では、区分2の選定根拠と同様に、以下を主要な参照元とする。

### 4.1 ISO/IEC 25010:2023

[ISO/IEC 25010:2023 - Product quality model](https://www.iso.org/standard/78176.html)

ソフトウェア・ICT製品の品質モデル。特に Functional Suitability 配下の以下を確認する。

- Functional Completeness
- Functional Correctness
- Functional Appropriateness

「必要なものを網羅していること」「正しい結果を提供すること」「目的達成に適していること」を別概念として扱っている点を、区分境界の参考とする。

### 4.2 ISO/IEC 25059:2023

[ISO/IEC 25059:2023 - Quality model for AI systems](https://www.iso.org/obp/ui#iso:std:iso-iec:25059:ed-1:v1:en)

AIシステム向け品質モデルとして、Functional Correctness、Robustness等を含む。プロンプトUTの出力評価に直接置くものと、システム・モデル品質として別に扱うものを切り分けるために参照する。

### 4.3 Amazon Bedrock Model Evaluation

[Amazon Bedrock - Use metrics to understand model performance](https://docs.aws.amazon.com/bedrock/latest/userguide/model-evaluation-metrics.html)

主な評価軸として以下を確認する。

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

### 4.4 Microsoft Foundry Evaluators

[Microsoft Foundry - Evaluate generative AI applications](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/evaluate-generative-ai-app)

主な品質評価として以下を確認する。

- Customer Satisfaction
- Task Completion
- Coherence
- Groundedness
- Response Completeness
- Fluency
- Relevance

Safety Evaluatorが品質評価とは別に扱われていることも、事業リスクとの分離の参考とする。

### 4.5 Google Vertex AI Evaluation

[Google Cloud - View and interpret evaluation results](https://cloud.google.com/vertex-ai/generative-ai/docs/models/eval-python-sdk/view-evaluation)

Coherence、Fluency、Instruction Following、Text Quality等を確認する。

### 4.6 DeepEval

[DeepEval - Evaluation Metrics](https://deepeval.com/docs/metrics-introduction)

Answer Relevancy、Faithfulness、Conversation Completeness、Contextual Relevancy等を確認する。

また、Task Completionは「タスク達成」という品質概念のクロスチェックとして参照する。ただし、DeepEvalのTask Completion実装はAgentのtrace / trajectoryを前提とする評価であり、本ガイドラインの単一プロンプトUTへ直接適用するものではない。

### 4.7 Ragas

[Ragas - Response Relevancy](https://docs.ragas.io/en/latest/concepts/metrics/available_metrics/answer_relevance/)

Response Relevancy、Faithfulness、Answer Correctness等を確認し、「関連性」と「正確性」が別概念として扱われることを参考とする。

### 4.8 OpenAI Evals / Graders

[OpenAI Evals](https://platform.openai.com/docs/api-reference/evals)

[OpenAI Graders](https://platform.openai.com/docs/api-reference/graders)

固定された一律の品質分類だけでなく、ユースケースごとにTesting CriteriaやRubricを設計する考え方を、上位の共通区分から案件固有の観点・項目へ展開する設計の参考とする。

---

## 5. 外部評価軸のカバレッジ確認

主要な外部評価概念を、現在の評価体系へマッピングする。

| 外部評価概念 | 今回の配置 | 判定理由 |
|---|---|---|
| Task Completion | **目的達成性** | LLM機能固有の目的・タスクを達成できるかを扱う |
| Functional Appropriateness | **目的達成性** | タスク・目的達成への適合を扱う |
| Helpfulness | **目的達成性を中心に分解** | 包含範囲が広いため独立区分にせず、固有価値は目的達成性、一般品質は他区分へ分解する |
| Completeness | **完全性** | 必要な内容が不足していないかを扱う |
| Response Completeness | **完全性** | 上記と同様 |
| Functional Completeness | **完全性** | 上記と同様 |
| Relevance / Answer Relevancy / Response Relevancy | **関連性** | 目的・要求と関係する内容へ焦点を当てているかを扱う |
| Following Instructions / Instruction Following | **指示遵守** | 明示された条件・形式・制約等への適合を扱う |
| Logical Coherence / Coherence | **表現品質** | 論理展開・文章構造の品質として扱う |
| Fluency | **表現品質** | 自然さ・読みやすさとして扱う |
| Professional Style and Tone / Text Quality | **表現品質** | 用途に適した文体・トーンとして扱う |
| Conciseness / Verbosity | **表現品質** | 表現上の簡潔性・冗長性として扱う |
| Correctness / Accuracy / Functional Correctness | **事業リスク：誤情報・誤誘導** | 入力・参照情報・事実との不整合を二重評価しないため |
| Faithfulness / Groundedness | **事業リスク：誤情報・誤誘導** | 根拠にない生成・参照情報との矛盾を二重評価しないため |
| Harmfulness / Violence / Self-harm等 | **事業リスク：有害・危険コンテンツ** | 固定済み事業リスクで扱う |
| Stereotyping / Hate / Unfairness | **事業リスク：公平性・差別** | 固定済み事業リスクで扱う |
| Privacy関連 | **事業リスク：プライバシー・機密情報** | 固定済み事業リスクで扱う |
| Intellectual Property関連 | **事業リスク：権利侵害** | 固定済み事業リスクで扱う |
| Refusal | **独立区分にしない** | 不当な拒否は目的達成性または指示遵守、安全上必要な拒否は事業リスク側との関係で判断する |
| Customer Satisfaction | **独立区分にしない** | 利用者が感じる総合成果を捉えるholisticな評価であり、目的達成性・関連性・表現品質等と包含関係が大きい。MicrosoftのEvaluatorは実ユーザーアンケートを必須とせず、会話・応答から満足度を推定するが、共通区分2としては粒度が上位すぎる |
| Robustness | **スコープ外 / 別軸** | 条件変化に対して品質を維持できるかというテスト設計・モデル品質の論点 |
| Context Precision / Context Recall | **スコープ外** | Retriever / RAG検索品質の論点 |
| Cost / Latency / Efficiency | **スコープ外** | 本検討では効率性を対象外としている |
| 本番KPI（CVR、返信率、売上等） | **スコープ外** | プロンプトUTだけでは直接評価できない事業成果 |

### 5.1 網羅性に関する判定

上表のとおり、主要なLLM出力品質の評価概念は、以下のいずれかに配置できる。

```text
ビジネス価値 5区分
または
固定済み事業リスク 5区分
または
プロンプトUTのスコープ外
```

現時点では、**プロンプトUTで評価すべき主要なビジネス価値の評価軸で、配置先が存在しない重要な概念は確認できない**。

そのため、網羅性の観点から区分2を追加する必要はないと判断する。

---

## 6. 区分2同士の重複検討

網羅性に大きな問題はない一方、現5区分は定義を広く取りすぎると重複する。

特に以下の組み合わせについて境界を定める。

1. 目的達成性 × 他4区分
2. 完全性 × 指示遵守
3. 関連性 × 表現品質
4. 指示遵守 × 表現品質

---

## 7. 目的達成性と他4区分の境界

### 7.1 重複する理由

「目的を達成できるか」を広義に捉えると、以下すべてが目的達成に影響する。

- 必要な情報が欠けている
- 関係のない内容が多い
- 明示された指示を守っていない
- 読みにくい・不自然である

そのため、目的達成性を総合品質として定義すると、他4区分を包含してしまう。

### 7.2 境界ルール

目的達成性は、以下に限定する。

> **他の共通品質区分では捉えきれない、そのLLM機能固有の目的・期待価値に適合した出力になっているか**

例：スカウト文面自動生成

- 候補者に合わせた訴求になっているか
- 求人の魅力が伝わる内容になっているか
- 候補者の興味喚起につながる文面になっているか

例：求人要約

- 原文を読まなくても、求人の特徴を短時間で把握できる要約になっているか

### 7.3 判断

**目的達成性は維持するが、ユースケース固有価値を扱う区分として定義を狭める。**

これにより、完全性・関連性・指示遵守・表現品質との重複を抑える。

---

## 8. 完全性と指示遵守の境界

### 8.1 重複する例

プロンプトに「求人の魅力を3つ含める」と書かれているにもかかわらず、2つしか出力されなかった場合、以下の両方に見える。

- 必要な内容が足りない → 完全性
- 指示を守っていない → 指示遵守

### 8.2 境界ルール

| 区分2 | 扱う対象 |
|---|---|
| **完全性** | 出力として必要な**内容・情報・論点**が揃っているか |
| **指示遵守** | 明示された**形式・条件・制約・出力方法等のルール**を守っているか |

例：

| ケース | 配置先 |
|---|---|
| 求人の重要な魅力が欠けている | **完全性** |
| 必要な論点が抜けている | **完全性** |
| 300文字以内の指定を超えている | **指示遵守** |
| JSON指定なのに自然文で返す | **指示遵守** |
| 箇条書き禁止なのに箇条書きで出力する | **指示遵守** |

内容の「有無」と形式・制約の「遵守」を分けることで、二重評価を抑える。

---

## 9. 関連性と表現品質の境界

### 9.1 重複する例

長く冗長な出力は、以下の両方に見える場合がある。

- 不要な内容が多い → 関連性
- 冗長で読みにくい → 表現品質

### 9.2 境界ルール

| 区分2 | 判断軸 |
|---|---|
| **関連性** | **何を述べているか**。目的・要求と無関係な内容が含まれていないか |
| **表現品質** | **どう述べているか**。関連する内容を、論理的・自然・簡潔・読みやすく表現できているか |

例：

| ケース | 配置先 |
|---|---|
| スカウト文面で、候補者・求人と無関係な会社沿革を長く説明する | **関連性** |
| 求人の魅力自体は関連しているが、同じ意味を何度も繰り返す | **表現品質** |
| 話題が要求から逸れている | **関連性** |
| 内容は要求に沿っているが、文章構造が不自然で読みにくい | **表現品質** |

---

## 10. 指示遵守と表現品質の境界

### 10.1 重複する例

プロンプトで「丁寧な文章」と指定したにもかかわらず、乱暴な文体が出力された場合、指示遵守と表現品質の両方に見える。

### 10.2 境界ルール

> **明示された条件・制約への違反は「指示遵守」を優先し、明示されていないが用途上不適切な表現は「表現品質」で扱う。**

例：

| ケース | 配置先 |
|---|---|
| 「敬体で」と明示されているが常体で出力 | **指示遵守** |
| トーン指定はないが、顧客向け文章として不適切に乱暴 | **表現品質** |
| 「300文字以内」と明示されているが500文字 | **指示遵守** |
| 文字数指定はないが、不必要に冗長で読みづらい | **表現品質** |

---

## 11. 事業リスクとの重複検討

ビジネス価値側では、固定済みの事業リスクと同一内容を評価しない。

### 11.1 正確性・根拠整合性

外部フレームワークでは以下が重要な品質軸として扱われる。

- Correctness
- Accuracy
- Functional Correctness
- Faithfulness
- Groundedness

しかし、本ガイドラインでは以下を事業リスク「誤情報・誤誘導」で評価する。

- 入力・参照情報と異なる内容を生成する
- 根拠にない内容を事実として生成する
- 確認可能な事実に反する内容を生成する
- 利用者の誤認につながる内容を生成する

したがって、ビジネス価値側に「正確性」「忠実性」等の区分を追加しない。

### 11.2 安全性・公平性・プライバシー・権利

以下もビジネス価値側には追加しない。

| 評価概念 | 配置先 |
|---|---|
| Harmfulness / Dangerous Content | **事業リスク：有害・危険コンテンツ** |
| Bias / Stereotyping / Unfairness | **事業リスク：公平性・差別** |
| Privacy / Confidentiality | **事業リスク：プライバシー・機密情報** |
| Intellectual Property / Rights | **事業リスク：権利侵害** |

### 11.3 判断

**事業リスク側は現段階の5区分で固定し、ビジネス価値側から重複候補を除外する方針を維持する。**

---

## 12. 最終的な区分境界

MECE検討後の区分2は、以下の問いで切り分ける。

| 区分2 | 主な問い | 主に扱わないもの |
|---|---|---|
| **目的達成性** | 他の共通品質では捉えきれない、機能固有の目的・期待価値を実現できているか | 完全性・関連性・指示遵守・表現品質で評価できる一般品質 |
| **完全性** | 必要な内容・情報・論点が不足していないか | 情報の正誤、形式・文字数等の制約 |
| **関連性** | 目的・要求と関係する内容に焦点を当てているか | 文章そのものの読みやすさ・自然さ |
| **指示遵守** | 明示された形式・条件・制約等を守っているか | 明示されていない暗黙的な文章品質 |
| **表現品質** | 関連する内容を論理的・自然・簡潔・用途に適した形で表現できているか | 明示的な指示違反、内容の誤り |

---

## 13. 二重評価を防ぐ共通ルール

案件で観点・評価項目を作成する際は、以下を共通ルールとする。

> **同一の問題を複数区分で二重評価しない。複数区分に該当し得る場合は、その問題を最も直接的・具体的に表す区分へ寄せる。**

優先的な判断例は以下とする。

```text
事実・根拠との不整合
→ 事業リスク「誤情報・誤誘導」

必要な内容の欠落
→ 完全性

無関係な内容への逸脱
→ 関連性

明示された条件への違反
→ 指示遵守

内容は適切だが表現が不自然・冗長
→ 表現品質

上記では捉えきれないユースケース固有価値の未達
→ 目的達成性
```

---

## 14. 選定根拠ドキュメントへの影響

MECE検討の結果、**区分2の追加・削除・名称変更は不要**と判断する。

一方、選定根拠については以下を精緻化する必要がある。

1. 「目的達成性」を、他の共通品質では捉えきれない**ユースケース固有価値**に限定する
2. 「完全性」と「指示遵守」の境界を明文化する
3. 「関連性」と「表現品質」の境界を明文化する
4. 「指示遵守」と「表現品質」の優先ルールを明文化する
5. Customer Satisfactionの除外理由を、「実ユーザーの反応が必須」ではなく、**holisticで包含範囲が広く、共通区分2として上位すぎるため**と修正する
6. DeepEvalのTask Completionについて、**Agent traceを前提とする実装を直接採用するのではなく、タスク達成概念のクロスチェックとして参照した**ことを明確にする

上記は区分選定の結論を変えるものではなく、**区分間の重複を減らし、運用時の判断再現性を高めるための精緻化**である。

---

## 15. 今後の運用

各案件では、Done定義シートで区分2ごとに対象 / 対象外を判断し、対象となったDone定義から案件固有の観点を作成する。

観点を作成する際には、本ドキュメントの境界ルールを参照し、同一内容を複数区分へ重複登録しない。

```text
Done定義
  ↓
対象 / 対象外・判断理由
  ↓
観点
  ↓
評価項目
  ↓
5段階の評価基準
  ↓
合格基準
```

さらに、すべての観点・評価項目について、`Done定義No → 観点No → 項目No` で前工程まで突合可能な状態とする。

---

## 16. 参考文献

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
