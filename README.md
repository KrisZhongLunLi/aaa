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

私鑰（Private Key）： 私鑰是完全保密的，它應該只存儲在你的設備上並且不與任何人分享。它的主要作用是用來解密由相應的公鑰加密的信息，並在進行身份驗證時生成數字簽名。

公鑰（Public Key）： 公鑰可以公開分享給其他人或服務器。它的作用是加密信息，這些信息只能由擁有相應私鑰的人解密。當你將公鑰放置在遠程伺服器的 ~/.ssh/authorized_keys 文件中時，該伺服器就能夠使用公鑰來驗證連接嘗試是否來自持有私鑰的合法使用者。

# 1.2 禁止 Root 登入
禁止 Root 使用 SSH 直接登入的重要性：

在許多 Linux 系統中，root 是擁有最高權限的使用者。允許 root 通過 SSH 直接登入可能會給系統帶來很大的安全風險。攻擊者如果猜測或破解了 root 密碼，將會擁有系統的完全控制權。因此禁止 root 使用 SSH 直接登入，改用普通使用者登入後再通過 sudo 或 su 切換到 root。

sudo nano /etc/ssh/sshd_config
PermitRootLogin no
sudo systemctl restart sshd

這樣就禁止了 root 使用 SSH 登入。

# 1.3 使用者免密碼登入 / 使用 SSH-Key
為什麼使用 SSH 密鑰進行免密碼登入：SSH 密鑰認證比傳統的密碼認證更加安全，因為密鑰難以被暴力破解。此外，SSH 密鑰支持長度更長的字串，這使得暴力破解更加困難。使用 SSH 密鑰進行免密碼登入能夠提高安全性，同時也免去了記住和管理多個密碼的麻煩。

生成一對密鑰，其中私鑰保存在本地，公鑰可以用來配置免密碼登入：ssh-keygen -t rsa -b 4096 -C "email"
使用以下命令將公鑰複製到你希望免密碼登入的遠程伺服器：ssh-copy-id username@remote_host
登入遠端伺服器：ssh username@remote_host

# 1.4 禁止使用者使用密碼登入，只能使用 Key
限制登入方式以提高安全性：
為了進一步提高 SSH 的安全性，可以完全禁用密碼認證，強制要求所有使用者使用 SSH 密鑰進行登入。這樣即使攻擊者獲得了某個使用者的密碼，也無法直接登入系統。

sudo nano /etc/ssh/sshd_config
PasswordAuthentication no
UsePAM no  # 禁用 PAM
sudo systemctl restart sshd

# 1.5 設定個別使用者 SSH Config
為每個使用者設置專屬的 SSH 配置：每個使用者都可以根據不同的需求，在本地配置自己的 SSH 設置。這些設置存儲在 ~/.ssh/config 文件中，可以用來為不同的遠程伺服器設置不同的連接參數，例如使用不同的密鑰、端口或特定的登入選項。

nano ~/.ssh/config
Host example
    HostName example.com
    User username
    IdentityFile ~/.ssh/id_rsa
    Port 2222

Host example：這是一個標籤，代表一個特定的連接配置。
HostName example.com：遠程伺服器的主機名或 IP 地址。
User username：連接到遠程伺服器時使用的使用者名稱。
IdentityFile ~/.ssh/id_rsa：指定使用的 SSH 私鑰文件。
Port 2222：遠程伺服器的 SSH 端口號（如果默認的 22 被改變）。

# 1.6 透過 SSH 直接執行指令（如關機、重啟服務）
SSH 不僅可以用來進行遠程登入，還可以直接在遠程伺服器上執行命令，而不需要進行完整的登入會話。這非常有助於進行簡單的系統管理任務，如重啟服務或關閉系統。

執行單個命令：ssh username@remote_host 'sudo systemctl restart apache2'
執行系統關機：ssh username@remote_host 'sudo shutdown -h now'
執行系統重啟：ssh username@remote_host 'sudo reboot'

# 1.7 Port Forwarding
SSH 端口轉發允許通過 SSH 隧道將一個網絡端口的流量重定向到另一個端口。這通常用於安全地訪問內部網絡資源，或者繞過防火牆或 NAT 限制。

本地端口轉發（Local Port Forwarding）： 將本地端口的流量轉發到遠程服務器上的端口。
ssh -L 8080:localhost:80 username@remote_host

遠程端口轉發（Remote Port Forwarding）： 將遠程服務器端口的流量轉發到本地端口。
ssh -R 8080:localhost:80 username@remote_host

# 1.8 X11 Forwarding
X11 轉發允許在遠程伺服器上運行的圖形應用程序顯示在你的本地桌面上。這對於需要使用 GUI 應用程序的遠程管理非常有用。

sudo nano /etc/ssh/sshd_config
X11Forwarding yes
sudo systemctl restart sshd
ssh -X username@remote_host

# 1.9  Allow User / Group Login
可以限制哪些使用者或群組可以通過 SSH 登入系統。這是一種有效的安全策略，特別是在管理大量使用者時。

sudo nano /etc/ssh/sshd_config
僅允許列出的使用者登入：AllowUsers user1 user2
僅允許列出的群組成員登入：AllowGroups sshgroup
sudo systemctl restart sshd

