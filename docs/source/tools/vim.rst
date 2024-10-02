Vim
===

Hints
-----
* `vim galore <https://awesomeopensource.com/project/mhinz/vim-galore>`_
* `patient vimmer <https://romainl.github.io/the-patient-vimmer/2.html>`_
* `interesting layout <https://spacevim.org/documentation/>`_

TODO
----
* file explorers - nerdtree or generally default `netrw <https://shapeshed.com/vim-netrw/>`_
* fuzzy finder
* spacevim - layers concept
* how to use registers
* folding - `Folding Fun <https://www.linux.com/training-tutorials/vim-tips-folding-fun/>`_ https://learnvimscriptthehardway.stevelosh.com/chapters/48.html https://learnvimscriptthehardway.stevelosh.com/chapters/49.html
* syntax highlighting - syntastic https://learnvimscriptthehardway.stevelosh.com/chapters/45.html
* visual mode
* :h jumplist, :h changelist

Concepts
--------
Folding
*******
* z action is mostly used for folding
* folds remain for the current editing session except:
   * saved with :mkview and restored with :loadview
      * au BufWinLeave * mkview
      * au BufWinEnter * silent loadview
   * zd - delete fold at cursor
   * zE - delete all folds
* multiple modes for working with folds (:set foldmethod=indent, :set fdm?)
   * manual - (default) - defined with cursor movement etc
   * indent - based on file text indentation
   * syntax - based on file syntax
   * marker - based on markers within file - triple brace open '{{{' close '}}}'
      * set foldmarker=openstring,closestring

Buffers - file, plugin info - has unique number and a name
************************************************************
* delete - :quit, :bdeleteID
* list - :ls, :ls!, :buffers, :files
   * List format - bufnum state name cursorPos
   * states - h (hidden - not display with :ls), a (active), % (current), # (alternate - previous buffer)
* switch buffer - :bID - e.g. :b1, :b# (switch to previous buffer)
* close/delete buffer - 13,15bd - delete buffers 13,14,15

Windows - viewport onto single buffer
*************************************
* :winc :wincmd can usually replace <c-w>
* create - :split, :vsplit, :new, :vnew
* navigate to other windows - <c-w> h|j|k|l
* navigate windows to other positions - <c-w> H|J|K|L
* resize window - <c-w> ><+-_|= - wider/narrower/higher/shorter/max height/max width/all equal
* close window - :clo[se] <c-w> c
* https://vim.fandom.com/wiki/Switch_between_Vim_window_splits_easily[simplifying window movement]
* open file in existing window
* open file in new window - :vs[plit] filename, :sp[lit] filename

Tabs - collection of one or more windows
****************************************
* create - :tabnew, :tabedit FileName
* navigate - gt,gT - go to next/previous tab
* move current window to new tab - Ctrl-WT
* open file in new tab - :tabedit filename

