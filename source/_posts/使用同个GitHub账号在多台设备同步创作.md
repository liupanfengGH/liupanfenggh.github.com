---
title: 使用同个GitHub账号在多台设备同步创作
date: 2022-11-14 20:40:52
tags: Web
---

前言:

​		由于想在家里和公司都能编写博客，即研究了一下如何实现多台设备同步的方案。

### 1.配置Github分支

1. 进入自己的github博客仓库

2. 在当前页面  点击Switch branches or tags 按钮

3. 在当前页面  点击View all branches

4. 在当前页面  点击New branch按钮

5. 在当前对话框页面 Branch name 下框输入 分支名字并点击Create branch按钮

6. 在当前页面 点击Settings按钮------>点击左侧的Branches文本按钮

7. 在当前页面  Default branch 说明处 点击 Switch to another branch按钮 选择创建的分支名称，并点击Update按钮---->确认更改默认分支。

   

### 2.克隆分支到本地

1. 复制仓库链接

2. 使用命令行工具进入自己的博客目录

3. 使用命令: git clone 粘贴仓库链接 ，等待clone完成 

4. 打开克隆的仓库目录 删除除了 .git 目录以外的所有文件及文件夹 (文件夹选项需要 勾选显示隐藏的项目)

5. 使用命令行工具进入本地仓库目录

   使用命令:git add -A

   使用命令:git commit -m "清理分支仓库"

   使用命令:git push origin 分支仓库名称

   等待删除完毕.

6. 将 .git 目录 剪切放入上层的博客目录,并关闭 命令行工具

7. 使用命令行工具打开 博客目录

   使用命令:git add -A

   使用命令:git commit -m "提交工程"

   使用命令:git push origin 分支仓库名称

   去自己的GitHub远程仓库查看

   

**此时仓库中有了 2个分支 1个是用于存放生成的博客静态资源文件(master)分支，1个是用于存放工程文件的(hexo)分支。工程分支 修改完毕 测试完成 提交到自己的（hexo)分支，根据需要 使用 hexo 命令 调试 和 发布到 (master)分支。**



### 3.多设备同步博客

1. 在另一台设备装了git工具包的机器中 使用 Git bash 功能将 远端仓库工程代码拉取下来
2. 将Hexo框架需要的工具包重新安装一遍(注:不要使用 hexo init 命令了，因为原来的机器已经配置好了)
3. 每次操作前拉取最新的 工程文件(hexo)分支 再进行修改
