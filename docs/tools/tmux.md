# tmux

[tmux][1] is an open-source terminal multiplexer for Unix-like  operating systems. It allows multiple terminal sessions to be accessed  simultaneously in a single window. It is useful for running more than  one command-line program at the same time.

All commands in `tmux` start with the prefix. tmux default prefix id ++ctrl+b++

## tmux sessions

Starting a new session:

 `tmux new -s <name_of_session>`

- -s - name of session 

Nested tmux sessions (tmux in tmux) are not allowed by default. A new session can be created with the `-d` parameter (detach)

`tmux new -s <name_of_session> -d`

By default, tmux assigns each session and number, starting with 0. This can be changed with 

**Renaming session**: ++ctrl+b++ ++"$"++


A session can be detached (will not be terminated). With `tmux ls` the detached sessions can be listed and with `tmux attach -t <session_name>`  reattached

**Exiting session without terminating it**: ++ctrl+b++ ++d++

A session can be terminated/killed with

 `tmux kill-session -t <session_name>`

> Anything open in the session that will be terminated, will be lost. Handle with care and save the work before killing a session

It is possible to change sessions witout detaching an attachin sessions. 

++ctrl+b++ ++s++ offers to select the session with arrow-keys


To close all sessions except the one specified by the `-t` argument.

`tmux kill-session -a -t <session_name>`

The base directory is the directory where tmux was started. All new windows and panes will have that same base directory. To change the  base directory

++ctrl+b++ ++":"++ - to get into the command prompt and type

`attach -c /dir/ectory/path` or `a -c /dir/ectory/path` or

## tmux windows

Starting a new session, automatically starts a new windows as well. 

If a new window inside the same session is needed, it can be created with ++ctrl+b++ ++c++

++ctrl+b++ ++comma++ renames the current window and you switch through windows with

Cycling between windows is done with ++ctrl+b++ ++n++ or ++ctrl+b++ ++p++. Another way is ++ctrl+b++ ++w++ wich will display all windows from wich we can select the window (and pane) we want to activate with the ++left++ ++right++ ++up++ ++down++

To close an unresponsive window (and all panes in it) we can use ++ctrl+b++ ++"&"++

## tmux panes

A window can be split horizontally with ++ctrl+b++ ++"\""++  and vertically with

++ctrl+b++ ++"%"++ Using ++ctrl+b++ ++left++ ++right++ ++up++ ++down++ changes the current active pane

++ctrl+b++ ++ctrl+left+right+up+down++ resize the pane. Typing `exit` closes the current pane. 

++ctrl+b++ ++0++ - ++9++ switches to the pane with the associated number. ++ctrl+b++ ++space++ toggles between different pane layout.

++ctrl+b++ ++o++ switches the most used panels, if pressed at the same time it switches the panes to the pane whith the current active prompt 

If a pane is unresponsive it can be killed with ++ctrl+b++ ++x++ and confirm with `Y`

To move the pan clockwise it ++ctrl+b++ ++"{"++ can be used. For counterclockwise movement use ++ctrl+b++ ++"}"++

Converting a pane to a window is done with ++ctrl+b++ ++"!"++. ++ctrl+b++ ++q++ shows the panel numbers.

## tmux copy mode

Enter with ++ctrl+b++ ++"["++. With ++ctrl+s++ the whole text wall can be searched.

[1]: https://en.wikipedia.org/wiki/Tmux	"tmux on wikipedia"
