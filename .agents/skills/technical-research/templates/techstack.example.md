# 技術棧總覽：崇鉅生技人資與薪資管理系統

## 技術選用總覽

### 開發基礎與共用契約

| 項目 | 採用技術與版本基線 | 研究依據 |
| --- | --- | --- |
| 執行環境 | Node.js 24.21.0 LTS | [R02](./research.example.md#r02--版本基線與相容性) |
| 開發語言 | TypeScript 6.0.3 | [R02](./research.example.md#r02--版本基線與相容性) |
| 套件與 workspace | pnpm 12.10.1／workspaces | [R01](./research.example.md#r01--monorepo-與責任邊界)、[R02](./research.example.md#r02--版本基線與相容性) |
| 執行階段契約 | Zod 4.6.5 | [R01](./research.example.md#r01--monorepo-與責任邊界)、[R02](./research.example.md#r02--版本基線與相容性)、[R12](./research.example.md#r12--postgresqldrivermigrations-與原生約束) |

### 前端應用

| 項目 | 採用技術與版本基線 | 研究依據 |
| --- | --- | --- |
| 前端介面 | React／React DOM 19.3.0 | [R02](./research.example.md#r02--版本基線與相容性)、[R03](./research.example.md#r03--前端狀態與資料流) |
| 前端建置 | Vite 8.3.3／React plugin 6.1.2 | [R02](./research.example.md#r02--版本基線與相容性)、[R03](./research.example.md#r03--前端狀態與資料流) |
| 路由 | React Router 8.4.0，Data Mode | [R02](./research.example.md#r02--版本基線與相容性)、[R03](./research.example.md#r03--前端狀態與資料流) |
| 伺服器狀態 | TanStack Query 5.104.1 | [R02](./research.example.md#r02--版本基線與相容性)、[R03](./research.example.md#r03--前端狀態與資料流) |
| 表單 | React Hook Form 7.89.0／resolvers 5.9.1 | [R02](./research.example.md#r02--版本基線與相容性)、[R03](./research.example.md#r03--前端狀態與資料流) |
| 樣式 | Tailwind CSS／Vite plugin 4.3.3 | [R02](./research.example.md#r02--版本基線與相容性)、[R03](./research.example.md#r03--前端狀態與資料流) |
| HTTP transport | 原生 fetch／AbortSignal | [R03](./research.example.md#r03--前端狀態與資料流) |

### 後端與資料庫

| 項目 | 採用技術與版本基線 | 研究依據 |
| --- | --- | --- |
| 後端框架 | Fastify 5.12.5 | [R02](./research.example.md#r02--版本基線與相容性)、[R04](./research.example.md#r04--後端框架與-postgresql-邊界)、[R10](./research.example.md#r10--執行環境驗證與學習) |
| 資料庫與代管 | PostgreSQL 18／Neon | [R04](./research.example.md#r04--後端框架與-postgresql-邊界)、[R12](./research.example.md#r12--postgresqldrivermigrations-與原生約束) |
| 資料庫 driver | pg 8.23.1；@types/pg 版本待固定 | [R12](./research.example.md#r12--postgresqldrivermigrations-與原生約束)、[R13](./research.example.md#r13--neon-pooling休眠與地域) |
| 資料庫約束 | UNIQUE／FK／CHECK、btree_gist／daterange exclusion | [R12](./research.example.md#r12--postgresqldrivermigrations-與原生約束) |
| Schema migrations | node-pg-migrate 9.0.0／SQL migration 檔 | [R12](./research.example.md#r12--postgresqldrivermigrations-與原生約束) |

### 身分、安全與檔案

| 項目 | 採用技術與版本基線 | 研究依據 |
| --- | --- | --- |
| 身分服務與 SDK | Firebase Auth；web SDK 13.0.0／Admin SDK 14.5.0 | [R02](./research.example.md#r02--版本基線與相容性)、[R04](./research.example.md#r04--後端框架與-postgresql-邊界)、[R05](./research.example.md#r05--正式身分與立即撤銷) |
| 公開端點保護 | Firebase App Check | [R09](./research.example.md#r09--demo-隔離重設與清理) |
| 檔案儲存 | 私有 Cloud Storage | [R04](./research.example.md#r04--後端框架與-postgresql-邊界)、[R08](./research.example.md#r08--附件與-pdf) |
| PDF 與字型 | PDFKit 0.19.1／Noto Sans CJK TC | [R02](./research.example.md#r02--版本基線與相容性)、[R08](./research.example.md#r08--附件與-pdf) |

### 雲端部署與維運

| 項目 | 採用技術與版本基線 | 研究依據 |
| --- | --- | --- |
| 前端託管與 API 部署 | Firebase Hosting／Cloud Run | [R04](./research.example.md#r04--後端框架與-postgresql-邊界)、[R10](./research.example.md#r10--執行環境驗證與學習) |
| 背景工作與排程 | Cloud Tasks／IAM worker／Scheduler | [R09](./research.example.md#r09--demo-隔離重設與清理)、[R10](./research.example.md#r10--執行環境驗證與學習) |
| 雲端身分與秘密管理 | ADC／WIF／Secret Manager | [R10](./research.example.md#r10--執行環境驗證與學習) |

### 品質驗證與設計輔助

| 項目 | 採用技術與版本基線 | 研究依據 |
| --- | --- | --- |
| 靜態檢查 | ESLint 10.12.0／typescript-eslint 8.71.1；React Hooks 7.1.1／Refresh 0.5.7 | [R02](./research.example.md#r02--版本基線與相容性)、[R10](./research.example.md#r10--執行環境驗證與學習) |
| 單元與 UI 驗證環境 | Vitest 5.0.3／jsdom 30.1.2 | [R02](./research.example.md#r02--版本基線與相容性)、[R10](./research.example.md#r10--執行環境驗證與學習) |
| 使用者操作測試 | Testing Library React 16.3.3／user-event 14.6.7／DOM 10.4.2／jest-dom 7.0.1 | [R02](./research.example.md#r02--版本基線與相容性)、[R10](./research.example.md#r10--執行環境驗證與學習) |
| 瀏覽器與跨層驗證 | Playwright 1.64.0 | [R02](./research.example.md#r02--版本基線與相容性)、[R10](./research.example.md#r10--執行環境驗證與學習) |
| 本機與 CI 整合環境 | 隔離 PostgreSQL 18／Auth 與 Storage emulators | [R10](./research.example.md#r10--執行環境驗證與學習)、[R12](./research.example.md#r12--postgresqldrivermigrations-與原生約束) |
| 設計輔助 | Impeccable 4.5.0，來源 commit 見研究 | [R11](./research.example.md#r11--impeccable-與-chong-ju-品牌色系) |

## 待定事項

| 項目 | 已有採用基線 | 仍待確定的內容 | 研究依據 |
| --- | --- | --- | --- |
| 精確依賴與環境相容性 | 採上表的研究版本組合 | 實作開始前重查 patch、engines、peerDependencies 與瀏覽器支援；固定 @types/pg、精確依賴及 lockfile，確認實際開發／CI 組合 | [R02](./research.example.md#r02--版本基線與相容性)、[R10](./research.example.md#r10--執行環境驗證與學習)、[R12](./research.example.md#r12--postgresqldrivermigrations-與原生約束) |
| 雲端地域與連線配置 | 採用 Neon 與 Cloud Run；Neon 候選 AWS Singapore，Cloud Run 候選 asia-east1 | 跨區延遲、冷連線、逾時及斷線恢復的驗證結果，以及據此選定的地域與配置 | [R10](./research.example.md#r10--執行環境驗證與學習)、[R13](./research.example.md#r13--neon-pooling休眠與地域) |
| 正式環境方案、備份與成本 | 合成 dev／demo 可由 Neon Free 起步 | 正式方案、備份頻率與保存期限、還原機制及實際費用；須符合研究所記載的五年保存需求 | [R14](./research.example.md#r14--成本備份及遷移) |
