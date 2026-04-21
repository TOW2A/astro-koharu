---
title: 环境配置
link: environment-configuration
catalog: true
date: 2026-4-21 05:13:00
tags:
  - 笔记
  - cpp
  - C
  - golang
  - 环境配置
categories:
  - 笔记
---

# 开发环境配置

:::info
这篇文章将辅助您以TOW的习惯与标准搭建一个适合于C/CPP,Golang,nodejs,服务器运行维护为一体的环境
:::
:::warning
本文章适用于windows10及其以上系统
:::

## 基础开发环境

:::info
本文章默认使用者具备一定程度的代理能力,虽然部分网站不使用代理也可以访问,但代理可以有效加速过程并显著提高成功率
:::

### 配置VScode+Vim的编辑器组合

:::info
这里采用VScode+Vim是因为在终端环境下,VScode启动过于缓慢,且终端环境大部分时间是修改配置文件之类的短平快内容,在这种情况下,终端环境使用Vim无疑是最合适的选择
:::

[VScode](https://code.visualstudio.com/)使用微软官方的下载渠道即可,TOW的习惯是使用最新版本  
Vim将在配置包管理器环节下载

### 配置基础终端环境

:::info
在win11中系统自带{终端^Windows Terminal}应用,win10中并不自带,所以需要提前在微软商店中下载
:::

#### 安装pwsh

```bash
winget install --id Microsoft.PowerShell --source winget
```

在这里,我们只进行pwsh7的安装,编写配置文件之类更加具体的设置将在安装完毕相关软件后继续

### 配置包管理器

包管理器选择Scoop

+++primary 如何修改Scoop的下载,安装位置

进入`环境变量`选项卡  
在用户变量中添加`SCOOP`,变量值为你希望的安装路径  
在系统变量中添加`SCOOP_GLOBAL`,变量值同上

+++

#### 允许 PowerShell 执行本地脚本

```powershell
set-executionpolicy remotesigned -scope currentuser
```

#### 安装Scoop

```powershell
irm get.scoop.sh | iex
```

#### 安装Scoop必要程序

```bash
scoop install 7zip git vim
```

#### 导入Scoop第三方bucket

这里推荐导入`dorado`和`abyss`两个第三方bucket,`dorado`将作为后续安装LLVM-mingw的bucket,而`abyss`将提供大量可安装软件,同时,`abyss`也将提供==PSCompletions==,一款相当好用的powershell补全工具

```bash
scoop bucket add dorado https://github.com/chawyehsu/dorado
scoop bucket add abyss https://github.com/abgox/abyss
```

#### 导入Scoop官方bucket

这里推荐导入`extras`,有版本管理需求可以导入`versions`

```bash
scoop bucket add extras
```

#### 安装常见环境

其中,`llvm-mingw`提供==Clang==编译器和==Clangd=={LSP^Language Server Protocol}  
`nodejs`提供Node.js环境  
`GoLang.Go`提供Golang编译环境

```bash
scoop install abyss/abgox.PSCompletions
scoop install llvm-mingw
scoop install nodejs
scoop install GoLang.Go
```

在安装完毕以后可以通过

```bash
clang --version
```

验证`C/CPP`编译器是否就位

```bash
go version
```

验证`Go`环境

```bash
node -v
```

验证`Node.js`环境

### 配置终端环境

#### 安装oh my posh

`oh my posh`是一款类似于`on my zsh`的终端美化环境,安装相当的简单而且好看,有许多可选的主题,使用以下命令以安装

```bash
winget install JanDeDobbeleer.OhMyPosh --source winget
```

#### 安装字体

由于Scoop提供的字体实际上只是把字体下载到硬盘中,并没有安装,所以我们这里选择使用`oh my posh`安装字体

```bash
oh-my-posh font install JetBrainsMono
```

:::info
oh my posh也提供了{TUI^Terminal User Interface}界面,可以使用以下命令使用

```bash
oh-my-posh font install
```

:::

安装字体以后在{终端^Windows Terminal}中的设置-默认值-外观-字体,TOW的习惯是选择{NL^No Ligatures}

#### 编写终端配置文件

`pwsh`的配置文件是==$PROFILE==,可以使用任何喜欢的文本编辑器,但是TOW的习惯是使用`vim`在终端中编写

```powershell title="$PROFILE"
Import-Module PSCompletions # 导入PSCompletions
oh-my-posh init pwsh --config "catppuccin_mocha" | Invoke-Expression # 配置ohmyposh的主题
Set-PSReadLineOption -PredictionViewStyle ListView # 设置历史提示为列表形式显示
```
