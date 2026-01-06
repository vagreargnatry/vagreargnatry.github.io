+++
date = '2026-01-04T22:44:50+08:00'
draft = false
title = '使用 Hugo 和 Github Pages 搭建博客'
description = '在 Arch Linux 上的操作过程记录'
+++

## 大致过程

* 使用 `sudo pacman -S hugo` 安装 Hugo
* 跟随教程 https://gohugo.io/getting-started/quick-start/
* 在 GitHub 上建立一个名字是 `<owner>.github.io` 的仓库
* 跟随教程 https://gohugo.io/host-and-deploy/host-on-github-pages/

## Git 相关

* 在 .gitconfig 中使用 `includeIf` 来让某个文件夹使用其特定的 .gitconfig 文件
* 使用 ssh-keygen 生成密钥。然后在 GitHub 上的 SSH Keys 将公钥添加到 Authentication keys 和 Signing keys。可以通过 `https://github.com/<username>.keys` 得到这个用户的 SSH 公钥。
* 使用 ssh-agent 减少输入私钥密码的次数。跟随 https://wiki.archlinux.org/title/SSH_keys#SSH_agents 来启用 openssh 提供的 `ssh-agent.service` systemd 用户服务。再创建 `~/.config/environment.d/90-ssh-agent.conf` 并写入 `SSH_AUTH_SOCK=$XDG_RUNTIME_DIR/ssh-agent.socket`

## 插曲

我在使用 Hugo 时看到 Box-drawing characters 没对齐，想起我一周前使用 minikube 时也没对齐。于是怀疑是不是我的 locale 设置有问题。在 #archlinux-cn 群友的帮助下，我发现是这两个 Go 程序使用了同一个库。库作者非常热情，在我提交 issues 后很快就修改好了

* [Ambiguous width characters](https://www.unicode.org/reports/tr11/#Ambiguous)
