---
date: 2015-06-29 17:10
status: draft
title: '1 Info documentation'
---

Info file search path

The Info manual tells us about variable Info-default-directory-list:

(eval-after-load 'info
  '(progn
     (push "/opt/local/share/info" Info-default-directory-list)
     (push "~/.emacs.d/info" Info-default-directory-list)))
The first is the directory for info files installed by my package manager (macports); modify the path according to your own system. The second directory is where we’ll install info files manually.

glibc

glibc (“gee lib cee”) is the GNU implementation of libc, the Unix standard C library. Don’t confuse it with glib, which is a library from the GNOME desktop project.

If you’re not developing for a GNU/Linux system, but you don’t find info-format documentation for your system’s own libc, you might still find the GNU docs useful. glibc follows the ISO C 99 and POSIX standards, and its documentation is always careful to point out where a feature is non-standard. Common Unix variations are covered (e.g. the BSD and SysV styles of signal handling).

Download “Info document (gzipped tar file)” from the GNU website.

Untar into ~/.emacs.d/info/

cd ~/.emacs.d/info
install-info libc.info dir

install-info is provided by the texinfo package, which you can install with your system’s package manager.

The generated dir file contains an entry for every function and constant in libc. These entries are added to your top-level Info menu. It is easy to remove these from the top-level menu (you will still be able to find them via Info’s index command or by searching): The dir file is plain text, so you can edit it and remove those entries. Leave the single libc entry under “Libraries”.

Now visit a C file and use info-lookup-symbol to look up, say, socket. Go up to the main sockets node for introductory material.

Python

Install pydoc-info.el (we covered elisp package installation in a previous chapter) and follow the instructions under “Setup and Install” in pydoc-info’s README.

Perl

I’ve found info docs for perl 5.6 (over a decade old now). If you know of a more recent version, or know how to build info files from the latest perl docs, please send me detailed instructions.

And spread the word! It’s a shame that such a well-integrated documentation system hasn’t received more support from popular software distributions.

