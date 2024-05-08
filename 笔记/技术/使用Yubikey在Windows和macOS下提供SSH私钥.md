---
title: 使用Yubikey在Windows和macOS下提供SSH私钥
date: 2023-11-04 17:43:22
tags:
    - yubikey
    - gpg
    - ssh
    - Windows
    - macOS
---

# 使用 Yubikey 在 Windows 和 macOS 下提供 SSH 私钥

看本文之前，你应该已经会使用 yubikey 官方的工具来创建 PIV 和 gpg-key。

<!--more-->

## Windows
根据[该教程](https://github.com/buptczq/WinCryptSSHAgent/blob/master/doc/wsl_tutorial.md)执行操作。

创建好PIV后，右键 `WinCryptSSHAgent` 托盘图标，点击 `Show Public Keys`，并把其添加到 Github 中。

根据你使用的环境，可以把对应的 Settings 添加到你的配置文件中。

** 可以把 `SSH_AUTH_SOCK` 加到系统的环境变量中，这样在 VSCode 里面也可以使用 **

可以把 `WinCryptSSHAgent` 添加到 `shell:startup` 目录下，以便做到开机自启动。

最后可以通过 `ssh -T git@github.com` 检查是否生效。

## macOS
macOS 无法使用 PIV 来生成 ssh-key，应该使用 gpg-agent 和 pinentry-mac 来实现。

首先，通过 brew 来安装 `brew install gnupg pinentry-mac`。

其次，确保你已经在 yubikey 中生成了 gpg 并把公钥导入到了本地，可以通过 `gpg -K` 来查看。

然后，在你的 `.zshrc` 中添加如下两行。

```bash
export SSH_AUTH_SOCK=$(gpgconf --list-dirs agent-ssh-socket)
gpgconf --launch gpg-agent
```

在 `~/.gnupg/gpg-agent.conf` 中添加如下几行。
```bash
pinentry-program /opt/homebrew/bin/pinentry-mac # 改成你自己的位置
enable-ssh-support
```

最后，通过 `ssh-add -L` 来查看是否生效。

如果遇到问题，使用 `gpg-connect-agent /bye` 来重新连接 gpg-agent。
