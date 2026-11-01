# AI BDD Skills

在 AI BDD 課程學習與實作過程中，持續整理、調整與延伸的個人 agent skills 專案。

本專案以學習[水球軟體學院](https://github.com/Waterball-Software-Academy) AI BDD 課程為起點，結合課程練習、個人理解與使用需求，探索如何將軟體工程方法應用於 AI 輔助開發。

我希望透過需求澄清、可驗證規格、設計契約與階段性品質檢查，降低開發過程中的需求歧義、交接落差與過早完成判定，讓 AI 輔助軟體開發更加穩健、可檢查且可追溯。

> **開發中，尚未完成。** 現有 skills、規則與模板會隨學習及實際使用持續調整。目前主要涵蓋需求到系統分析階段；與正式實作、自動化驗收的串接，以及對開發品質的改善效果，仍待進一步建立與驗證。

## 目前的開發流程

目前的流程輪廓是：需求輸入 → 規格撰寫與必要澄清 → 技術研究 → 系統分析 → API／資料／UI 分析 → 跨產物一致性檢查。

各 skill 也可依工作範圍獨立使用。實際順序由必要輸入與依賴決定，後續發現的問題需要回到受影響的來源與產物修正。

| Skill | 工作內容 | 主要交付 |
| --- | --- | --- |
| [specify](.agents/skills/specify/SKILL.md) | 將功能描述整理成可驗證的需求，檢查完整性與規劃就緒程度。 | `spec.md`、規格品質清單 |
| [clarify](.agents/skills/clarify/SKILL.md) | 釐清會影響範圍、角色權限、流程、規則或驗收的缺口，保存已確認答案。 | 更新後的規格或澄清結果 |
| [technical-research](.agents/skills/technical-research/SKILL.md) | 查核官方來源，記錄技術選擇、替代方案、限制與未定事項。 | `research.md`、`techstack.md` |
| [system-analysis](.agents/skills/system-analysis/SKILL.md) | 整理系統輪廓，依責任與依賴安排領域分析，核對交付的一致性。 | `plan.md` 與分析產物索引 |
| [api-plan](.agents/skills/api-plan/SKILL.md) | 分析服務操作、身分、資料交換、事件與失敗恢復契約。 | OpenAPI YAML |
| [data-plan](.agents/skills/data-plan/SKILL.md) | 分析資料模型、讀寫、交易、授權與生命週期。 | DBML 與必要 Notes |
| [ui-ux-plan](.agents/skills/ui-ux-plan/SKILL.md) | 將操作、角色可見性、非同步回饋與恢復行為製作成可操作原型。 | 單檔 HTML、簡短 UI／UX 紀錄 |

目前對 BDD 的練習聚焦在需求探索與行為範例的規格化；將範例接上自動化測試，並持續對照實際軟體行為，是後續要補齊的方向。

## 我的學習與延伸方向

在課程練習與個人調整中，我持續關注以下問題：

- 需求的可驗證性：將使用者目標、功能需求、驗收條件與品質限制建立關聯，保留假設與決策來源。
- 分析工作的交接：明確定義責任、必要輸入、唯一寫入位置與下游需要的結果，核對 API、資料模型與 UI 對同一件事的理解。
- 失敗與不確定性的處理：在分析時考慮拒絕、重試、結果未知與恢復，也記錄缺少決策時受影響的範圍。
- Skills 本身的維護：將主流程、規則、模板與腳本分開，按需讀取；從實際產物與使用問題回頭調整指引。
- 完成狀態的判定：區分文件檢查、假資料原型操作與真實服務驗證，讓完成狀態對應實際證據。

目前的實作方式可參考[領域分析共同交接契約](.agents/skills/system-analysis/references/interface-analysis-contract.md)與[規格品質驗證規則](.agents/skills/specify/rules/specification-quality-validation.md)。這些設計仍需要透過更多案例檢驗與修正。

## 建立與維護 skills 的工具

除了軟體開發流程，repo 也包含整理 skills 本身的工作方法：

| 類型 | Skills | 用途 |
| --- | --- | --- |
| 從產物反推流程 | [artifact-to-skill-engineering](.agents/skills/artifact-to-skill-engineering/SKILL.md) | 先收斂目標範例，再抽取模板，推導生成同類產物所需的 skill。 |
| 建立與優化 | [skill-engineering](.agents/skills/skill-engineering/SKILL.md) | 整理問題與根因、規劃修改範圍，編排模組與驗證方式。 |
| 檔案撰寫 | [skill-form-sop](.agents/skills/skill-form-sop/SKILL.md)、[skill-form-rule](.agents/skills/skill-form-rule/SKILL.md)、[skill-form-template](.agents/skills/skill-form-template/SKILL.md)、[skill-form-script](.agents/skills/skill-form-script/SKILL.md) | 撰寫主流程、規則、模板與 Python 腳本。 |
| 從步驟展開資源 | [skill-derive-rule](.agents/skills/skill-derive-rule/SKILL.md)、[skill-derive-template](.agents/skills/skill-derive-template/SKILL.md)、[skill-derive-script](.agents/skills/skill-derive-script/SKILL.md) | 從既有流程步驟展開必要資源，並回掛到主流程。 |

Skill 目錄依實際需要包含下列內容：

```text
.agents/skills/<skill-name>/
├── SKILL.md       # 使用時機與主要流程
├── rules/         # 判斷與品質要求
├── templates/     # 產物骨架與對應範例
├── references/    # 參考資料
└── scripts/       # 可重複執行的處理工具
```

## 如何開始嘗試

1. 在可讀取本機專案檔案的 AI coding agent 中開啟這份 repo。
2. 選擇一個 skill，讓 agent 讀取它的 `SKILL.md`，並依流程讀取所需規則、模板與參考資料。
3. 提供具體需求、既有決策與輸出位置，檢查實際產物、未決事項及驗證紀錄。

例如，可以從規格撰寫開始：

```text
請依照 .agents/skills/specify/SKILL.md，協助整理以下需求：

我想做一個團隊共用的待辦清單。
成員可以建立任務、指派負責人、設定到期日及更新完成狀態。

請將規格寫入 specs/my-feature/spec.md。
會影響規則或驗收的關鍵缺口，請先依流程澄清；
並保留已確認決策、假設及待確認事項，交付品質檢查結果。
```

不同 agent 的 skills 載入與呼叫方式仍待整理及驗證，以上是以明確檔案路徑指定工作流程的示例。`docs/` 與 `specs/` 目前列入 `.gitignore`；若需公開新產物，需另行安排版本控制。

## 開發狀態與下一步

目前已整理需求、研究、分析與 skills 維護流程，並提供對應規則、模板與參考資源。接下來希望逐步完成：

- 整理課程練習、參考來源與個人調整的對照，補充具體修改理由。
- 持續檢查 skills、模板與引用連結的一致性。
- 建立可重複使用的評估案例，記錄需求缺漏、產物矛盾及人工修正情形。
- 補充不同 agent 的使用方式與實際驗證環境。
- 探索從行為規格、自動化測試到實作的串接方式。
- 累積失敗案例與修正紀錄，再依證據調整流程。

目前尚未建立系統性的比較評估，無法據此宣稱已提升開發效率或降低缺陷率。文件與原型的檢查，仍需與正式系統的服務、授權、併發及部署驗證分開看待。

## 來源與致謝

- [水球軟體學院](https://github.com/Waterball-Software-Academy)：本專案的 AI BDD 課程學習起點。Repo 內容包含跟隨課程的練習，以及依個人理解與使用需求進行的調整與延伸；此處呈現個人的學習與實驗成果。
- [GitHub Spec Kit](https://github.com/github/spec-kit)：本專案的 `clarify` 衍生自其 clarify 命令樣板，已在 [skill 說明](.agents/skills/clarify/SKILL.md)保留上游來源與版本，並附上[原始 MIT 授權與著作權聲明](.agents/skills/clarify/LICENSE)。

歡迎透過 Issues 分享使用情境、發現的問題，或對流程與驗證方式提出建議。
