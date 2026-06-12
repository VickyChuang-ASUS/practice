# 🚀 Git & GitHub 實戰特訓筆記

本專案用於記錄從 **SVN 集中式思維** 完美轉化為 **Git 分散式思維** 的核心觀念與實戰指令。專為軟體工程師轉入 Git 新手村所打造的生存指南。

---

## 🔗 核心本質：本地資料夾與 GitHub 的幾何關係

許多 SVN 開發者最常卡關的，就是分不清楚「我電腦裡的資料夾」跟「GitHub 網站」到底是怎麼連線的：

1. **本地資料夾（獨立的密封世界）**
   * **工作區**：肉眼看到、用編輯器修改檔案的地方（未追蹤時顯示紅色）。
   * **本地資料庫**：隱藏的 `.git` 資料夾。它是一個完整獨立的資料庫，儲存了你過去每一次 commit 的「全景快照歷史」，不需要網路即可運作。
2. **GitHub（雲端鏡像庫）**
   * 本質上是另一台長年不關機的雲端電腦。你在 GitHub 上建的 Repository，就是雲端伺服器上的一個資料夾。
3. **兩者的綁定關係**
   * 透過 `git remote add origin <網址>`，在本地大腦拉一條隱形電話線，將雲端網址代號定為 `origin`。
   * `git push`：把本地 `.git` 的快照歷史打包複製「推」上雲端。
   * `git pull`：把雲端資料夾的新進度「拉」回本地。

---

## 🧠 思維模型對照：SVN vs. Git

| 動作 | SVN 指令 | Git 指令 | 核心差異與內部原理說明 |
| :--- | :--- | :--- | :--- |
| **同步最新進度** | `svn update` | `git pull` | 把雲端別人寫好的新快照拉回你電腦並融合。 |
| **標記準備送出** | *(無)* | `git add` | **【Git 獨有暫存區】**把修改過的檔案搬上準備打包的舞台（變綠色）。 |
| **提交變更** | `svn commit` | `git commit` | **僅**提交到你電腦本地的 `.git` 資料庫，完全不需網路。 |
| **推上雲端** | *(自動包含)* | `git push` | 正式把本地大腦的歷史紀錄，沿著電話線推上 GitHub。 |

---

## 🛠️ 從零開始：搭建環境並連上 GitHub 全步驟

1. **全域識別設定**（每台新電腦只需做一次）：
   ```powershell
   git config --global user.name "Your Name"
   git config --global user.email "your_email@example.com"```
   
2. **本地初始化**：
   ```powershell
   cd ~\Desktop
   mkdir git-practice
   cd git-practice
   git init
   ```
3. **身分驗證登入**：
   第一次執行 `git push -u origin main` 時，系統會彈出 **Git Credential Manager** 視窗，點擊 **"Sign in with your browser"** 並在瀏覽器按 **Authorize** 授權，憑證就會安全存入 Windows 憑證管理員，之後免輸入密碼。

---

## 🌐 一鍵入魂：GitHub Pages 靜態網站發布 SOP

### 1. 檔名的神聖慣例：`index.html`
網頁伺服器遵循**「慣例優於設定 (Convention over Configuration)」**。網址在沒有指定具體檔案時，預設會自動開啟 `index.html`。如果使用 `singapore.html`，發布後的網址尾巴就必須手動補上 `/singapore.html`。**建議首頁一律用小寫的 `index.html`**。

### 2. 後台發布流程
1. 將程式碼 `git commit` 並且 `git push` 到 GitHub。
2. 進入該 GitHub 專案網頁，點擊右上方 **Settings** (齒輪圖示)。
3. 在左側選單點擊 **Pages**。
4. 在 **Build and deployment -> Branch** 下，將 `None` 改選為 **`main`** (或 `master`) 分支，並點擊 **Save**。
5. 等待約 60 秒，重新整理該頁面，頂端即會顯示專屬網站網址。

---

## 🪤 新手村終極脫困密技

### 陷阱一：直接執行 `git commit` 忘記加訊息，被抓進 Vim 編輯器？
在充滿波浪符號 `~` 的黑暗畫面中，請冷靜依序按下：
1. 連續按鍵盤左上角的 `Esc` 鍵數次（確保退出編輯模式）。
2. **想瘋狂逃走（不儲存）**：盲打輸入 `:q!`，再按 `Enter`。
3. **想直接存檔離開**：盲打輸入 `:wq`，再按 `Enter`。

### 陷阱二：程式碼 push 上去後反悔了，用 Reset 還是 Revert？
* 核心鐵律：**已經 Push 上雲端的紀錄，一律用 `git revert`！**
* `git reset`（時光倒流）：直接抹殺過去，會導致本地歷史比雲端舊，下次 push 會被雲端無情拒絕。
* `git revert`（往前推進的反向操作）：保留過去，建立一個「全新」的 commit 來抵銷上一次的修改（例如把改掉的檔名再改回來）。歷史線完美，推上雲端絕對不打架。
  ```powershell
  git log --oneline     # 查出想反悔的那筆 Hash（如 a1b2c3d）
  git revert a1b2c3d    # 進入 Vim 後輸入 :wq 儲存離開
  git push              # 順暢推上雲端！
  ```

---

## 💻 PowerShell 常用實戰指令速查

```powershell
# 建立 UTF-8 編碼檔案 (PowerShell 專用)
New-Item -Path . -Name "index.html" -ItemType "file" -Value "Hello Git"

# 檢查目前檔案狀態
git status

# 搬上暫存舞台 / 提交
git add .
git commit -m "Your commit message"

# 查看歷史簡短紀錄
git log --oneline
```