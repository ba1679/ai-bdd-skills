---
name: data-plan
description: 整合本輪資料責任、模型、讀寫、交易、授權與生命週期，以一份 DBML 結構與精簡 Notes 交付；可獨立使用或承接 system-analysis 委派。
---

# SOP

## Phase 1 -- 確認資料範圍與來源

1. READ 讀取 `../system-analysis/references/interface-analysis-contract.md`、本次需求及實際可用的規格、研究、既有資料與相鄰產物，辨認已確認決策、設計推導及缺口。
2. THINK 收斂本輪整合的資料責任、必要輸入與唯一 DBML 路徑；預設 `analysis/data-model.dbml`，獨立呼叫不要求 Plan。缺少必要業務或權限決策時回報局部影響，繼續不受影響的部分。

## Phase 2 -- 形成可交接的資料設計

1. THINK 分析模型與存取時，先讀取 `rules/data-interface-design.md`，再釐清資料權威、識別與關聯、值域、讀寫及狀態規則、交易競態、重試、授權與本輪必要生命週期約束。
2. THINK 比對已有服務與 UI 契約，收斂資料投影、可信輸入及下游可用結果；需要新決策時標明責任與影響，將相鄰領域的 wire、session 與互動設計交回其主責。

## Phase 3 -- 保存整合 DBML

1. THINK 安排結構與 Notes 時，先讀取 `rules/dbml-artifacts.md`，再決定本輪擁有的模型、引用占位與最相關的說明位置，讓共用結論只維護一處。
2. WRITE 產出或更新 DBML 前，讀取 `templates/schema.dbml` 與 `templates/schema.example.dbml`；依骨架的可重複語法區塊、參考完整範例改寫本輪模型與必要 Notes。來源及未決引用 Plan 的局部影響；無 Plan 時保存在相關 Notes，更新時沿用有效內容。

## Phase 4 -- 檢查模型並交回

1. READ 依已載入規則檢查 DBML 語法、鍵與參照、結構和 Notes、來源與服務投影是否一致；修正缺漏並記錄實際檢查，將未執行的資料庫驗證與文件檢查分開。
2. WRITE 回報 DBML 路徑、可用模型與規則、檢查結果、未決影響及新依賴，供呼叫者驗收；必要結論未就緒時保留待補或待修正，不複製範例的完成紀錄。

## 工作邊界

本 skill 只交付 DBML；模型可涵蓋同一資料責任的多個界面，不因界面數量拆檔。關聯式、文件、檔案、物件、快取與事件來源依實際需求分析；不適用的關聯式結構以 Notes 說明。SQL、migration、資料改寫及部署由後續實作承接；Plan、OpenAPI、HTML 與 UI／UX Markdown 由各自主責維護。
