---
name: artifact-to-skill-engineering
description: 從參考產物反推能生成同類產物的目標 Skill。適用於先選定並互動客製化 Example、經使用者確認後委派 skill-form-template 抽取骨架與範例，再委派 skill-engineering 建立推導流程與必要規則的工作；可接續已有成果的階段。
---

# 從產物反推 Skill

以使用者認可的產物為目標，編排 Example 定稿、Template 抽取與目標 Skill 開發。對話中已確認的選擇持續有效；本 Skill 負責階段、確認閘門與交接，專職工作的細節由受委派 Skill 維護。

# SOP

## Phase 1 -- 選定產物基準與目前階段

1. READ 讀取目前需求、相關對話、候選產物與目標 Skill 目錄，辨識主要 Example、可編輯位置、目標產物類型、目標 Skill 名稱及本次授權範圍。已有骨架、範例、提案或實作時，核對它們對應的版本與確認紀錄。
2. THINK 選定基準與判斷階段時，先讀取 `rules/artifact-baseline-and-gates.md`，再依完成證據收斂目前最早尚未完成的成果，決定從哪一階段接續。
3. WRITE 若主要 Example、可編輯位置或目標 Skill 仍有會改變工作對象的歧義，提出最小必要確認並等待答案；資訊充分時直接進入對應階段。本次只要求提案或某一階段時，依該範圍交付。

## Phase 2 -- 互動修改並確認 Example

1. WRITE 依已載入規則及使用者客製化方向，以適合該產物格式的工具修訂主要 Example 工作檔中已明確的部分，標示未決內容，讓本輪改動、建議與需決策事項可辨識。
2. DELEGATE 若歧義會改變產物結構、欄位語意、範圍或後續 Skill 的生成判斷，讀取 `../clarify/SKILL.md` 並呼叫 `/clarify`，交付產物類型、相關內容、已確認決策、缺口及本輪澄清狀態。非規格類產物採需求模式交回決策，再由本流程更新 Example。
3. WRITE 回報本輪已完成改動、具體建議與待確認點，取得使用者對目前 Example 版本的認可。已有對該版本有效的確認時沿用；尚未認可或仍有阻塞決策時，停留在本階段。

## Phase 3 -- 由定稿 Example 抽取 Template

1. READ 抽取或核對模板雙檔前，讀取 `rules/skill-inference-and-validation.md`，確認產物契約與專職委派的要求。
2. DELEGATE Example 已認可且本次範圍包含模板抽取時，呼叫 `/skill-form-template`，交付定稿 Example、目標 Skill 的模板目錄、產物格式及使用者指定命名，建立或更新骨架與範例雙檔；已有一致雙檔時沿用。
3. READ 核對雙檔是否忠實對應定稿 Example，以及固定結構與可變內容是否分工清楚。差異涉及產物設計時回到 Example 確認，抽取錯誤則在本階段修正；雙檔就緒後才交接 Skill 開發。

## Phase 4 -- 以產物目標反推 Skill

1. THINK 準備反推目標 Skill 時，若本輪尚未載入 `rules/skill-inference-and-validation.md` 則先補讀；再收斂代表性 Input Prompt、預期產物與驗收依據，確認交接資料就緒並辨識已有成果可接續的範圍。
2. DELEGATE 交接資料就緒且本次範圍包含 Skill 開發時，呼叫 `/skill-engineering`，交付目標 Skill 目錄、Template 與 Example、代表性輸入、已確認設計及本次提案或實作範圍，依已載入規則完成編排交接。依該 Skill 的流程處理工程提案與實作；保留使用者已有授權及尚待確認的事項。

## Phase 5 -- 驗證來源一致性並交付

1. READ 成果驗證時，若本輪尚未載入 `rules/skill-inference-and-validation.md` 則先補讀；依已載入規則核對 Example、雙檔、目標 Skill 與驗證結果的一致性及實際產出。上游改動時回到最早受影響階段，保留仍有效的成果。
2. WRITE 回報已完成階段、認可的產物基準、檔案位置、實際執行的驗證及剩餘事項；只完成提案、模板或待確認草稿時，按實際狀態交付。
