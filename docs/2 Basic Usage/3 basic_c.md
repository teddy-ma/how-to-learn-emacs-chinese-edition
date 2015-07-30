---
date: 2015-06-18 23:11
status: public
title: '基本的 Unix/C 工作流'
---

本章使用了 GUN Radline 库的源码来作为操作的例子。你可以通过命令 `git clone git://git.savannah.gnu.org/readline.git` 来获取它，或者也可以使用你自己的 C 或者 C++ 代码。

## c-mode

启动 Emacs 并且打开(C-x C-f)文件 `readline/examples/rl.c`.

如果你不小心再次调用了 C-x C-f，或者其他等待你输入的命令，记住你可以通过按下 C-g (确保聚焦在 minibuffer) 来取消掉。

因为这个是一个 C 语言的源文件，Emacs 已经自动激活了一个编辑模式叫做 `c-mode`，它提供了一些自定义的按键绑定和 C 语言的缩进规则和语法高亮的知识。
我们在后面会更深入的学习 c-mode。

## shell

运行 shell 命令 (就是 M-x shell RET)。

这会创建一个新的缓冲区来运行环境变量指定的 shell。[^2] 这个缓冲区被称为 *shell* (星号是名字的一部分, 所以你不得不在使用 `C-x b` 切换时输入它们)。

cd 到 readline 的目录然后运行下面的命令：

``` shell
./configure
make
```
这个 *shell* 缓冲区的编辑模式是 shell-mode。你可以在 shell-mode 中运行任何 shell 命令；例外的情况是执行一些需要真正终端支持的，比如分页 (less) 和基于鼠标的程序。
你失去了 bash 的 readline 补全和其他任何自定义的 bash-completion 脚本, 或者在这个模型下你的 shell 中的等效选择(虽然 Emacs 提供了它自己的命令历史和 tab 补全)。出于这些原因，我倾向于使用一个真正的终端(在 Emacs 之外)运行一些任务，就像在一个 shell 中运行 Emacs 一样。 (尝试去在 Emacs 中做所有的事情就得不偿失了)。

shell-mode 的一大优势就是所有的输出都是可以被你搜索，复制，粘贴到的，或者其它行为，都可以用 Emacs 的标准命令来操作；你不需要移动你的手到鼠标那儿仅仅为了选择一些文字。

shell-mode 加入了一些按键绑定：

    输入 C-h m 然后阅读 shell-mode 的文档和它的按键绑定。

（目前为止, 不要操心去阅读帮助文档中关于自定义的部分 —  shell-mode 提供的变量和钩子去改变它的行为。在 shell-mode 之后的文档还有关于所有的激活状态的副模式的文档；你也不需要阅读它们）。

特别有趣的是按下 RET 或者 C-c RET 在前一个输入行中；C-<up> and C-<down> (or M-p and M-n) 来循环上一个命令；M-x 来循环目录。

    指出如何发送信号(比如: C-c) 给 shell。(提示：搜索帮助缓冲区的内容，关键字 "interrupt" 和 "stop")

    指出 C-M-l(字母 l, 不是1) 和 C-c C-s 是做什么的 (这些都是在运行一个 shell 命令产出大量的输出时很有用的)[^3]

## eshell

如果你不是特别的执着于 bash 或者其它任何 shell，除了 shell 以外你可以考虑使用 eshell，一个完全由 Emacs lisp 实现的 shell。先不说其他优势，在 Windows 上运行 eshell 不需要 Cygwin；你可以输入 lisp 代码或者通过名字直接在 shell 提示符上执行任何 Emacs 命令；而且你可以重定向命令输出到 Emacs 剪切板或者任何打开的 Emacs 的缓冲区。

我自己不使用 eshell，但是很多人会用。目前为止它可能是最简单的可以(运行在 Emacs 内部的)长期使用的 shell。

## ansi-term

