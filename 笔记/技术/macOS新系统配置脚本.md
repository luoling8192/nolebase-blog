---
title: macOS新系统配置脚本
date: 2023-10-27 16:31:00
tags:
    - macOS
    - Homebrew
---

# macOS 新系统配置脚本

## 介绍
最近在玩黑苹果，因为系统不太稳定，就总需要重装，而且我不太喜欢用时间机器恢复数据。

于是我就写了一个脚本来进行一些重装以后的操作，比如安装需要的软件，改一些系统配置之类。

如果 App Store 有的应用我会默认使用它来安装，其次再使用 Homebrew 安装软件，比如微信之类的（装在容器里更放心）。

你可以自己修改这个脚本来适应你的情况。

<!--more-->

## 代码
```bash
#!/bin/sh

sudo -v

# 修改系统配色
defaults write -g NSColorSimulateHardwareAccent -bool true
defaults write -g NSColorSimulateHardwareEnclosureNumber -int 4

# Dock
defaults write com.apple.dock "tilesize" -int 50
defaults write com.apple.dock "magnification" -int 1
defaults write com.apple.dock "largesize" -int 58
killall Dock

# Finder
defaults write com.apple.finder "ShowPathbar" -bool true
defaults write com.apple.finder "_FXSortFoldersFirst" -bool true
defaults write com.apple.finder "_FXSortFoldersFirstOnDesktop" -bool true
killall Finder

# Trackpad
defaults write com.apple.AppleMultitouchTrackpad "FirstClickThreshold" -int 0
#defaults write com.apple.driver.AppleBluetoothMultitouch.trackpad "Clicking" -int 1
defaults write com.apple.AppleMultitouchTrackpad "TrackpadThreeFingerDrag" -bool true # 三指拖移

# 安装Xcode工具
if ! command -v git > /dev/null 2>&1; then
    xcode-select --install
fi

# 安装Homebrew
if ! command -v brew > /dev/null 2>&1; then
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
fi

# 安装zim:fw
if ! command -v zimfw > /dev/null 2>&1; then
    curl -fsSL https://raw.githubusercontent.com/zimfw/install/master/install.zsh | zsh
fi

# 安装桌面软件
brew install arc google-chrome visual-studio-code webstorm warp steam telegram moonlight postman notion neteasemusic

# 安装终端工具
brew install wget btop exa neovim neofetch python3 zulu

# 安装辅助软件
brew install stats monitorcontrol rectangle appcleaner # alttab

# 安装黑苹果工具
brew install hackintool opencore-configurator

# 安装GPG
brew install gnupg pinentry-mac

# 安装Node环境
brew install node pnpm

# 配置Node环境
pnpm setup
source ~/.zshrc

# 安装pm2
pnpm install -g pm2

# 添加alias
echo "alias ls='exa'" >> ~/.zshrc
echo "alias ll='exa -lah'" >> ~/.zshrc
echo "alias vim='nvim'" >> ~/.zshrc

# 安装字体，可以创建自己的字体包文件
# unzip fonts.zip -d ~/Library/Fonts/

# pm2初始化
pm2 startup
mkdir -p ~/Library/LaunchAgents
sudo pm2 startup launchd -u $USER --hp $HOME
#pm2 start "aria2c -c ~/.config/aria2/aria2.conf" --name "aria2"
#pm2 save
```

## 字体
关于字体包，我是自己下载了一些字体自己打包的。下面是字体列表：

- HarmonyOS Sans
- JetBrains Mono
- SF Mono Font
- SourceHanSerif
- SourceHanSans

注意，不要使用 Homebrew 安装字体。
