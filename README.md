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

'20240729'

Root 為管理員根目錄，使用者從 Home 開始，管理員可以將 user 分組，每個 user 可以同時在多個群組。

每組功能與權限不同，可以使用 sudo 調整，或是透過 yast 調整。


## 1.1 su
### 1.1.1 su的功用主要在於使用者的切換

      'su [-lm] [-c 指令] [username]'
      
      'exit' 離開su的環境
### 1.1.2 su跟su-的差別
      'su' 讀取的變數設定方式為non-login shell，導致許多原本的變數不會被改變。即便將原先的使用者切換到root下，也不會進到root所在的環境中。
      
      'su -' 讀取的變數設定方式為login shell。將使用者切換到root後，透過密碼的驗證便能進入root所在的環境。便能直接使用root中的指令，PATH等路徑也都能導向root。
# 2 sudo

  sudo相對於su，不用輸入root的密碼，只需輸入自己帳號的密碼，或可設定不需輸入密碼。因此需被root加到/etc/sudoers的用戶才能使用。
  
  'sudo [-b] [-u 新使用者帳號]'
## 2.1 edit /etc/sudoers

    'visudo' 叫出/etc/sudoers的檔案進行vi修改。
    
    'XXX    ALL=(ALL)' 讓XXX可以變換身分成任何人，執行任何指令。  
## 2.2 限制使用者 使用sudo command by user

    'myuser1 ALL=(root)  !/usr/bin/passwd, /usr/bin/passwd [A-Za-z]*, !/usr/bin/passwd root' 以此為例，由root操作，加入"!"指令的設定意思為不讓user進行特定的指令，以保護root的權益。
## 2.3 限制使用者 使用sudo command by group

    '%wheel     ALL=(ALL)    ALL' 同樣的意思，加入了%代表的是一個group的意思，前述為開放全部指令。若要限制該群組的指令權限，同樣也可透過加入"!"指令來進行限制。
## 2.4 使用 user alias, command alias, host alias

    'User_Alias ADMPW = pro1, pro2, pro3, myuser1, myuser2'
    
    'Cmnd_Alias ADMPWCOM = !/usr/bin/passwd, /usr/bin/passwd [A-Za-z]*, !/usr/bin/passwd root'
    
    'ADMPW   ALL=(root)  ADMPWCOM'  使用user alias, command alias, host alias, 可以分別將數個帳號名稱，數個指令，數個主機來源，設定成一新的別名，方便後續指令修改的簡單跟彈性化。
## 2.5 使用 使用者自己密碼切換身份

    只要使用者在etc/sudoers的檔案中有使用sudo的權限，只需透過輸入自己的密碼便能切換到root或其他開放權限之使用者。
## 2.6 使用 yast 設定 sudo

    "yast-> Security and Users"-> Sudo" 便可在此新增、更改或刪除使用者及其權限，同樣也能透過Alias執行相同的效果。

yast 可以新增帳號，或是使用 useradd, userdel。帳號 id 範圍為 0 ~ 2^n 次方，通常 0 為管理員，999 以內預留給其他程式，user 從 1000 開始。
# 3 account
### 3.1.1 新增，移除帳號 使用yast

    "yast-> Security and Users"-> User and Group Managment" 可在此新增、更或移除帳號。 
### 3.1.2 指定 home dir, shell, password 使用 yast

    順著3.1.1的路徑，選擇"Edit"，便能進入帳號中設定home dir, shell, 及password
### 3.2.1 新增，移除帳號 useradd, userdel

    'useradd [-u UID] [-g 初始群組] [-G 次要群組] [-mM]\' 不須透過yast，也可直接使用useradd的指令來新增帳號。"-m"在用於強制執行家目錄
    
    'userdel [-r] username' 透過userdel的指令，連同該使用者之家目錄都一併刪除。 
### 3.2.3 finger chfn

# 4.chmod 可以設定權限，可分為 3 組二進位數字：
第 1 組為檔案擁有者權限；

第 2 組為所在群組的權限；

第 3 組為其他 user 的權限。

每組內有 3 個數字，依序為 read (r), write (w), execute (x)

執行時使用指令：chmod (+ | -) (r | w | x) file_name

還可以在前面加入指定對象：a (all), u (user), g (group), o (other)

例如：chmod +x file_name 為指定所有人可執行；chmod g-w file_name 為指定群組移除編輯權限

也可以直接使用十進位數字轉換，例如 777 表示將 user, group, other 的權限都打開。
