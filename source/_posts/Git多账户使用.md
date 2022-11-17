---
title: Git多账户使用
date: 2022-11-16 21:00:19
tags: Git
---

### （个人）Git 与 （公司）TortoiseGit

------

​		由于我个人使用的是Git原生工具，公司使用的是局域网搭建的TortoiseGit仓库，但是都需要使用Git的功能,故在此记录一些要点。

1. 重新设置一下全局的git的信息

   `命令:git config --global --unset user.name`

   `命令:git config --global --unset user.email`

2. 生成git账户的ssh-key

   `命令: ssh-keygen -t rsa -C "自己的Github邮箱" -f ~/.ssh/github_rsa`

   由于公司使用的时本地局域网（TortoiseGit）即这里可以不生成公钥。

3. 使用ssh代理

   `命令:ssh-agent bash`

   `命令:ssh-add ~/.ssh/github_rsa`

   如果出现Identity added: /c/Users/Administrator/.ssh/github_rsa (xx@xx.com)代表添加成功。

4. 将公钥添加到自己的git账户中

   将github_rsa.pub的内容粘贴到自己的GitHub页面的SSH  Keys---->New SSH Key中去。

5. 在.ssh目录下，新创建一个config文件，去掉后缀的 txt文件，使用记事本打开

   `Host home.github.com`
   `Hostname github.com`
   `IdentityFile ~/.ssh/github_rsa`
   `User home`

   如果有多个账户 以上面的 方式 隔开 已组的方式添加即可。

   保存文件，测试。

   命令:ssh -T git@User字段对应的内容***home.***主机名对应的内容***github.com***

   出现 Hi.....表示配置成功

6. 使用Git Bash Here 工具进入 不同的本地仓库设置不同的用户名和邮箱。

   `命令:git config --local user.name “自己的github名”`

   `命令:git config --local user.email ”自己的github邮箱“`

   global 与 local 取决于 使用频繁度，比如公司 与 家里，公司用global 家里用 local，在家里就相反操作即可。





