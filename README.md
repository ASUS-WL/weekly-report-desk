# 週報彙整台

貼上 Outlook 郵件、Teams 訊息、GitHub issue/PR 內容,一鍵請 AI 依專案／主題整理成週報草稿,整理完可直接編輯後複製貼到你的週報裡。

這是一個純前端的靜態網頁,沒有後端伺服器。每個使用者用自己的 Anthropic API Key 和 GitHub Personal Access Token 才能使用 AI 整理與 GitHub 自動抓取,兩者都只存在使用者自己瀏覽器的 `localStorage`,不會經過任何其他伺服器,也不會被其他使用者看到。

## 功能

- 依來源(Outlook / Teams / GitHub / 其他)貼上或拖曳文字內容,整理成本週清單
- 按下「整理本週重點」,呼叫 Anthropic Messages API,依專案／主題分類產生條列摘要,可直接編輯後複製
- GitHub 來源可貼上單一 issue/PR 連結自動抓取標題與內容,或一鍵抓取「本週指派給我」的所有 issue/PR
- 每週資料分開儲存在瀏覽器本機,用上方 ‹ › 切換週次

## 使用前設定

點右上角齒輪圖示開啟設定:

1. **Anthropic API Key** — 到 [console.anthropic.com](https://console.anthropic.com/) 的 API Keys 頁面建立一支 key,貼上後按「儲存」。
2. **GitHub Personal Access Token** — 到 GitHub 的 Settings → Developer settings → Personal access tokens 建立。若用 fine-grained token,只需要勾選對應 repo 的 `Issues: Read-only` 與 `Pull requests: Read-only`;若用 classic token,勾選 `repo` 範圍即可。貼上後按「測試並儲存」,驗證成功會顯示你的 GitHub 帳號名稱。

沒有設定 GitHub Token 時,單一連結抓取仍可用於公開 repo(未登入身份、有速率限制);「抓取本週指派給我的動態」一定需要 Token,因為要用你的身份查詢。

## 安全性注意事項

- 這兩個值儲存在瀏覽器的 `localStorage`,只有這台裝置、這個瀏覽器能讀到。
- 頁面程式碼只會把它們直接送到 `api.anthropic.com` 和 `api.github.com`,不會送到其他地方。
- 不建議在公用電腦上輸入;GitHub Token 請只給最小必要權限,並定期更換。
- 這個頁面部署後(例如透過 GitHub Pages)網址本身是公開的,但金鑰不會出現在網頁原始碼裡 —— 每個訪問者都要自己輸入自己的 key/token,才會存在他自己的瀏覽器裡。

## 部署到 GitHub Pages

1. 到 GitHub 建立一個新的空白 repo(不要勾選 Add README,避免衝突),例如叫 `weekly-report-desk`。
2. 在本機這個資料夾(`weekly-report-desk-repo/`,裡面有 `index.html` 和這份 `README.md`)執行:

   ```bash
   cd weekly-report-desk-repo
   git init
   git add .
   git commit -m "Initial commit: weekly report desk"
   git branch -M main
   git remote add origin https://github.com/<你的帳號>/weekly-report-desk.git
   git push -u origin main
   ```

3. 到 repo 的 **Settings → Pages**,Source 選擇 **Deploy from a branch**,Branch 選 `main` 與 `/ (root)`,按 Save。
4. 等 1 分鐘左右,網站會上線在:

   ```
   https://<你的帳號>.github.io/weekly-report-desk/
   ```

5. 之後若要更新網頁,修改 `index.html` 後執行:

   ```bash
   git add .
   git commit -m "更新"
   git push
   ```

   GitHub Pages 會自動重新部署。

## 分享給同事使用

把 GitHub Pages 網址分享出去即可,每個打開網頁的人都要各自到設定裡填入自己的 Anthropic API Key 與 GitHub Token 才能使用 AI 整理與 GitHub 抓取功能;沒有設定的人仍可以手動貼上內容、編輯、複製使用。
