"# 2024 0722"

# linux 指令
## 內容
### cd 進入路徑
### ls/ll 查看內容
### cat  查看內容
### vi 修改內容

## 路徑
### ./.../.../... .../ 絕對路徑
### /xxx 相對路徑，以當前路徑為出發點

## 網路
連接方法：

# 2024 0729
Root 為管理員根目錄，使用者從 Home 開始，管理員可以將 user 分組，每個 user 可以同時在多個群組。
每組功能與權限不同，可以使用 sudo 調整，或是透過 yast 調整。

yast 可以新增帳號，或是使用 useradd, userdel。帳號 id 範圍為 0 ~ 2^n 次方，通常 0 為管理員，999 以內預留給其他程式，user 從 1000 開始。

chmod 可以設定權限，可分為 3 組二進位數字：
第 1 組為檔案擁有者權限；
第 2 組為所在群組的權限；
第 3 組為其他 user 的權限。
每組內有 3 個數字，依序為 read (r), write (w), execute (x)
執行時使用指令：chmod (+ | -) (r | w | x) file_name
還可以在前面加入指定對象：a (all), u (user), g (group), o (other)
例如：chmod +x file_name 為指定所有人可執行；chmod g-w file_name 為指定群組移除編輯權限
也可以直接使用十進位數字轉換，例如 777 表示將 user, group, other 的權限都打開。
