#vim & ideavim

## vim 移动

```
% 跳转到相配对的括号

gD 跳转到局部变量的定义处

''跳转到光标上次停靠的地方, 是两个', 而不是一个 "

mx 设置书签, x 只能是 a-z 的 26 个字母

`x 跳转到书签处 ("`" 是 1 左边的键)

> 增加缩进,"x>" 表示增加以下 x 行的缩进

< 减少缩进,"x<" 表示减少以下 x 行的缩进

{ 跳到上一段的开头

} 跳到下一段的的开头

( 移到这个句子的开头

) 移到下一个句子的开头

[[跳转至上一个函数 (要求代码块中'{'必须单独占一行)

]] 跳转至下一个函数 (要求代码块中'{'必须单独占一行)

C-] 跳转至函数或变量定义处

C-O 返回跳转前位置 

C-T 同上 

nC-T 返回跳转 n 次

0 数字 0, 跳转至行首 

^ 跳转至行第一个非空字符 

$ 跳转至行尾 
```

# Cygwin

window下模拟linux环境，使用linux命令<br />​

自带的仿真终端很好用：mintty<br />​<br />
<a name="lWVQj"></a>

## 安装apt-cyg  指令安装器
<br />
<a name="w2qf1"></a>

## 配置cygwin-terminal

<a name="N1KlA"></a>

### mintty终端配置
 `vim ~/.minttyrc`
```bash
#字体名称，可以使用系统字体名称   ps:option-ui中的字体不全
Font=幼圆
#字体大小
FontHeight=12
Columns=160
Rows=39
BoldAsFont=no
Locale=zh_CN
Charset=UTF-8
Language=zh_CN
ThemeFile=gruvbox

```
<a name="vfc7A"></a>
### vim配置
`vim ~/.vimrc`		:设置insert/normal模式游标样式
```bash
set nu
let &t_ti.="\e[1 q"
let &t_SI.="\e[5 q"
let &t_EI.="\e[1 q"
let &t_te.="\e[0 q"
```
<a name="dRsb7"></a>
### 安装tmux管理多窗口
tmux attach-session 0 / 1   进入窗口<br />`**ctrl + b**`** **窗口指令

- `↑`：命令键长按 配合方向键，调整窗口大小
- `%`: 垂直拆分
- `"` :水平拆分
- `d`：detech 退出，并没有关闭会话
- `w`: 窗口管理器
   - : 输入指令
      - rename
      - kill-pan 删除窗口
      - rename-window ： 重命名窗口
      - rename-session: 重命名会话

`vim ~/.tumx.conf`<br />修改快捷键 和 设置全局开启鼠标：
> 开启鼠标后无法从buffer中复制 ，尤其是vim   
> vim 需要支持clipboard 和 xterm_clipboard，然后使用增强指令赋值粘贴 可以绕过冲突
> "+y  复制
> " +p 粘贴
> 或者按住shift然后选取

```bash
# VIM模式
bind-key k select-pane -U # up
bind-key j select-pane -D # down
bind-key h select-pane -L # left
bind-key l select-pane -R # right
#开启鼠标
set -g mouse on
```
<a name="jHKlW"></a>
### git配置
~~cygwin ~ 路径为 /home/user~~<br />~~git-bash路径和windows user路径一直~~<br />cygwin默认会使用windows 的openssh，在验证身份的时候 config文件不能访问<br />​

需要使用cygwin 安装的openssh
```bash
# 安装openssh
$ apt-cyg install openssh
# 配置git文件 （多git仓库，ssh-keygen 先生成秘钥 然后配置文件指向生成好的秘钥）
$ vim ~/.ssh/config

Host github.com
User 邮箱地址
PreferredAuthentications publickey
IdentityFile ~/.ssh/github-rsa
```
# 更改权限
$ chomod 700 ~/.ssh/config
# 测试是否成功
$ ssh -T git@github.com
Hi henghengyang! You've successfully authenticated, but GitHub does not provide shell access.


```

<br />
<br />需要把windosuser路径中的  `.ssh  文件夹`，`.gitconf` 考到cygwin路径下  保持一致<br />​

chmod 十进制权限说明:
```bash
-rw------- (600)    只有拥有者有读写权限。
-rw-r--r-- (644)    只有拥有者有读写权限；而属组用户和其他用户只有读权限。
-rwx------ (700)    只有拥有者有读、写、执行权限。
-rwxr-xr-x (755)    拥有者有读、写、执行权限；而属组用户和其他用户只有读、执行权限。
-rwx--x--x (711)    拥有者有读、写、执行权限；而属组用户和其他用户只有执行权限。
-rw-rw-rw- (666)    所有用户都有文件读、写权限。
-rwxrwxrwx (777)    所有用户都有读、写、执行权限。
```
<a name="WzFiM"></a>
### sshd配置
依赖： openssh<br />cygwin使用windows用户名密码<br />​<br />
```bash
$ ssh-host-config

