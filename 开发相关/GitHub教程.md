### 准备工作 ###

首先下载两个软件 ——

- **[TortoiseGit](https://code.google.com/p/tortoisegit/)**
 
- **[Msysgit](https://code.google.com/p/msysgit/)**


在**[github官网](https://github.com/)**申请自己的用户名和密码;

> 下载完成后可以在setting-contextMenu选项中设置右键菜单,将常用的命令拉到最前面

### 第一步:fork ###

	1. 网页中打开github主页,此时应为登陆状态.
	2. 搜索框中输入Pamakids找到公司代码库.
	3. 点击进入想要的代码库.
	4. 右上角有Fork按钮,点击后,就会将公司代码库添到你自己的代码库中.

### 第二步:clone ###

	1. 右键单击------选择 ”Git Clone”.
	2. 在URL一栏输入要克隆的远程路径,如 https://github.com/Pamakids/PamakidsLib.git.
	3. 点击确定.显示success则成功克隆.

### 第三步:setting ###

	1.	右键单击克隆下来的文件夹,点击 Settings 弹出面板.
	2.	选择 Remote 选项.
	3.	可设置远程地址, 如https://github.com/Pamakids/PamakidsLib.git, 默认名为origin.
	4.	按 Add New/Save 就可以添加其他的地址.

### 第四步:fetch ###
	这一步将公司远程的修改拿到,右键单击选择 Git-Fetch

### 第五步:merge

	将第四步拿到的修改合并到本地,右键单击选择 Git-Merge

### 第六步:改动代码 ###

	1. 如果修改了本地的代码,则需要commit一下,才能上传到自己的远程地址,这时文件夹左下角会显示红色的叹号.
	2. 右键单击,选择 Git—>commit ”origin”,显示commit面板.
	3. 在message里面可以随意填写本次提交的代号.
	4. 点击OK提交成功.

### 第七步:push; ###

	1. 改动之后的代码想要提交到你自己远程代码库中,需要 push 一下,
	2. Commit完成后,右键点击选择 "Git push" ,会弹出账号和密码输入框
	3. 这些账号同你在一开始申请的github账号信息一样;
	4. 填入后回车,则push成功
	5. 如果要融入公司代码库,需要 pull request 一下,发送请求信息给公司代码库.

### 提示 ###

> 在使用github时会有两套登录系统,ssh和http,必须从始至终只选择一套,以免不必要的麻烦;


Merge别人的代码
---

1. git checkout -b `kinnyoku87-dev` dev <br> 先创建一个新的分支并切换到该分支 
2. git pull https://github.com/`kinnyoku87`/PamakidsApps.git dev <br> 从别的分支里取最新的更新
3. git branches -v 获取分支列表
3. 修复好冲突后，git status 查看当前状态
4. git add . 把所有文件重新添加到库里
5. git checkout dev 切换到dev分支
6. git merge `kinnyoku87-dev` 本地合并分支

#### rest ####
- git log 查看合并日志
- git reset --hard commit_sha
- git reset --hard HEAD~5 重置5个commit


