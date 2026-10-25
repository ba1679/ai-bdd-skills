<!-- 骨架依使用者指定命名為 spec.template.md；對照成品為同目錄的 spec.example.md。 -->
<!-- 填寫提示：替換所有填位符號，依實際數量重複故事、需求與清單項目，完成後移除填寫提示。 -->

# 功能規格：{{FEATURE_NAME}}

**功能分支**：`{{FEATURE_BRANCH}}`（{{FEATURE_BRANCH_CONTEXT}}）

**建立日期**：{{CREATED_DATE}}

**狀態**：{{SPEC_STATUS}}（{{SPEC_STATUS_CONTEXT}}）

**需求輸入**：{{USER_INPUT}}

<!-- 按實際來源重複補充輸入欄位，可記錄規劃、設計或資料背景；沒有補充輸入時刪除此欄位。 -->
**補充輸入（{{SUPPLEMENT_CONTEXT}}；{{SUPPLEMENT_DATE}}）**：{{SUPPLEMENT_INPUT}}

## 需求澄清

<!-- 只記錄實際提問與已接受的答案；依澄清日期重複紀錄，最新日期在前。沒有澄清紀錄時填「無」。 -->

### 澄清紀錄 {{CLARIFICATION_DATE}}

- 問：{{CLARIFICATION_QUESTION}} → 答：{{ACCEPTED_CLARIFICATION_ANSWER}}

## 使用者故事與需求（必填）

每個使用者故事（US）分別列管驗收條件、功能需求（FR）與非功能需求（NFR）。驗收條件描述可觀察的通過情境；FR 描述應提供的行為與規則；NFR 描述品質、限制及驗證標準。

- FR 與 NFR 在主要 US 下定義一次，其他受影響的 US 以需求 ID 引用。
- 全域需求只收錄無法合理歸屬單一 US 的共同規則，逐條標明適用 US 及全域原因；同時適用多個 US 的需求仍優先保留主要歸屬。
- US、FR、NFR 與 SC 的 ID 在各自類型內唯一；需求移動歸屬時保留 ID，重新分類時保留原 ID 對照。關鍵實體與可量測成果維持獨立章節。

<!-- 按故事數量重複下列完整 US 區塊。US、FR、NFR、SC 的編號填入三位數字；FR 與 NFR 在整份規格內各自編號，跨 US 與全域章節皆不重複。 -->

### 使用者故事 {{US_NUMBER}} — {{US_TITLE}}（優先級：{{US_PRIORITY}}）

**故事 ID**：US-{{US_ID}}

{{USER_STORY_DESCRIPTION}}

**優先級理由**：{{PRIORITY_RATIONALE}}

**獨立驗證方式**：{{INDEPENDENT_TEST_DESCRIPTION}}

#### 驗收條件（Acceptance Criteria）

<!-- 按實際情境增減條目，涵蓋主要成功流程及影響此故事的失敗或邊界情境。 -->

1. **Given** {{AC_SUCCESS_PRECONDITION}}，**When** {{AC_SUCCESS_ACTION}}，**Then** {{AC_SUCCESS_OBSERVABLE_RESULT}}。
2. **Given** {{AC_BOUNDARY_PRECONDITION}}，**When** {{AC_BOUNDARY_ACTION}}，**Then** {{AC_BOUNDARY_OBSERVABLE_RESULT}}。

#### 功能需求（FR）

<!-- 每條 FR 描述一項可驗證的行為或規則，按需重複條目。引用欄位只列 ID、主要歸屬及引用原因；沒有適用項目時填「無」。 -->

- **FR-{{FR_ID}}**：{{REQUIREMENT_SUBJECT}} MUST {{REQUIRED_BEHAVIOR_AND_RULE}}。

<!-- 只有需求涉及角色作業權限時才保留此表；依實際角色增減欄位，依作業增減列。 -->

**角色與作業權限表**

| 作業 | {{PRIMARY_ROLE_NAME}} | {{SECONDARY_ROLE_NAME}} |
| --- | --- | --- |
| {{OPERATION_NAME}} | {{PRIMARY_ROLE_PERMISSION}} | {{SECONDARY_ROLE_PERMISSION}} |

**跨 US 引用**：FR-{{REFERENCED_FR_ID}}（US-{{REFERENCED_FR_OWNER_US_ID}}；{{FR_REFERENCE_REASON}}）。

**適用全域 FR**：FR-{{GLOBAL_FR_ID}}。

