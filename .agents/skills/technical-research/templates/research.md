<!-- 依定稿的 research.example.md 抽取；填寫後移除提示、替換所有填位符號。 -->

# Phase 0 技術研究：{{RESEARCH_SUBJECT}}

**研究日期**：{{RESEARCH_DATE}}（{{TIMEZONE}}；{{BASELINE_CONTEXT}}）
**輸入依據**：{{INPUT_REFERENCES_AND_CONFIRMED_CONSTRAINTS}}

<!-- 說明研究範圍、來源種類與證據限制；沿用歷史基線時交代其日期，沒有額外基線說明時刪除日期括號內的分號及對應槽位。 -->
{{RESEARCH_SCOPE_AND_EVIDENCE_LIMITS}}

<!-- 依實際研究問題重複下列完整區塊，研究 ID 依序填為 R01、R02 等；主題與數量依需求決定。 -->
## {{RESEARCH_ID}} — {{RESEARCH_TOPIC}}

**決策**： {{DECISION_AND_APPLICABLE_CONDITIONS}}

<!-- 僅在本研究項目涉及套件版本與相容性時保留版本表，按實際依賴增減資料列；其他項目刪除此表。 -->
| 依賴 | {{VERSION_CHECK_DATE}} 查核基線 | 用途 |
| --- | --- | --- |
| {{DEPENDENCY_NAME}} | {{VERIFIED_VERSION_OR_SUPPORT_RANGE}} | {{DEPENDENCY_PURPOSE}} |

**理由與來源**： {{RATIONALE_WITH_SOURCE_LINKS}}

<!-- 需要補充適用條件、跨項影響或相容性依據時保留，可按實際內容展開為段落；沒有補充時刪除此槽位。 -->
{{SUPPORTING_ANALYSIS_WITH_SOURCE_LINKS}}

**替代方案**： {{ALTERNATIVES_AND_REJECTION_REASONS}}

<!-- 研究項目區塊結束。 -->

## 研究結論與待驗證事項

{{RESEARCH_CONCLUSIONS_AND_REMAINING_LIMITS}}

<!-- 依實際待驗證事項增減條目，引用相關研究 ID 並說明要驗證的條件；沒有待驗證事項時填「無」。 -->
- **{{VALIDATION_TOPIC}}（{{RELATED_RESEARCH_IDS}}）**：{{VALIDATION_ACTION_AND_ACCEPTANCE_CONDITION}}
