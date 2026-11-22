---
name: ui-ux-plan
description: 依需求與已確認契約製作可直接開啟的單檔 HTML 互動原型，搭配簡短 UI／UX 紀錄；涵蓋操作、角色可見性、非同步回饋及恢復。可獨立使用或承接 system-analysis，不連接真實服務或部署。
---

# SOP

## Phase 1 -- 確定體驗範圍與來源

1. READ 讀取委派或使用者需求、專案規範及 `../system-analysis/references/interface-analysis-contract.md`，確認本輪使用者目標、相關界面、HTML 與 Markdown 的唯一寫入位置，取得需求、既有設計及可用契約；獨立呼叫不要求先有 Plan。
2. THINK 判定本輪是否有 UI 責任，無則依共同契約回報不適用；有則區分已有依據、可先推導的互動及必要決策缺口，向呼叫者回報局部影響並沿用其餘成果，不以完整 API 尚未定稿阻止全部體驗分析。
3. READ 本輪有 UI 責任時，讀取目標 repo 的 `constitution/README.md`，再依[憲法套用約定](../constitution/SKILL.md#憲法套用約定)載入適用的 `constitution/shared/rules.md`、HTML 原型的 `constitution/ui-prototype/rules.md` 與 UI／UX 紀錄的 `constitution/ui-plan/rules.md`。需裁決呈現方式時，先讀 `rules/ui-ux-interface-analysis.md` 的可覆寫項目。

## Phase 2 -- 收斂可操作的互動模型

1. THINK 分析本輪操作時，讀取 `rules/ui-ux-interface-analysis.md`，整理角色、前置條件、輸入、權威資訊、狀態轉移、拒絕與恢復；將已確認契約與分析建議分開，選出原型必須能走過的情境。
2. THINK 準備畫面與原型時，讀取 `rules/visual-research-and-design-delegation.md`，核對既有設計、內容與裝置要求，決定沿用或補足視覺依據；需要外部研究入口時才讀取 `references/external-design-capabilities.md`，依實際能力取得候選並篩選。

## Phase 3 -- 製作 HTML 與簡短紀錄

1. WRITE 建立或更新原型時，依已載入共同契約選擇建立或更新方式；需要骨架時讀取 `templates/prototype.html`，觸發範例對照時讀取 `templates/prototype.example.html` 的適用範圍，以記憶體假資料製作單檔 HTML，完成主要流程及適用的拒絕／未知結果／恢復；正常產品控制與示範控制分開，不照抄範例角色、數值或視覺版型。將 HTML 的語言標記設成實際原型的介面語言。
2. WRITE 撰寫 UI／UX 紀錄時，若尚未載入 `rules/ui-ux-interface-analysis.md` 則先補讀，再依已載入共同契約選擇建立或更新方式；需要骨架時讀取 `templates/ui-ux-plan.md`，觸發範例對照時讀取 `templates/ui-ux-plan.example.md` 的適用範圍，整理原型入口、操作方式、畫面意圖、UI 實作注意事項及限制。

## Phase 4 -- 實際檢查並交接

1. READ 以瀏覽器開啟 HTML，操作主要流程、適用角色及代表性拒絕／未知結果／恢復，檢查鍵盤、焦點、狀態提示與指定裝置配置；對照來源及服務／資料契約修正差異，重新檢查受影響部分。核對原型適用的憲法，包括實際介面語言與 HTML 語言標記是否一致。
2. WRITE 將實際執行、未執行及其影響簡記於 Markdown，移除填位符號與不適用空殼。寫入後核對該紀錄適用的憲法，再回傳兩份產物的入口、各自的檢查結果、證據和待驗證條件。必要原型未產出或未能檢視時保留待補／待修正，區分假資料行為與真實服務、讀屏及部署驗證。
