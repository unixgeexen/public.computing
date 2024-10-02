X Keyboard Extension (xkb)
==========================
Translates keyboard codes into key functions. xmodmap was an older equivalent with lower function.

References
----------
* `Arch Linux xkb <https://wiki.archlinux.org/title/X_keyboard_extension>`_
* `Enhance xkb - Pascal & Toman <https://www.x.org/releases/current/doc/xorg-docs/input/XKB-Enhancing.html>`_
* `Pascal xkb Extension - Wayback <https://web.archive.org/web/20190724015820/http://pascal.tsu.ru:80/en/xkb/>`_
* `Good practical example with code <https://medium.com/@damko/a-simple-humble-but-comprehensive-guide-to-xkb-for-linux-6f1ad5e13450>`_
* `ubuntu community <https://help.ubuntu.com/community/Custom%20keyboard%20layout%20definitions>`_
* `Excellent description - charvolant - firefox WARNING <https://www.charvolant.org/doug/xkb/html/node5.html>`_
* Files
   * /usr/share/X11/xkb
   * /etc/default/keyboard - options to pass to xkb
   * /etc/X11/xorg.conf OR find /usr/share/X11 -name "\*.conf" - configuration for X service

Modal Keyboard based on an escape key
--------------------------------------
Use the semicolon (or some specified key e.g. backslash ) as an escape key which allows full functionality for limited key usage (by choice or because of low keycount keyboard).  Achieved by applying modal concept to key entry.

Usage:

* ;; - results in a single semicolon
* ;<key> - escapes normal operation of <key> to provide modes and other special operation
   * sticky modes - switch on/off
      * sticky function keys
      * window management
      * menu management
      * application modes???
      * mouse movement/action
   * quick modes
      * function and other keys - single key
         * 1-0 to + - Fn key
         * hjkl - vim direction keys
         * page movement - page up(u), down(d), home(s), end(e)
         * insert(i), delete(x), backspace(b)
         * ctrl(c), alt(a), meta(m) - incorporate with sticky keys in OS
         * application leader - tmux(t), vi/vim(v)
* howto
   * repeat keys - arrow, paging
   * make use of shifted keys - repeats?, stronger action
   * a key to show current mode/status

Keycode Translation
-------------------

Definitions
***********
* keycode/keyboard scan code - code sent from keyboard to X
   * names - traditional meanings e.g. <SPCE> for space bar
   * relative position - e.g <AE01> ! on US keyboards
* group - max 4, usually reprepsents a layout e.g. US-English - defined in symbols
* symbols - key values used within X
* state - encodes 8bits of modifiers (0>8) - Shift, Lock, Control, Mod1, Mod2, Mod3, Mod4, Mod5
* keysym
* action - single action can be triggered e.g. set or lock modifier bit
   * (keysym, state) > action
* type - which keys are affected by modifiers e.g. TWO_LEVEL
   * e.g. (state, TWO_LEVEL) > level = ((state >> 0) & 0x01) = state & 0x01 i.e. 0 or 1
   * S[keycode][0..4][0..1] instead of possible S[keycode][0..4][0..256]
* modifier bits - affect/are the state? Shift, Lock, Control and Mod1-Mod5 e.g. Mod1, Mod2 can be given a name Alt, Meta using virtual modifiers

Specification
*************
* symbols
   * name[Groupn]= "GroupName";
   * shortform: key <keycode> { [Group1Level1,Group1Level2], [Group2Level1,Group2Level2]};
   * longform: key <keycode> { symbols[Group1] = [Level1,Level2], symbols[Group2] = [Level1,Level2]};
   * partial (overrides to selected group/level): key <AD01> { [], [], [ q, Q ] };
   * types: key <keycode> { type=<typename> , ... }
   * virtual modifiers: key <keycode> { symbols ..., virtualMods=<virtmodname> };
   * control keys: modifier_map <modifiername> { <symbol>, <symbol>, ...};
* types
   * type <typename> { modifiers, map, level_name};
      * modifiers - modifiers= <modifier1>+<modifier2> ...; # modifiers considered for this type
      * map - map[<modifier>] - <level_name>; # level_name is active when modifier active
      * level_name - level_name[<levelname>] = "<displayname>" - gives level a user friendly name
* compat
   * default xkb_compatibility "<compatname>" { virtual_modifiers <modifiername>,<modifiername>...;
      * virtual_modifiers - which virtual modifiers might be set or examined
      * interpret <keyname>+AnyOf(<key>+<key>...) {action= <actionname>(modifiers=<key>)} # which modifier key triggers action
      * indicator <indicatorname> { whichModState= ; modifiers- ;}; # conditions to light indicator
   * virtual modifier: virtual_modifiers <modifiername>;

Translation
***********
* Overall (keycode, group, state) > keysym[,action]
* Internally
   * types: (keycode [,group] > type
   * types: (state, type) > level
   * symbols: (keycode, group, level) > S[keycode][group][level]
   * compat: keysym > action

xkb file layout
***************
* xkb_keycodes - define symbolic keylabels associated with keycode for use in rest of file
* xkb_types - define the types e.g. modifiers= Shift+NumLock+LevelThree; map[Shift+LevelThree]= Level4; 
   * ONE_LEVEL - not affected by modifiers e.g enter, space, escape, Fn, shift, alt, ctrl ...
   * TWO_LEVEL - keysym affected by modifier state
   * ALPHABETIC - like TWO_LEVEL but also affected by capslock
* xkb_compatibility (compat) - action definitions (interpret) and keyboard LEDs (inidicator) - internal behaviour of modifiers
   * action types - set (while key is pressed), latch (until next key pressed), lock (until unlocked), mouse
   * action destination - modifier state, group, controls, mouse movement/buttons
* xkb_symbols - define key - 
   * key <keylabel> { 'G<GroupNum>L<LevelNum>, ... }
   * key <...> { type = "T", [ .... ], [ .... ] };
   * key <...> { type[1] = "T1", type[2] = "T2", [ ... ], [ ... ] };
   * name[1] = "EN"; # set group names
* xkb_geometry - not required - diagram of keyboard
* grouping components
   * semantics - named types of types and compat components (not used often)
   * key maps - named map specifying includes of each main type
   * rules - user oriented specification based on matching names (for more unusual keyboards) - available layouts and variants (children)

Implementation and Investigation Commands
-----------------------------------------
* xkbcomp - xkb compiler - manipulate xkb data
   * xkbcomp $DISPLAY output,xkb - get current configuration
   * xkbcomp input.xkb $DISPLAY - upload to current configuration
   * xkbcomp input.xkb - check syntax and create .xkm file
* load on startup
   * save a layout as ~/.Xkeymap
   * load on startup ~/.xinitrc: test -f ~/.Xkeymap && xkbcomp ~/.Xkeymap $DISPLAY
* xev - display keycodes and troubleshoot layout
   * xev -event keyboard # shows extensive information about keyboard events - code, state ...
* setxkbmap
   * setxkbmap -layout us # load a layout on the fly
   * setxkbmap -layout us,us -variant ,dvorak # set us layout with dvorak variant
   * setxkbmap -print -verbose 10 # show current variant
   * setxkbmap -layout us,us -variant ,dvorak -option # reset options
   * setxkbmap -layout us,us -variant ,dvorak -option "lv3:rwin_switch,grp:alt_space_toggle" # set options
   * xmodmap -e "keycode 66 = Escape" # simpler bypass xkb to map CapsLock to Esc
