---
name: system-analysis
description: 依需求、研究及既有成果整理精簡 plan.md，按責任與實際依賴委派 ui-ux-plan、data-plan、api-plan，再核對跨產物一致性。交付限 Plan、OpenAPI YAML、DBML、HTML prototype 與簡短 UI／UX Markdown；可建立、更新或接續分析。
---

# SOP

## Phase 1 -- 收斂來源與本輪交付

1. READ 讀取需求、專案規範及已有規格、研究、技術棧與分析成果，確認本次建立、更新或接續的範圍。
2. THINK 判斷來源、規劃粒度及缺口影響時，讀取 `rules/planning-scope-and-sources.md` 與 `references/interface-analysis-contract.md`，確定可用依據、交付位置及停止條件。
3. WRITE 只對阻止合理分析的缺口提出必要問題；其餘未知連同影響與接手時點記入計畫，繼續可完成部分。

## Phase 2 -- 形成輪廓與分析分工

1. THINK 依已載入規則整理系統邊界、核心流程、共同約束、目標專案結構及本輪可交付內容。
2. THINK 安排工作時，讀取 `rules/endpoint-interface-wave-planning.md` 與 `rules/planner-dispatch-and-execution.md`，收斂各主責的整合任務、必要輸入、依賴及可沿用結果。
3. READ 寫入計畫前，依已載入共同契約選擇建立或更新方式；需要骨架時讀取 `templates/plan.md`，觸發範例對照時讀取 `templates/plan.example.md` 的適用範圍，確認本案應保留的內容與深度。
4. WRITE 保存精簡 `plan.md`，以一份索引記錄主責、實際產物、必要依賴與未決影響；本次只要求規劃時，在核對輪廓與交付邊界後回報並停止。

## Phase 3 -- 委派領域分析並承接成果

1. THINK 核對各任務的必要輸入與既有成果，選出可啟動工作；來源或依賴變動時先調整受影響部分。
2. DELEGATE 依已載入規則呼叫 `/ui-ux-plan`、`/data-plan` 或 `/api-plan`，交付責任範圍、可用來源、必要前置結果及唯一寫入位置；無相互依賴的工作可平行，同一產物保持單一主責。
3. READ 讀取回傳的實際產物及其檢查結果，核對覆蓋、格式與下游可用內容；有缺漏或衝突時交回原主責修正，保留已確認可用的部分。
4. WRITE 更新計畫索引、未決影響及可承接結果，重複本階段直到必要分析齊備，或剩餘工作都需要尚未取得的輸入。

## Phase 4 -- 核對交付與接續

1. READ 依已載入規則核對跨產物名稱、資料含義、授權、狀態、失敗恢復與可見性，以及實際連結、格式與適用原型；發現落差則回到受影響主責修正。
2. WRITE 更新 `plan.md` 的實際狀態與下一步，回報本輪五類交付中適用的成果及待補事項，區分文件檢查、原型操作與真實服務驗證。
