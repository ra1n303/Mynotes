# git源码泄露（.git）

Git是一个开源的分布式版本控制系统，在执行git init初始化目录的时候，会在当前目录下自动创建一个.git目录，用来记录代码的变更记录。当发布代码的时候，如果没有把.git这个目录删除，就直接发布到了服务器上，攻击者就可以通过它来恢复源代码

 

可利用工具：Githack

github项目地址：

[GitHub - lijiejie/GitHack: A `.git` folder disclosure exploit](https://github.com/lijiejie/GitHack)

修复建议：删除.git目录或者修改中间件配置进行对.git隐藏文件夹访问

 

 

也可以使用wget 下载文件





# svn源码泄露（.svn）

SVN是一个开放源代码的版本控制系统。

在使用SVN管理本地代码的过程中，会自动生成一个名为.svn的隐藏文件夹，其中包含重要的源代码信息。网站管理员在发布代码的时候，没有使用“导出”功能，而是直接复制代码文件夹到web服务器上，这就使.svn隐藏文件夹被暴露于外网环境，可以利用.svn/entries文件，获取到服务器源码

 

可利用工具：Svnhack

github项目地址：

[GitHub - callmefeifei/SvnHack: 一个Svn信息泄露辅助工具，可以使用这个脚本列取网站目录，读取源码文件以及下载整站代码。](https://github.com/callmefeifei/SvnHack)

 

修复建议：删除web目录中所有.svn隐藏文件夹，开发人员在使用SVN的时候，严格使用导出功能，禁止直接复制代码。





# hg源码泄露（.hg）

Mercurial是一种轻量级分布式版本控制系统，使用hg init的时候会生成.hg

 

漏洞利用工具：dvcs-ripper





# CVS泄露

CVS是一个C/S系统，多个开发人员通过一个中心版本控制系统来记录文件版本，从而达到保证文件同步的目的。

主要是针对CVS/Root以及CVS/Entries目录，直接就可以看到泄露的信息。

 

漏洞利用工具：dvcs-ripper





# 网站备份压缩文件

管理员将网站源代码备份在Web目录下，攻击者通过猜解文件路径，下载备份文件，导致源代码泄露。

 

常见的备份文件后缀：

.rar

.zip

.7z

.tar.gz

.bak

.txt

.old

.temp

 

漏洞利用工具：御剑







# WEB-INF/web.xml泄露

WEB-INF是java的web应用的安全目录，如果想在页面中直接访问其中的五年间，必须通过web.xml文件对要访问的文件进行相应映射才能访问。

 

WEB-INF主要包含以下文件或目录：

 

WEB-INF/web.xml : Web应用程序配置文件, 描述了servlet和其他的应用组件配置及命名规则.

WEB-INF/database.properties : 数据库配置文件

WEB-INF/classes/ : 一般用来存放Java类文件(.class)

WEB-INF/lib/ : 用来存放打包好的库(.jar)

WEB-INF/src/ : 用来放源代码(.asp和.php等)

通过找到web.xml文件，推断class文件的路径，最后直接class文件，再通过反编译class文件，得到网站源码







# DS_Store文件泄露（.DS_store）

.DS_Store是Mac下Finder用来保存如何展示文件/文件夹的数据文件，每个文件夹下对应一个。如果将.DS_Stroe上传部署到服务器，可能造成文件目录结构泄露，特别是备份文件，源代码文件

 

漏洞利用工具：ds_store_exp

Github项目地址：

[GitHub - lijiejie/ds_store_exp: A .DS_Store file disclosure exploit. It parses .DS_Store file and downloads files recursively.](https://github.com/lijiejie/ds_store_exp)







# swp文件泄露

swp即swap文件，在编辑文件时产生的临时文件，它是隐藏文件，如果程序正常退出，临时文件自动删除，如果意外退出就会保留，文件名为：.filename.swp

利用：直接访问.swp文件，下载回来后删掉末尾的.swp，获取源码文件。

 

 

也可以在linux中执行vim -r .filename.swp进行修复后读取