在 Emacs 中运行一个 shell 的最后一个选择就是 ansi-term 了。这是一个完全的 terminal 模拟器，而且它将会把大多数的按键直接发送给 terminal 中运行的程序。
包含 TAB，所以会是 shell，而不是 Emacs，来执行 tab 补全。

C-x 和 C-c 仍然会被 Emacs 处理；对于 M-x 你就得输入 C-x M-x 来实现了。
你可以从 raw 字符模式转换到 line 模型来获得普通的 Emacs 行为（比如：移动光标周围这样你可以复制一些前面的输出）；直到你输入回车，没有东西会被发送给终端。

想要获得帮助，你不能使用 C-h m 因为 C-h 被传给 shell 了（很可能意味着回退）。应该使用 f1，或者通过名字调用 describe-mode 。

## shell-command

想要运行一次性的 shell 命令而不想打开一个完整的 shell 缓冲区的话，使用 shell-command （M-！）。

    M-! date RET

shell-command-on-region (M-|) 比较类似但是它是发送当前的选择区到 shell 命令的标准输入。
让我们使用这招来计算 rl.c 的 main 函数的行数吧：

    切换到 rl.c 的缓冲区（使用 C-x b）。

    移动光标到 main 函数（通过 C-s 来搜索）。现在移动光标让它位于 main 函数的左花括号前。

    按下 C-SPC 来设置标记。

    按下 C-M-f 来移动光标到对应的右花括号前面。现在选择区覆盖了 main 函数的整个方法体。

    M-| wc -l RET

如果你忘记了这些命令的名字，输入 M-x shell TAB TAB。

    阅读 shell-command-on-region 的帮助。你可以通过按键绑定 (C-h k) 或者函数名字 (C-h f) 来查找到。

这个帮助文档特别的长，但是重要信息都在顶部。无视编码系统那段，和一切关于非交互式的参数（就是关于从 lisp 脚本调用这个函数的，对应我们要使用的交互式调用）。

“Prefix arg means replace the region with [the output of the shell command]” 说的是提供命令一个数值参数。你将会在本教程中学习数值参数。在这个例子中参数的值并没有意义，所以 M-2 M-| 或者 C-u M-| 都可以。

    试试看！

现在撤销 (C-/) 你在缓冲区做的事情。注意显示模式的那行的指示器的变化当缓冲区有未保存的修改时。

## sh-mode

打开文件 file readline/configure。

如果你不是特别喜欢 shell 脚本，我很抱歉把你丢进一个 1200 行的程序生成的脚本，但是这是最接近我需要的例子了。

编辑 shell 脚本的模式叫做 sh-mode （对应运行交互式 shell 会话的 shell-mode，我们前面看过了）。如果 Emacs 没有辨认出那个文件是一个 shell 脚本
（也许它没有在第一行显示 shebang）你可以输入。。。你猜对了。。。M-x sh-mode。

我对 sh-mode 真没啥好说的。它会语法高亮和缩进各类 shell；并且它提供按键绑定来运行脚本，和插入某些 shell 结构（case，loops 等）和纠正当前 shell 的语法。
如果 Emacs 搞错了它（由于没有 shebang）你可以告诉 Emacs 去用哪个 shell。

现在你知道在哪里去发现如何解决问题了。

## Info documentation

回到配置脚本，调用 info-lookup-symbol 然后根据提示输入 test。

假设 bash shell 的 Info 文档被正确的安装在你的系统上（至少在任何 Linux 或者 OS X 系统上是这样的） 你将会看到一个 Info 窗口展示 bash 手册在 test 命令那行。

根据链接跳转到 『Bash Conditional Expressions』。如果你不想要使用鼠标，用 TAB 移到到下一个链接点然后回车也行。

和往常一样，C-h 将会显示所有的 info 模式的按键绑定。再说一次，不要去死记硬背所有的按键绑定，也不要通读单调乏味的 Info 教程。你所需要知道的就是：

