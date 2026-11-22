# Tasks：{{PROJECT_NAME}} — {{FEATURE_NAME}}

**任務用途**：{{TASK_DOCUMENT_PURPOSE_AND_PATH_BASE}}

**功能範圍**：{{FEATURE_SCOPE_EXISTING_CAPABILITIES_AND_EXCLUSIONS}}

**閱讀方式**：走到任務時才載入指定區段。Must Read／必讀提供該任務的行為、介面或技術約束，執行前須掌握；Optional／按需讀取列出觸發條件，成立時即須讀取。每筆引用包含來源文件、可定位區段及用途；依任務決定等級，同一文件在不同任務可為不同等級。已讀、未變更且仍在上下文的區段可沿用；來源變更或上下文遺失時重讀。標示「前置任務產生」的紀錄執行到該任務時才查閱。

**定位方式**：Markdown 使用章節與需求／驗收編號；OpenAPI 使用 method＋path、operationId、schema／response 名稱；DBML 使用表、欄位、index／Notes。來源已有固定行號時可加行號輔助定位，但仍保留語意定位。各故事的責任與落點依 {{RESPONSIBILITY_AND_PATH_SOURCE}} 分組。

## 專案輸入索引
<!-- 有適用憲法時，在此記錄採用的規則版本，並於對應任務的必讀與完成條件引用規則 ID、契約定位與所需證據。 -->

以下列出本輪實際採用的輸入；此索引只供尋找來源，每項任務另指定要讀取的最小相關範圍。

<!-- 依實際輸入重複資料列並連結既有來源。範例內的虛構路徑只示範寫法；本輪生成時須改為可查核的真實來源，不複製範例路徑。 -->
| 文件 | 定位單位 |
| --- | --- |
| [{{INPUT_DOCUMENT_LABEL}}]({{INPUT_DOCUMENT_PATH}}) | {{INPUT_DOCUMENT_LOCATOR_UNITS}} |

<!-- 使用說明：依需要重複各階段中的任務、故事階段及責任分組；只保留實際需要的階段、責任、閱讀列與紀錄欄位，並同步調整階段編號、相依圖及統計。新建清單的任務編號全檔唯一且連續，填成 T001 等形式；更新既有清單時保留有效 ID 與完成證據，新增任務使用不衝突的新 ID，不為連號全檔重編。故事 ID 與 [US] 標記須與來源對應；來源無故事 ID 時依需求增量分組，新建的標記須說明為本輪整理。[P] 僅保留於前置滿足且可獨立修改檔案的任務，其餘移除。刪除填寫用註解後交付。 -->

## Phase 1：Setup — {{SETUP_PHASE_NAME}}

**目的**：{{SETUP_PHASE_PURPOSE}}

- [ ] T{{SETUP_TASK_NUMBER}} 在 `{{SETUP_TASK_PATH}}` {{SETUP_TASK_ACTION_AND_COMPLETION_CONDITION}}。

  **Must Read／必讀**：[{{SETUP_SOURCE_LABEL}}]({{SETUP_SOURCE_PATH}}) {{SETUP_SOURCE_SEMANTIC_LOCATOR}}。用途：{{SETUP_SOURCE_READING_PURPOSE}}。

**檢查點**：{{SETUP_CHECKPOINT_AND_UNPROVEN_BEHAVIOR}}

## Phase 2：Foundational — {{FOUNDATION_PHASE_NAME}}

**目的**：{{FOUNDATION_PHASE_PURPOSE}}

**前置**：{{FOUNDATION_PREDECESSOR_TASK_IDS_AND_REQUIRED_RESULTS}}。

- [ ] T{{FOUNDATION_TASK_NUMBER}} 在 `{{FOUNDATION_TASK_PATH}}` {{FOUNDATION_TASK_ACTION_AND_COMPLETION_CONDITION}}。

  **Must Read／必讀**：[{{FOUNDATION_SOURCE_LABEL}}]({{FOUNDATION_SOURCE_PATH}}) {{FOUNDATION_SOURCE_SEMANTIC_LOCATOR}}。用途：{{FOUNDATION_SOURCE_READING_PURPOSE}}。

**檢查點**：{{FOUNDATION_CHECKPOINT_AND_UNPROVEN_BEHAVIOR}}

