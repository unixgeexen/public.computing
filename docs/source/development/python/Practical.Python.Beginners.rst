Practical Python for Beginners
==============================

References
----------
* `pluralsight course <https://app.pluralsight.com/courses/162256df-2d1b-4100-9780-9a931bf22855/table-of-contents>`_

Course Overview
------------------------------

Data Types, Input and Output
------------------------------
* Variables and numbers
* Tax Calculator
* Strings, Input and Output

Conditionals and Imports
------------------------------

Lists and Loops
------------------------------

Dictionaries and Reading JSON
------------------------------

Functions
------------------------------

Concepts
--------
Buffers - file, plugin info - has unique number and a name
************************************************************
* delete - :quit, :bdeleteID
* list - :ls, :ls!, :buffers, :files
   * List format - bufnum state name cursorPos
   * states - h (hidden - not display with :ls), a (active), % (current), # (alternate - previous buffer)
* switch buffer - :bID - e.g. :b1, :b# (switch to previous buffer)
* close/delete buffer - 13,15bd - delete buffers 13,14,15

Configuration
-------------
Commands ::

   :version - where is my .vimrc
   :scriptnames
   :!echo $MYVIMRC
   :so $MYVIMRC - re-source vimrc file if you've changed it
   :so % - re-source the current file - expect it's vimrc compatible
   :mkv [file] - create .vimrc from current settings, file defaults to .vimrc, ! to overwrite
