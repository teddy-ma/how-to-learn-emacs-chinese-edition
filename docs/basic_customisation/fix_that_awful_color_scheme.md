---
date: 2015-06-17 22:57
status: draft
title: '2 Fix that awful color scheme'
---

Fix that awful color scheme

The default Emacs color scheme is affectionately known as “angry fruit salad”.

Default font

“Fonts” in the Emacs manual[1] says:

Click on ‘Set Default Font’ in the ‘Options’ menu. To save this for future sessions, click on ‘Save Options’ in the ‘Options’ menu.[2]

“DejaVu Sans Mono” is a good choice (also known as “Bitstream Vera Sans Mono”, or “Menlo” on OS X 10.6+). Do choose a fixed-width font, even if you like proportional fonts for programming. We’ll cover Emacs’s handling of proportional fonts later.

The “Save Options” menu command automatically modified your init file with the new settings (and told you so in the echo area, now in the *Messages* buffer if you missed it).

There is a known bug on OS X that prevents “Save Options” from saving changes to the default font. OS X users will have to use the customize-face mechanism I explain below, specifying default as the face name.

Syntax highlighting colors

Font Lock mode is the minor mode responsible for syntax highlighting. You can read up on font-lock mode if you want to figure out how Emacs decides what is a comment or a keyword or a variable, or how to add your own keywords.

But to merely change the colors:

Figure out the name of the face you want to change: describe-face (defaults to the face at point) or list-faces-display.

modify-face will prompt you for a face and for each attribute to change. Leave attributes as “unspecified” (the default) to inherit from the default face. When it comes to the foreground and background colors, you can use the predefined colors shown by tab-completion, or you can specify RGB hex values like #3f7f5f.

customize-face brings up the “easy customization” interface I warned you against in the previous chapter. Click the State button and select Save for Future Sessions. (You can make changes here too, but I prefer modify-face because it gives you tab-completion on the possible values for each attribute.)

Apart from describe-face, which I found in the Help menu, I found all these in the Emacs manual under “Faces”, “Standard Faces” and “Customizing Faces”.

Now let’s figure out how to bypass the “easy customization” interface:

Visit your init file to see what “easy customization” added. If you already had the file open, you may need revert-buffer to see the latest changes.

init.el
(custom-set-variables
  ;; custom-set-variables was added by Custom.
  ;; If you edit it by hand, you could mess it up, so be careful.
  ;; Your init file should contain only one such instance.
  ;; If there is more than one, they won't work right.
 )
