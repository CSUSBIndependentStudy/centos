# Working with vi

## Overview

Vi is a text editor found in most UNIX environments such as OS X and Linux. 
Vi is convenient for system administrators because it works inside both
local and remote terminal windows.

What is normally installed is a program called _vim_, 
which stands for vi improved. 
You can start vi (or vim) by typing _vi_ at the command prompt.

## Buffers

In vi, there is a general purpose buffer and 36 named buffers. 
Whenever a block of text is yanked (cut), 
it is placed in the general purpose buffer. 
Prefixing a yank action with an apostrophe 
followed by a letter or number 
places the yanked text into the buffer with that letter or number as its name. 
For example, the following command yanks 3 words into buffer m:

    'm3yw

## VI paradigm

Vi combines an action with a movement -- 
the action is performed on all lines or characters 
between the current cursor position and the destination cursor position. 
The general syntax is _number verb target_, 
where _number_ defaults to one, 
_verb_ defaults to move, 
and _target_ is implied for some verbs.
For example, the following command copies 3 lines from the current line 
into the general purpose buffer.

   3yy

## The .vimrc file

Each time vi starts to run, it looks for a file in your home directory called _.vimrc_. 
If it finds this file, it will execute the instructions contained within it. 
Thus, you can place vi commands in this file 
to set preferences that you want to start with in each of your vi sessions. 
The following is the contents of a .vimrc file that I sometimes use.

~~~~
:set ts=3
:fixdel
~~~~

The first line sets tab spacing (ts) to 3, which causes vi to display tabs as 3 spaces 
rather than the common default of 8 spaces. 
Remember, the contents of the file are not changed; 
only the manner in which a tab character is displayed is changed. 

The second line allows the DEL key to work as expected on PC keyboards. 

Some other settings that may be useful include the following:

~~~~
:set noautoindent
:set nocindent
:hi Search NONE
~~~~

The last line turns off color highlighting of strings located in searches. 
(Searches are done with the slash (/) character.

<h2>VIM Commands</h2>

<h3>Miscellaneous</h3>

<pre>
DEL     cancel operation
ESC     leave insert mode
^G      display file statistics in bottom line
.       repeat last change
:r f    read file f, place after current line
~       change the case of a char
J       join current line with next
^Vk     used in insert mode to enter keystroke k
        into the edit buffer 
</pre>

<h3>Commands inside insert mode</h3>

<pre>
^U      erase to start of insert line
</pre>

<h3>Searching</h3>

<pre>
/abc    search forward for abc
/^abc   search for occurrances of abc 
        only at the beginning of lines
/abc$   search for occurrances of abc 
        only at end of lines
?abc    search backward for abc
n       go to next occurrance in same direction
/       same as n, but for foward searching
N       go to next occurrance in opposite direction
5G      go to line 5
G       go to eof
</pre>

<h3>Substitute</h3>

<pre>
s/abc/abcd/      substitue abc with abcd
%s/abc/abcd/g    substitute abc with abcd globally between 1,$
s/\.doc/\.txt/   substitue .doc with .txt
                 (you must escape special characters 
                 used for regular expressions, such as ".") 
</pre>

<h3>File Commands</h3>

<pre>
:w             write file
:w filename    write over filename
:wq            write and quite
ZZ             same as&nbsp;:wq
:q!            quit without writing
</pre>

<h3>Delete (always places text in general buffer)</h3>

<pre>
dw     del word (from current pos to end of word)
db     del backwards (del previous word)
d4w    del 4 words
dd     del current line
3dd    del 3 lines
'b3dd  del 3 lines into buffer b
d^     del from current pos to beginning of line
d$     del from current pos to end of line
x      del char
3x     del 3 chars
cc     del line and enter insert mode
dL     del lines from current pos to end of file
d3L    del lines from current pos to 3rd line from bottom
</pre>

<h3>Copy (Yank)</h3>

<pre>
y     same as d, but copies rather than cuts
</pre>

Paste (Note: paste is from general buffer unless otherwise specified)

<pre>
p     paste after current pos
P     paste before current pos 
3p    paste 3 copies
'kp   paste from buffer k
'k3p  paste 3 copies from buffer k
</pre>

<h3>Insert</h3>

<pre>
i  enter insert mode before cursor
a  enter insert mode after cursor
A  enter insert mode after end of current line
3i inserted text is replicated 3 times
o  create new line below current line, 
   and enter insert mode
O  create new line above current line, 
   and enter insert mode
</pre>

<h2>Navigate</h2>

<pre>
:5   go to line 5
5G   go to line 5
G    go to last line in document
L    go to last line in screen
5L   go to line 5 from bottom of screen
h    left 1 char
3h   left 3 chars
j    down
k    up
l    right
^    beginning of line
$    end of line
%    press % over an openning or closing bracket or 
     parenthesis in order to jump to its closing or 
     openning partner
^D   scroll down one screen
^U   scroll up one screen
^F   go forward, like ^D, but overlap a few lines
^B   go backward, like ^U, but overlap a few lines
``   return to previous pos in file
+    go to first non-white space of next line
-    go to first non-white space of previous line
w    beginning of next word
e    end of word (if at end, then end of next word)
b    back (go to beginning of previous word)
</pre>

<h2>Replace</h2>

<pre>
r  c  replace current char with c
3r c  replace current char with 3 c's
</pre>

<h2>Undo</h2>

<pre>
u   undo previous change
3u  undo last 3 changes
U   undo changes made to current line
:e! undo all changes since last save
</pre>

<h2>Indenting</h2>

<pre>
&gt;&gt;            indent line one shift width
3&gt;&gt;           indent 3 lines one shift width
:set sw=4     set shift width to 4
:syntax off   turn off syntax recognition (and coloring)
:syntax on    turn on syntax recognition
</pre>

<h2>Settings</h2>

<pre>
:set ai        set autoindent
:set noai      unset autoindent
:set nu        show line numbers
</pre>

<h2>Customization</h2>

Place a list of vi commands -- such as the following -- into a file called .vimrc in your home directory. These commands will be run by vi when it starts.

<pre>
:fixdel
:set ts=3
:set noautoindent
:set nocindent
:hi Search NONE
:filetype indent off
:filetype off
</pre>

