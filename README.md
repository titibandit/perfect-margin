perfect-margin
==============

Perfect-margin is a global minor mode to auto center windows, work with minimap and/or linum-mode.

A picture is more than one thousand words, here is how it looks like with minimap and linum-mode.

<span class="image-600">![perfect-margin](https://raw.githubusercontent.com/mpwang/perfect-margin/master/perfect-margin.gif)</span>

_theme: doom-dracula from beautiful [doom-themes](https://github.com/hlissner/emacs-doom-themes)_

[minimap](http://elpa.gnu.org/packages/minimap.html) from MELPA

# Usage

Put perfect-margin under your Emacs load path, and add this to your init.el

```lisp
(require 'perfect-margin)
```


Use `M-x perfect-margin-mode` to turn on/off perfect-margin.

To make it permanent add this to your init.el after require.
```lisp
(perfect-margin-mode 1)
```

## Important Note
when using together with minimap or linum, make sure you place config for perfect-margin `AFTER` minimap and linum.

# Customization

Via `M-x customize-group` and enter perfect-margin.

Change `perfect-margin-visible-width` and `Apply and Save`. That's it.

*Or* you can change the visible window width by setup `perfect-margin-visible-width` on the init.el.

```lisp
(setq perfect-margin-visible-width 128)
```

## Customize what window to ignore setting margins

perfect-margin by default ignore setting margins for minibuffer window and any window whos name starts with "*".
Many of them are for special purpose.

You can change this behavior by setting `perfect-margin-ignore-regexps` and `perfect-margin-ignore-filters`.

This behavior might be too conservative, you can
- set `perfect-margin-ignore-filters` to `nil` to auto-center minibuffer windows
- set `perfect-margin-ignore-regexps` to `nil` to auto-center special windows like the HELM windows
- or simply set both variables to `nil` to let perfect-margin auto-center all windows no matter what.

```lisp
(setq perfect-margin-ignore-filters nil)
(setq perfect-margin-ignore-regexps nil)
```

Default value for these two variables are listed below.

```lisp
(defcustom perfect-margin-ignore-regexps
  '("^minibuf" "^[*]")
  "List of strings to determine if window is ignored.
Each string is used as regular expression to match the window buffer name."
  :group 'perfect-margin)

(defcustom perfect-margin-ignore-filters
  '(window-minibuffer-p)
  "List of functions to determine if window is ignored.
Each function is called with window as its sole arguemnt, returning a non-nil value indicate to ignore the window."
  :group 'perfect-margin)
```


## Customize window fringes

perfect-margin by default set both left and right fringe of all windows to zero, this might cause problem in some corner cases if
another package which also manipulate fringes is enabled.

You can set `perfect-margin-hide-fringes` to `nil` to tell perfect-margin not to manipulate fringes.

# Additional binding on margin area

You can place this in your init.el to make mouse wheel scroll on margin area just like it scroll on the visible window.

```lisp
(dolist (margin '("<left-margin> " "<right-margin> "))
  (global-set-key (kbd (concat margin "<mouse-1>")) 'ignore)
  (global-set-key (kbd (concat margin "<mouse-3>")) 'ignore)
  (dolist (multiple '("" "double-" "triple-"))
      (global-set-key (kbd (concat margin "<" multiple "wheel-up>")) 'mwheel-scroll)
      (global-set-key (kbd (concat margin "<" multiple "wheel-down>")) 'mwheel-scroll)))
```

# for *use-package* user

```lisp
(use-package perfect-margin
  :custom
  (perfect-margin-visible-width 128)
  :config
  ;; enable perfect-mode
  (perfect-margin-mode t)
  ;; auto-center minibuffer windows
  (setq perfect-margin-ignore-filters nil)
  ;; auto-center special windows
  (setq perfect-margin-ignore-regexps nil)
  ;; add additinal bding on margin area
  (dolist (margin '("<left-margin> " "<right-margin> "))
  (global-set-key (kbd (concat margin "<mouse-1>")) 'ignore)
  (global-set-key (kbd (concat margin "<mouse-3>")) 'ignore)
  (dolist (multiple '("" "double-" "triple-"))
      (global-set-key (kbd (concat margin "<" multiple "wheel-up>")) 'mwheel-scroll)
      (global-set-key (kbd (concat margin "<" multiple "wheel-down>")) 'mwheel-scroll))))
```

# Emacs Rocks and happy hacking!
