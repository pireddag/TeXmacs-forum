Many aspects of TeXmacs' behaviour can be changed/extended by adding Scheme code to the right initialization files. This wiki post collects various such snippets of code to adjust the behaviour of TeXmacs to your liking.

Feel free to add your own. The snippets do not have to be perfect. The intention is very much for users to improve and extend each others' code. Longer pieces of code may be better suited for the [TeXmacs Forge](https://github.com/texmacs/tm-forge).

# How to edit the TeXmacs user initialization files 

The code snippets can be copy-pasted into one of your TeXmacs user initialization files. The section headings indicate in which file they should go.

To edit the files in the TeXmacs editor, first enable `Tools→Developer Tool`, then:

* For code that should go into **`my-init-texmacs.scm`**:
  
  Click `Developer → Open my-init-texmacs.scm`. 

* For code that should go into **`my-init-buffer.scm`**:

  Click `Developer → Open my-init-buffer.scm`.

Paste in the snippet of your choice. To paste code from this page you may have to use `Edit → Paste from → Html` in TeXmacs (`Edit → Paste from → Verbatim` is also possible).

Save the edited file. 

Finally, for the customization to take effect:
* If you have changed **`my-init-texmacs.scm`**:
  
  Restart TeXmacs.

* If you have changed **`my-init-buffer.scm`**:

   Open a file. (Note that some snippets may be written to only work on **new** files, not existing files)

# Code for `my-init-texmacs.scm`

### Export to a pdf with the same main file name as the current TeXmacs document

    (define (propose-pdf-name)
      (with name (propose-name-buffer)
      (if (string-ends? name ".tm")
        (string-append (string-drop-right name 3) ".pdf"))))

    (define (export-to-proposed-pdf)
      (let ((pdf-name (propose-pdf-name)))
        (if (url-test? pdf-name "f")
          (user-confirm "File already exists. Really overwrite?" #f
            (lambda (answ)
            (when answ (wrapped-print-to-file pdf-name))))
          (wrapped-print-to-file pdf-name))))
    
    (kbd-map
      ("M-e p" (export-to-proposed-pdf)))

This will add the keyboard shortcut `Meta+e p` to export directly to a pdf with the same file name.

The `Meta` key is the Windows key in Windows or Linux, or the Command key on Apple systems. When using these keys, press Meta and e together, then press p.

The Escape key can serve as a fallback Meta key. To use this key, first press `Meta`, then release and press the `e` key, finally press the `p` key.

See `Help → Manual → Getting started` for more details.

### Add extra fonts to the font menu in the toolbar

    (delayed (lazy-initialize-force) (menu-bind document-short-font-menu
    (former)
    ---
    ("Cantarell" (init-font "Cantarell"))
    ("Kepler" (init-font "Kepler"))
    ("Garamond" (init-font "Garamond Libre"))
    ("Montserrat" (init-font "Montserrat"))))


### Enable multiline input by default for sessions

    (use-modules
     ((dynamic session-edit) #:select (session-multiline-input)))
      (ahash-set! session-multiline-input (cons "julia" "default") #t)
      (ahash-set! session-multiline-input (cons "python" "default") #t)
      (ahash-set! session-multiline-input (cons "jupyter" "julia-1.6") #t)

### Extend the line length for LaTeX export
See thread http://forum.texmacs.cn/t/how-do-i-export-to-latex-without-it-inserting-newlines-into-the-paragraphs/386/7

    (delayed (:idle 1000) (output-set-line-length 1000000000))

### Evaluate all fields in a plugin session inside a document

See thread http://forum.texmacs.cn/t/evaluate-all-in-document/431

    (define (ev-all)
    (for-each (lambda (ses) (begin (tree-go-to ses :last 1 0) (session-evaluate-all)))
    (tree-search (buffer-tree) subsession-document-context?)))

    (kbd-map
    ("A-e" (ev-all)))

This will add the keyboard shortcut `Alt+e` to evaluate all cells in the document.

Optionally add an entry to the context menu inside sessions.

    (tm-menu (texmacs-popup-menu)
    (:require (inside? 'session))
    (former)
    ---
    ("Evaluate all fields in document" (ev-all)))

For older versions of TeXmacs `tree-search` isn’t in global namespace and has to be imported using

    (use-modules
    ((link link-extract)
    #:select (tree-search)))

### Assign arrow movements to key combinations

See thread http://forum.texmacs.cn/t/how-to-modify-the-arrow-keys/342

    (delayed
    (lazy-keyboard-force)
    (kbd-map ("C-a" (kbd-left)))
    (kbd-map ("C-f" (kbd-right)))
    (kbd-map ("C-s" (kbd-up)))
    (kbd-map ("C-d" (kbd-down))))

This will add keyboard shortcuts `Ctrl+a`, `Ctrl+f`, etc. as alternatives to the keyboard arrows.

### Insert subscript and superscript into sessions

    (delayed (lazy-keyboard-force) (kbd-map
      (:mode in-session?)
      ("\\ _ var" (make-script #f #t))
      ("\\ ^ var" (make-script #t #t))))
This will add Jupyter style keyboard shortcuts `\ _ Tab` and `\ ^ Tab` to insert subscript and superscript (only when in sessions).

### Editor shortcuts

In addition to macros, one can enter in TeXmacs "editor shortcuts" using "LaTeX style commands". One example is text color; the key sequence `\red`, followed by a return, is transformed by TeXmacs into `<with|color|red|>` (visible in source mode) which localizes the environment variable `color` so that text entered into the empty slot is colored in red.

These editor shortcuts are defined in Scheme code using the macro `kbd-commands`.

Here is an example; we define a shortcut for the color `"dark green"`. LaTex mode commands do not allow entering spaces, so we name the command `"darkgreen"` without space. Place the snippet in `my-init-texmacs.scm`.

```
(kbd-commands
  ("darkgreen" "" (make-with "color" "dark green")))
```

With this command, entering `\darkgreen` will result in `<with|color|dark green|>` (in source mode---in text mode one gets an empty slot where typing results in dark green text).

# Code for `my-init-buffer.scm`

### Add a style file to every new buffer
See thread http://forum.texmacs.cn/t/solutions-in-html-export-of-exams-using-the-education-package/268/3

    (if (not (buffer-has-name? (current-buffer)))
      (begin
      (set-style-list (append (get-style-list) '("no-solutions-in-html")))
      (buffer-pretend-saved (current-buffer))))

This will append a style file (in this case `no-solutions-in-html`, change this to the style file of your preference) to every **new** file. It will not operate on existing files.