Arguments
*********
* :help argument-list
* Initially set to set of filenames with which vim was opened
* Can be modified with :argadd :argdelete :args <FileList>
* Use case - can operate on multiple files using :argdo ::

   :args **/*.[ch] :argdo %s/foo/bar/ge | update - updates foo to bar in all *.c *.h files in current dir

Register - a place where vim stores text
****************************************
* TODO

Motions, Operators, Text Objects
********************************
* Motions - :h navigation - move cursor - accept count - e.g. hjklwb/
* Operators - :h operator - act on a region of text - accept count e.g. d ~ gU >
   * Normal mode - Operator first, then motion
   * Visual mode - Operator acts on selection
* Text Objects - :h text-objects - act on surrounding area (objects) - accept count - e.g. word, sentence, surrounded by parentheses

Modes
*****
* :showmode will show the mode when other than normal mode
* :help mode-switching - help on switching between modes
* Insert "-- INSERT --" - ioa - insert new data into buffer
* Normal - start mode and return from other modes with ESC- general buffer navigation, normal editor commands
* Visual "-- VISUAL --" - movement commands expand selection, non-movement commands executed for highlighted area
   * visual mode - v - selection of lines of text
   * visual block mode ctrl-v - selection of column/blocks
* Select "-- SELECT --" - like MS-Windows selection mode
* Command-line/Cmdline - command line at bottom - Ex commands(:), search (?/), filter (!)
* Ex - like Cmdline but with very limited editing of command line
* Operator-pending - like Normal mode but vim is waiting for {motion} command to specify the text the operator will function on
* Replace "-- REPLACE --" - special case of Insert but overwrites
* Virtual Replace "-- VREPLACE --" - like replace but replace screen real estate instead of file characters
* Insert Normal "-- (insert) --" - <C-O> from Insert mode - like normal mode but returns to insert mode after executing one vim command
* Insert Visual "-- (insert) VISUAL --" - Visual mode <C-O> v|V|<C-V> - do visual selection and then return to Insert mode
* Insert Select "-- (insert) SELECT --" - Insert mode (start select mode with <S-Right> or mouse drag - when select mode ends return to insert mode

All commands list https://vimhelp.org/index.txt.html
-----------------------------------------------------
* Also check in your installation at something like /usr/share/vim/vim74/doc
   * :scrip[tnames] will give you an idea of where installation files might be
* :help index - gives a list of all commands within the editor
* quickref may have a list of all commands - index.txt seems better formatted
   * Format
      * Tagged *Q_qf*
   * Desired output - tag  - command text, applicable modes, description, vimversion

Configuration
-------------
Commands ::

   :version - where is my .vimrc
   :scriptnames
   :!echo $MYVIMRC
   :so $MYVIMRC - re-source vimrc file if you've changed it
   :so % - re-source the current file - expect it's vimrc compatible
   :mkv [file] - create .vimrc from current settings, file defaults to .vimrc, ! to overwrite

Help
----
* :help SearchTerm<C-D> - get a list of matching search terms
* :help Mode^V - search for help on <C-V> in specified mode - type caret and V not <C-V>
* :helpgrep SearchTerm - list files which match SearchTerm

Overlaying command maps
-----------------------
* References
   * `vim tutorial <https://vim.fandom.com/wiki/Mapping_keys_in_Vim_-_Tutorial_(Part_1)>`_
   * `vim keymap info <https://romainl.github.io/the-patient-vimmer/2.html>`_
   * `write your own vimrc <https://github.com/romainl/idiomatic-vimrc>`_
   * `nice looking vimrc <https://github.com/ashfinal/vimrc-config>`_
* Mapping ideas
   * how often is the function required
   * difficulty of hitting the mapping keys - how many keys, how long does it take to type
   * is it modal - only used in certain part of work
* basic approach is to use the first letter of an operator in normal mode followed by a character which is not associated with a motion
   * List of such combinations
      * ca  cd  cm  co  cp  cq  cr  cu  cv  cx  cy  cz cA  cD  cO  cP  cQ  cR  cU  cV  cX  cY  cZ
      * da  dc  dm  dq  dr  du  dv  dx  dy  dz dA  dC  dQ
      * va  vc  vd  vm  vo  vp  vq  vr  vs  vu  vv  vx  vy  vz vA
      * ya  yc  yd  ym  yo  yp  yq  yr  ys  yu  yv  yx  yz yA
      * gb  gc  gl gB
      * zp  zq  zu  zy
   * All function keys (and shifted) except <F1>
   * , comma and _ underscore
* Leader key - written as <leader>, default /, :let mapleader=',"
   * noremap <leader>W :w !sudo tee % > /dev/null
   * can give your own mappings a different namespace to vim's
* Local leader key :let maplocalleader = "\\"
   * special leader key meant to be based on filetype/buffer
   * first \ is escape for vimscript
* Mapping command mode indicators ::

   n  Normal mode map. Defined using ':nmap' or ':nnoremap'.
   i  Insert mode map. Defined using ':imap' or ':inoremap'.
   v  Visual and select mode map. Defined using ':vmap' or ':vnoremap'.
   x  Visual mode map. Defined using ':xmap' or ':xnoremap'.
   s  Select mode map. Defined using ':smap' or ':snoremap'.
   c  Command-line mode map. Defined using ':cmap' or ':cnoremap'.
   o  Operator pending mode map. Defined using ':omap' or ':onoremap'.
   <Space>  Normal, Visual and operator pending mode map. Defined using ':map' or ':noremap'.
   !  Insert and command-line mode map. Defined using 'map!' or 'noremap!'.

* Mapping rhs indicators ::

   *  The {rhs} of the map is not re-mappable. Defined using the ':noremap' or ':nnoremap' or ':inoremap', etc. commands.
   &  Only script local mappings are re-mappable in the {rhs} of the map. The map command has the <script> attribute.
   @  A buffer local map command with the <buffer> attribute.

* Redirecting vim command output ::

   :redir! > vim_maps.txt
   :map
   :map!
   :redir END

quickfix
--------
* :vimgrep remedy tickets/* # generate a quickfix list
* :copen  # open the quickfix list
* :cn :cp # goto previous location in quicklist
* :ccl # close quicklist- file explorers

File Explorers
--------------
* netrw
   * Invoke - :e. :edit . :sp . :vsp .

Visual Mode
-----------
* Character (v), Line (V), Block <C-v>
* Select text - arrows, movement
* Perform action e.g. y,d

Registers
---------
* :global/TODO/yank A # appends all lines that match TODO into register a
* :reg a - displays contents of register a
* capital letter register name appends the latest action to the lower case register name
* special registers
   * numbered
   * %
   * named - identified by a letter of the alphabet as above
* There are nine types of registers:          *registers* *E354* ::

   * The unnamed register "" - populated by d,c,s,x,y e.g. last used register
   * 10 numbered registers "0 to "9 - most recently deleted text
   * The small delete register "- - deletions of less than one line
   * 26 named registers "a to "z or "A to "Z - user defined/specified registers
   * three read-only registers ":, "., "% - . (last inserted text) % (current filename) : (most recent executed cmd)
   * alternate buffer register "# - alternate file for current window - most recent file in this buffer - switch with <C-^>
   * the expression register "=
   * The selection and drop registers "*, "+ and "~
   * The black hole register "_ - NOP - a spot to put something you cannot retrieve
   * Last search pattern register "/

Vim Notes from usr*.txt manuals
*******************************

Admin
*****
* for stuff based on vim user guide the best approach might be to write up the basic approach and give the help commands to find it e.g. :help help-summary

From usr_*.txt manuals
**********************
* user manuals - task oriented
* reference manual - precise description of how everything works
* quickref - quick reference of vim commands
* index.txt - list of all vim commands in categories - reasonably well formatted

using the manuals
*****************
* hyperlinks - <C-]> - jump to link, <C-O> return down chain of jumps - may need tags
* TODO how to create tags file for vim help and tag following

check vi compatibility - :set compatible?
*****************************************
:e! - replaces current file with it's original version and continues editing

* help
   * :help CTRL-A - type the letters don't press the CTRL key
   * :help index - list of all commands - probably index.txt file
   * :help i_CTRL-H - gives help for CTRL-H in insert mode - TODO list all mode codes
      * none (normal mode), i (insert), v(visual), c(command line)
   * :help 'number' - gives help for the option number
   * :help <Up> - gives help for special key Up
   * :help E37 - help for error code
   * see help-summary for all help options

* getting around
   * set [no]number - set line numbering off/on
   * <C-F>, <C-B> - scroll forward/backward a whole page
   * zz,zt,zb - scroll current line to middle,top,bottom of window
   * /<Up>|<Down> - scans through search history
   * :<Up>|<Down> - scans through command history
   * /\<the\> - search for "the" as a whole word .i.e. start \< and \> end of word
   * set [no]hlsearch - highlight off/on for search
   * :scriptnames - list all script names including path of .vimrc