#### 非功能需求（NFR）

<!-- NFR 寫明適用條件、品質目標與驗證方式。驗收指標引用對應 SC；若已有完整驗證依據而無對應 SC，可刪除該引用欄位。無本故事獨立定義的 NFR 時填「無」，保留適用引用即可。 -->

- **NFR-{{NFR_ID}}**：在{{NFR_APPLICABLE_CONDITION}}下，{{QUALITY_SUBJECT}} MUST {{QUALITY_TARGET_OR_CONSTRAINT}}；驗證方式：{{QUALITY_VERIFICATION_METHOD}}。驗收指標：SC-{{SC_ID}}。

**跨 US 引用**：NFR-{{REFERENCED_NFR_ID}}（US-{{REFERENCED_NFR_OWNER_US_ID}}；{{NFR_REFERENCE_REASON}}）。

**適用全域 NFR**：NFR-{{GLOBAL_NFR_ID}}。

<!-- 本 US 區塊結束；重複時以分隔線區分各故事，並同步填寫新的 ID 與引用。 -->

---

## 邊界情境

<!-- 按情境重複條目，說明條件、預期處理及受影響的故事或需求。 -->

- {{EDGE_CASE_CONDITION}}：{{EXPECTED_EDGE_CASE_HANDLING}}；適用：{{EDGE_CASE_US_OR_REQUIREMENT_IDS}}。

## 全域需求

<!-- 只有存在無法合理歸屬單一 US 的共同規則時才保留本節；FR 或 NFR 分組無條目時填「無」。適用 US 可逐一列出並補充適用範圍。 -->

本節定義無法合理歸屬單一 US 的共同 FR 與 NFR；各 US 已定義的需求以其唯一 ID 引用。

### 功能需求（FR）

- **FR-{{GLOBAL_FR_ID}}**：{{GLOBAL_REQUIREMENT_SUBJECT}} MUST {{GLOBAL_REQUIRED_BEHAVIOR_AND_RULE}}。

  **適用 US**：{{GLOBAL_FR_APPLICABLE_US_IDS}}。**全域原因**：{{GLOBAL_FR_JUSTIFICATION}}。

### 非功能需求（NFR）

- **NFR-{{GLOBAL_NFR_ID}}**：在{{GLOBAL_NFR_APPLICABLE_CONDITION}}下，{{GLOBAL_QUALITY_SUBJECT}} MUST {{GLOBAL_QUALITY_TARGET_OR_CONSTRAINT}}；驗證方式：{{GLOBAL_QUALITY_VERIFICATION_METHOD}}。

  **適用 US**：{{GLOBAL_NFR_APPLICABLE_US_IDS}}。**全域原因**：{{GLOBAL_NFR_JUSTIFICATION}}。

<!-- 需求若由既有類型重新分類，在新定義下加上「來源對照：原需求 ID 與重新分類原因」；此規則適用於 US 內及全域需求。 -->

## 關鍵實體（涉及資料時保留）

<!-- 按實體數量重複條目，描述其業務意義、識別、主要屬性與關聯。 -->

- **{{ENTITY_NAME}}**：{{ENTITY_DESCRIPTION}}。

## 成功標準（必填）

### 可量測成果

<!-- 按成果數量重複條目，描述適用條件、通過門檻與量測方式；與 NFR 共用指標時以 SC ID 追溯同一標準。 -->

- **SC-{{SC_ID}}**：在{{SUCCESS_CRITERION_CONTEXT}}下，{{MEASURABLE_OUTCOME_AND_THRESHOLD}}；量測方式：{{OUTCOME_MEASUREMENT_METHOD}}。

## 假設

### 範圍與預設

- {{SCOPE_AND_DEFAULT_ASSUMPTION}}。
- 本次範圍不包含{{OUT_OF_SCOPE_CAPABILITY}}。

### 附檔核對與遷移

<!-- 涉及參考資料、既有結果核對或資料遷移時保留本節，否則刪除。 -->

- 參考來源與可採信範圍：{{REFERENCE_SOURCE_AND_INTERPRETATION}}。
- 既有差異與處理原則：{{LEGACY_DIFFERENCE_AND_MIGRATION_POLICY}}。

### 依賴

- {{DEPENDENCY_PROVIDER}}提供{{REQUIRED_DEPENDENCY}}；適用期間或確認依據：{{DEPENDENCY_VALIDITY_OR_CONFIRMATION_BASIS}}。
