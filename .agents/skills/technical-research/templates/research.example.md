# Phase 0 技術研究：崇鉅生技人資與薪資管理系統

**研究日期**：2026-10-10（Asia/Taipei；保留 2026-10-08 原查核基線）
**輸入依據**：[spec.md](./spec.md)、專案憲章 v2.0.0、使用者的 monorepo、學習與公開 demo 要求，以及採用 Neon／PostgreSQL 的既定約束。

研究涵蓋套件相容性、前端狀態、Node API、Neon／PostgreSQL、Firebase Auth/Storage、文件交付、特休精度與 demo 邊界，以官方文件及 npm 套件 metadata 支持規劃所需的決策、理由與替代方案。本文保留 2026-10-08 至 2026-10-10 的研究基線；所列版本與服務條件不代表最新查核結果，選型依據也不代替實際安裝、測試或部署證據。待驗證事項集中列於文末。

## R01 — Monorepo 與責任邊界

**決策**： 使用 pnpm workspaces，僅建立 `apps/web`、`apps/api`、`packages/contracts`。內部依賴用 `workspace:*`；根 scripts 統一格式、lint、型別、測試及建置。共用 package 放 DTO、嚴格執行階段 schemas 與 error codes；計薪規則及 persistence adapters 留在 API，UI 留在 web。

