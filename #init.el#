;; scratchの初期メッセージ消去
(setq initial-scratch-message "")

;; タイトルバーにファイルのフルパス表示
(setq frame-title-format
(format "%%f - Emacs@%s" (system-name)))

;; 行番号表示
(global-linum-mode t)
(set-face-attribute 'linum nil
:foreground "#400"
:height 0.9)
;; 行番号フォーマット
(setq linum-format "%4d ")

;桁番号をモードラインに表示。
(setq column-number-mode t)
; 括弧の対応を常に表示。
(show-paren-mode t)
; Xのクリップボードと同期を取るらしい。
(setq x-select-enable-clipboard t) 

;; タブをスペースで扱う
(setq-default indent-tabs-mode nil)
; タブ幅
(setq default-tab-width  2)

;; yes or noをy or n
(fset 'yes-or-no-p 'y-or-n-p)

;; C-Ret で矩形選択
;; 詳しいキーバインド操作：http://dev.ariel-networks.com/articles/emacs/part5/
(cua-mode t)
(setq cua-enable-cua-keys nil)

; 補完機能
(add-to-list 'load-path "~/.emacs.d/")
(require 'auto-complete-config)
(add-to-list 'ac-dictionary-directories "~/.emacs.d//ac-dict")
(ac-config-default)
(setq ac-use-menu-map t)
;; デフォルトで設定済み
(define-key ac-menu-map "\C-n" 'ac-next)
(define-key ac-menu-map "\C-p" 'ac-previous)

; Shiftで範囲選択
(define-key input-decode-map "\e[1;2D" [S-left])
(define-key input-decode-map "\e[1;2C" [S-right])
(define-key input-decode-map "\e[1;2B" [S-down])
(define-key input-decode-map "\e[1;2A" [S-up])
(define-key input-decode-map "\e[1;2F" [S-end])
(define-key input-decode-map "\e[1;2H" [S-home])

; うまく動かん
(define-key input-decode-map "\e202" [S-C-left])
(define-key input-decode-map "\e206" [S-C-right])
(define-key input-decode-map "\e[216" [S-C-down])
(define-key input-decode-map "\e[220" [S-C-up])

(defun my-count-lines-window ()
    "Count lines relative to the selected window. The number of lines begins 0."
      (interactive)
        (let* ((window-string (buffer-substring-no-properties (window-start) (point)))
                        (line-string-list (split-string window-string "\n"))
                                 (line-count 0)
                                          line-count-list)
              (setq line-count (1- (length line-string-list)))
                  (unless truncate-lines      ; consider folding back
                          ;; `line-count-list' is list of the number of physical lines which each logical line has.
                          (setq line-count-list (mapcar '(lambda (str)
                                                                                                  (/ (my-count-string-columns str) (window-width)))
                                                                                            line-string-list))
                                (setq line-count (+ line-count (apply '+ line-count-list))))
                      line-count))

(defun my-count-string-columns (str)
    "Count columns of string. The number of column begins 0."
      (with-temp-buffer
            (insert str)
                (current-column)))

(defadvice scroll-up (around scroll-up-relative activate)
    "Scroll up relatively without move of cursor."
      (let ((line (my-count-lines-window)))
            ad-do-it
                (move-to-window-line line)))

(defadvice scroll-down (around scroll-down-relative activate)
    "Scroll down relatively without move of cursor."
      (let ((line (my-count-lines-window)))
            ad-do-it
                (move-to-window-line line)))


; 以下、Windows風選択の実装。

(defvar win-sel-v nil)
(defun win-sel ()
	  (if (or (null (mark t)) (not mark-active))
				      (setq win-sel-v nil))
		  win-sel-v)
(defun win-sel-begin ()
	  (transient-mark-mode 1)
		  (if (not (win-sel)) (set-mark-command nil))
			  (setq win-sel-v t))
(defun win-unselect ()
	  (transient-mark-mode -1)
		  (setq win-sel-v nil))
(defun win-copy (BEG END)
	  (interactive (list (region-beginning) (region-end)))
		  (copy-region-as-kill BEG END)
			  (win-unselect))
(defun win-paste () (interactive)
	  (if (win-sel) (delete-region (region-beginning) (region-end)))
		  (yank))
(defun win-delete ()
	  (interactive)
		  (if (win-sel)
					      (delete-region (region-beginning) (region-end))
				    (delete-char 1)))
(defun win-backspace ()
	  (interactive)
		  (if (win-sel)
					      (delete-region (region-beginning) (region-end))
				    (backward-delete-char 1)))
(defun sel-forward-char ()
	  (interactive) (win-sel-begin) (forward-char))
(defun sel-backward-char ()
	  (interactive) (win-sel-begin) (backward-char))
(defun sel-next-line ()
	  (interactive) (win-sel-begin) (next-line 1))
(defun sel-previous-line ()
	  (interactive) (win-sel-begin) (previous-line 1))
(defun sel-beginning-of-line ()
	  (interactive) (win-sel-begin) (beginning-of-line))
(defun sel-end-of-line ()
	  (interactive) (win-sel-begin) (end-of-line))
(defun unsel-forward-char ()
	  (interactive) (win-unselect) (forward-char))
(defun unsel-backward-char ()
	  (interactive) (win-unselect) (backward-char))
(defun unsel-next-line ()
	  (interactive) (win-unselect) (next-line 1))
(defun unsel-previous-line ()
	  (interactive) (win-unselect) (previous-line 1))
(defun unsel-beginning-of-line ()
	  (interactive) (win-unselect) (beginning-of-line))
(defun unsel-end-of-line ()
	  (interactive) (win-unselect) (end-of-line))

(setq meta-org (make-sparse-keymap))
(global-set-key "\M-o" meta-org)
(define-key meta-org "\C-c" mode-specific-map)
(define-key meta-org "\C-x" ctl-x-map)
(global-set-key "\C-m" 'newline-and-indent)
(global-set-key "\C-h" 'win-backspace)
(global-set-key "\C-d" 'win-delete)
(global-set-key "\C-x" 'kill-region)
(global-set-key "\C-c" 'win-copy)
(global-set-key "\C-v" 'win-paste)
(global-set-key "\C-s" 'save-buffer)
(global-set-key "\C-w" 'save-buffers-kill-emacs)
(global-set-key "\C-o" 'find-file)
(global-set-key "\C-z" 'undo)
(global-set-key (kbd "C-<") 'forward-word)
(global-set-key (kbd "C->") 'backward-word)
; うまく動かん　検索どうしよう
(global-set-key (kbd "C-/") 'isearch-forward)
(define-key global-map (kbd "C-r")
    '(lambda () (interactive) (scroll-up (/ (window-height) 4))))
(define-key global-map (kbd "C-u")
    '(lambda () (interactive) (scroll-down (/ (window-height) 4))))
; うまく動かん
(global-set-key "\222" 'query-replace)