# 1.10 Deny User / Group Login
禁止特定使用者或群組登入：與 AllowUsers 和 AllowGroups 相對應，你也可以指定哪些使用者或群組不能通過 SSH 登入。這對於禁用特定使用者帳戶非常有用。

DenyUsers user1 user2
DenyGroups nogroup
sudo systemctl restart sshd

1.1 setup nis server and client
在伺服器上：
安裝NIS套件：sudo apt-get install nis
配置NIS域名： 編輯/etc/default/nis文件：sudo nano /etc/default/nis
將NISSERVER設置為master，並提供域名，例如mydomain。

配置NIS服務： 編輯/etc/yp.conf，以包含伺服器的主機名或IP地址。
domain mydomain server server_hostname

啟動並啟用NIS服務：sudo systemctl start nis
sudo systemctl enable nis
初始化NIS映射：sudo /usr/lib/yp/ypinit -m
在客戶端上：
安裝NIS套件：sudo apt-get install nis
配置NIS域名： 編輯/etc/default/nis文件：sudo nano /etc/default/nis
將NISSERVER設置為none，並指定NIS域名為mydomain。

配置NIS服務： 編輯/etc/yp.conf，以包含伺服器的主機名或IP地址。domain mydomain server server_hostname
配置nsswitch.conf文件： 修改/etc/nsswitch.conf文件，使其使用NIS進行某些服務：
passwd:         compat nis
group:          compat nis
shadow:         compat nis
啟動並啟用NIS服務：
sudo systemctl start nis
sudo systemctl enable nis

1.2 allow client change password
為了允許客戶端更改密碼，伺服器上需要運行rpc.yppasswdd服務。

安裝rpc.yppasswdd：sudo apt-get install rpcbind
啟動rpc.yppasswdd服務：
sudo systemctl start rpcbind
sudo systemctl enable rpcbind
確保伺服器上的/etc/ypserv.conf文件允許更改密碼：sudo nano /etc/ypserv.conf
確保包含以下行：passwd:      all

1.3 server change password
伺服器管理員可以使用以下命令更改用戶的密碼：sudo yppasswd username


1.4 setup/assign start uid
要為NIS管理的用戶指定起始UID，需在NIS伺服器上的/etc/login.defs文件中進行配置。

編輯login.defs文件：sudo nano /etc/login.defs
設置UID_MIN和UID_MAX值：
UID_MIN 1000
UID_MAX 60000


1.5 list file/dir of server config
NIS常見的配置文件和目錄包括：

/etc/yp.conf：配置NIS域和伺服器。
/var/yp/：包含NIS映射。
/etc/default/nis：包含NIS域配置。

1.6 service port and protocol      
NIS使用幾個基於RPC的服務，通常在以下端口和協議上運行：

端口111：由rpcbind使用。
端口834：由ypserv使用。
端口835：由ypbind使用。
端口836：由yppasswdd使用。

# 20240909
# 1. Telnet
## 1-1: Setup Telnet Server
安裝 Telnet 服務器：
sudo apt-get install xinetd telnetd
啟用 Telnet 服務：
打開配置文件 /etc/xinetd.d/telnet，確保 disable = no。
如果文件不存在，創建該文件並寫入以下內容：
service telnet
{
   flags       = REUSE
   socket_type = stream
   wait        = no
   user        = root
   server      = /usr/sbin/in.telnetd
   log_on_failure += USERID
   disable     = no
}
sudo service xinetd restart

## 1-2: List File/Dir of Server Config
Telnet 服務的主要配置文件位於：

/etc/xinetd.d/telnet
/etc/xinetd.conf（可選）
## 1-3: Service Port and Protocol
Port: 23
Protocol: TCP

# 2. FTP
## 2-1: Setup FTP Server
安裝 FTP 服務器（如 vsftpd）：
sudo apt-get install vsftpd
禁用匿名登入：anonymous_enable=NO
啟用本地用戶登入：local_enable=YES  write_enable=YES
重啟 FTP 服務：sudo service vsftpd restart

## 2-2: Anonymous Browse Dir/File
要啟用匿名瀏覽，請在 /etc/vsftpd.conf 中設置以下選項：anonymous_enable=YES

## 2-3: List File/Dir of Server Config
FTP 服務器的配置文件：

/etc/vsftpd.conf
用戶目錄默認位於 /srv/ftp 或 /var/ftp。

## 2-4: Service Port and Protocol
Port: 21 (控制連接)，20（數據傳輸）
Protocol: TCP

# 3. rsh / rlogin
## 3-1: Setup rlogin
安裝 rsh-server 和 rsh-client：sudo apt-get install rsh-server rsh-client
確保 /etc/inetd.conf 中啟用了 rlogin 服務，如果該行存在但被註釋掉，取消註釋：login stream tcp nowait root /usr/sbin/tcpd /usr/sbin/in.rlogind
編輯 /etc/hosts.equiv 或 ~/.rhosts 文件，允許指定主機進行無密碼登入。

/etc/hosts.equiv 允許全局主機。
~/.rhosts 允許特定用戶的遠程登入。

重啟 inetd：sudo service inetd restart