**理由與來源**： 兩個應用能透過 pnpm filter/recursive scripts 管理，共用安全契約能減少前後端欄位漂移，仍可清楚看懂一次操作跨越哪些層。[pnpm workspaces](https://pnpm.io/workspaces)

**替代方案**： 分開 repo 增加契約同步成本；Turbo/Nx 暫無建置圖或遠端快取需要；共用所有 business code 會讓瀏覽器依賴後端責任；獨立 UI/config packages 等實際重複出現再建立。

## R02 — 版本基線與相容性

**決策**： Node 採 24 LTS，React 採查核當日最新 stable；TypeScript 採 lint 工具正式支援的 6.0.3。實作開始重查 patch，固定 `.node-version`、根 `packageManager`、精確依賴及 `pnpm-lock.yaml`，CI 使用 frozen lockfile。

| 依賴                               | 2026-10-08 查核基線   | 用途                     |
| ---------------------------------- | --------------------- | ------------------------ |
| Node.js / pnpm                     | 24.21.0 LTS / 12.10.1 | runtime／workspace       |
| React / React DOM                  | 19.3.0 / 19.3.0       | 最新 stable UI           |
| Vite / React plugin                | 8.3.3 / 6.1.2         | SPA 開發與建置           |
| TypeScript                         | 6.0.3                 | 前後端 strict            |
| ESLint / typescript-eslint         | 10.12.0 / 8.71.1      | 支援 TS 6.0 的 lint 組合 |
| React Hooks / Refresh lint         | 7.1.1 / 0.5.7         | React 使用規則           |
| React Router                       | 8.4.0                 | Data Mode 路由           |
| TanStack Query                     | 5.104.1               | 伺服器狀態               |
| React Hook Form / resolvers        | 7.89.0 / 5.9.1        | 表單與 schema 回饋       |
| Zod                                | 4.6.5                 | 共用執行階段契約         |
| Tailwind / Vite plugin             | 4.3.3 / 4.3.3         | 中央樣式 token           |
| Fastify                            | 5.12.5                | Node API                 |
| Firebase web SDK / Admin SDK       | 13.0.0 / 14.5.0       | Auth／後端 Firebase 存取 |
| PDFKit                             | 0.19.1                | 繁中 PDF                 |
| Vitest / jsdom                     | 5.0.3 / 30.1.2        | 單元及 UI 驗證           |
| Testing Library React / user-event | 16.3.3 / 14.6.7       | 使用者操作測試           |
| Testing Library DOM / jest-dom     | 10.4.2 / 7.0.1        | DOM queries／assertions  |
| Playwright                         | 1.64.0                | 跨層與瀏覽器驗證         |

**理由與來源**： Node 24 滿足 Vite、Router、Admin SDK、jsdom 的 engines。TypeScript 當日 latest 是 7.0.2，但 typescript-eslint 支援範圍仍為 `<6.1.0`，因此不採 TS 7；憲章要求最新 stable 的是 React。[Node 24 LTS](https://nodejs.org/en/blog/release/v24.21.0)、[React versions](https://react.dev/versions)、[typescript-eslint 支援範圍](https://typescript-eslint.io/users/dependency-versions/)

版本與 peer/engines 依官方 registry 核對：[pnpm](https://registry.npmjs.org/pnpm/latest)、[Vite](https://registry.npmjs.org/vite/latest)、[React plugin](https://registry.npmjs.org/@vitejs%2fplugin-react/latest)、[Router](https://registry.npmjs.org/react-router/latest)、[Query](https://registry.npmjs.org/@tanstack%2freact-query/latest)、[RHF](https://registry.npmjs.org/react-hook-form/latest)、[resolvers](https://registry.npmjs.org/@hookform%2fresolvers/latest)、[Zod](https://registry.npmjs.org/zod/latest)、[Tailwind](https://registry.npmjs.org/tailwindcss/latest)、[Vitest](https://registry.npmjs.org/vitest/latest)、[jsdom](https://registry.npmjs.org/jsdom/latest)、[Playwright](https://registry.npmjs.org/@playwright%2ftest/latest)。後端依 [Fastify releases](https://github.com/fastify/fastify/releases)、[Firebase web release notes](https://firebase.google.com/support/release-notes/js)、[Admin release notes](https://firebase.google.com/support/release-notes/admin/node)、[PDFKit releases](https://github.com/foliojs/pdfkit/releases)。

**替代方案**： 不用 `latest` 浮動安裝；不用尚非 LTS 的 Node Current；不把 metadata 相容宣稱為已通過實際測試。

## R03 — 前端狀態與資料流

**決策**： Vite SPA + React Router Data Mode；TanStack Query 是唯一伺服器資料快取 owner，loader 若需要資料則委派 QueryClient。RHF 管理表單，URL 管理月份與篩選，component state 管理視窗及附件縮放。HTTP transport 使用原生 fetch、AbortSignal、Firebase token、response schema 與集中 error mapping。

**理由與來源**： 確認、計薪、審核都需要讀到當下後端真值。路由 lazy loading 控制首次載入；不以 Effect 同步可推導資料，不把事件處理放 Effect。[Router modes](https://reactrouter.com/start/modes)、[React Effects 原則](https://react.dev/learn/you-might-not-need-an-effect)、[Query defaults](https://tanstack.com/query/latest/docs/framework/react/guides/important-defaults)

Query keys 包含環境及 principal/session revision。登出、降權或切換 demo persona 時先遮蔽授權畫面、取消請求、清掉 query/mutation cache 與 Blob URLs，再建立新 QueryClient；舊 callbacks 不可寫入新 session。401/403 不自動 retry，mutation 不自動 retry；金額及核准結果不做 optimistic update，成功後依 feature 明確 invalidate。[Query cancellation](https://tanstack.com/query/latest/docs/framework/react/guides/query-cancellation)、[QueryClient](https://tanstack.com/query/latest/docs/framework/react/reference/classes/QueryClient)

Tailwind 主題及實際共用 UI primitives 集中在 web；驗證行政桌面與員工手機。採現代 Chrome/Edge/Firefox/Safari，最低支援範圍跟隨 Tailwind 4。[Tailwind browser support](https://tailwindcss.com/docs/compatibility)

**替代方案**： Next/SSR 無目前內部工具需求；Redux/Zustand、Axios、SWR、Formik、完整 UI framework 尚無必要。Vercel React skill 僅採適用 SPA 的平行資料讀取、lazy import、state/Effect 原則，Next/RSC 特有規則不套用。

## R04 — 後端框架與 PostgreSQL 邊界

**決策**： Fastify 5 模組化單體，feature 的 routes/application/domain 分層；業務與平台 metadata 採 PostgreSQL 18／Neon。Firebase Auth、私有 Cloud Storage、Hosting 沿用；browser 只 Auth SDK，資料／檔案經 Node API。使用 pg＋SQL migrations，詳 R12；不新增 ORM／微服務或泛用 BaseRepository。

**理由與來源**： Fastify validation／serialization／inject 搭配 strict schemas，資料庫替換不要求重寫安全 DTO。兩人公司選 SQL 是維護與原生關聯約束決策，不是 Firestore 容量不足的量測結論。遷移應先覆蓋平台資料及既有業務行為，再擴展新功能。

**替代方案**： 延續 Firestore 最少即時成本；MongoDB 原生 unique／transaction 可用，但仍需協調生效區間及跨來源；使用者已選 Neon/PostgreSQL。保留 Firebase Auth/Storage 可縮小遷移面。資料存取規劃使用 DB roles／scope FK／API 授權，Storage Rules 維持 deny-all。

## R05 — 正式身分與立即撤銷

**決策**： 正式環境 Google 登入，Firebase SDK 明確設 `browserSessionPersistence`；demo 採 `inMemoryPersistence` + anonymous Auth。SDK token persistence 之外，任何 HR cache、草稿及文件皆不寫 browser storage。API `verifyIdToken(token, true)`，再查 authoritative membership。敏感寫入的 transaction 再讀授權版本。

**理由與來源**： Google 登入只證明身分，不代表有角色。未綁定者不取得業務資料；首次 owner 走私有初始化，禁止第一個登入者自動升級。停用或解綁先使 membership 失效，再由可重試工作撤銷 Auth refresh tokens；Auth 與 PostgreSQL 不宣稱跨產品原子。[Persistence](https://firebase.google.com/docs/auth/web/auth-state-persistence)、[Token verification](https://firebase.google.com/docs/auth/admin/verify-id-tokens)、[Revocation](https://firebase.google.com/docs/auth/admin/manage-sessions)

**替代方案**： HttpOnly server session 可行，但需 exchange、CSRF 及 cookie 路由策略；只驗 JWT 或只信 custom claims 無法即時反映解綁。Auth 服務失敗回 503 並拒絕存取，不能略過撤銷查核。

## R06 — 一致性、計算與文件版本

**決策**： 純 domain functions 輸入明確 snapshot 與日期，無 DB/Firebase/時鐘副作用。公司來源修改在同一 transaction 遞增 `payrollSourceRevision`；計算完成、提交、核准核對來源及規則 revision。每員工一份 immutable statement；批次分批準備後驗完整 manifest，小 transaction 切換完整 run pointer。mutable aggregate 有 version，命令有 caller-scoped idempotency key/request hash。

**理由與來源**： 只讀已存在來源會漏掉新請假；snapshot 不能自行防止過期核准。SQL 命令重跑必須明確 rollback/retry，所有 queries 在同一 client；外部 PDF/Storage 留在 transaction 外，分批 commits 不當作整批原子。[PostgreSQL isolation](https://www.postgresql.org/docs/18/transaction-iso.html)、[pg transactions](https://node-postgres.com/features/transactions)

公司來源 revision 是首版保守失效邊界：不相關員工的修改也可能要求未核准批次重算，以小公司資料量換取容易驗證的正確性。已核准內容保持不變；後續影響另建更正或追補。付款年度累計另有 employee/year revision，確認實際給付前再核對。

**替代方案**： 單一巨型 JSONB 增加驗證／讀寫成本；設定葉節點投影需與 immutable payload 分開；只 disable button 不足避免重複；不採 event sourcing/CQRS，採有限狀態、immutable revisions、必要 ledger 與 audit。

## R07 — 日期、精度與台灣規則

**決策**： date-only、YearMonth、Asia/Taipei local work intervals 與 UTC audit instant 分開。金額 `amountTwd` 用安全整數；運算內部 BigInt 有理數，跨 JSON/PostgreSQL 用約分後 `{numerator:string, denominator:string}`。實際請假為整數分鐘，各日正常工時固定於確認時 snapshot。

**理由與來源**： 法定權利期間與曆年交集採含首尾日的實際日數；年度 raw 合計最後進位一次。法定 entitlement 是唯一權利來源，跨年 allocations 引用它，不能再把法定台帳與曆年餘額相加。離職按已取得權利扣已用、已結算及較優已約定量核對差額。[勞動部曆年與離職說明](https://www.mol.gov.tw/1607/28690/2282/2284/2292/81173/)

保費分制度保存官方版本與取整策略；不足月勞保採 30 日制，健保採有效月末所屬單位。高額獎金當次費基採 `min(當次獎金, max(0, 含當次全年累計 − 給付時投保金額×4), 10000000)`，再套有效費率及官方取整。依實際給付 ledger 計算，發布不等於已付；投保額變動時不能用兩次累計超額差代替。[勞保不足月](https://www.bli.gov.tw/0014162.htm)、[健保公式](https://www.nhi.gov.tw/ch/cp-4516-74b0f-2613-1.html)、[獎金案例](https://www.nhi.gov.tw/ch/cp-2489-1e757-3151-1.html)

**替代方案**： binary float、固定 365、逐段/日薪提前取整、把所有假別扣特休、用照片推算出勤均不符合 spec。官方規則作版本化資料與具來源 golden cases，不靠舊實領反推費率。

## R08 — 附件與 PDF

**決策**： API multipart stream，單檔 10 MiB 上限，JPG/PNG/PDF 的 MIME、magic bytes、涵蓋年月及員工驗證；私有 object key 由後端產生。Storage 與 metadata 採 uploading/ready/failed 狀態及 orphan cleanup。下載／預覽經授權 API stream，`Cache-Control: private, no-store`。PDFKit + 具 OFL 授權的 Noto Sans CJK TC 字型，兩類獨立模板，來源為固定 snapshot。

**理由與來源**： signed URL 持有人在期限內可讀，不能滿足解綁後每次重驗；不回 public URL/Firebase download token。PDF 固定 template/font/output hash，生成失敗可重試，發布前所有必要文件 ready。[Signed URLs](https://docs.cloud.google.com/storage/docs/access-control/signed-urls)、[PDFKit fonts](https://pdfkit.org/docs/text.html)、[Noto CJK license](https://github.com/notofonts/noto-cjk/blob/main/Sans/LICENSE)

**替代方案**： HTML→Chromium 對複雜版型有優勢，首版明細表不需 headless runtime；附件不 OCR。字型檔及 license 可公開，員工原始附件不可入 repo。

## R09 — Demo 隔離、重設與清理

**決策**： demo 使用獨立 Firebase/GCP 與 Neon projects、database roles、bucket、API 與 service accounts。anonymous 訪客每人工作區，後端 persona 對應預設三角色及固定 employee scope。可改日期、金額及月結流程；身分、事由、備註等自由文字入口改合成模板／理由代碼，禁止任意上傳、匯入、帳號綁定及對外傳送。

**理由與來源**： 三組共用密碼會讓訪客互相覆寫；只有 seed 合成卻允許任意姓名/備註/檔案，無法維持 demo 只含合成資料。每次請求檢查 owner、當前 workspace/generation/session revision、persona scope、status 及 24h expiry。reset 完整播種新 generation 後原子切換 pointer，舊請求回 `WORKSPACE_REPLACED`。[Anonymous Auth](https://firebase.google.com/docs/auth/web/anonymous-auth)

每 15 分鐘獨立 cleanup job 處理到期／retired workspace，工作區外 manifest 保存 SQL scope／待刪 tables／Storage prefix 與進度，分批刪 rows 與 objects，失敗重試。24h 到期立即拒絕授權；不依賴 Firestore TTL。demo bucket 不啟用 versioning/soft delete，正式保留設定分開。[Storage soft delete](https://docs.cloud.google.com/storage/docs/soft-delete)

專案初始限額：每工作區 10 員工、2 月、每業務類型 100 筆、總業務紀錄 500（含 staged output，內部 current 投影不重複計數）；一般 body 64 KiB；UID 60 requests/min、20 mutations/min、5 PDF jobs/min、3 resets/hour；IP 5 bootstraps/hour；全站 100 新 workspace/day。以分散式限額、Cloud Run max instances、停止新工作區及寫入開關控制；超限 429/Retry-After。App Check 保護公開 bootstrap/reset/PDF，emulator 才允許測試 bypass。[App Check custom backend](https://firebase.google.com/docs/app-check/custom-resource-backend)

**替代方案**： 只讀 demo 缺少完整月結；同正式 Neon project/DB 的 demo 分支或欄位隔離增加誤連與正式資料複製風險。Identity Platform 的匿名帳號自動清理至少 30 天，不能代替 workspace 24h 邊界。限額是本案政策，需用 cloud smoke/load 證據調整，非官方最佳預設。

## R10 — 執行環境、驗證與學習

**決策**： Firebase Hosting rewrite Cloud Run，背景工作採 Cloud Tasks IAM worker／Scheduler。Neon DB 候選 Singapore；與 Cloud Run asia-east1 的跨區配置需實測後定案。local/CI 規劃使用隔離 PG18 + Auth/Storage emulators，無 Neon secrets；若仍有 Firestore consumers，完成遷移後才移除相關 emulator/JDK 依賴。Firebase runtime 用 ADC、trusted deploy 用 WIF；Neon secret 使用 Secret Manager/runtime role，DDL／備份不同角色。

**理由與來源**： Hosting→Cloud Run 有 60 秒請求上限，不能回應後 fire-and-forget。PDF、seed/reset/import 有 job status、重試與完成 manifest；本機測試用顯式 local worker。公開 cloud demo 需要 billing/Blaze，設定費用及停止開關；本機 emulator 路線不要求雲端部署。[Hosting Cloud Run](https://firebase.google.com/docs/hosting/cloud-run)、[Cloud Run background activity](https://docs.cloud.google.com/run/docs/tips/general#avoiding_background_activities)、[Cloud Tasks HTTP](https://docs.cloud.google.com/tasks/docs/creating-http-target-tasks)、[Storage billing](https://firebase.google.com/docs/storage/faqs-storage-changes-announced-sept-2024)、[WIF](https://docs.cloud.google.com/iam/docs/workload-identity-federation-with-deployment-pipelines)

依風險驗證純計算／契約、Testing Library 表單、Fastify inject + PostgreSQL/Auth/Storage、Storage deny-all Rules、Playwright 三角色月結與越權。獨立 `tsc`，不以 Vite build 代替。local PostgreSQL/emulators 不模擬 Neon pooling／休眠／還原、IAM、bucket 保留等，因此公開 demo 上線前另做隔離 cloud smoke。[Auth emulators](https://firebase.google.com/docs/emulator-suite/connect_auth)、[Storage emulator limitations](https://firebase.google.com/docs/emulator-suite/connect_storage)、[Playwright assertions](https://playwright.dev/docs/test-assertions)

**替代方案**： 真 Neon/Firebase 作公開 PR 測試需憑證且可能污染資料；完整效能/無障礙稽核依憲章延後，但表單 label、鍵盤操作、結果回饋仍落實。學習依垂直功能逐次增加短筆記，說明問題、選擇、資料流及驗證，不另寫泛用教材或大量逐行註解。

## R11 — Impeccable 與 CHONG JU 品牌色系

**決策**： 依使用者指定使用 [Impeccable](https://github.com/pbakaus/impeccable) 的 project-local Codex skill。2026-10-08 查閱的 skill 為 4.5.0，來源 commit `778c8a7b71ccd5bfe3ca6ac68c15d9d872d0f87d`；安裝時固定經驗證的發行包／來源及 license，保留既有 Spec Kit skills。安裝、hooks 設定與 UI 設計列入後續規劃。

**理由與來源**： [官方skill](https://github.com/pbakaus/impeccable/blob/778c8a7b71ccd5bfe3ca6ac68c15d9d872d0f87d/.agents/skills/impeccable/SKILL.md) 以Operate模式處理app/admin工具，操作清楚、資訊掃描和一致性優先；`init`記錄穩定產品脈絡、`shape`先產出brief而非code，確認後再建立設計及樣板，`critique`/`adapt`/`harden`處理實際畫面，最後集中`polish`。既有spec已回答角色/目的/流程/技術，不能重新要求完整需求訪談。

使用者 [Logo](https://www.postmall.com.tw/uploads/shop_logo/3588/a4906d87-472f-4a90-bdc9-91265f464fdb.png) 的珊瑚橘、湖水藍、嫩綠、亮黃、淡粉底作品牌色；圖片觀察色票與語意用法需在後續 UI 契約中定義。深色文字及明確狀態文案另定，保留繁中／TWD／金額與公司行政桌面、員工手機的操作需求。

**替代方案**： 只在最終polish套配色會讓前期各頁自行長出不同樣式，故設計brief/tokens/合成樣板列在foundation、所有story沿用；全面效能/無障礙稽核維持憲章延後，本次設計選擇不新增此gate。

## R12 — PostgreSQL、driver、migrations 與原生約束

**決策**： Neon 支援 PG18，新環境固定 PostgreSQL 18 major；local/CI 相同 major。2026-10-10 官方 npm registry 查核 pg **8.23.1**（Node >=16）、node-pg-migrate **9.0.0**（Node >=20.11、pg <9）與 Node24 基線相容；實作時固定 @types/pg／精確 lockfile，驗 ESM/TS 與實際 migration，不使用 alpha 文件中的未發布功能。SQL migration 檔放 database/migrations，以工具原生排序／migration history／locking 執行，無自製 migration engine。

**理由與來源**： Fastify/Cloud Run 是長存活 Node runtime，pg 支援讀後判斷再寫的 interactive transaction；同一 checked-out client 執行 BEGIN/COMMIT/ROLLBACK。[pg transactions](https://node-postgres.com/features/transactions)、[Neon version compatibility](https://neon.com/docs/reference/compatibility)、[node-pg-migrate](https://salsita.github.io/node-pg-migrate/)、[pg metadata](https://registry.npmjs.org/pg/latest)、[migration tool metadata](https://registry.npmjs.org/node-pg-migrate/latest)

UNIQUE／composite FK／CHECK 管理工號、綁定、來源及 scope；btree_gist + daterange exclusion 管理四張 current 表的現行區間。immutable 歷史與原更正版可以重疊，不能把 exclusion 套整張歷史表。SQL row locks 保護 ETag、授權、餘額及 pointer，既有 domain 保留鏈末端／非法更正規則。[PostgreSQL constraints](https://www.postgresql.org/docs/18/ddl-constraints.html)、[range constraints](https://www.postgresql.org/docs/18/rangetypes.html#RANGETYPES-CONSTRAINT)、[Neon extensions](https://neon.com/docs/extensions/pg-extensions)

**替代方案**： Prisma/Drizzle 可降低部分查詢手寫量，但本案仍需理解 exclusion、鎖序、FK、交易及 migrations；先用 feature-local SQL adapter 與 Zod row mapping，減少新增概念。Neon HTTP driver transaction 適合預先固定 queries，本案命令需互動讀寫；改用其 WebSocket Pool 可行但此 Node runtime 無必要。不得以 driver 替換讓 domain import SQL types。[Neon driver](https://neon.com/docs/serverless/serverless-driver)

## R13 — Neon pooling、休眠與地域

**決策**： API／worker 使用 TLS pooled URL、小型 pg Pool；migration/pg_dump/restore 用 direct URL。runtime 不依賴 session SET、LISTEN、session advisory locks；liveness /health 不查 DB，部署／受控 readiness 才測 DB。候選 DB AWS Singapore，與原 API Taiwan 需跨區量測。[Neon pooling](https://neon.com/docs/connect/connection-pooling)、[regions](https://neon.com/docs/introduction/regions)

**理由與來源**： PgBouncer transaction mode 支援同一 BEGIN–COMMIT 交易，結束後釋放 server connection；下一次 transaction 不能假設 session 狀態。Neon Free 約 5 分鐘 idle 後休眠，下次 query 喚醒，冷連線逾時及斷線恢復需明確驗證。region 建立後不能直接改。常態探測或 cleanup SQL 會增加活躍時間，不能以兩人公司推定用量。[Scale to zero](https://neon.com/docs/introduction/scale-to-zero)、[connection latency](https://neon.com/docs/connect/connection-latency)

**替代方案**： 直接 runtime URL 可用但 instances 增加容易耗用連線；付費禁休眠會增加 compute，首版依實測需要決定。為「保持醒著」加 ping job 沒有目前需求。移 API region 需核 Hosting 支援與其他 GCP 成本，不先改部署配置。

## R14 — 成本、備份及遷移

**決策**： 合成 dev/demo 可以 Neon Free 起步；正式資料導入前先完成 backup/restore、DB roles、region 與費用評估。2026-10-10 官方 Free 額度為每 project 1 GB、100 CU-hours/月、6h history window；沒有將短期 history 宣稱五年備份。公司目前兩名員工，容量預估較小，但 Cloud Run/Storage/egress/backup 分別計費。[Neon plans](https://neon.com/docs/introduction/plans)、[Free storage update](https://neon.com/blog/neon-free-plan-1-gb-per-project)、[Firebase Storage billing](https://firebase.google.com/docs/storage/faqs-storage-changes-announced-sept-2024)

**理由與來源**： 已核准薪資、來源及 audit 至少保存五年；備份目標是可恢復與明確頻率／保存期限，不能用供應商免費容量代替。若已有正式資料，先隔離演練、核 IDs/UIDs/generations/chains/hashes，停止寫入後單次切換；若只有合成資料，重新 seed。詳細切換與回復界線須在後續遷移規劃中定義。

**替代方案**： 同時 Firestore/PG 雙寫會新增一致性責任；直接刪舊資料／假設無正式資料無驗證依據。Neon branch 不作公開 demo 的正式資料副本，正式與 demo 不同 project/role。

## 研究結論與待驗證事項

規劃採 pnpm monorepo、React SPA、Fastify 模組化單體及 PostgreSQL／Neon，搭配 Firebase Auth／Storage／Hosting。研究選型與來源可作為後續技術規劃的輸入；metadata 相容性與文件說明仍須透過實作驗證。

- **版本與工具相容性（R02）**：實作開始前重查 patch、engines 與 peerDependencies，固定精確依賴及 lockfile；確認 TypeScript／lint 組合與 Chromium／Firefox／WebKit 在開發環境及 CI 的實際相容性。
- **資料庫與一致性（R04、R06、R12）**：驗證乾淨建庫、升版與失敗 rollback、runtime permissions、同 key／ETag／區間／撤權競態、audit 失敗 rollback，以及 date／bigint 映射與既有 domain 行為。ESM／TypeScript、migration runner 與同 client 交易須以實際環境驗證。
- **Neon 與雲端環境（R10、R13）**：確認 pooled／direct URL 分工、TLS、休眠喚醒、冷連線逾時與斷線恢復；量測 Singapore 資料庫與 Cloud Run 候選地域的跨區延遲，再定部署地域。本機 PostgreSQL 與 emulators 不代替雲端驗證。
- **備份、遷移與成本（R14）**：正式資料導入前完成還原演練、保存期限、DB roles、切換及回復規劃，並依當時的方案與實際用量評估費用；免費額度與短期 history 不代替五年保存需求。
- **正式資料與公開 demo（R05、R07、R09、R10）**：正式 Google 綁定、投保與歷史資料缺口仍須依 spec 補齊，才可進入正式計薪。公開 demo 上線前驗證角色與工作區隔離、重設與清理、限額及停止開關；初始限額須依隔離 cloud smoke／load 結果調整。
