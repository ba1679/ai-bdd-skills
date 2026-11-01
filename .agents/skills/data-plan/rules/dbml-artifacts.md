# Rule 1 - 本輪資料責任以整合 DBML 為唯一來源

- Level: `MUST`
- 預設交付 `analysis/data-model.dbml`，使用者指定位置優先；同一資料責任的模型與存取規則整合於此，不按界面數量分檔。確需分開的獨立資料責任先確認各自所有權與寫入位置。
- 關聯式結構使用表、欄位型別、鍵、索引、參照、空值、預設、枚舉及可表達的檢查約束；來源、適用性、交易、併發、資料權限、RLS、生命週期與交接保存在 Notes，不另建一份 schema 欄位清單或獨立說明產物。
- Project Note 簡述來源、所有權、共用交易／授權／投影約束及本輪限制；Table／欄位 Notes 記錄局部規則。已能由結構閱讀的型別與鍵不重列，共用未決引用 Plan 並補局部影響，沒有 Plan 才在本檔保留必要說明。
- 只交付 DBML，不另交資料說明 Markdown、`.dbdiagram` 或圖表設定檔；交接與檢查結果可在相關 Note 或回覆說明，不固定展開任務摘要、下游矩陣及完成表格。只有一個主責寫入整合檔，跨領域變更交回呼叫者協調。

## Good Example

- 這個例子是好的，因為主檔明確且平行工作不會寫入同一檔案。

```md
analysis/data-model.dbml：房間、成員、答案、猜測及 receipt 的共同模型與規則。
Project Note 引用 spec、研究及 Plan；rooms、command_receipts 的 Notes 說明局部交易與回放。
同一 data-plan 主責整合這些資料責任，交回呼叫者時指出 API 可引用的物件與未知影響。
```

## Bad Example

- 這個例子是壞的，因為結構有兩份來源，且多工作會覆寫共用檔。

```md
每個操作各寫一份 DBML，另交資料說明及 .dbdiagram，分別維護同一批欄位與規則。
多個 Planner 同時改整合 DBML，沒有唯一主責。
```

# Rule 2 - 關聯設計必須有可追溯且可使用的結構

- Level: `MUST`
- 依實際模型輸出 `Table`、欄位與型別、主鍵、必要的 `unique` 或複合鍵、索引、`null`／`not null`、有依據的 `default`、`Enum` 及 `Ref`；不適用的構造可省略，不能為湊齊語法新增業務規則。
- 可由 DBML 表達的單欄或跨欄約束以 `check` 或 `checks` 定義，檢查運算式使用反引號；跨記錄交易、版本競爭、去重期限、RLS 與動態授權等能力在最相關的 Notes 說明設計及執行責任，不能靠註解宣稱已強制。
- 以中文 `note` 記錄表與關鍵欄位用途、規則依據及局部限制。未知的保留期限、識別碼格式、容量與服務能力維持待補或明列設計提案，不能填入看似確定的值。
- 範例示範完整成品，骨架的 Enum／Table／欄位／index／check／Ref 區塊按模型重複或省略；單欄與複合鍵依識別語意選用。範例表數、receipt、version、PVP 或庫存操作不構成新需求。
- 局部可用時在相關 Notes 說明可用物件、限制及所需輸入；由呼叫者驗收後供下游承接，不為狀態管理複製一份模型。必要資料權威、讀寫、不變條件、權限、交易及生命週期分析仍須存在；遷移／復原依本輪必要深度處理。

## Good Example

- 這個例子是好的，因為關鍵結構可由 DBML 直接辨識，交易限制保存在表 Note。

```dbml
Table inventory {
  warehouse_id text [not null, note: '倉庫隔離範圍；來源見 Project Note「輸入來源」']
  item_id text [not null]
  quantity bigint [not null, check: `quantity >= 0`, note: '件數不可為負']
  version bigint [not null, default: 0, check: `version >= 0`]
  indexes {
    (warehouse_id, item_id) [pk]
  }
  Note: '交易：在同一原子操作內檢查餘額並扣減、遞增版本；去重保留期限見 Project Note「未決事項」'
}
```

## Bad Example

- 這個例子是壞的，因為只有表名與文字，缺少可用的資料庫結構。

```dbml
Table inventory {
  data text [note: '包含所有欄位，詳見另一份欄位清單']
  Note: '系統自動保證交易、RLS、永久去重及所有限制'
}
```

# Rule 3 - 引用既有模型時保留其所有權與原始定義

- Level: `MUST`
- 其他領域已擁有的表、鍵與枚舉，採實際取得且已確認的模型；在相關 Note 引用來源物件，不重新決定其型別、唯一性或刪除行為。
- 若為讓本界面 DBML 可獨立解析而需要引用表占位，只帶入關聯所需的已確認欄位與鍵，在該表 `note` 明列「引用表占位」、權威來源、承接範圍及本次未擁有其完整結構。占位不是新增資料表或取代來源模型。
- 尚未取得的表模型不能用猜測的占位補成定案；列明缺少來源及受影響關聯。需要改動上游結構時先回報主流程，不直接改寫他人產物。
- 外部系統識別碼、快取鍵、文件引用或邏輯關聯，只有確認存在資料庫參照約束時才使用 `Ref`。非關聯式來源可僅使用 Project Note，明示「DBML 關聯式結構不適用」，並記錄實際鍵／文件／物件／事件模型、讀寫、權限、生命週期與可用來源；必要原生格式在 Notes 說明或引用既有來源，不額外交付 schema 或虛構表、外鍵。

## Good Example

- 這個例子是好的，因為引用欄位有來源，且不把外部識別碼偽裝成資料庫外鍵。

```dbml
Table warehouses {
  id text [pk, not null]
  Note: '引用表占位；權威來源為已確認倉庫模型的 warehouses.id；只承接主鍵，不代表完整表設計'
}
Table inventory {
  warehouse_id text [not null]
  external_tracking_key text [null, note: '第三方追蹤識別值，無本資料庫外鍵']
}
Ref: inventory.warehouse_id > warehouses.id
```

## Bad Example

- 這個例子是壞的，因為尚未取得來源就重新決定既有鍵，且製造不存在的外鍵。

```dbml
Table warehouses {
  id uuid [pk, note: '沒讀上游模型，猜它應該使用 UUID']
}
Ref: inventory.external_tracking_key > third_party_service.id
```

# Rule 4 - 語法檢查與資料庫執行保證分別記錄

- Level: `MUST`
- 以 [DBML 官方規格](https://dbml.dbdiagram.io/docs/) 為語法依據；需要查核不熟悉或版本相關語法時查官方文件。完成產物須移除槽位及填寫提示，檢查每個 `Ref` 的表、欄位、型別與基數對應。
- 有可用的官方 `@dbml/core` parser 時，對本次產出的 DBML 執行解析並記錄方式及結果；未執行 parser 時明示實際檢查與影響，不宣稱通過。更新來源或範例時重新檢查受影響內容，不能沿抄先前的檢查結果。
- parser 通過只證明工具接受 DBML；資料庫方言、約束強制、交易、RLS、migration 及執行結果須由後續研究、實作與驗證承接。

## Good Example

- 這個例子是好的，因為檢查證據與其能支持的結論一致。

```md
已以 @dbml/core 解析本界面 DBML，語法可接受；尚未執行 migration、資料操作或 RLS 驗證。
```

## Bad Example

- 這個例子是壞的，因為從文件解析跳到資料庫運作保證。

```md
DBML parser 沒報錯，因此資料庫交易及資料隔離均已通過驗證，可以部署。
```
