---
title: 配置github的git bash环境
---

# 本地端配置

1. 生成ssh key: `ssh-keygen -t rsa -C "github_email@example.com"`
2. 提示输入名字，即新生成的ssh key 文件的名字
3. 生成ssh key的时候还要求输入私钥密码，在后边导入私钥的时候要用到
4. 将私钥添加到 ssh agent: `ssh-add /path/to/sshfile`。如果出现`Could not open a connection to your authentication agent.`，可以执行命令: `ssh-add bash`来解决

# github 端配置

Setting -> SSH and GPG keys-> New SSH key
将生成的ssh key文件中的 sshfile.pub中的内容拷贝到 key 中，title随意命名即可。
测试：`ssh -T git@github.com`