(custom-set-faces
  ;; custom-set-faces was added by Custom.
  ;; If you edit it by hand, you could mess it up, so be careful.
  ;; Your init file should contain only one such instance.
  ;; If there is more than one, they won't work right.
 '(default ((t (:height 120 :family "Menlo"))))
 '(font-lock-comment-face ((t (:foreground "#3f7f5f")))))

You could configure the remaining font-lock faces by adding arguments to the custom-set-faces form, but they might be overwritten if you use the easy customization framework in future. Inspired by the name custom-set-faces, I searched for functions beginning with set-face and found set-face-attribute:

(set-face-attribute 'default nil :family "Menlo" :height 120)
(set-face-attribute 'font-lock-comment-face nil :foreground "#3f7f5f")
(set-face-attribute 'font-lock-string-face nil :foreground "#4f004f")
(set-face-attribute 'font-lock-constant-face nil :foreground "#4f004f")
(set-face-attribute 'font-lock-keyword-face nil :foreground "#00003f")
(set-face-attribute 'font-lock-builtin-face nil :foreground "#00003f")
(set-face-attribute 'font-lock-type-face nil :foreground "#000000")
(set-face-attribute 'font-lock-function-name-face nil
                    :foreground "#000000" :weight 'bold)
(set-face-attribute 'font-lock-variable-name-face nil
                    :foreground "#000000" :weight 'bold)
The above settings produce a very conservative dark-on-white color scheme. I like it because in many languages it highlights variable and function definitions but not their uses.

For all the face attribute names see “Face Attributes” in the Emacs Lisp manual. As to elisp syntax, keyword symbols like :foreground and :weight are constants (that evaluate to themselves so you don’t have to quote them).

diff-mode

While we’re on colors, let’s add some helpful coloring to diff-mode (which we came across earlier, in the section on the vc version control interface).[4]

init.el
(eval-after-load 'diff-mode
  '(progn
     (set-face-foreground 'diff-added "green4")
     (set-face-foreground 'diff-removed "red3")))
eval-after-load does what it says: The first time diff-mode is loaded, evaluate the following form. Emacs tends not to load every elisp package on startup, but waits until you actually use diff-mode to load it.

If you used (set-face-foreground 'diff-added ...) directly in your init file, you would get the error “invalid face diff-added”. You could explicitly load diff-mode (using require) in your init file, but that would increase Emacs’s startup time—ever so slightly—every time you run Emacs in the terminal for a quick job that won’t even use diff-mode.

eval-after-load takes a single form to evaluate, but we want to make two function calls, so we wrap them in progn which merely evaluates a bunch of forms sequentially. We quote it so that it doesn’t get evaluated right now, before even being passed to eval-after-load.

diff-mode.el.gz
 
;; provide the package
(provide 'diff-mode)
The first argument to eval-after-load has to match the name that the package provides (this usually matches the name of the command to enable the mode, but to double-check: C-h f diff-mode, follow the link to diff-mode.el.gz, and find the provide form near the bottom).

Color themes

Note: This section and the next, which deal with the color-theme package, were written for Emacs 23. color-theme is obsolete in Emacs 24, so I need to find a better example for installing third-party packages.

Maybe you already have a favourite color theme, like Zenburn or Solarized. There are Emacs implementations (Zenburn, Solarized) built on top of the color-theme minor mode. color-theme is a third-party package; it doesn’t come with Emacs.

Note that Emacs 24 provides a built-in mechanism for defining multiple color themes. Nevertheless, I’ll walk you through the process of installing third-party packages using color-theme and the Solarized theme as examples.

Emacs 24 also comes with a package management system, so—as long as the package’s author has taken advantage of the new mechanism—the manual installation procedure described below will not be necessary either.

Installing third-party elisp packages

I’ll assume that you are keeping your ~/.emacs.d under version control using git.[3]

color-theme’s installation instructions recommend using your package manager (apt-get install emacs-goodies-el or port install color-theme-mode.el, etc.) but I prefer to manage all my Emacs extensions in a single place. That place is my ~/.emacs.d directory, because I will eventually want an extension that isn’t provided by my system’s package manager.

Download the color-theme.6.6.0.tar.gz and extract into ~/.emacs.d.

git add color-theme.6.6.0;
git commit -m "color-theme 6.6.0 from http://www.nongnu.org/color-theme/";

Add the color-theme directory to your Emacs load-path:

init.el
(add-to-list 'load-path "~/.emacs.d/color-theme-6.6.0")
And actually load it:

(require 'color-theme)
Now you can select a theme with color-theme-select.

Installing packages from github

The Solarized color theme for Emacs is available on github. If you are keeping your ~/.emacs.d under version control using git, you can use git submodules to simplify the management of such third-party packages.

cd ~/.emacs.d;
git submodule add https://github.com/sellout/emacs-color-theme-solarized.git;

In future you can use git pull from ~/.emacs.d/emacs-color-theme-solarized/ to get the latest changes.

Add ~/.emacs.d/emacs-color-theme-solarized/ to your Emacs load-path, and require 'color-theme-solarized, as you did for the color-theme package.

Now you can activate the theme with color-theme-solarized-light (or -dark).

Increasing the font size

C-x C-+, C-x C--, and C-x C-0. See “Temporary Face Changes”.

variable-pitch-mode

You can toggle between fixed- and variable-width fonts in a particular buffer with variable-pitch-mode. The face to customize is variable-pitch.

To automatically enable variable-pitch-mode, add it to the hooks for all the major modes where you want it to take effect. For example, to text-mode-hook for editing plain text.

If you really want it everywhere, I suppose there’s no harm in setting the default face to a proportional font. The great advantage of variable-pitch-mode, though, is that it is so easy to switch between proportional- and fixed-width fonts when you come across some ascii art or an ascii table in a comment.

[1]: I found this—after some dead ends—with Info’s index command, typing “font”, tab-completing, and trying whatever looked promising. I could just have explored the Options menu instead, but—silly me—I had disabled the menus because people on the internet said “real Emacs power users disable the menus.” That might make sense on a text terminal, where you can’t click the menu anyway, but on OS X, where there’s only one menu bar at the top of the screen, it’s just silly. See the next footnote.

[2]: If you have disabled the menu, perhaps because you’re using an init file copied from someone else or something like the Emacs starter kit, you can re-enable it just for this session with M-x menu-bar-mode.

[3]: cd ~/.emacs.d; git init .; git add init.el; git status; git diff --cached;
git commit -m "My emacs init file."

[4]: This customization—with several others—is borrowed from the Emacs Starter Kit.

Next: General customization