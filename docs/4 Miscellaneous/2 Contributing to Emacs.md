---
date: 2015-06-29 17:10
status: draft
title: '2 Contributing to Emacs'
---

Contributing to Emacs

Please consider contributing patches to Emacs, even if you’re an Emacs novice. Especially if you’re an Emacs novice—no-one is better qualified to judge the introductory documentation. Contribute now, before Stockholm syndrome sets in!

Reporting a bug

M-x report-emacs-bug will open a mail buffer with a template ready for you to fill with information. You probably haven’t configured Emacs to be able to send email, but you can copy from this buffer and paste into your favorite email client.

Do follow the instructions in that buffer. In particular, make sure you can reproduce the bug having run Emacs with emacs -Q (to confirm that it is a bug in Emacs itself and not in your customizations or in a third-party package).

A simple documentation change

The documentation for ido-find-file says:

*Help*
C-j Select the current prompt as the buffer or file.
If no buffer or file is found, prompt for a new one.
 
-U:%%-  *Help*         32% L22    (Help View)-----------------------------------------------
 
After reading that I still didn’t know what C-j (at the ido-find-file prompt) actually does. It turns out to mean “use whatever you’ve typed verbatim” instead of using ido’s fuzzy matching. I’ll submit a patch to the documentation string.

Making a patch, the 1980s way

M-x find-function ido-find-file.

Save a copy of this buffer (with C-x C-w) to ~/.emacs.d/ido.el and another copy to ~/.emacs.d/ido.el.orig.

Modify ~/.emacs.d/ido.el. To test the change, evaluate (C-M-x) the defun form you changed.

Generate a patch with diff -cp ido.el.orig ido.el.[1]

ido.el.patch
*** ido.el.orig 2012-04-07 10:20:31.000000000 +0100
--- ido.el      2012-04-07 10:28:59.000000000 +0100
*************** (defun ido-switch-buffer ()
*** 4041,4048 ****
  RET Select the buffer at the front of the list of matches.  If the
  list is empty, possibly prompt to create new buffer.

! \\[ido-select-text] Select the current prompt as the buffer.
! If no buffer is found, prompt for a new one.

  \\[ido-next-match] Put the first element at the end of the list.
  \\[ido-prev-match] Put the last element at the start of the list.
--- 4041,4047 ----
  RET Select the buffer at the front of the list of matches.  If the
  list is empty, possibly prompt to create new buffer.

! \\[ido-select-text] Use the current input string verbatim.

  \\[ido-next-match] Put the first element at the end of the list.
  \\[ido-prev-match] Put the last element at the start of the list.
*************** (defun ido-find-file ()
*** 4128,4135 ****
  RET Select the file at the front of the list of matches.  If the
  list is empty, possibly prompt to create new file.

! \\[ido-select-text] Select the current prompt as the buffer or file.
! If no buffer or file is found, prompt for a new one.

  \\[ido-next-match] Put the first element at the end of the list.
  \\[ido-prev-match] Put the last element at the start of the list.
--- 4127,4133 ----
  RET Select the file at the front of the list of matches.  If the
  list is empty, possibly prompt to create new file.

! \\[ido-select-text] Use the current input string verbatim.

  \\[ido-next-match] Put the first element at the end of the list.
  \\[ido-prev-match] Put the last element at the start of the list.
-U:---  ido.el.patch   Top L1    (Diff)-----------------------------------------------
 
Making a patch in the 21st century[2]

Ideally you’d make the patch against the latest (unreleased) version of the Emacs source code, instead of version 24.1. Someone else might even have fixed your problem already! The Emacs source code is hosted at savannah.gnu.org and you can grab the source code from the official Bazaar repository:

bzr branch bzr://bzr.savannah.gnu.org/emacs/trunk

Or, if you prefer git, from the git mirror (which can allegedly be up to one day behind the Bazaar repo):

git clone git://git.savannah.gnu.org/emacs.git

Note that either one of the above commands will take quite a while, but you’ll only do it once. I’ll use git in the following instructions.

Create a private (local) branch for your changes:
git checkout -b ido-el-documentation --track master

Make the change on the source files directly. Test.

git commit -am "* ido.el: Documentation for C-j in ido-find-file and ido-switch-buffer."

git format-patch master

In future, before starting on another change you’ll want to pull the latest changes from upstream:

git checkout master
git pull --rebase[3]

Send the patch

Send the patch to the same email address as M-x report-emacs-bug. But first read the instructions for contributing to Emacs and the Emacs Lisp tips and conventions.

Contributing to third-party packages

Try to find contribution guidelines on the package’s website: Where to email your patch, the preferred formatting of the patch and its changelog message, etc.

If the package is hosted on github, the package maintainers may be open to pull requests via github’s interface.

[1]: Don’t ask me why the Emacs maintainers don’t like unified diffs (diff -u). They specifically ask for diff -c.

[2]: The 21st century requires half a gig of disk space and a 30-minute download.

[3]: The --rebase is to avoid complicated merges in the history if you have local un-pushed commits on the master branch. Though if you’ve followed these instructions, your local changes should only ever be on private branches, not on master.

Next: Ergonomics