** Info: Generating /etc/ssh_host_key
*** Info: Generating /etc/ssh_host_rsa_key
*** Info: Generating /etc/ssh_host_dsa_key
*** Info: Generating /etc/ssh_host_ecdsa_key
*** Info: Creating default /etc/ssh_config file
*** Info: Creating default /etc/sshd_config file
*** Info: Privilege separation is set to yes by default since OpenSSH 3.3.
*** Info: However, this requires a non-privileged account called 'sshd'.
*** Info: For more info on privilege separation read /usr/share/doc/openssh/READ
ME.privsep.
*** Query: Should privilege separation be used? (yes/no) no
*** Info: Updating /etc/sshd_config file

*** Query: Do you want to install sshd as a service?
*** Query: (Say "no" if it is already installed as a service) (yes/no) yes
*** Query: Enter the value of CYGWIN for the daemon: netsec] netsec
*** Info: On Windows Server 2003, Windows Vista, and above, the
*** Info: SYSTEM account cannot setuid to other users -- a capability
*** Info: sshd requires.  You need to have or to create a privileged
*** Info: account.  This script will help you do so.

*** Info: You appear to be running Windows XP 64bit, Windows 2003 Server,
*** Info: or later.  On these systems, it's not possible to use the LocalSystem
*** Info: account for services that can change the user id without an
*** Info: explicit password (such as passwordless logins [e.g. public key
*** Info: authentication] via sshd).

*** Info: If you want to enable that functionality, it's required to create
*** Info: a new account with special privileges (unless a similar account
*** Info: already exists). This account is then used to run these special
*** Info: servers.

*** Info: Note that creating a new user requires that the current account
*** Info: have Administrator privileges itself.

*** Info: No privileged account could be found.

*** Info: This script plans to use 'cyg_server'.
*** Info: 'cyg_server' will only be used by registered services.
*** Query: Do you want to use a different name? (yes/no) yes
*** Query: Enter the new user name: sony
*** Query: Reenter: sony

*** Warning: Privileged account 'sony' was specified,
*** Warning: but it does not have the necessary privileges.
*** Warning: Continuing, but will probably use a different account.
*** Warning: The specified account 'sony' does not have the
*** Warning: required permissions or group memberships. This may
*** Warning: cause problems if not corrected; continuing...
*** Query: Please enter the password for user 'sony':
*** Query: Reenter:


*** Info: The sshd service has been installed under the 'sony'
*** Info: account.  To start the service now, call `net start sshd' or
*** Info: `cygrunsrv -S sshd'.  Otherwise, it will start automatically
*** Info: after the next reboot.

*** Info: Host configuration finished. Have fun!
```
启动服务<br />net start cygsshd<br />关闭服务<br />net stop cygsshd
<a name="aApYr"></a>
#### 配置秘钥登录
```bash
sony@sony-VAIO ~ $ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/sony/.ssh/id_rsa):
Created directory '/home/sony/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/sony/.ssh/id_rsa.
Your public key has been saved in /home/sony/.ssh/id_rsa.pub.
The key fingerprint is:
e8:38:5e:e3:bb:cf:76:03:61:5f:f2:68:ed:a3:49:db sony@sony-VAIO
The key's randomart image is:
+--[ RSA 2048]----+
|                 |
|                 |
|                 |
|       .o . .    |
|      ..So *     |
|     o  . + o    |
|    o +  o..     |
|   . + o..o+o    |
|    . +=o.+oE.   |
+-----------------+

sony@sony-VAIO ~ $ cd .ssh/

sony@sony-VAIO ~/.ssh $ ls
id_rsa  id_rsa.pub

sony@sony-VAIO ~/.ssh $ cp id_rsa.pub authorized_keys

sony@sony-VAIO ~/.ssh $ ls
authorized_keys  id_rsa  id_rsa.pub
```
<br />
<a name="zi8pf"></a>

## idea集成

shell path:
```bash
"c:\cygwin64\bin\sh" -lic "source ~/.bash_profile; cd ${OLDPWD-.}; bash"
```
另外需要把esc快捷键取消掉，不然 terminal 中vim无法从insert模式变成normal模式

