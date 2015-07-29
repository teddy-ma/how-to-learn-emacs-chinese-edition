---
date: 2015-06-17 22:57
status: draft
title: '1 cc-mode customization'
---

cc-mode customization

I should probably lead you through the relevant parts of the Emacs, Emacs Lisp, and CC Mode manuals, but they’re just. so. long! The mere table of contents for the Emacs manual is longer than our previous “Unix/C workflow” chapter.[1]
我应该带领你了解 Emacs，Emacs Lisp 的关系，和 CC 模式的手册，但是它们是那么的。。。长！Emacs手册的目录都比我们的前一章 "Unix/c workflow" 要长。
Instead I’ll show you how I try to find the information I need from the Emacs source code (most of which, thankfully, is in Lisp rather than C). The manuals I dissed a moment ago are incredibly useful. It is cause for wonder that an open-source project has produced such comprehensive documentation! But it is impossible to read those manuals cover-to-cover; you must learn how to efficiently find the right information. We’ve already covered the basic tools for that, and now we will exercise them a lot more.

然而我会向你展示我是如何尝试去发现我需要的信息在Emacs的源码中的（谢天谢地，大多数都是lisp而不是C）。

By the way, c-mode, c++-mode, objc-mode, java-mode, and a few others are all aliases for cc-mode, but with slight configuration changes to support the respective languages. From here on I’ll use the name cc-mode.

顺便说一下， c-mode，都是 cc－mo de 的别名， 但是支持其他的

House coding style
家里的代码风格
Are you supposed to indent with 2, 3, 4 or 8 spaces, or with tabs? How big is a tab? Whatever you want, I’m going to assume it’s not the indentation that cc-mode provides by default, so that we can walk through the procedure for changing it.


Visit (open) a C or C++ file (you can use readline/examples/rl.c from the previous chapter).
访问(打开)一个 C 或者 C++ 文件 (你可以使用 reading/example/rl.c 在前面的章节)

Pressing TAB anywhere on a line indents it to the appropriate position according to the current indentation rules (to insert a literal tab, use C-q, a.k.a. quoted-insert).
在任何地方按下 TAB 

Let’s find out what Emacs does behind the scenes when we press TAB:

让我们看看 Emacs 在我们按下 TAB 时在后台做了什么:

C-h k TAB

TAB runs the command c-indent-line-or-region,
which is an interactive compiled Lisp function in `cc-cmds.el`.
TAB 运行了命令 c-indent-line-or-region, 这是一个交互的被编译的 Lisp 函数在 `cc-cmds.el` 中.

-U:%%-  *Help*         Top L1     (Help View)-----------------------------------------------
Type C-x 1 to delete the help window.
If you don’t see the link to cc-cmds.el, then you don’t have the elisp sources installed. Use your system’s package manager to install the emacs-el package and try again.

Follow the link to cc-cmds.el. This will position you right where c-indent-line-or-region is defined. Bring the whole definition into view (C-M-l) if necessary.

(defun c-indent-line-or-region (&optional arg region)
  "Indent active region, current line, or block starting on this line.
In Transient Mark mode, when the region is active, reindent the region.
Otherwise, with a prefix argument, rigidly reindent the expression
starting on the current line.
Otherwise reindent just the current line."
  (interactive
   (list current-prefix-arg (use-region-p)))
  (if region
      (c-indent-region (region-beginning) (region-end))
    (c-indent-command arg)))

Emergency elisp

A brief parenthesis is needed to explain the above code. Let’s start with the if statement at the end:

  (if region
      (c-indent-region (region-beginning) (region-end))
    (c-indent-command arg)))
For clarity, let’s simplify it to:

  (if region
      a
    b)
If you still don’t understand what that means, pretend that if is actually a function, and that it looks like this:

  if(region, a, b)
(Moving that parenthesis to the left of the function is probably the largest single barrier to Lisp’s adoption by the rest of the world.)[2]

What do the three arguments region, a and b mean? Make a guess, and then check the answer at C-h f if.

The definitions of COND, THEN and ELSE should be pretty clear. But what does it mean by “if is a special form”?

