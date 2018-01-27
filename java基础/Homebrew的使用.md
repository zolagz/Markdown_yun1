###Homebrew的使用

Homebrew 的安装

	1.	安装方法极其简单，使用系统终端应用 Terminal 输入以下命令行（注意双引号）： /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
	2.	使用命令 brew help 测试，Homebrew 是否正确安装。
	3.	若输入命令提示：brew：command not found，则需要进行环境配置，若成功则跳过该步骤：
	1.	终端输入：sudo vim .bash_profile
	2.	在 .bash_profile 文件的末尾添加如下代码： export PATH=/usr/local/bin:$PATH
	3.	在 vim 模式下，按下 i键 进入编辑模式；编辑完成后，按 Esc键 退出编辑模式；输入 :wq 保存退出（ w 为 write 写入，q 为 quit 退出）;
	4.	刷新环境变量，输入命令：source .bash_profile
	5.	再次输入 brew help 测试。

Homebrew 常用命令

	•	软件安装命令，如 brew cask install alfred ，支持多个同时安装，用 空格 隔开。
	brew cask install <软件名> 
	
	•	软件搜索命令，支持关键字搜索。如果我们想安装一款软件 Alfred ，但不知道 Homebrew 是否支持安装该款应用，我们可通过该方法查询。如输入 brew cask search alf 会列出所有符合条件的结果。
	brew cask search <关键字> 
	•	更新 Homebrew，想要获取最新的包，首先得更新 Homebrew 本身。
	brew update 
	•	更新包，如 brew upgrade $highlight
	brew upgrade #更新所有的包 brew upgrade $<软件包> #更新指定的包 
	•	查看 Homebrew 下载的包存放路径
	brew --cache 
	•	列出已安装的包
	brew list 
	•	列出可更新的包
	brew outdated 
	•	清理旧版本的包，如 brew cleanup $wget
	brew cleanup #清理所有旧版本的包 brew cleanup $<软件包> #清理指定的旧版本包 brew cleanup -n #查看可清理的旧版本包 
	•	彻底卸载某个包，如 brew uninstall wget --force
	brew uninstall <软件包> --force 
	•	锁定某个不想更新的包，如 brew pin $wget
	brew pin $<软件包> #锁定指定包 brew unpin $<软件包> #取消锁定指定包 
	•	查看已安装包的依赖
	brew deps --installed --tree 
	•	查看包的信息，如 brew info $wget
	brew info $<软件包> #显示某个包信息 brew info #显示安装的包数量、文件数量以及占用空间 