* SPC 和 DEL 来滚动；当你达到了一个 『节点』（一页或者一个章节）的末尾，再次按下 SPC 就会顺序到达下一个节点。我必须警告你，DEL 有时会很淘气当你达到一个章节的第一个节点时。

* l（字母 ell）回退历史。

* u 或者 ^ 向上移动到最接近的目录。重复按下 u 会把你带到主 Info 目录，那包含所有的 Info 手册在你的系统中。（Emacs 相关的手册在互联网上可以能找到。）

* 使用常见的机制来搜索 (C-s 和 C-r)；当你达到一个节点的结尾时，再次按下 C-s 将会继续搜索余下的手册。

正如你可以在 Info 目录下看到的，有很多 Info 手册，包含 Emacs 自己的手册，一个 Emacs Lisp 的参考手册和介绍，和更复杂的 Emacs 编辑模式的手册，比如 cc-mode。

Info 文档简直太好用了。如果你在维护一个 Makefile 你可以查找深奥的符号比如 $@ 的意义。需要写一些 socket 相关的程序？在后面的章节中我们将会安装 glibc (the GNU implementation of the Unix standard library) 的 Info 手册。

info-lookup-symbol 使用当前缓冲区的模式来定义查看哪个手册。有时候它也说不清楚，所以它会提示你：输入 sh-mode 或者 makefile-mode 或者 c-mode
或者其他 （按下 TAB 来显示所有可选项）。使用前缀 C-u 来调用 info-lookup-symbol 的话它会总是询问你。

## compile

    有必要的话切换到 rl.c 缓冲区（使用 C-x b）。

