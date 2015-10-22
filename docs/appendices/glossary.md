---
date: 2015-06-29 17:14
status: draft
title: '4 Glossary'
---

## Emacs key names:


| 快捷键                                | 意义    | 说明                                                           |
|:------------------------------------|:-----------------------------------------------------------------|
| C-x        | C-x C-f and C-x C-s                                              |
| C-x C-s                    | C-x b and C-x C-b                                                |
| C-x k             | C-SPC                                                            |
| M-x                       | C-w, C-k, C-y, M-y                                               |
| 向前和向后搜索:                     | C-s, C-r                                                         |
| 通过名字调用命令:                   | M-x                                                              |
| 撤销:                               | C-/                                                              |
| 取消输入到一半的命令:               | C-g                                                              |
| 获得编辑模式的帮助，按键绑定和命令: | C-h m, C-h k, C-h f, C-h a (只要记住 C-h 然后阅读小缓冲区的提示) |
| 退出 Emacs:                         | C-x C-c                                                          |


## Frames:
Emacs 称作你的window管理的windows。【Emacs 手册】

## Windows:
一个 Frame 中的独立视图。【Emacs 手册】

## Buffers:
你在编辑的文字。一个缓冲区和一个文件的区别是，他们的内容可能是不同的直到你保存你的改变；并且有些缓冲区根本没有对应的文件
（比如 一个 *compilation* 或者 *Help* 缓冲区）。【Emacs 手册】

## Mode line:
Describes the buffer in the window above the modeline. Hover your mouse over each indicator in the modeline for a description. [Emacs manual]

## Echo area:
The line at the very bottom of the frame, used to display small informative messages. [Emacs manual]
frame 的最底下的一行，用来显示小的提供信息的消息。

## Minibuffer:
The echo area when it is prompting for user input. [Emacs manual]
提示用户输入的 echo area。

## The point, the mark and the region:
The point is where the cursor is. Set the mark with C-<SPC>, move the point, and now the region is the area between point and mark. [Emacs manual]
point 就是光标的位置。使用 C-<SPC> 来设置标记，移动光标，那么 region 就是 point 和 mark 之间。

## Killing and yanking:
Killing = cutting. Yanking = pasting (sorry vi users!). [Emacs manual]

## Major mode or Editing mode:
A major mode is a customization of Emacs for editing text of a particular sort (e.g. a particular programming language). A buffer can only have one major mode active at a time. [Emacs manual]

## Minor mode:
Multiple minor modes can be enabled at once; they add functionality ranging from syntax highlighting to automatic spell-checking to modifying the behavior of common Emacs commands. [Emacs manual]
