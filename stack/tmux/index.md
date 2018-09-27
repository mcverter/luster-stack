***TMUX***
==========

Resources
=========
[A gentle guide to TMUX](https://hackernoon.com/a-gentle-introduction-to-tmux-8d784c404340)

Summary
=======
* A screen multiplexer
* Allows you to divide the screen into several regions in which you can run several shells
* Most useful for monitoring processes

Command Line
============
* tmux to start
* exit to exit
* command line is (ctrl-b :)

Sessions
========
* Detach session (PREFIX d)
* `tmux ls` to see active sessions
* `tmux new -s $SESSION_NUME` create named session
* `tmux a -t $SESSION_NUMBER` or `tmux a -t $SESSION_NAME` attach session
* `tmux a #` last session

Working With Panes
==================
* Panes can be split horizontally (ctrl+b ") and vertically (ctrl+b %)
* Panes can be navigated to (ctrl+b ARROW)
* Panes can be resized using the prompt line


Summary of Commands
===================
Start new named session:
tmux new -s [session name]

Detach from session:
ctrl+b d

List sessions:
tmux ls

Attach to named session:
tmux a -t [name of session]

Kill named session:
tmux kill-session -t [name of session]

Split panes horizontally:
ctrl+b "

Split panes vertically:
ctrl+b %

Kill current pane:
ctrl+b x

Move to another pane:
ctrl+b [arrow key]

Kill tmux server, along with all sessions:
tmux kill-server

view raw