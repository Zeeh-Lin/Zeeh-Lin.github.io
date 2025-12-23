---
title: "WSL 下使用 SSH 访问 GitHub 受阻的配置经验"
excerpt_separator: "在某些网络环境下，直接通过 SSH 访问 GitHub 会受阻，比如 22 端口被屏蔽。这篇文章总结了我在 Windows + WSL 环境下，利用 **Clash Verge 的 TUN 模式** 配置 SSH 代理的经验。"
tags:
  - git
  - WSL
---

在某些网络环境下，直接通过 SSH 访问 GitHub 会受阻，比如 22 端口被屏蔽。这篇文章总结了我在 Windows + WSL 环境下，利用 **Clash Verge 的 TUN 模式** 配置 SSH 代理的经验。

---

## 背景
Windows 上安装了 Clash Verge 并启用 TUN 模式，Socks5 代理端口为 7897。Windows 系统下 Git 通过以下 SSH 配置可以成功连接 GitHub：
```
Host github.com
    HostName ssh.github.com
    Port 443
    User git
    ProxyCommand YOUR_GIT\mingw64\bin\connect.exe -S 127.0.0.1:7897 %h %p
```
我们的目标是在 WSL 下也能顺利使用 `ssh -T git@github.com`。然而我在 WSL 下直接设置环境变量：export ALL_PROXY="socks5://127.0.0.1:7897"
然后执行 `ssh -T git@github.com`，会得到：`Connection to github.com 22 port [tcp/ssh] succeeded!`然后卡住。

### 原因分析

默认 SSH 连接使用 22 端口，很多网络环境屏蔽了 22 端口。

### 解决方案
1. 将 Windows 的 connect.exe 添加到 WSL 路径在 WSL 的终端中，添加 connect.exe 的路径：export PATH=$PATH:/mnt/YOUR_GIT/mingw64/bin
2. 在 WSL 中修改 SSH 配置编辑 ~/.ssh/config，添加 GitHub 的代理配置：
```
Host github.com
    HostName ssh.github.com
    Port 443
    User git
    ProxyCommand /mnt/YOUR_GIT/mingw64/bin/connect.exe -S 127.0.0.1:7897 %h %p
```

### 注意事项

1. /mnt/c/... 是 WSL 访问 Windows 文件路径的方式。
2. Port 443 避免了网络屏蔽 22 端口的问题。ProxyCommand 指定了使用 Socks5 代理。
3. 测试连接执行以下命令测试连接：ssh -T git@github.com
如果配置正确，你会看到认证成功的提示，表明连接已成功通过代理。

## 总结经验

1. 端口选择很重要：22 端口可能被屏蔽，使用 443 端口更稳妥。
2. WSL 可以直接调用 Windows 可执行文件，路径使用 /mnt/c/...。配置一次后，无需每次导出环境变量，~/.ssh/config 会自动生效。

通过以上步骤，WSL 下的 SSH 可以顺利通过 Clash Verge TUN 代理 访问 GitHub，解决了网络限制问题。希望这个经验分享能对同样受限网络环境的开发者有所帮助。
下图是配置成功后 WSL 下的下载速度
![image](https://img2024.cnblogs.com/blog/3565016/202509/3565016-20250903111259262-416667978.png)
