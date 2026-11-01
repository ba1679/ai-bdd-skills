---
name: api-plan
description: 整合服務操作、身分、事件與恢復契約，以 OpenAPI YAML 的標準結構及具名擴充區交付；可獨立分析、更新既有契約或承接 system-analysis 委派。
---

# SOP

## Phase 1 -- 確認責任與有效輸入

1. READ 讀取 `../system-analysis/references/interface-analysis-contract.md` 及本次需求或委派，確認服務責任、來源、必要依賴、唯一寫入位置與已有狀態；獨立呼叫無須先有 Plan。
2. READ 讀取相關規格、研究、既有契約及可用專案現況，辨識有效輸入與已確認決策；只補足影響本輪契約的資料。
3. THINK 判斷輸入是否足以分析；必要決策缺口或矛盾交回呼叫者，標示受影響結論，其餘部分繼續並沿用有效成果。

## Phase 2 -- 整合服務與恢復契約

1. THINK 分析呼叫、身分、資料交換及失敗恢復時，先讀取 `rules/service-interface-analysis.md`，整合同責任的命令、查詢、session 與非 HTTP 協定，形成接收端可實作的契約。
2. THINK 對照有效資料規則與 UI 所需結果，區分已確認語意、服務設計與新增依賴；涉及其他主責的新決策交回呼叫者，避免各自定義衝突契約。

## Phase 3 -- 保存並驗收 OpenAPI

1. WRITE 產出或更新整合契約時，先讀取 `rules/openapi-contract.md`、`templates/openapi.yaml` 與 `templates/openapi.example.yaml`；複製骨架、對照範例並依本輪分析改寫適用區塊，保存至指定的 OpenAPI YAML。
2. READ 依已載入判準檢查格式、引用、範例、擴充契約及跨產物語意，修正本檔缺漏；記錄實際執行與未執行檢查、未決影響及可用範圍。
3. WRITE 回傳產物位置、可用結論、檢查結果與必要依賴；由呼叫者更新 Plan，服務分析完成與實際整合／部署結果分別陳述。
