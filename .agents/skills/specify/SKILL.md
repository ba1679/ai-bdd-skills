---
name: specify
description: 將使用者的功能描述、補充需求或既有規格整理成可驗證的 spec.md，依專案樣板建立或更新 US、驗收條件、FR、NFR 與成功標準，保存品質檢查與就緒判定。需求無法確認或存在影響規格的歧義時，交給 clarify 收斂後接續撰寫。
---

# 功能規格撰寫

以使用者輸入、適用的專案資料與已確認決策作為需求依據，產出繁體中文功能規格。樣板負責產物格式，範例供理解完整度；故事數量、角色、政策與數值依本次需求決定。

# SOP

## Phase 1 -- 收斂需求與範圍

1. READ 讀取使用者輸入、相關上下文及適用的專案規範。
2. READ 如果要修改既有規格，先讀取原檔；如果只是繼續先前的品質檢查，就讀取原規格與品質清單，從 Phase 4 接著做。
3. READ 讀取 `templates/spec.template.md`，確認輸出結構；如果對需求應寫到多細，或不同章節如何銜接仍不確定，再參考 `templates/spec.example.md` 的相關章節，區分通用寫法與個案決策。
4. THINK 整理使用者目標、角色、主要流程、本次範圍及依賴，區分已確認內容、可明列的低影響假設與待確認事項，保留各項決策的來源。
5. DELEGATE 若缺口會影響角色權限、流程狀態、業務規則、範圍或驗收結果，呼叫 [$clarify](../clarify/SKILL.md)，交付目前需求、已確認決策、缺口、已有的規格路徑與本輪澄清狀態；接回結果後繼續撰寫，仍有關鍵缺口時保留待確認草稿。

## Phase 2 -- 推導故事與需求

1. THINK 建立或調整 US 時，先讀取 `rules/story-and-requirement-derivation.md`，再依使用者目標收斂故事邊界、預期成果、優先級理由及獨立驗證方式。
2. THINK 依已載入規則，沿各故事的操作與資料變化推導功能行為、業務規則、相關實體及品質限制，確認其來源、適用條件與範圍。
3. THINK 建立驗收與品質目標時，先讀取 `rules/acceptance-and-quality-criteria.md`，再依已載入規則收斂驗收情境、品質要求及可量測成果，並依樣板完成需求分類、主要歸屬、ID 與引用關聯。

## Phase 3 -- 寫入功能規格

1. WRITE 依已載入規則與已讀樣板，將功能規格寫入使用者指定或專案慣用的位置，整合需求、已接受的澄清答案、明列假設與適用章節，按實際數量展開故事及條目。新規格的建立日期與分支反映實際環境，無適用分支時標示「不適用」；狀態反映確認程度，未解事項以具體「待確認」內容呈現。完成填位替換與填寫提示清理；更新既有規格時修改原檔，保留原建立日期、原 ID 及未受影響內容。

## Phase 4 -- 驗證並交付

1. READ 補讀本輪尚未載入的 `templates/spec.template.md`、`rules/story-and-requirement-derivation.md` 與 `rules/acceptance-and-quality-criteria.md`；讀取 `rules/specification-quality-validation.md`，確認現行規格的檢查判準與本輪驗證狀態。
2. WRITE 讀取 `templates/requirements-checklist.template.md` 與 `templates/requirements-checklist.example.md`，再依骨架複製結構、參考範例改寫填位符號，對照原始輸入、補充決策與現行規格完成初驗或接續重驗，在功能目錄的 `checklists/requirements.md` 建立或更新品質檢查紀錄。
3. WRITE 依已載入規則，在修正上限內修正未通過項目、重驗並更新同一份品質清單；需要使用者決策時回到 Phase 1 的澄清步驟，交付相關檢查缺口與本輪修正紀錄，接回結果後重新驗證。
4. WRITE 依已載入規則判定就緒狀態，交付規格與品質清單的實際路徑、主要內容、檢查結果及剩餘事項，交回原需求流程。
