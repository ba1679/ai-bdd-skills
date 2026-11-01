# 視覺研究能力接入

只在既有視覺依據不足、需要取得候選時讀取本檔。原型與檢查由 UI 主流程承擔，外部研究不改變 HTML＋短 Markdown 的交付邊界。

從目前可用 skill 清單解析 `ui-ux-pro-max` 的實際位置並讀取其 `SKILL.md`，依該版本使用 `<skill-base>/scripts/search.py`；不假設一定安裝在本專案相鄰目錄。常見入口：

```bash
python3 "<skill-base>/scripts/search.py" "<本輪產品與視覺問題>" --design-system
python3 "<skill-base>/scripts/search.py" "<局部問題>" --domain <domain>
```

新方向採 design-system，局部問題採對應 domain；實際參數以已讀版本為準。只取候選時採不持久化查詢，避免另外生成視覺主檔、管理報告或相互競爭的規範。採用方向與必要理由放入 UI 短紀錄，字形、對比與配置仍以實際 HTML 檢查。

已有設計系統或 Pencil 等畫布可引用為畫面來源；解析既有內容只用當前實際可用的工具與文件，不預設 MCP／CLI 名稱、登入或可取得的節點。來源不可取得時保留缺口及局部影響，能由已有需求完成的原型部分繼續；不為取得新畫布阻塞可直接製作的 HTML。