<!-- 故事階段：依來源故事重複整個階段。優先級沿用來源；端數、技術、責任分組與任務數依實際設計決定。 -->
## Phase {{STORY_PHASE_NUMBER}}：US-{{STORY_ID}} — {{STORY_NAME}}（優先級：{{STORY_PRIORITY}}）

**目標**：{{STORY_USER_VISIBLE_OUTCOME}}

**獨立驗證**：{{STORY_FIXTURE_ENTRY_ACTION_EXPECTED_RESULT_AND_REQUIRED_DEPENDENCIES}}

<!-- 責任分組：依實際責任與檔案重複。以本案需要的測試、實作及接線任務填寫；可重複任務和閱讀列，按需加入 Optional。 -->
### {{RESPONSIBILITY_GROUP_NAME}}：{{RESPONSIBILITY_COMPONENT_NAME}}

**責任與落點**：`{{RESPONSIBILITY_TARGET_PATH}}`；{{RESPONSIBILITY_BOUNDARY}}。

- [ ] T{{RESPONSIBILITY_TASK_NUMBER}} [P] [US{{STORY_MARKER_NUMBER}}] 在 `{{RESPONSIBILITY_TASK_PATH}}` {{RESPONSIBILITY_TASK_ACTION_AND_COMPLETION_CONDITION}}。

  **Must Read／必讀**：[{{TASK_SOURCE_LABEL}}]({{TASK_SOURCE_PATH}}) {{TASK_SOURCE_SEMANTIC_LOCATOR}}。用途：{{TASK_SOURCE_READING_PURPOSE}}。

  **Optional／按需讀取**：[{{TASK_OPTIONAL_SOURCE_LABEL}}]({{TASK_OPTIONAL_SOURCE_PATH}}) {{TASK_OPTIONAL_SOURCE_SEMANTIC_LOCATOR}}。觸發條件：{{TASK_OPTIONAL_READING_TRIGGER}}；用途：{{TASK_OPTIONAL_READING_PURPOSE}}。

<!-- 閱讀列的定位依實際來源填寫：規格指明 US 的 AC 與相關 FR／NFR／SC；OpenAPI 指明 method＋path、operationId、相關 parameters／requestBody／responses 及具名 schema／response；DBML 指明表、欄位、index 及具體交易／授權 Notes；Markdown 指明章節及狀態分支。需要多份來源時各自列出，用途須對應當前任務。無相關來源格式時省略該格式，不以想像的契約填補。 -->

<!-- 故事整合驗證：每個故事在所需責任分組就緒後驗證可獨立展示的增量。依實際專案決定驗證層級與證據位置。 -->
- [ ] T{{STORY_VALIDATION_TASK_NUMBER}} [US{{STORY_MARKER_NUMBER}}] 在 `{{STORY_VALIDATION_TASK_PATH}}` {{STORY_VALIDATION_ACTION_AND_COMPLETION_CONDITION}}；執行 {{STORY_REQUIRED_VERIFICATION_TASK_IDS}}，於 `{{VALIDATION_RECORD_PATH}}` 記錄證據。

  **Must Read／必讀**：[{{STORY_ACCEPTANCE_SOURCE_LABEL}}]({{STORY_ACCEPTANCE_SOURCE_PATH}}) {{STORY_ACCEPTANCE_SEMANTIC_LOCATOR}}。用途：{{STORY_ACCEPTANCE_READING_PURPOSE}}。

  **Optional／按需讀取**：[{{STORY_VALIDATION_OPTIONAL_SOURCE_LABEL}}]({{STORY_VALIDATION_OPTIONAL_SOURCE_PATH}}) {{STORY_VALIDATION_OPTIONAL_SEMANTIC_LOCATOR}}。觸發條件：{{STORY_VALIDATION_OPTIONAL_READING_TRIGGER}}；用途：{{STORY_VALIDATION_OPTIONAL_READING_PURPOSE}}。

**檢查點**：{{STORY_CHECKPOINT_DEMONSTRABLE_INCREMENT_AND_REQUIRED_EVIDENCE}}

## Phase {{POLISH_PHASE_NUMBER}}：Polish — 品質與交付

<!-- 品質任務：依來源的跨故事需求與實際技術工具重複，保留可重現驗證與最終證據核對。 -->
- [ ] T{{QUALITY_TASK_NUMBER}} 在 `{{QUALITY_TASK_PATH}}` {{QUALITY_TASK_ACTION_AND_COMPLETION_CONDITION}}，結果記錄於 `{{VALIDATION_RECORD_PATH}}`。

  **Must Read／必讀**：[{{QUALITY_SOURCE_LABEL}}]({{QUALITY_SOURCE_PATH}}) {{QUALITY_SOURCE_SEMANTIC_LOCATOR}}。用途：{{QUALITY_SOURCE_READING_PURPOSE}}。