It turns out that if isn’t a regular function. The elisp rule for evaluation of “normal” forms—where “form” means a parenthesized “shape” like (a b c) or (a b (c d))—is to evaluate each argument, and then pass the resulting values to the function.

Let’s consider the function + (elisp doesn’t have special infix operators, so + is just a function). If x is a variable containing the value 5, and y is a variable containing the value 2, then the following two expressions are identical:

  (+ x y)
  (+ 5 2)
The function + never sees x; it only sees 5.

Evaluate the form (+ 5 2): Switch to the *scratch* buffer, type (+ 5 2) on a line of its own, and press C-M-x to evaluate the form and display the result in the echo area.

The very first element in the form (+, in this case) gets evaluated too. + is actually a variable[3] whose value is the function that adds numbers.

Anyway, back to if. if is a “special” form, which means that the elisp interpreter treats if as a special case. Upon reflection, it is obvious that the normal function evaluation rules are not suitable for if: We wouldn’t want to evaluate ELSE, with its possible side-effects, when COND is “true” (non-nil).

All this is explained in the Emacs Lisp manual:

C-h S if

In the *info* buffer press i (for index) and enter special forms.

Go back to the cc-cmds.el.gz buffer.

Enable the eldoc minor mode (M-x eldoc-mode). Now positioning the point over the if statement will show some brief documentation in the echo area.

Enable show-paren-mode too. This should help clarify where the THEN and ELSE clauses begin and end.

Now, back up to the defun:

(defun c-indent-line-or-region (&optional arg region)
  "Indent active region, current line, or block starting on this line.
In Transient Mark mode, when the region is active, reindent the region.
Otherwise, with a prefix argument, rigidly reindent the expression
starting on the current line.
Otherwise reindent just the current line."
  (interactive
   (list current-prefix-arg (use-region-p)))
  (if region
      (c-indent-region (region-beginning) (region-end))
    (c-indent-command arg)))
You now have three ways to get help on defun: eldoc’s summary in the echo area, the reference provided by C-h f, and the more comprehensive Info manual at C-h S. Take your pick.

If you run across the word “lambda”, it’s the same as the “function” keyword in javascript for an anonymous function.

To reiterate:

defun defines a function named NAME.
ARGLIST is a list of arguments for the function. In elisp, a list is enclosed in parens: (a b c). In this case it isn’t evaluated as a function call, because defun is a special form that treats this particular list in a special way. When defuning a function that takes no arguments, ARGLIST would be the empty list ().
The optional DOCSTRING is used by the C-h f help system (yes, even for functions you define yourself!).
BODY is one or more lists that are evaluated when you call the function.
…except for the (interactive ...) form.
C-h S interactive

Don’t get too bogged-down by the explanation; reading the first two paragrahs is enough. Learn to find just the information you need, or you will be easily overwhelmed. Right now we don’t need to know how to use interactive, only what it means.

Configuring indentation

So. c-indent-line-or-region is a function that optionally takes arguments arg and region, which, when the function is called interactively (for example by pressing TAB), are set to the prefix argument (if specified with C-u or similar) and “true” if the region is active.

Right now we care about indentation when operating not on the region but on a single line (i.e. region is nil), so let’s look at the ELSE clause:

  (if region
      (c-indent-region (region-beginning) (region-end))
    (c-indent-command arg))
Use find-function to jump to the definition of c-indent-command.

This is a long and scary function, but luckily it has a good documentation string. Now that we know the name of this function, we can view the same documentation in a help buffer, with C-h f c-indent-command.

The documentation mentions a couple of interesting variables: c-basic-offset and indent-tabs-mode.

C-h v c-basic-offset

That talks about “buffer-local” and “file local” variables. What?

C-h S buffer-local

From the elisp Info node for Buffer-Local Variables, search for “file local” (you can use C-s or the index i).

As you can see you sometimes have to try different searches to find the right information. “Buffer-local” happened to be in the index, so the symbol search (C-h S) found it; “File local” has a space, but the symbol search doesn’t allow spaces, so you had to search from within the Info buffer itself. You could also have gone up (u) from the “Buffer-Local Variables” Info node and scanned the “Variables” table of contents.

