---

layout: default
author: lunarwaterfox
title: Git 对多个 ssh key 的管理
categories: git

---


# 使用 ~/.ssh/config

> [ssh config](https://www.ssh.com/ssh/config/)

通过设置 ssh 配置，来根据不同的 host，使用不同的 ssh key

```console
touch ~/.ssh/conifg
```

在 config 中写入

```text
# github
Host github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/path_to_github_ssh_key
```


# 使用 GIT_SSH

> [Git Environment Variables](https://git-scm.com/book/en/v2/Git-Internals-Environment-Variables)

如果设置了 GIT_SSH，git 会用它来进行 ssh 连接，这样可以指定 key 来操作 git

GIT_SSH 会执行以下命令

```console
$GIT_SSH [username@]host [-p <port>] <command>
```

GIT_SSH 本身不能再附带参数，所以使用 GIT_SSH 时一般要做一下 wrapper

例如，用指定 ssh-key clone github 的工程可以

```console
~ $ cd ~/directory_of_github_ssh_key
~ $ touch github_ssh
~ $ chmod +x github_ssh
~ $ ln -s ~/directory_of_github_ssh_key/github_ssh /usr/local/bin
```

github_ssh 的内容如下

```shell
#!/bin/sh

ssh -i ~/path_to_github_ssh_key $@
```

在 clone 时，这样使用

```console
~ $ GIT_SSH=github_ssh git clone git@github.com:your_project.git
```