移动光标到 main 函数（使用 C-s 搜索）。故意在 { 花括号那里制造一个错误，就像下面的例子一样，然后保存你的修改。

M-x compile (并且接受默认的 shell 命令 make -k)。


    让激活的鼠标仍然处于 rl.c 的窗口，输入 C-x 0（零）来最大化 *compilation* 窗口。

(C-x 1, 你在教程中学到过的, 隐藏了所有的窗口除了当前被选中的那个；C-x 0 做了相反的事)

    之后, 使用 C-x 2 和 C-x 3 C-x o (字母 o) 来在窗口之间跳转。

好了, 回到 *compilation* 窗口. Compilation-mode 当然有它自己的特定的按键绑定:

    使用 C-h m 来获得 compilation-mode 的帮助和它的按键绑定。 好消息是：这个模式的帮助文档很短。然后，不要去无聊的阅读所有的激活状态的副模式, 除非你确定你想看。

    试试看 compilation-next-error 和 compilation-previous-error 的按键绑定。如果 next-error-follow-minor-mode 听起来很迷人，也可以试试。


The compilation command is run from the buffer’s associated directory; normally this is the directory containing the buffer’s file. To run make on a different Makefile, you could specify the -C (--directory) or -f (--makefile) make options; or you can change the buffer’s default directory with M-x cd before M-x compile.

## rgrep

To search for all occurrences of a string (or regular expression) in a project, use rgrep.[4] This puts the matches in a new buffer, which you can navigate just like the compilation buffer (in fact, grep-mode inherits its functionality and keybindings from compilation-mode).
为了搜索所有的文档中的一个字符串（或者正则表达式）, 使用 rgrep[^4] 这输出一个 matches 在一个新的缓冲区中,
你可以像编辑缓冲区(事实上, grep-mode 继承了它的函数和按键绑定从 compilation-mode.


Use rgrep to search the readline source (*.[ch] files) for “rl_insert_comment” (make sure you search in the readline directory, not in readline/examples).
使用 rgrep 来搜索 readline 的源码

You can control the exact grep command line used: Read the documentation for rgrep to find out how, or use grep or grep-find instead. The C-h f documentation is an adequate reference, but the Info manual (C-h F rgrep) provides a better introduction.
你可以控制

## vc

Emacs has a set of commands, all starting with “vc-”, that provide a consistent interface to various version control systems. The readline files we are using in this tutorial are in a git repository, but the basic commands are the same for subversion or cvs. More advanced commands (such as altering repository settings, or pushing and pulling to remote repositories in git and other distributed vc systems) you will still have to do outside of Emacs.
Emacs 有一些列命令, 都以 "vc-" 开头, 它们为不同的版本控制系统提供了一致的命令. 我们正在教程中使用的 readline 文件是在一个 git 仓库下的, 但是基本命令和 subversion 或者 cvs 一样. 更高级的命令(比如更改仓库设定, 或者 push 或者 pull 远程仓库在 git 或者其他分布式 vs 系统中) 你仍然需要在 Emacs 之外进行操作.

Switch to buffer rl.c (with C-x b) if necessary.
如果有必要, 切换回 rl.c 缓冲区(使用 C-x b).

(If you find yourself missing clickable tabs for switching between buffers, remember that C-x b is more scalable—it works better when you have hundreds of buffers open. Later on we will see how to save a lot of typing under C-x b.)
(如果你发现你失去了可以在缓冲区之间切换的可点击的标签, 记得使用 C-x 的扩展性更强 - 当你有上百个缓冲区被打开时.
等会儿我们将会看到输入 C-x 会节省很多击键)

rl.c probably has changes from when we were messing with it earlier. Invoke vc-revert to, well, revert it to the latest version in the repository.
rl.c 大概已经有了很大的变化了. 调用 vc-revert 来撤销到仓库的最新版本.

Now for argument’s sake, let’s say we find rl.c’s documentation for the “-u” command-line flag somewhat confusing:
由于参数的缘故, 假设我们查询 rl.c 的

Find (with C-s) both instances of the documentation for “-u” and replace the word “unit” with “fd”. Save your changes.


Ask Emacs to show you a diff of this file. (If you need a hint, it’s in the first sentence of this section.)
让 Emacs 来展示你一个文件的 diff. (如果你需要一个提示, 它在本节的第一段)

The diff will be shown in diff-mode, which is also used for viewing patch files. diff-mode has some useful tricks—read its documentation when you have a spare minute.
diff 会在 diff-mode 中被显示, 它也会被用于查看补丁文件. diff-mode 有很多有用的诀窍 - 阅读它的文档如果你有空的话.

All the vc commands are bound to key sequences starting with C-x v. If you were paying attention when you invoked the diff command by its full name, you may have noticed that Emacs told you the corresponding keybinding.
搜有的 vc 命令都被限定在 C-x 开头的按键序列中. 如果你有想过通过全名来调用 diff 命令, 你可能已经注意到了 Emacs 告诉你对应的按键绑定了.

Press C-x v C-h to see all keybindings starting with C-x v.
按下 C-x v C-h 来查看所有的以 C-x v 开头的按键绑定.

This also works with any other prefix. Try it with C-x 4—you might notice some parallels with keybindings you already know.
这也能工作于其他的前缀. 试试 C-x 4 - 你可能注意到一些类似的你已经知道的按键绑定.

To commit the file, look into C-x v v (vc-next-action). Or enter vc-dir-mode with C-x v d and find out how to mark files, for performing vc- actions on multiple files at once.
为了提交文件, 看看 C-x vv (vc-next-action). 或者输入 vc-dir-mode 使用 C-x v d 并且找出如何标记文件, 为了能在多个文件一起执行 vc 行为.
vc has a (long!) section in the Emacs manual.
vc 在 Emacs 手册中有一个很长的章节.

Personally, I prefer to use the gitk and git gui tools, or git’s command-line interface directly, for adding, staging, committing, reverting, merging, branching. But I do use the Emacs vc-diff and vc-print-log (and vc-annotate!) extensively.
个人观点, 我更喜欢使用 gitk 和 git 图形界面工具, 或者直接使用 git 的命令行接口, 来  adding, staging, committing, reverting, merging, branching.  但是我会大量使用 Emacs 的 vc-diff 和 vc-print-log ( 和 vc-annotate!) .

Because vc is limited to the common denominator of the backend systems it supports, people have written custom modes for specific version control systems. Magit is one such mode for git; but as we haven’t yet learned how to install extensions, we’ll stick with vc. Anyway, vc can do a thing or two even magit doesn’t do—have I mentioned vc-annotate?
因为 vc 是被限制在通用特性的后端系统支持的, 人们已经卸了自定义的模式对于特定的版本控制系统. Magit 是就是一个这样的模式针对 git; 但是因为我们还没有学会如何去安装扩展, 我们还是坚持使用 vc. 无论如何, vc 可以做一到两件甚至 magit 也不能做的事情 - 我提到 vc-annotate 了吗?

## vc-annotate

A couple of sections ago, I asked you to grep the readline sources for “rl_insert_comment”.
在几章节之前, 我让你去 grep readline 的源码搜索 “rl_insert_comment”.

Switch to the *grep* buffer if you still have it around, or do a new search.
切换到 *grep* 缓冲区如果你还在绕着它, 或者做一次新的搜索.

Remember, when switching buffers with C-x b you have to enter the asterisks as part of the buffer name. (Incidentally, the asterisks are for buffers not associated with a file on disk. This is just a convention; you could always rename the *grep* buffer to something without asterisks —see rename-buffer— or save it to disk if you wanted to.)
记得, 当使用 C-x b 来切换缓冲区时, 你不得不输入星号作为名字的一部分. (对了, 缓冲区的星号和磁盘上的文件并没有关联. 这只是一个管理; 你总是可以重命名 *grep* 缓冲区为一个不带星号的名字 -- 查看 rename-buffer -- 或者保存到磁盘上如果你想要这样做的话).

The first hit should have been in emacs_keymap.c. Jump to that match.

Let’s find out which release of readline added the Meta-# keybinding:

Delete all Emacs windows other than the annotate window (you might also want to resize the frame so that it is large enough).
删除所有的 Emacs 窗口除了注解窗口(你可能也需要调整 frame 的尺寸让它足够大)

With point on the “Meta-#” line, press d to view the diff of the revision where this line was last changed.
把光标移到 "Meta-#" 这行, 按下 d 来查看这一行上一次修改的版本的 diff.

Now we’re in diff-mode. It seems that this revision consisted mostly of whitespace changes. Find the exact change to “Meta-#”. You can press C-c C-w to hide whitespace-only changes in the hunk surrounding point, and n to jump to the next hunk.
现在我们在 diff-mode 了. 看上去这个版本由大量的空格组成. 找到确切的 "Meta-#" 的改变. 你可以按下 C-c C-w 来
隐藏只有空格的改变在光标周围的大片地方, 然后按下 n 跳到下一块.

Nope, this revision wasn’t it. Press q to exit the diff buffer, then a to run annotate again, starting from the revision before this line’s revision.
不, 这个版本不是. 按下 q 离开 diff 缓冲区, 然后再次运行 annotate , 从在这一行的前面开始.
Now make sure your point is on the right line (if the new revision is different enough from the newer one, the line we’re interested in may have moved around). Then press d again, and in diff-mode n until you find the right hunk. Yes, this is the change we’re looking for!
现在确保你的光标在

Go back to the annotate buffer (q). Press l to view the log message for this revision. See, readline’s “Meta-#” binding dates back to version 2.1! If you want some context you can press D (that’s shift-d) to view the diff of all files changed in this revision.


Of course, annotate is more useful when the commits are more granular and the commit messages are more descriptive, with links to bug tracker entries and so on. But you get the idea.

Right now I don’t expect you to remember all these keybindings, but I do expect you to know how to find them when you need them.
现在我不期待你可以记住所有的这些按键绑定, 但是我希望你知道如何找到它们当你需要时.

## ediff

ediff is a more powerful mode for viewing differences between files or revisions.
ediff 是一个更强大的模式来查看不同的文件和版本的区别.

Switch to buffer rl.c if necessary.
如果有必要起换到缓冲区 rl.c.

M-x ediff-revision. Accept the default values of rl.c, its latest revision, and its current state.

This opens a new frame from which you control ediff; always make sure the ediff frame has focus when you’re giving it a command.

Press ? for help. Figure out how to step through each diff. Figure out how to show the files side-by-side. q when you’re done.

When invoking ediff-revision you can supply any two revisions, not just the latest revision and the current working copy. You can also diff any 2 buffers (ediff-buffers) or files (ediff-files).

I suggest you stay away from ediff-directories (if you want to know why, try using it).

## etags

etags is a program that indexes source files and creates a TAGS file that Emacs can use to find definitions of variables, functions and types.
etags 是一个程序用来索引源文件的, 然后创建一个 TAGS 文件这样 Emacs 可以使用它来找到定义的变量, 函数和类型.

Run the shell command make TAGS in the readline directory (either from a shell buffer, or with M-x compile).
运行 shell 命令 make TAGS 在 readline 目录中 (从一个 shell 缓冲区或者使用 M-x compile).

The Makefiles of most open-source projects include the TAGS target. For those that don’t, you can use a combination of find, xargs and etags from the shell to generate the TAGS file manually (read the man pages for those tools if you need to).
大多数开源项目的 Makefile 都包含 TAGS 目标. 你可以使用一个

The etags you just ran was probably the one that shipped with Emacs. There’s an alternate implementation called “Exuberant ctags” that supports more languages. Install it with your package manager (e.g. port or yum install ctags) and call it with ctags -e.
你刚才运行的 etags 是 Emacs 自带的一种. 也有其他的可选实现叫做 "Exuberant ctags", 它支持更多的语言.
使用你的包管理器来安装它(比如 port 或者 yum install ctag) 然后使用 ctags -e 来调用它.

Back in Emacs, invoke find-tag (M-.). Enter rl_insert_comment as the tag to find, and then the location of the TAGS file you just generated.
回到 Emacs, 调用 find-tag (M-.). 输入 rl_insert_comment 作为 tag 来查找, 然后你刚才生成的 TAGS 文件的位置.

From the help for find-tag, figure out how to jump back to the location you were at before invoking the command, and how to find the next match if there is more than one. (You might find the documentation from C-h F find-tag clearer than that from C-h f.)
根据 find-tag 的帮助, 找出如何回跳到你调用命令之前的位置, 和如何找到下一个符合的位置如果大于一个的话.(你可以找到文档从 C-h F find-tag 更清楚比从 C-h f).

To use a different TAGS file visit-tags-table.
要使用不同的 TAGS 文件, visit-tags-table.

If you’re a C++ programmer, you’ll soon find that ctags/etags is not perfect when it comes to classes and namespaces. As a 90% solution, it’s good enough, most of the time. Hopefully someone will come up with a clang-based indexer sometime soon.
如果你是一个 C++ 程序员, 你很快就会发现 ctags/etags 不是很完美当碰到类和命名空间时. 在 90% 的情况下它表现良好. 希望有人可以开发出一个基于 clang 的索引.

## gdb

We’re going to run the rl program under the GNU debugger, gdb.
我们要在 rl 程序中运行 GNU 吊事起, gdb.

First run compile in readline and then in readline/examples.
首先在 readline 中编译然后在 readline/examples.

The compilation should succeed,[5] as long as you haven’t introduced any errors in any of the source files. If you have, you know how to view and back out your changes.
只要你没有引入任何错误到任何源文件中, 编辑应该是成功的.
如果不成功, 你知道如何回滚.

M-x gdb. When prompted for the command-line to use, specify the program examples/rl as an argument to gdb: if you’re in the readline/examples directory, the command-line will look like gdb ‑‑annotate=3 ./rl. The default options for gdb may differ on your system so you may want to use them instead.

You are now running gdb inside Emacs, so the standard gdb commands apply. Let’s set a breakpoint in the readline function, start the rl program, and when we reach the breakpoint step through a few lines.
现在你正在运行 gdb 在 Emacs 中, 所以标准 gdb 命令运行着.
让我们设置一个断点在 readline 函数中. 开始 rl 程序, 我们会到达断点.

Type the following commands at the gdb prompt:

break readline
run
print rl_pending_input
next
step
backtrace
frame 1

Note how Emacs displays the corresponding source file and line in a separate window.

The above commands are all interpreted directly by the gdb program. gdb allows you to abbreviate them to r, p, n, s, bt, and f, respectively. Pressing return on an empty line repeats the previous command—useful for multiple next commands. help displays gdb’s built-in help.

For serious debugging, Emacs can open additional windows showing the current stack frames, breakpoints, local variables and registers. See the Emacs manual for details: C-h r opens the Emacs manual in the Info browser, then search for the chapter titled “GDB Graphical Interface”.
对于严肃的调试, Emacs 可以打开额外的窗口来显示当前的堆栈, 断点, 本地变量和注册者. 可以查看 Emacs 的手册获取更多细节: C-h r 开发 Emacs 手册在 Info 浏览器, 然后搜索章节标题 "GDB Graphical Interface".

## Homework

You may be feeling somewhat overwhelmed at this point. Spend a few days using Emacs as your main editor, painful though it may be, before moving on to the next chapter of this guide. If you’ve forgotten a command I’ve taught you, try to figure it out yourself (use C-h m, C-h a, C-h f, C-h k and C-h F) before looking it up here.
你现在可能觉得要奔溃了. 花几天时间来使用 Emacs 作为你的主力编辑器, 这个过程可能是痛苦的, 在查看本手册的下一章之前. 如果你忘了一个我教过你的命令, 试着靠你自己找出它 (使用 C-h m, C-h a, C-h f, C-h k and C-h F) 在查看这里之前.

As part of your commitment to learning Emacs, try to run your shell within Emacs as much as possible. (If you need help configuring your shell to work well within Emacs, or configuring Emacs to work well with your shell, we will revisit shell-mode in the “General customization” chapter of this guide.)
作为你学习 Emacs 的承诺之一, 尽量的尝试在 Emacs 中运行你的 shell.(如果你需要帮助来配置你的 shell 能在 Emacs 中工作, 或者配置 Emacs 来很好的在你的
shell 中工作, 我们将会重读 shell-mode 在 General customization”章节.


[^1]: You will need git, a source control system; the Git Book has installation instructions.
        你需要 git, 一个源码控制系统; Git Book 有安装说明.

[^2]: On Windows you will need a shell provided by Cygwin or similar.
       在 Windows 系统中你需要一个 Cygwin 或类似的东西提供的 shell.

[^3]: These keybindings aren’t arbitrary. C-c C-s mirrors C-x C-s, which you already know for saving the entire buffer to a file (“global” keybindings tend to start with C-x, whereas C-c is a prefix for mode-specific bindings). And C-M-l performs a similar function in other editing modes—in c-mode it tries to bring into view the whole function surrounding the point. If you’re feeling overwhelmed by the number of keybindings to memorize, don’t worry; you can always find them again with C-h m.

[^4]: On Windows you will need find and grep commands provided by Cygwin or similar.
    在 Windows 系统中你需要一个 Cygwin 或类似的东西提供的 shell.

[^5]: I haven’t tested this on Windows, but I’m assuming that Cygwin installs the necessary compiler, headers and libraries.
     我没有在 Windows 上测试过, 我假设 Cygwin 安装了必要的编译器, 头文件和库