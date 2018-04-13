# ATOM COMMANDS
---

### GENERAL SHORTCUTS

```
C-,             open settings
C-.             display keybindings resolver
C-P             Command Palette
C-I             developer tools
C-t             open fuzzy finder
Alt-t           toggle tree view
```

## FILES AND TABS
```
M-n             jump to tab n
C-`             close current tab
C-T             show current file in tree view
C-P [trad]      start typing 'tree add' to add a new file in the current dir
C-u <dir>       split pane up/down/right/left
C-u C-<dir>     move focus to <dir> pane
alt-o           move focus to next pane
```


## NAVIGATION
```
M-right         move to end of the line
M-left          move to first character of the line
M-up            move to top of paragraph
M-down          move to bottom of paragraph
alt-<           move to top of document
alt->           move to bottom of document
C-a             move to beginning of line
C-e             move to end of line
C-m             jump to matching bracket/tag
alt-g           go to line and optionally column (row:col)
```


## SELECTION
```
C-h             select all
C-A	            select to beginning of line
C-E	            select to end of line
C-l             select the current line
M-shift-up/down	mark and select up or down
C-click         duplicate cursor at click point

```


## EDITING:
```
C-z			    undo
C-Z			    redo
C-x             core cut
C-w	            core cut
C-k             kill to end of line
M-w	            copy
C-y	            paste
C-j             join next line to end of current line
C-D             duplicate current line

C-/	            toggle comment selected text or current line
C-i	            auto-indent selected text
C-’	            fold selection
C-”	            unfold row
C-alt-[         fold block
C-alt-]         unfold block
```

## FIND/REPLACE
```
C-f             find in file
C-shift-f       find within entire project
```

## PLSCRIPT
```
alt-shift-p		toggle plscript mode on
C-b			    set breakpoint
alt-shift-c		clear breakpoints
```

## MARKUP
```
C-shift-m       open markup preview
C-shift-c       print to pdf
```

## LOOK AND THEMES

FONTS:

* BPmono
* Fira
* Roboto Light
* SK Modernist
* Office Code Pro

Edit colors in `styles.less`.

Add functions for custom commands in `init.coffee`

In the command palette, `window:reload` will refresh the view after installing new packages, etc