Let’s check the current value of c-basic-offset:

Switch to buffer rl.c (c-basic-offset is buffer-local, so it matters which buffer we’re in).

M-x eval-expression RET c-basic-offset RET

Now change it to 4:

M-x set-variable c-basic-offset 4

Find a line to re-indent and press TAB.

Repeat the same investigation with variable indent-tabs-mode.

Setting variables from elisp code

Switch to the *scratch* buffer.

Anything starting with a ; is a comment.

Type in this form and evaluate it with C-M-x:

(set indent-tabs-mode nil)

You triggered an error, and Emacs brings up the backtrace in an elisp debugger. You tried to set a constant to nil, which is clearly an error.

Explain why this happened, with your knowledge of the previous value of indent-tabs-mode and of the elisp rules for evaluating functions.

We can quote the name of the variable so that it doesn’t get evaluated:

(set 'indent-tabs-mode nil)

Read more about quoting:

C-h S quote

C-h S setq

The following forms do exactly the same; the preferred form is setq.

(set 'indent-tabs-mode nil)
(set (quote indent-tabs-mode) nil)
(setq indent-tabs-mode nil)
One last thing: indent-tabs-mode is buffer-local, so setting it here only affects the *scratch* buffer. To make the change global, you must use setq-default.

Init file

Your changes to these variables will be lost when you restart Emacs. You need to put your settings into an initialization file that Emacs will load each time it starts.

In the Emacs manual (C-h r) table of contents, search for “init file” and read the first paragraph.

There are several places you can put your init file; I suggest the one that goes inside the ~/.emacs.d directory, so you can organize your customizations by keeping multiple elisp files in the same directory, and loading them from the main init file. Put this directory under version control.

Visit (open) the init file you’ve chosen (if the file doesn’t exist, Emacs will create it when you save the buffer).

Add the following lines:

(setq-default c-basic-offset 4)
(setq-default indent-tabs-mode nil)

(or whatever values you have chosen).

Restart Emacs, visit rl.c, and verify that your settings are in effect.

Hooks

By default, cc-mode automatically re-indents the line whenever you type a character like ; or }. These are called “electric characters” and you can disable this behavior in a particular buffer with c-toggle-electric-state (C-c C-l).

To always disable electric characters we can have Emacs call c-toggle-electric-state each time it loads cc-mode.

C-h m (from the rl.c buffer, or any other cc-mode buffer) to find out the names of the hooks provided by cc-mode.

A hook is a variable containing a list of functions to be run, usually upon entry to a particular editing mode. For C code we have two hooks: One for all of cc-mode’s supported languages, and one just for C. We’ll use the first one, c-mode-common-hook.

Add the following to your init file:

(defun my-disable-electric-indentation ()
  "Stop ';', '}', etc. from re-indenting the current line."
  (c-toggle-electric-state -1))
(add-hook 'c-mode-common-hook 'my-disable-electric-indentation)
First we defined a function that takes no arguments and calls (c-toggle-electric-state -1). Then we added the function to the c-mode-common-hook.

The -1 argument tells c-toggle-electric-state to disable, rather than toggle, the electric behavior (I learned this from C-h f c-toggle-electric-state; some functions might want nil, but this one wanted a negative number).

You could add an anonymous function to a hook directly:

(add-hook 'c-mode-common-hook
          (lambda () (c-toggle-electric-state -1)))
but then you have no way of referring to the function by name, so you can’t remove it from the hook with remove-hook.

The cc-mode style system

There is more to coding style than the size of indentation. Where should opening braces go? Should they be indented too?

The C-h v documentation for c-basic-offset mentioned a “style system”, and referred us to c-default-style.

C-h v c-default-style

c-default-style is a variable defined in `cc-vars.el'.
Its value is ((java-mode . "java")
 (awk-mode . "awk")
 (other . "gnu"))
Elisp syntax for a list is (a b c), and for a pair is (a . b). Pairs are called “cons cells” in lisp terminology, and you access the first element with the function car, the second with the function cdr (pronounced “could-er”).

So the value of c-default-style is a list containing 3 pairs; it’s used as a lookup dictionary where java-mode, awk-mode and other are the keys, and "java", "awk" and "gnu" are the values (in this case, names of styles to use for each of the editing modes represented by the keys). These lookup dictionaries are called “alists”.

C-h S alist

In your init file you could set c-default-style so that the default style for other[4] is something other than "gnu".

If you need to customize anything (including c-basic-offset and indent-tabs-mode) to something different than any of the built-in styles, I recommend you define your own style: Thus if you work on different projects with different styles, you will be able to switch easily (with c-set-style). The way we set c-basic-offset earlier will automatically create a style called “user”.

For help see “Configuration basics”, “Customizing indentation”, and “Sample .emacs file” in the CC Mode Manual.

Binding keys

I often come across source code where one file expects tabs to equal 8 spaces, and another file in the same directory—or even other lines within the same file—want a tab to be 4 spaces. Let’s create a function that cycles tab-width between 2, 4 and 8 spaces.

(I found the variable tab-width by using apropos-variable to search for “tab”.)

(defun my-tab-width ()
  "Cycle tab-width between values 2, 4, and 8."
  (interactive)
  (setq tab-width
        (cond ((eq tab-width 8) 2)
              ((eq tab-width 2) 4)
              (t 8)))
  (redraw-display))
The cond expression evaluates to 2 if tab-width equals 8; to 4 if tab-width equals 2; and to 8 otherwise. Look up cond and eq in the Info manuals if you like.

I’ve been naming all my functions “my-something” because elisp doesn’t have separate namespaces for each mode or package; this way I can be sure my functions won’t accidentally re-define an existing function that some editing mode relies on.

Now let’s bind our new function to a key sequence, so we can invoke it conveniently. C-c followed by a letter is reserved for users to define, so we’ll use C-c t:

(global-set-key (kbd "C-c t") 'my-tab-width)

The meaning of “global” in global-set-key should be obvious. If you’d like a keybinding just for cc-mode, use define-key to add the binding to the mode’s keymap:

(define-key c-mode-base-map (kbd "C-c t") 'my-tab-width)
I discovered c-mode-base-map with C-h v c-mode- TAB TAB. There is also a c-mode-map which is just for the C language, rather than all languages supported by cc-mode.

Associating file extensions with an editing mode

Say you want .h files to open in c++-mode rather than c-mode:

C-h v auto-mode-alist

C-h f add-to-list

(add-to-list 'auto-mode-alist
             '("\\.h$" . c++-mode))
-U:---  *scratch*      All L6      (Lisp Interaction)----------------------------------------
Regexp I-search: \.h$
The trickiest part will be getting the regular expression right—elisp doesn’t have syntax for a regexp literal, so you have to put it inside a string, and then the string backslash-escaping makes the regexp rather awful. See “Regexps” in the Emacs manual.

Homework

If you have a spare 30 minutes read Steve Yegge’s Emergency Elisp.

If you have a spare 6 months work through Structure and Interpretation of Computer Programs, a famous textbook from MIT that teaches important (and some quite advanced) programming concepts and techniques using a simple dialect of Lisp called Scheme. If you like “mathy” things like the Sieve of Eratosthenes for finding prime numbers, Heron’s method for calculating square roots, or estimating the value of pi using Monte Carlo simulation, then you’ll love this book.

If you had a collection of .emacs customizations collected from the web before you started this guide to Emacs, go over them now and try to understand them thoroughly.

In future, when you need a particular customization try to find the solution from the manuals or the elisp sources before reaching for Google.

Don’t try to modify the elisp files that are part of the Emacs distribution. For one, it won’t be easy to merge your changes when you update Emacs to a newer version. Second, the files are byte-compiled so you’d have to recompile them. Third, even if you did it still wouldn’t help because the core elisp functions are built into the Emacs image, so you’d have to recompile the whole program. Instead, use the variables and hooks provided for customization.

Don’t pay attention to the Emacs manual whenever it tells you to use the “Easy customization” facility (in some help buffers it will say “you can customize this variable”). It’s referring to a really awkward semi-graphical interface for setting Emacs variables. Best to keep all your customizations in your own init file.
