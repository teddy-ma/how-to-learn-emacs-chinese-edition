---
date: 2015-06-21 21:46
status: public
title: 安装正确的Emacs
---

安装最新版的 [GNU Emacs]( http://www.gnu.org/s/emacs/) (目前是24.5)。不是XEmacs，也不是 EmacsW32，不是 AquaMacs，也不是 Carbon Emacs。

## Linux

Emacs 很可能已经安装了；如果没有，使用你的包管理器（yum, apt-get 等等）。

如果你的发行版只有旧的 Emacs 包，你可以尝试使用一个第三方的仓库（比如下面列出的），或者 [从源码构建最新的版本](http://www.gnu.org/software/emacs/emacs-faq.html#Compiling-and-installing-Emacs)

## OS X

如果你使用 [macports]( http://www.macports.org/)，安装 `emacs-app`，它给了你一个本地的 OS X 程序。默认情况下 macports 会安装`Emacs.app` 到 `/Applications/MacPorts`，如果你使用笔记本电脑，我建议你使用 `fullscreen` 变体（比如： `port install emacs-app +fullscreen`）来增加命令 `ns-toggle-fullscreen`。

如果你使用 [homebrew]( http://mxcl.github.com/homebrew/)：
`brew install emacs --cocoa`。

另外，你可以使用一个 [预编译的emacs](http://emacsformacosx.com/)，但是考虑下使用 macports 或者 homebrew 这样就可以很容易的使用它们来安装其他 Unixy 工具。

## Windows

可能你是一个 Windows 用户，但是我不是。我很抱歉如果后面的一些例子在你的系统上不能工作。因为我没有在 Windows 上测试过。

你可以从 <http://ftp.gnu.org/pub/gnu/emacs/windows/> 下载最新的 `emacs-xx.x-bin-i386.zip` 并且解压。然后在 `bin` 目录下运行 `addpm.exe`，记得用管理员权限。

有些 Emacs 功能需要 Unixy 工具比如 `find` 和 `grep`，你可以通过安装可移植性操作系统接口仿真环境比如 [Cygwin](http://www.gnu.org/software/emacs/windows/Other-useful-ports.html)。
你需要确认可移植性操作系统接口工具在系统 PATH 中，这样 Emacs 才能找到它们。

更多帮助参考 [GNU Emacs FAQ for MS Windows](http://www.gnu.org/software/emacs/windows/) 和 (经常过时的)[EmacsWiki]( http://www.emacswiki.org/emacs/MsWindowsInstallation)。

## 移除任何存在的 .emacs 配置文件。

如果你有任何存在的 Emacs 配置在一个 `.emacs` 文件或者一个 `.emacs.d` 目录中，你应该删除它，如果你想要你的 Emacs 和手册中的例子拥有一样的行为的话。
