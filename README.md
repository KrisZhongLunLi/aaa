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

'20240805'
# 1 add repo
'zypper ar "dir:///<path>/<folder>" <repo> 透過zypper在選定的檔案新增repo (ar 即是add repo)

'zypper ar "iso:/?iso＝/<path>/<img>.iso" <repo> 透過zypper將iso檔新增到repo中

# 2 install package mlocate
運用rpm來進行package mlocate的掛載

確認光碟有存入VM當中

'su -' 在 VM 中以管理員身分登入

'mount dev/sr0 /mnt' 用mount的指令進行掛載到前述的位址中

進入到mnt目錄中即可確認掛載完成

'cd Mount-Basesystem/x86_64' 進入到database中x86的版本

'rpm -ql mlocate' 確認目錄中是否有mlocate

'ls mlocate-*' 找尋mlocate的.rpm檔

'rpm -ivh mlocate-xxx.rpm' 執行並下載mlocate.rpm

-i ：install 的意思

-v ：察看更細部的安裝資訊畫面

-h ：以安裝資訊列顯示安裝進度

'rpm -ql mlocate' 確認mlocate裝載完成

# 3 install pattern devel_kernel
'zypper in devel_pattern' 在可用的repo中找尋所求的pattern再進行安裝


'20240812'
# 1.1 Public Key & Private Key 解釋
公鑰與私鑰的基本概念：

SSH（Secure Shell）使用公鑰加密技術來實現安全的身份驗證和通信。這種技術依賴於一對密鑰：公鑰（Public Key）和私鑰（Private Key）。

私鑰（Private Key）： 私鑰是完全保密的，它應該只存儲在你的設備上，並且永遠不應該與任何人分享。它的主要作用是用來解密由相應的公鑰加密的信息，並在進行身份驗證時生成數字簽名。

公鑰（Public Key）： 公鑰可以公開分享給其他人或服務器。它的作用是加密信息，這些信息只能由擁有相應私鑰的人解密。當你將公鑰放置在遠程伺服器的 ~/.ssh/authorized_keys 文件中時，該伺服器就能夠使用公鑰來驗證連接嘗試是否來自持有私鑰的合法使用者。

密鑰生成：
在 Linux 系統上，你可以使用 ssh-keygen 工具來生成公鑰和私鑰對。這個命令會根據你的選擇生成不同類型的密鑰對（如 RSA、DSA、ECDSA、ED25519 等）。生成密鑰後，系統會提示你選擇保存密鑰的位置。可以選擇設置一個密碼來保護你的私鑰，這樣即使私鑰文件被盜，攻擊者仍需知道這個密碼才能使用密鑰。
# 1.2 禁止 Root 登入
說明： 禁止 root 使用 SSH 直接登入是提高系統安全性的一種常見做法，因為 root 擁有最高權限，攻擊者利用 root 進行攻擊的風險很大。
# 1.3 使用者免密碼登入 / 使用 SSH-Key
說明： 免密碼登入允許使用者使用 SSH 密鑰而非密碼來進行認證，這通常比使用密碼更加安全。
