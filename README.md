auto-complete-etags-with-docs
=============================

Popup the docuementation when using TAGS with auto-complete.

Tested in GNU EMACS 24.2.1.

Requires: 
    auto-complete-etags.el in `depends/' directory
	etags-select
	
Sample configuration:

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; auto-complete
(require 'auto-complete-config)
(ac-config-default)

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; etags-table
(require 'etags-select)
(require 'etags-table)
(setq etags-table-search-up-depth 10)
(setq etags-table-alist
      (list
       ;; For jumping to Mercury standard headers:
       '(".*\\.m" "~/sys-tags/mercury/TAGS")
       ;; For jumping to XSB system's libraries:
       '(".*\\.P$" "~/sys-tags/xsb/TAGS")
       '(".*\\.pl$" "~/sys-tags/xsb/TAGS")
       ;; For jumping to ECLiPSe system's libraries
       '(".*\\.ecl$" "~/sys-tags/eclipse/TAGS")
       ;; '(".*\\.pl$" "~/sys-tags/eclipse/TAGS")
       ;; For jumping to C system's libraries
       ;; '(".*\\.[ch]$" "~/sys-tags/c/TAGS")  <-- always crashes
       ;; For jumping across project:
       ;; '("/home/devel/proj1/" "/home/devel/proj2/TAGS" "/home/devel/proj3/TAGS")
       ;; '("/home/devel/proj2/" "/home/devel/proj1/TAGS" "/home/devel/proj3/TAGS")
       ;; '("/home/devel/proj3/" "/home/devel/proj1/TAGS" "/home/devel/proj2/TAGS")
       ))
(defun etags-select-get-tag-files ()
  "Get tag files."
  (if etags-select-use-xemacs-etags-p
      (buffer-tag-table-list)
    (mapcar 'tags-expand-table-name tags-table-list)
    (tags-table-check-computed-list)
    tags-table-computed-list))

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; auto-complete-etags
(require 'auto-complete-etags)
;; (add-to-list 'ac-sources 'ac-source-etags)
;; (setq ac-etags-use-document t)

(defun add-ac-source-etags ()
  (add-to-list 'ac-sources 'ac-source-etags))

(add-hook 'prolog-mode-hook 'add-ac-source-etags)
(add-hook 'eclipse-mode-hook 'add-ac-source-etags)

(require 'auto-complete-etags-docs)
(aced-update-ac-source-etags)
