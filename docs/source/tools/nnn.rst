nnn Notes
==============================

References
----------
* `nnn documentation and repo <https://github.com/jarun/nnn/wiki>`_
* `basic use cases <https://github.com/jarun/nnn/wiki/Basic-use-cases>`_
* `man page <https://www.mankier.com/1/nnn>`_
* `nnn archlinux <https://wiki.archlinux.org/title/Nnn>`_

TODO
----
* review basic use cases and add to notes

Basic Usage
------------------------------
* hjkl - navigation
* ? - help - provides key bindings
    * ?h - provides more detailed key bindings
* export VISUAL=nvim

Detailed Usage
--------------
* navigation
    * tab,shift-tab: navigate contexts (1-4)
* session - save/restore states of work
   * @ - auto session
   * stored in ${XDG_CONFIG_HOME:-$HOME/.config}/nnn/sessions
* filters - search as you type - file names
   * / - toggle string vs regex
* type to navigate - directories are opened in filter mode
   * -n arg or ^N
* find and list
   * find -maxdepth 1 -size +1M -print0 | nnn
   * nnn < list.of.files
* bookmarks
   * create
      * export NNN_BMS="d:$HOME/Documents;u:/home/user/Cam Uploads;D:$HOME/Downloads/"
      * B - create symlinked bookmark
      * Add manually to ~/.config/nnn/bookmarks
      * , mark the current directory
   * list - b (NNN_BMS and ,)
   * jump 
* open nnn
   * as root: alias N='sudo -E nnn -dH'
   * list the files you select: ls -l $(nnn -p -)
   * attach files and send mail: neomutt -a $(nnn -p -) ...
* file actions
    * ^R - duplicate a file
    * 
