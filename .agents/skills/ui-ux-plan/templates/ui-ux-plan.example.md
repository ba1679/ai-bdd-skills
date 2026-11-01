# 玩家體驗紀錄：PVP 1A2B

主要交付為 [互動 prototype](prototype.example.html)：直接以瀏覽器開啟，無須安裝、建置或服務。本文只保留畫面意圖、契約對照及實作注意事項。依據：[需求](../../../../specs/online-pvp-1a2b/spec.md) US-001～US-003、[計畫](../../system-analysis/templates/plan.example.md) 共同約束、[研究](../../../../specs/online-pvp-1a2b/research.md) R02～R04。

## 原型怎麼看

- 自由操作：輸入四位配對碼建立房間，展開「原型評審工具」讓另一位加入／準備；也可切換 A、B 視角，分別輸入自己的答案與猜測。
- 流程導覽：選「主要流程」「格式與業務拒絕」「未知結果與重連」，依序按導覽步驟。每次選情境會重置假資料。
- 畫面以玩家任務為主；雙玩家切換、回覆遺失、斷線、舊回覆及重置都只屬評審工具，不是正式玩家功能。
- 純記憶體假資料；沒有 Auth、網路、資料庫或跨分頁同步。工具「重載房間」只模擬同一 session 的重新讀取，真正重新整理 HTML 會重置。正式同瀏覽器刷新恢復仍須實作及驗證。

## 畫面與操作

| 畫面 | 呈現與可用操作 |
| --- | --- |
| 配對入口 | 四位配對碼，允許首位 0／重複數字；建立／加入後顯示房間及本人角色。 |
| 等候／準備 | 配對碼、兩人容量與 readiness；滿兩人後提交本人答案。接受後清除輸入，只顯示「已準備」。 |
| 開始 | 僅房主顯示開始按鈕，兩人及兩份有效答案齊備才啟用；其他情況有文字說明。 |
| 自由猜測 | 四位不重複、首位非 0；不分回合，可重複猜相同數字。只顯示自己的猜測與服務 A／B。 |
| 結束 | caller 相對勝負、本人紀錄；停用猜測，不提供重開局／答案揭露／對手活動。 |

## 非同步與恢復

| 情境 | 玩家可見處理 |
| --- | --- |
| 載入／送出中 | 明確文字狀態，暫停重複提交；格式錯誤就地說明並將焦點移至欄位。 |
| 確定拒絕 | 顯示安全原因，不先新增本地紀錄；有 snapshot 時接受其權威狀態。 |
| 結果未知 | 保留原 commandId／operation／body，停用新命令，提供「確認這次操作」重送同一筆；GET 不能單獨證明該命令是否生效。 |
| stale／斷線／讀取失敗 | 保留舊畫面及未提交輸入，標示可能過期並暫停 mutation；重新授權訂閱後 GET，同步成功才恢復。未知命令仍須另外確認。 |
| 舊回覆／通知 | 同房 stateVersion 用 BigInt 比較，不倒退；通知只觸發讀取，不能新增猜測或裁決勝負。 |
| session／讀取拒絕 | 正式實作須處理 401 session 恢復、opaque 404 清除 locator、429／503 退避；prototype 僅示範斷線與一般讀取失敗，未模擬完整 Auth／限流。 |

## 開發者交接

- [HTTP](../../api-plan/templates/openapi.example.yaml)：四個 POST 與 `GET /get-room-snapshot?roomId=...` 共用 `RoomSnapshot`。讀取 `isCallerHost`、`readiness.callerReady/opponentReady`、`callerResult`、`callerGuesses`；`requestId` 是每次診斷碼，`commandId` 才是重送識別。
- [Realtime 契約](../../api-plan/templates/openapi.example.yaml) 的 `x-realtime`：private join 與重連後 GET；新通知合併讀取，較低於通知版本的回覆仍 stale。原型以記憶體快照模擬這段結果，不包含真實 WebSocket／重試排程。
- [Data](../../data-plan/templates/schema.example.dbml)：所有權限與投影由服務強制；隱藏按鈕不是授權。正式前端不讀取對手答案；原型的假服務為了可離線示範才在同一檔持有兩方假資料，不構成安全驗證。
- 匿名 session 未就緒前不可讀寫；保留 server-issued roomId 以恢復指定房間，不假設一 UID 只有一個房間。刷新後持久化與未知命令恢復策略須在正式實作完成。
- 版面以桌面雙欄、窄畫面直向排列；文字與色彩共同提示狀態。表單 label／描述、鍵盤操作、可見焦點、live status 與至少 44px 控制尺寸須保留；實際讀屏與雙瀏覽器驗證留待實作。

未決集中於 [plan.md](../../system-analysis/templates/plan.example.md)：U1 不採用錯誤算例；U2 不新增 TTL／離開／自動轉移；U5/U6 目前不揭露答案與對手猜測。waiting 答案修訂尚未確認，原型只提供首次提交；示範工具的固定答案是 A=`1234`、B=`5678`，不代表產品預設值。既有視覺稿可作來源；本例只以 HTML 承載可操作原型，不另交畫布檔或固定預覽圖。

## 本次檢查

以下為認可產物於 2026-11-01 的歷史檢查，作為填寫方式範例；重新套用本模板時須改為當次實際結果，不得沿抄通過結論。

- Redocly CLI 1.34.5 recommended lint 通過（略過本地設計文件未指定的 `info-license`）；範例與 152 個內部引用有效。HTTP／Realtime 版本格式一致，現行 Markdown 連結無失效。
- `@dbml/core` 11.0.0 解析通過：6 tables／8 refs／4 enums；對照原檔，欄位、check、index、Enum、Ref 宣告完全保留，僅精簡 Project／Table Notes。
- 瀏覽器走過三組導覽；另以實際表單檢查格式錯誤焦點、Enter 提交、不同操作重複猜測。確認未知結果回放只有一筆、舊版本不倒退、非房主／終局拒絕，以及重連後取得結束結果。375px 與桌面配置的 DOM 寬度無水平溢出，已檢視畫面；瀏覽器沒有 error／warn。

以上是文件與假資料原型的檢查。真實 Auth／SQL／RLS／WebSocket、雙瀏覽器、併發、讀屏及公開部署均待實作驗證。
