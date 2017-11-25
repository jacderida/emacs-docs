# Emacs Configuration

As of November 2017, I had been using Vim as my main editor for around 3 years. I put a lot of effort into customisation, and generally loved the experience using it. I came from a Microsoft background, so I had a history of using Visual Studio as my main development environment. After configuring Vim with plugins like [YouCompleteMe](https://github.com/Valloric/YouCompleteMe), I found I had a very reasonable approximation of a full IDE experience, and loved how light weight Vim was in comparison to Studio. Then, after learning of the existence of [Evil Mode](https://www.emacswiki.org/emacs/Evil) and watching [this talk](https://www.youtube.com/watch?v=JWD1Fpdd4Pc) by Aaron Bieber, I was inspired to switch to Emacs. Initially, I considered using Spacemacs, but came to the conclusion it was adding too much, and learning Emacs from scratch was better option for me.

This document describes my journey to configure Emacs in such a way that it replicated all the functionality I enjoyed from Vim.

## Installation and Enabling Evil Mode

### Linux

On Linux, Emacs is available on most distributions via a package manager, so installation is extremely simple. At the time of writing, my distribution of choice is Fedora, so installation is incredibly straight forward:
```shell
sudo dnf install -y Emacs
```

### Windows

For me, one of the main reasons for switching to Emacs is having a good cross-platform configuration. I'm not going to get into the details here, but although Vim is reasonably cross-platform, you can only get an approximation of the Linux based setup in a Windows environment.

To be filled out.

### Initial Setup and Enabling Evil Mode

The first thing to do is make extra package sources available. I added these lines to the `~/.emacs` file:
```lisp
(require 'package)

(add-to-list 'package-archives '("org" . "http://orgmode.org/elpa/"))
(add-to-list 'package-archives '("melpa" . "http://melpa.org/packages/"))
(add-to-list 'package-archives '("melpa-stable" . "http://stable.melpa.org/packages/"))

(setq package-enable-at-startup nil)
(package-initialize)
```

Important note: the trailing slashes at the end of those URLs are required; without them, Emacs won't be able to download the extra package lists.

Start Emacs, then use `M-x` to enter the following commands:
```
package-refresh-contents
package-install RET evil-mode
```

Run `m-x evil-mode RET` to enter Evil mode, and at this point, you'll then be able to use hjkl and all the core Vim functionality, including using `:qa` to exit Emacs.

After exiting, Emacs will have added some stuff to your `~/.emacs`:
```
(custom-set-variables
 ;; custom-set-variables was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 '(package-selected-packages (quote (evil-visual-mark-mode))))
(custom-set-faces
 ;; custom-set-faces was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 )
```

At this point I'm not quite sure what the purpose of this is.

Moving forward, anything I add to `~/.emacs` will be done before the code Emacs has generated. The next step is to permanently enable Evil mode:
```
(require 'evil)
(evil-mode t)
```

If Emacs is now restarted, Evil mode should be enabled.
