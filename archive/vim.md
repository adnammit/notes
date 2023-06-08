MODES:

CUT/COPY AND PASTE:
in visual mode:
    v to select characters. highlight area to cut
    d to cut or y to copy
    P to paste before cursor or p to paste after


esc -- normal mode
i -- insert mode
o -- open: new line below, enter insert mode
O -- open new line above, enter insert mode
v -- visual mode
a -- append mode

COMMANDS:
--MOVING:
gg -- go to beginning of file
G -- go to end of file
0 -- go to beginning of line
$ -- go to end of line	    // same in regex
`. -- move to last edit
j -- move down one line		6j -- move down 6 lines
k -- move up one line		3k -- move up 3 lines
h -- left
l -- right
w -- moves forward one word
b -- moves to beginning of word
e -- moves to end of word
ctrl-b -- scroll back one window
ctrl-f -- scroll forward one window


--EDITING:
d -- delete
dw -- delete word
d0 -- delete to end of line
dd -- delete line
d$ -- delete to beginning of line
d^ --
dG --
dgg -- delete to beginning of file
dG -- delete to end of file
x -- delete one character
v -- highlight one character at a time
V -- highlight one line at a time
ctrl-v -- highlight by column
y -- yank (in visual mode)
4y -- yank 4 lines
p -- paste after current line
P -- paste on current line
dw -- delete word
shift+r -- replace character
shift+R
:read <filename> -- reads in entire contents of <filename> to cursor position

--SEARCHING:
/somephrase -- search forward for somephrase
?somephrase -- search backwards for somephrase
n -- repeat?
:u -- undo last change
:U -- undoes all edits on this line
^r -- redo last change

SAVING:
:q! -- quit without saving
ZQ -- same as :q!
ZZ -- same as :wq
:wq -- write and quit
:w TEST -- save file as TEST
:rm TEST -- delete TEST
:s/old/new/g -- replaces old with new in the first instance
:%s/old/new/g -- replaces every instance in the file
:%s/old/new/gc -- walk through each instance, prompt to sub or not
:help -- help!
:e ~/.vimrc -- use vimrc with more features
CTRL-D + tab -- autocomplete

PANES:
:vs <filename> -- vertical split and open filename
:sp <filename> -- horizontal file split
:qa -- quit all windows
:vs [tab] -- cycle through files in folder



COPY PASTE BETWEEN FILES:
^a [
space ...highlight text... space
^a ] to paste in the other file



