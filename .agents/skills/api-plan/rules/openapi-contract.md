# Rule 1 - 一份 OpenAPI 整合真實服務與非 HTTP 約定

- Level: `MUST`
- 預設交付指定位置的一份 `api.openapi.yaml`，本 skill 相容基線為 OpenAPI `3.1.2`；`info.version` 表達本案契約版本。格式依據為 [OpenAPI 3.1.2](https://spec.openapis.org/oas/v3.1.2.html)，使用者指定的相容版本優先。
- 真實 HTTP 操作放入 paths／webhooks／callbacks、parameters、requestBody、responses、components 及 security。HTTP 路由不由文件方便性推導；平台 SDK session、佇列或非 HTTP 事件不另造應用 route。
- 非 HTTP 契約保留於同檔具名擴充區，如 `x-session`、`x-realtime`、`x-interfaces`；純非 HTTP 時保留有效 metadata、`paths: {}` 及適用擴充區。不要另交獨立 session／事件／維運 YAML。
- 來源、設計理由與限制置於前言、對應 description 或具名擴充區。全域未知引用 Plan，無 Plan 時在本檔保留其影響；不要求固定 `x-analysis` 任務／追蹤大表。

## Good Example

```yaml
openapi: 3.1.2
info:
  title: 庫存通知契約
  version: 1.0.0
  description: 純佇列服務；來源為庫存通知規格，尚未部署。
paths: {}
x-interfaces:
  inventory-notifications:
    transport: message-queue
    delivery: at-least-once；相同訊息識別去重。
```

## Bad Example

```yaml
openapi: 3.1.2
info: {title: 庫存通知, version: 1.0.0}
paths:
  /queue:
    post:
      summary: 為了 OpenAPI 而新增的佇列路由
  /deploy:
    post:
      summary: 並不存在的部署端點
```

# Rule 2 - 共用模型、錯誤與診斷只有一個對外定義

- Level: `MUST`
- 同責任的 operations、session 及事件使用一致的識別、狀態與診斷命名；共用的 wire schemas、security、parameters、headers、responses 以 components 定義並引用。`requestId` 之類診斷識別與冪等操作識別明確分開，body 與 header 的對應關係須可理解。
- 每個操作有唯一 operationId、實際 method/path、成功與適用錯誤，path 槽位與 required path parameters 對應；明確 body 必要性、參數序列化、媒體型別與回應標頭。查詢、公開或無 body 能力不因模板而新增命令、登入或冪等鍵。
- 共用 middleware／執行邊界可能拒絕的請求，也須對應到每個受影響 operation 的 response；例如 Host、Origin、驗證或限流，不只寫在全域擴充區。只列真實適用的拒絕，不為消除 lint 警告虛構錯誤。
- Schema 區分 required、可省略、可空、值域、精度及額外欄位策略；OpenAPI 3.1 採 JSON Schema 空值表達，組合模型的閉合策略不得誤拒合法分支。機器可驗證限制寫進 schema，跨欄位／業務限制寫在相應 description 並保留檢查期待。
- security 宣告須連到真實 scheme；caller／tenant／角色授權、隱私遮蔽、狀態 guards、原子結果、重試與冪等映射於相關 operation／schema。匿名才明確免認證，不以 security scheme 取代資源授權。
- 成功、拒絕與未知結果 examples 使用安全虛構值並符合實際 schema。錯誤差異、可重試性、no-store、opaque not-found、Retry-After 與 CORS 暴露只按本案需求定義，不固定套用範例所有狀態碼。

## Good Example

```yaml
paths:
  /reservations/{reservationId}:
    get:
      operationId: getReservation
      description: 先驗 caller 的可讀範圍，再讀 caller-safe 投影；不存在與不可讀採相同 404。
      parameters:
        - name: reservationId
          in: path
          required: true
          schema: {type: string}
      responses:
        '200':
          description: 權威預留狀態。
          content:
            application/json:
              schema: {$ref: '#/components/schemas/Reservation'}
        '404':
          description: 不存在或目前 caller 不可讀；相同對外結果。
```

## Bad Example

```yaml
paths:
  /reservations/{reservationId}:
    get:
      parameters: []
      requestBody: {required: true}
      responses:
        '200':
          content:
            application/json:
              schema: {$ref: '#/components/schemas/另一份Reservation'}
# 命令叫 requestId，GET 改名 traceId，事件再叫 operationId，三者各有不同含義卻未交代。
# 全域說所有請求會因 Host 不符回 403，GET 卻未列該回覆。
```

# Rule 3 - 擴充契約保留傳輸語意及可檢查的 payload

- Level: `MUST`
- 非 HTTP 部分依協定組織於具名 `x-*`，包含實際 transport、channel/topic、訊息 label、結構化 payload schema、必要範例、授權及 publish／delivery／recovery；session 或環境約定可用簡短具名欄位，不強填不適用的事件格式。
- 訊息 payload 使用 JSON Schema（在 OpenAPI 3.1 文件中沿用相容的 JSON Schema 語法），保留 type、required、properties、值域及例子；可內嵌或透過本檔 `$ref` 共用 components。重複的欄位 schema 保持相同約束；傳輸特有的語意如 topic 與 payload 一致性另寫清楚。
- 引用 HTTP 查詢、UI 或資料產物時使用實際位置；local JSON Pointer 按規則 escape path 的 `/` 為 `~1`。不要把新協定字串偽裝成 OpenAPI operation。
- 擴充區的內部結構並非標準 OpenAPI operation；一般 OpenAPI lint 不能證明其 schema、範例或協定語意有效。必須另外解析所選 payload schema、解引用並驗證範例，且人工核對授權、提交時點、送達保證與恢復條件。

## Good Example

```yaml
x-realtime:
  related:
    snapshot: '#/paths/~1reservations~1{reservationId}/get'
  contract:
    transport: websocket
    topic: 'reservation:<id>'
    event: reservation_invalidated
    payload:
      type: object
      additionalProperties: false
      required: [reservationId]
      properties:
        reservationId: {type: string}
      example: {reservationId: r-demo}
    authorization:
      receive: 同資源 active member。
      publish: 僅受控 worker。
    delivery: 可能遺失、重複、亂序；新訊號經受權 GET 讀真相。
```

## Bad Example

```yaml
x-realtime:
  payload: 任意 JSON；格式以 UI 實作為準。
  validation: OpenAPI lint 通過，所以 private 授權及送達保證均已驗證。
```

# Rule 4 - 檔內有效性與實際驗證分別回報

- Level: `MUST`
- 交付前檢查 YAML 解析、OpenAPI 結構、local／external refs、安全 scheme、path parameters、schemas 及適用 examples。預設單檔；外部 schema 必須有隨交付可讀來源，不能留未提供的引用。
- 另檢查非 HTTP 擴充區的 payload schemas／examples、對應標準模型的型別與命名、傳輸語意，以及 HTTP、DBML、UI 的角色可見性和恢復是否一致。只檢查當前適用部分，無事件不建立空殼。
- 回報工具／方法與實際結果，將檢查證據與後續驗證期待分開；未能執行說明原因與影響。必要授權、schema 或狀態碼仍缺決策時，保留待補輸入／待修正，不能用語法通過取代分析就緒。
- 不抄範例的過往證據，不把文件檢查宣稱為真實服務、channel policy、socket 撤權、CORS 或公開部署驗證。

## Good Example

```md
本次：YAML、OpenAPI 結構、所有 local refs 與 HTTP examples 已檢查。
另外：x-realtime.payload 的 JSON Schema 及範例已驗證，publish／recovery 與 GET、DBML 做語意對照。
待後續：實際 policy、commit 後發布、已連線撤權與跨來源標頭；公開啟用限制見 Plan U3。
```

## Bad Example

```md
YAML 解析成功；沿用 Example 的全部驗證結果，宣告事件授權與部署完成。
x-* 有未解析引用但一般 lint 沒報錯，所以忽略。
```
