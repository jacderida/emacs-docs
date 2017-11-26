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
```lisp
package-refresh-contents
package-install RET evil-mode
```

Run `m-x evil-mode RET` to enter Evil mode, and at this point, you'll then be able to use hjkl and all the core Vim functionality, including using `:qa` to exit Emacs.

After exiting, Emacs will have added some stuff to your `~/.emacs`:
```lisp
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
```lisp
(require 'evil)
(evil-mode t)
```

If Emacs is now restarted, Evil mode should be enabled.

### Add the 'use-package' Macro

The [use-package macro](https://github.com/jwiegley/use-package) simplifies installation and configuration of packages. Enable it with the following code:
```lisp
(unless (package-installed-p 'use-package)
  (package-refresh-contents)
  (package-install 'use-package))
(eval-when-compile
  (require 'use-package))
```

Restarting Emacs should install it.

There's some good general documentation on package management [here](https://www.emacswiki.org/emacs/InstallingPackages). This also details how to setup a proxy to pull the package sources, which is useful if you're in a corporate environment.

## Helm

Helm is a package that provides the functionality you get from Ctrl-P in Vim, and much beyond. It can be enabled with the `use-package` macro:
```lisp
(use-package helm
  :ensure t
  :config
  (require 'helm-config)
  (helm-mode 1))
```

The `:ensure` symbol will install the package if it doesn't exist, and the `:config` symbol is used to apply any package-specific configuration. The `require 'helm-config` setting provides all the default key bindings, presumably among lots of other things.

### Use and Configuration

As in the above configuration snippet, simply enabling Helm mode doesn't do very much. Even the official documentation doesn't make it obvious how you actually launch and use it. Expand the configuration with this:
```lisp
(use-package helm
  :ensure t
  :config
  (require 'helm-config)
  (global-set-key (kbd "C-c h") 'helm-command-prefix)
  (global-set-key (kbd "M-x") 'helm-M-x)
  (define-key global-map [remap find-file] 'helm-find-files)
  (define-key global-map [remap occur] 'helm-occur)
  (helm-autoresize-mode 1)
  (helm-mode 1))
```

Rebinding the default `M-x` behaviour to Helm's equivalent makes it much easier to view and launch all the commands available throughout all of Emacs. The `helm-command-prefix` command, launched by using `C-c h`, gives you a bunch of shortcuts for running Helm commands. For example, `C-c h f` will launch `helm-find-files`. I'm just learning these as I go and adding them to a cheat sheet as I discover them.

After you understand the basics around how to launch and configure Helm, [this article](https://tuhdo.github.io/helm-intro.html) is very useful for describing the functionality available.

## Reference and Useful Resources

A collection of resources I've found useful at various points:

* [Aaron Bieber's talk that initially inspired me to switch](https://www.youtube.com/watch?v=JWD1Fpdd4Pc)
* [Aaron Bieber's blog post for getting started with Evil](https://blog.aaronbieber.com/2015/05/24/from-vim-to-emacs-in-fourteen-days.html)
* [Another very useful blog post on Evil configuration by Aaron Bieber](https://blog.aaronbieber.com/2016/01/23/living-in-evil.html)
* [Evil Guide](https://github.com/noctuid/evil-guide)
* [Long blog post on switching from Vim to Emacs](https://juanjoalvarez.net/es/detail/2014/sep/19/vim-emacsevil-chaotic-migration-guide/) (This post isn't very well structured, but nonetheless it's still very useful.)
