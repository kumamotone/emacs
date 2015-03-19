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

