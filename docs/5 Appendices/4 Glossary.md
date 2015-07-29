---
date: 2015-06-29 17:14
status: draft
title: '4 Glossary'
---

Glossary

Emacs key names:
C-x	means Control-x.
C-x C-s	means Control-x, then Control-s.
C-x k	means Control-x, then k on its own (without Control).
M-x	means Alt-x (Alt is called Option on some Mac keyboards).
The M stands for “Meta” which is presumably what the Alt key was called in the 70s.
M-x help	means press Alt-x, then type help, then press return.
C-M-x	means Control-Alt-x
S-x	means Shift-x
s-x	means Command-x (the ⌘ Command key on Mac keyboards).
The s stands for “Super”, from the days when keyboards had Meta, Super, and Hyper keys.
<RET>	means the Return or Enter key.
<SPC>	means the Space bar.
<DEL>	means Backspace
(not to be confused with the delete-forward key)
Frames:
rl.c
int
main (argc, argv)
     int argc;
     char **argv;
{ 
--:---  rl.c          59% L83   Git-master  (C/l Abbrev)-----------------------------------
make -k
rm -f rl.o
gcc -DHAVE_CONFIG_H -DREADLINE_LIBRARY -DRL_LIBRARY_VERSION='"6.2"'  -I. -I.. -I.. -g -O -c rl.c
rl.c: In function ‘main’:
-U:%*-  *compilation*   Top L1     (Compilation:exit [2])-----------------------------------
Compilation exited abnormally with code 2
One frame, two windows
(each with its own modeline)
What Emacs calls your window-manager’s windows. [Emacs manual]

Windows:
Subdivisions inside a Frame. [Emacs manual]

Buffers:
The text you are editing. The difference between a buffer and a file is that their contents may differ until you save your changes; and some buffers aren’t backed by files at all (e.g. a *compilation* or *Help* buffer). [Emacs manual]

Mode line:
Describes the buffer in the window above the modeline. Hover your mouse over each indicator in the modeline for a description. [Emacs manual]

Echo area:
The line at the very bottom of the frame, used to display small informative messages. [Emacs manual]

Minibuffer:
The echo area when it is prompting for user input. [Emacs manual]

The point, the mark and the region:
The point is where the cursor is. Set the mark with C-<SPC>, move the point, and now the region is the area between point and mark. [Emacs manual]

Killing and yanking:
Killing = cutting. Yanking = pasting (sorry vi users!). [Emacs manual]

Major mode or Editing mode:
A major mode is a customization of Emacs for editing text of a particular sort (e.g. a particular programming language). A buffer can only have one major mode active at a time. [Emacs manual]

Minor mode:
Multiple minor modes can be enabled at once; they add functionality ranging from syntax highlighting to automatic spell-checking to modifying the behavior of common Emacs commands. [Emacs manual]