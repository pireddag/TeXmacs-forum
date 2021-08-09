Many aspects of TeXmacs' behaviour can be changed/extended by adding Scheme code to the right initialization files. This wiki post collects various such snippets of code to adjust the behaviour of TeXmacs to your liking.

Feel free to add your own. The snippets do not have to be perfect. The intention is very much for users to improve and extend each others' code. Longer pieces of code may be better suited for the [TeXmacs Forge](https://github.com/texmacs/tm-forge).

# How to edit the TeXmacs user initialization files 

The code snippets can be copy-pasted into one of your TeXmacs user initialization files. The section headings indicate in which file they should go.

To edit the files in the TeXmacs editor, first enable `Tools→Developer Tool`, then:

Paste in the snippet of your choice. To paste code from this page you may have to use `Edit → Paste from → Html` in TeXmacs (`Edit → Paste from → Verbatim` is also possible).

Save the edited file. 

Finally, for the customization to take effect:
* If you have changed **`my-init-texmacs.scm`**:
  
  Restart TeXmacs.

* If you have changed **`my-init-buffer.scm`**:

   Open a file. (Note that some snippets may be written to only work on **new** files, not existing files)

