# Git & GitHub 實戰協作指南

本手冊涵蓋從個人基礎操作到多人團隊協作、Pull Request（PR）發送之完整工作流程。

---

## 任務一：建立帳號與 Fork 專案

1. 註冊並登入 [GitHub](https://github.com/) 帳號。
2. 前往課程主專案：。
3. 點擊頁面右上角的 **Fork** 按鈕，將專案複製一份至個人帳號底下。

## 任務二：設定與綁定 SSH Key

1. **生成 SSH 金鑰**（於終端機執行，一路按 Enter 即可）：
   ```bash
   ssh-keygen -t ed25519 -C "你的GitHub註冊信箱"
   ```
>補充：Ed25519 是一種基於橢圓曲線密碼學（Curve25519）的現代數位簽章演算法。相較於傳統的 RSA，它在提供極高安全性的同時，擁有更短的金鑰長度與更快的加密、驗證速度。

2. **複製公鑰內容**：
   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```
3. **新增至 GitHub**：
   * 前往 GitHub 網頁 $\to$ 點擊右上角頭像 $\to$ **Settings** $\to$ **SSH and GPG keys** $\to$ 點擊 **New SSH key**。
   * Title 填入裝置識別名稱，Key 欄位貼上剛才複製的整串公鑰 $\to$ 點擊 **Add SSH key**。
4. **驗證連線**：
   ```bash
   ssh -T git@github.com
   ```
   *(出現 `Hi <username>! You've successfully authenticated...` 即代表設定成功)*

## 任務三：Clone 專案至本地端

1. 進入個人 Fork 出來的專案頁面（`[https://github.com/](https://github.com/)<你的帳號>/oop-python-nycu`）。
2. 點擊綠色 **`<> Code`** 按鈕 $\to$ 選擇 **SSH** 分頁 $\to$ 複製 SSH URL。
3. 於本地終端機執行 Clone：
   ```bash
   git clone git@github.com:<你的帳號>/oop-python-nycu.git
   cd oop-python-nycu
   ls
   ```

## 任務四：檢查狀態與建立檔案

1. **確認目前分支狀態**：
   ```bash
   git status
   ```
   *(應顯示 `nothing to commit, working tree clean`)*

2. **建立個人資訊檔案**：
   ```bash
   # 使用編輯器建立 myname.txt
   vim myname.txt
   ```
   檔案內容填寫：
   ```text
   姓名
   學號
   ```

3. **再次檢查狀態**：
   ```bash
   git status
   ```
   *(會顯示 `myname.txt` 處於 Untracked 狀態)*

## 任務五：暫存、提交並推送變更

```bash
# 1. 將檔案加入暫存區
git add myname.txt

# 2. 提交版本變更並加上訊息
git commit -m "add myname.txt"

# 3. 推送至遠端倉庫
git push
```

## 任務六：驗證雲端更新

回到個人的 GitHub 專案網頁，重新整理頁面，確認 `myname.txt` 檔案已正確顯示在專案目錄中。

## 任務七：從遠端拉取最新更新（git pull）

1. 在 GitHub 網頁端直接點擊 `myname.txt` $\to$ 點擊鉛筆圖示進行編輯（例如於第三行加入「系級」資訊） $\to$ 點擊 **Commit changes**。
2. 回到本地終端機執行同步：
   ```bash
   git pull
   ```
3. 確認本地檔案內容已與雲端同步：
   ```bash
   cat myname.txt
   ```

## 任務八：團隊協作設定（組長操作）

1. **選定組長**：每組推派一名組長作為該組的 Central Repo 維護者。
2. **加入組員協作權限**：
   * 組長進入個人的 `oop-python-nycu` Repo。
   * 進入 **Settings** $\to$ 左側 **Collaborators** $\to$ 點擊 **Add people**。
   * 輸入組員的 GitHub 帳號或 Email 發送邀請。
   * **組員操作**：收信並點擊接受邀請（Accept invitation）。
3. **組長建立組別目錄**：
   ```bash
   # 於 tests/ 目錄下建立組別資料夾（例：group1）
   mkdir -p tests/group1
   git add tests/group1
   git commit -m "feat: 新增 group1 目錄"
   git push
   ```

## 任務九：組員協作與檔案整理

1. **組員 Clone 組長的專案**：
   ```bash
   git clone git@github.com:<組長帳號>/oop-python-nycu.git group_project
   cd group_project
   ```
2. **複製並重新命名檔案**：
   ```bash
   # 複製個人檔案並重新命名為 <學號>.txt，放置於組別資料夾內
   cp /path/to/myname.txt tests/group1/<你的學號>.txt
   ```
3. **提交並推送**：
   ```bash
   git add tests/group1/<你的學號>.txt
   git commit -m "add <你的學號>.txt"
   git push
   ```

## 任務十：發送 Pull Request (PR) 至主倉庫

所有組員檔案整理完畢後，由**組長**代表發起 PR：

1. 前往組長個人的 `oop-python-nycu` Repo 頁面。
2. 點擊 **Pull requests** $\to$ **New pull request**。
3. **確認分支比對設定**：
   * **Base repository**: `ARG-NCTU/oop-python-nycu`（base: `main`）
   * **Head repository**: `<組長帳號>/oop-python-nycu`（compare: `main`）
4. 點擊 **Create pull request**。
5. **填寫 PR 標題與內容**：
   * **Title**：`LAB2-group<組號>`（例：`LAB2-group1`）
   * **Description**（複製下方範本並修改）：
     ```markdown
     ## 更新內容
     新增 group1 組員名單與個人檔案。

     ## 組員名單與 GitHub 帳號
     - @組員A_GitHub帳號
     - @組員B_GitHub帳號
     - @組長_GitHub帳號

     Tag 助教進行驗收：
     @助教GitHub帳號
     ```
6. 點擊 **Create pull request** 送出，等待助教審查並進行現場 Demo。

## 任務十一：進階指令討論（git stash）

當本地端有尚未完成的程式碼修改，但需要緊急切換分支或執行 `git pull` 時，可使用暫存指令：

* **暫存當前工作區**：
  ```bash
  git stash
  ```
* **查看暫存清單**：
  ```bash
  git stash list
  ```
* **復原暫存變更**：
  ```bash
  git stash pop
  ```

> **思考與討論**：在多人共同開發期末專案時，若有組員臨時推送了修復程式碼，但你手邊的檔案修改到一半無法立即 commit，`git stash` 能如何協助避免版本衝突？

## 補充：Git 常用核心指令

### 一、 核心指令與關鍵參數解析

#### 1. 遠端同步與推送（Push / Pull / Remote）
| 指令與常用參數 | 功能說明 | 典型應用範例 |
| :--- | :--- | :--- |
| `git push -u <remote> <branch>` | **`-u` (`--set-upstream`)**：綁定本地分支與遠端分支追蹤關係。後續只需輸入 `git push`。 | `git push -u origin main` |
| `git push -f` | **`-f` (`--force`)**：強制以本地版本覆蓋遠端（危險指令，團隊協作時謹慎使用）。 | `git push -f origin main` |
| `git pull --rebase` | **`--rebase`**：拉取遠端最新代碼時，將本地 commit 疊加在遠端最新歷史之後，避免產生多餘的 Merge Commit。 | `git pull --rebase origin main` |
| `git remote -v` | **`-v` (`--verbose`)**：詳細列出所有遠端倉庫名稱及其對應的 Fetch / Push URL。 | `git remote -v` |
| `git remote set-url --add --push` | 同時為單一 remote 綁定多個推送目標（如個人與組織）。 | `git remote set-url --add --push origin <URL>` |

#### 2. 分支與工作目錄管理（Branch / Checkout / Switch）
| 指令與常用參數 | 功能說明 | 典型應用範例 |
| :--- | :--- | :--- |
| `git branch -a` | **`-a` (`--all`)**：列出本地與遠端（Remote Tracking）的所有分支。 | `git branch -a` |
| `git branch -r` | **`-r` (`--remotes`)**：僅列出遠端分支。 | `git branch -r` |
| `git branch -M <new_name>` | **`-M`**：強制重新命名當前分支（等同 `--move --force`）。 | `git branch -M main` |
| `git branch -d / -D <name>` | **`-d`**：安全刪除分支（已合併）；**`-D`**：強制刪除未合併的分支。 | `git branch -D feature-test` |
| `git checkout -b <name>` | **`-b`**：建立新分支並立即切換過去（新版 Git 推薦使用 `git switch -c <name>`）。 | `git checkout -b dev` |

#### 3. 檔案操作與暫存區（Add / Commit / RM / MV）
| 指令與常用參數 | 功能說明 | 典型應用範例 |
| :--- | :--- | :--- |
| `git add .` | 將當前目錄下所有新增與修改的檔案加入暫存區（不包含已刪除檔案需視版本）。 | `git add .` |
| `git add -A` | **`-A` (`--all`)**：將整個 Repo 內所有更動（新增、修改、刪除）全部暫存。 | `git add -A` |
| `git commit -m "msg"` | **`-m` (`--message`)**：直接在指令列輸入本次提交的說明訊息。 | `git commit -m "feat: 新增登入功能"` |
| `git commit --amend` | 修改最後一次的 Commit 訊息，或將新修改合併進最後一次 Commit。 | `git commit --amend -m "fix: 修正筆誤"` |
| `git rm -r <dir>` | **`-r` (`--recursive`)**：遞迴刪除整個資料夾及其內容並同步至 Git 暫存區。 | `git rm -r temp_folder/` |
| `git rm --cached <file>` | **`--cached`**：僅將檔案從 Git 版本控制中移除，但本機硬碟檔案保留（常用於誤傳 `.env` 等檔案）。 | `git rm --cached config.json` |
| `git mv <old> <new>` | 移動檔案或重新命名，並自動記錄至暫存區。 | `git mv old.txt docs/new.txt` |

#### 4. 暫存保存（Stash）
| 指令與常用參數 | 功能說明 | 典型應用範例 |
| :--- | :--- | :--- |
| `git stash` | 暫存已追蹤檔案的修改，使工作區恢復乾淨狀態。 | `git stash` |
| `git stash -u` | **`-u` (`--include-untracked`)**：連同尚未被追蹤的新檔案（Untracked files）一併暫存。 | `git stash -u` |
| `git stash pop` | 套用最近一次暫存內容，並將該筆紀錄從暫存清單中移除。 | `git stash pop` |
| `git stash list` | 檢視所有暫存紀錄清單。 | `git stash list` |
| `git stash drop stash@{N}` | 刪除指定的暫存紀錄。 | `git stash drop stash@{0}` |

#### 5. 檢視與除錯（Status / Log / Diff / Reset）
| 指令與常用參數 | 功能說明 | 典型應用範例 |
| :--- | :--- | :--- |
| `git status -s` | **`-s` (`--short`)**：以極簡格式輸出工作區狀態。 | `git status -s` |
| `git log --oneline --graph` | 以單行、圖形化 ASCII 方式呈現分支與提交歷史。 | `git log --oneline --graph -n 10` |
| `git diff` | 比對工作區與暫存區之間的檔案差異。 | `git diff` |
| `git reset --soft HEAD~1` | **`--soft`**：復原最後一次 Commit，但保留所有修改在暫存區（Staging Area）。 | `git reset --soft HEAD~1` |
| `git reset --hard HEAD~1` | **`--hard`**：完全捨棄最後一次 Commit 及工作區所有修改（無法復原，需謹慎）。 | `git reset --hard HEAD~1` |

---

### 二、 常見參數字母速查表

* **`-u`**
  * `git push -u`：設定上游分支（`--set-upstream`）。
  * `git stash -u`：包含未追蹤檔案（`--include-untracked`）。
* **`-r` / `-R`**
  * `git rm -r`：遞迴處理目錄（`--recursive`）。
  * `git branch -r`：檢視遠端分支（`--remotes`）。
* **`-m` / `-M`**
  * `git commit -m`：設定 Commit Message。
  * `git branch -m` / `-M`：重新命名分支（小寫一般改名，大寫強制改名）。
* **`-b` / `-B`**
  * `git checkout -b`：建立並切換新分支。
  * `git clone -b <branch>`：僅 Clone 指定分支。
* **`-f` / `-F`**
  * `git push -f`：強制推送（`--force`）。
* **`-a` / `-A`**
  * `git add -A`：暫存所有變更（`--all`）。
  * `git branch -a`：列出所有本地與遠端分支。
  * `git commit -a -m`：自動將已追蹤的修改檔案全部加入暫存並提交。
