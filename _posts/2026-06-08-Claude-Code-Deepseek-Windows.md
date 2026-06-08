---
title: "Claude Code + Deepseek 安装手册（Windows）"
date: 2026-06-08 16:56:13 +0800
categories: [技术]
tags: [Claude Code, Deepseek, 教程]
---
Claude Code + Deepseek 安装手册（Windows）
接下来，我将用最直白、最简洁、最不绕弯子、绝不拐弯抹角、绝不拖泥带水的方式来介绍怎么部署Claude Code + Deepseek，全程稳稳托住、妥妥接住你😀(doge)
一、安装运行环境
下载安装node.js（官网链接：https://nodejs.org/zh-cn/download）
![图片 1](/assets/img/feishu/Claude Code + Deepseek 安装手册（Windows）/img_01.png)

下载安装包并安装，可以修改node安装路径，其余步骤一律next就行。安装完成后，在cmd中输入以下命令来验证是否安装成功。

node -v
# 能弹出版本号就是安装成功了
npm -v
# 能弹出版本号就是安装成功了
设置国内镜像源
输入以下命令配置镜像源

npm config set registry https://registry.npmmirror.com
可以输入以下命令检验是否配置成功

npm config get registry
# 返回https://registry.npmmirror.com，就代表安装成功了
下载安装git
在官网下载对应配置的安装包（官网链接：https://git-scm.com/install/windows），一般的win电脑都是x64配置的。
![图片 2](/assets/img/feishu/Claude Code + Deepseek 安装手册（Windows）/img_02.png)

下载安装包后进行安装，一直next就可以（安装路径可以自行修改，非必选项）。安装成功后，重新打开cmd，输入以下命令验证是否安装成功

git -v
# 如果弹出git版本号，就代表安装成功了，例如：git version 2.49.0.windows.1
下载安装cc.switch（GitHub仓库链接：https://github.com/farion1231/cc-switch）
打开release链接（https://github.com/farion1231/cc-switch/releases/tag/v3.15.0），一直下翻，找到assets，如下图所示。Windows系统选msi版本的安装包，点击下载。
![图片 3](/assets/img/feishu/Claude Code + Deepseek 安装手册（Windows）/img_03.png)

下载安装包后进行安装，一直next就可以（安装路径可以自行修改，非必选项），安装成功后会打开如下应用界面。
![图片 4](/assets/img/feishu/Claude Code + Deepseek 安装手册（Windows）/img_04.png)

二、安装部署Claude Code
打开梯子（最好使用美国的VPN节点）后，在cmd中输入以下命令来安装Claude Code。

npm install -g @anthropic-ai/claude-code
安装后cmd界面如下图所示，就代表安装成功了。
![图片 5](/assets/img/feishu/Claude Code + Deepseek 安装手册（Windows）/img_05.png)

重启cmd，输入以下命令，进一步验证安装效果。

claude --version
# 弹出版本号就代表安装成功了，例如2.1.150 (Claude Code)
三、配置Claude
在C盘的个人用户文件夹内找到.claude.json（每个电脑的个人用户名可能会不同，也可以直接在c盘查找文件）。

PS：一定要在把查看里的“文件拓展名”和“隐藏文件”都勾选上，否则这个文件不显示。
![图片 6](/assets/img/feishu/Claude Code + Deepseek 安装手册（Windows）/img_06.png)

使用记事本打开这个文件，在配置文件中增加一个新字段。

"hasCompletedOnboarding":true
最后文件配置如下，保存文件。
![图片 7](/assets/img/feishu/Claude Code + Deepseek 安装手册（Windows）/img_07.png)

打开cmd，输入以下命令，即可打开Claude

claude

PS：出现这个提示后，按Enter键相信文件夹即可。
![图片 8](/assets/img/feishu/Claude Code + Deepseek 安装手册（Windows）/img_08.png)

这样就能正常打开Claude了。
四、接入deepseek API
打开deepseek API平台（官方链接：https://platform.deepseek.com）
创建一个API key，然后记录保存key

PS：仅创建的时候可复制粘贴，之后无法查看和复制这个key，只能重新创建新的key才行了。最好不要把个人的key发给其他人，不然token计费走的是自己的账户。
![图片 9](/assets/img/feishu/Claude Code + Deepseek 安装手册（Windows）/img_09.png)

在cc switch中配置API
点击右上角的加号，添加供应商
![图片 10](/assets/img/feishu/Claude Code + Deepseek 安装手册（Windows）/img_10.png)

选择DeepSeek
![图片 11](/assets/img/feishu/Claude Code + Deepseek 安装手册（Windows）/img_11.png)

然后下滑，填写刚刚创建的key
![图片 12](/assets/img/feishu/Claude Code + Deepseek 安装手册（Windows）/img_12.png)

以下字段是设置接入的deepseek模型型号的，新版的cc switch管理更新，目前deepseek可选用的模型分别是deepseek-v4-pro和deepseek-v4-flash，可以根据需求选择不同的模型型号。pro版本有折扣，两个模型相差价格不大。我个人使用体验是，flash就能cover一般的代码需求。如果需要更大的上下文窗格，可以勾选1M的上下文，否则默认是32K。
![图片 13](/assets/img/feishu/Claude Code + Deepseek 安装手册（Windows）/img_13.png)

然后点击“添加”，就可以保存deepseek模型接入开关了。
打开模型接入开关，开启Claude
![图片 14](/assets/img/feishu/Claude Code + Deepseek 安装手册（Windows）/img_14.png)

在cmd中运行claude命令后，即可用deepseek接入Claude Code了。

PS：在使用期间，CC Swtich最好一直在后台运行，不要关闭。这个插件需要的后台资源很小，可以放心使用。
![图片 15](/assets/img/feishu/Claude Code + Deepseek 安装手册（Windows）/img_15.png)

五、Vscode插件（可选，另一种使用方式）
大家也可以在vscode中下载Claude插件，也能自动被CC Swtich管理接入的模型。
![图片 16](/assets/img/feishu/Claude Code + Deepseek 安装手册（Windows）/img_16.png)

跟着前文的操作部署成功后，下载该插件后就可以使用了，使用效果如下图所示。
![图片 17](/assets/img/feishu/Claude Code + Deepseek 安装手册（Windows）/img_17.png)

以上就是安装手册，如有问题，欢迎随时与我联系（885112006@qq.com）