- [ ] T{{DELIVERY_TASK_NUMBER}} 核對 `{{DEVELOPMENT_RECORD_PATH}}` 與 `{{VALIDATION_RECORD_PATH}}` 的重現指令、結果與未通過項目；僅依實測證據標示完成，記錄 {{DELIVERY_REQUIRED_PRECONDITIONS_AND_SCOPE}}。

  **Must Read／必讀**：`{{DEVELOPMENT_RECORD_PATH}}` 的 {{DEVELOPMENT_RECORD_SEMANTIC_LOCATOR}}（T{{DEVELOPMENT_RECORD_PRODUCER_TASK_NUMBER}} 產生），以及 `{{VALIDATION_RECORD_PATH}}` 的 {{VALIDATION_RECORD_SEMANTIC_LOCATOR}}（{{VALIDATION_RECORD_PRODUCER_TASK_IDS}} 產生）。用途：依前置任務的實際產出核對交付狀態。

  **Must Read／必讀**：[{{DELIVERY_SOURCE_LABEL}}]({{DELIVERY_SOURCE_PATH}}) {{DELIVERY_SOURCE_SEMANTIC_LOCATOR}}。用途：{{DELIVERY_SOURCE_READING_PURPOSE}}。

## 相依關係與執行順序

<!-- 依實際保留的階段／故事重複節點和連線；獨立故事由其真實前置分支，共用檔案或行為有依賴才串接。節點 ID 使用 Mermaid 合法識別字。 -->
```mermaid
flowchart TD
    S["Setup：{{SETUP_TASK_ID_RANGE}}"] --> F["Foundation：{{FOUNDATION_TASK_ID_RANGE}}"]
    F --> {{STORY_DEPENDENCY_NODE_ID}}["US{{STORY_MARKER_NUMBER}}：{{STORY_TASK_ID_RANGE}}"]
    {{STORY_DEPENDENCY_NODE_ID}} --> Q["品質與交付：{{POLISH_TASK_ID_RANGE}}"]
```

<!-- 依實際階段與故事重複序列列項，明列每項任務的前置、接線等待及共用檔案順序。 -->
- **{{DEPENDENCY_GROUP_NAME}}**：{{TASK_PREDECESSORS_INTEGRATION_WAITS_AND_SHARED_FILE_ORDER}}。
- `[P]` 表示滿足前置後可與同批次獨立檔案工作平行；{{SHARED_FILE_SERIALIZATION_REQUIREMENTS}}。

## 平行執行範例

<!-- 依實際可平行的批次重複；沒有可平行工作時寫明順序及限制。 -->
```text
{{PARALLEL_BATCH_PRECONDITIONS}}：
  {{PARALLEL_TASK_IDS_AND_DISTINCT_FILE_RESPONSIBILITIES}}
  {{INTEGRATION_TASK_WAIT_ORDER}}
```

## 實作策略

<!-- 依最小可交付增量、後續故事與跨故事品質門檻重複並連續編號。 -->
1. {{IMPLEMENTATION_INCREMENT_AND_VALIDATION_GATE}}

## 任務統計與完成判準

<!-- 每個保留的階段／故事一列。總數及編號範圍由實際任務核算，不沿用範例數值。 -->
| 階段 | 任務 | 數量 |
| --- | --- | ---: |
| {{STATISTICS_PHASE_NAME}} | {{STATISTICS_PHASE_TASK_ID_RANGE}} | {{STATISTICS_PHASE_TASK_COUNT}} |
| **合計** | **{{ALL_TASK_ID_RANGE}}** | **{{TOTAL_TASK_COUNT}}** |

- 每項 task 依自身閱讀指引取用來源；引用可定位到實際區段，不能只寫「參考 API／規格」。
- 完成時保存變更與可重現驗證；來源缺失、測試失敗或尚未執行時保持未勾選。
- 新建清單及新增任務的 checkbox 維持未完成；更新時保留有效的既有完成狀態，證據失效則註明需重驗。輸入文件及計畫中的程式落點不能視為程式已建立或產品已驗收。
