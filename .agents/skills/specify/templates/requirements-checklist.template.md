<!-- 骨架依確認提案命名為 requirements-checklist.template.md；對照成品為 requirements-checklist.example.md。 -->
<!-- 填寫提示：替換填位符號；依實際數量重複檢查依據、問題及批次區塊，完成後移除填寫提示。 -->

# 規格品質清單：{{FEATURE_NAME}}

**用途**：檢查規格的完整性與可驗證性，供進入規劃前複核。

**建立日期**：{{CREATED_DATE}}

**規格**：[{{SPEC_FILE_NAME}}]({{SPEC_RELATIVE_PATH}})

## 內容品質

- [ ] 內容聚焦使用者價值與業務需要。
- [ ] 全文可由需求相關人理解，必要術語有清楚含義。
- [ ] 技術限制有指定來源，需求內容聚焦行為與成果。
- [ ] 必填章節已完成，選填章節依適用性處理。

## 需求完整性

- [ ] 沒有會改變範圍、權限、流程、規則或驗收的未解決決策。
- [ ] 各項需求明確且可判定通過或失敗。
- [ ] 每條 FR 的行為與適用條件均有可定位的驗收依據。
- [ ] SC 有依據、可量測，且可由需求成果驗證。
- [ ] 適用的 NFR 有條件、品質判準與驗證方式。
- [ ] 已識別適用的失敗與邊界情境，交代資料或狀態效果。
- [ ] 本次範圍與排除項清楚。
- [ ] 依賴、假設與需求來源可辨識。
- [ ] 需求 ID、主要歸屬與引用正確，沒有殘留填位或填寫提示。

## 規劃就緒

- [ ] 故事涵蓋主要流程，具備優先級理由與獨立驗證方式。
- [ ] 驗收、FR、NFR、實體、邊界與 SC 對相同條件的結果一致。
- [ ] 現有需求與驗收足以判定 SC 的成果。

## 檢查紀錄

<!-- 按批次重複，最新批次在前；各批次欄位記錄當時快照。 -->

### {{VALIDATION_BATCH_LABEL}}（{{CHECKED_AT}}）

**當次通過數**：{{PASSED_CHECK_COUNT}}/{{TOTAL_CHECK_COUNT}}。

**FR 驗收覆蓋**：{{COVERED_FR_COUNT}}/{{TOTAL_FR_COUNT}}。

**已用自動修正輪次**：{{AUTO_REPAIR_ROUNDS_USED}}。

**當次就緒判定**：{{READINESS_STATUS}}；{{READINESS_REASON}}。

**檢查依據與定位**

- {{CHECK_ITEM_OR_GROUP}}：{{SPEC_EVIDENCE_LOCATION_AND_RESULT}}。

**當次未通過與待確認事項**

- {{FAILED_CHECK_OR_PENDING_DECISION}}：{{SPEC_ISSUE_LOCATION}}；缺口：{{SPECIFIC_GAP}}；後續處理：{{NEXT_ACTION}}。

**當次狀態變化**

- 前次基準：{{PREVIOUS_VALIDATION_BASELINE}}。
- 新增通過：{{NEWLY_PASSING_ITEMS}}。
- 退步：{{REGRESSED_ITEMS}}。
- 仍未通過：{{STILL_FAILING_ITEMS}}。
