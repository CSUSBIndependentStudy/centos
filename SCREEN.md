# The Linux Screen Utility

You start the screen program by typing _screen_ at the command prompt. 
Screen first creates an initial window and places you inside of it.

To create a new window, and be placed inside of it, type _CTRL-a c_. 
(Hold down the control key while typing a followed by c.) 
Now you have two virtual windows and you are in the second one you created.

To navigate back to the first window, type _CTRL-a spacebar_. 
Type it again to return to the second window. 
_CTRL-a spacebar_ pages through the open windows.

To go to window 0, type _CTRL-a 0_. 
Likewise, to go to window 2, type _CTRL-a 2_.

In general, screen commands are accessed by typing _CTRL-a_ followed by a keystroke. 
The following gives a summary of screen commands for basic navigation.

|-------------------------|-----------------------|
| CTRL-a c                | Create a new window   |
| CTRL-a spacebar         | Go to next window     |
| CTRL-a backspace or del | Go to previous window |
| CTRL-a 2                | Go to window 2        |
| CTRL-a w                | list windows          |

To exit a window, just exit from the shell in that window, and the window is killed.

It is possible to detach a session so that
the session runs independent of your login window.
Consequently, when you exit your login window, the session continues to run. 
When you re-login later, you re-attach the session to your new login window. 
The following commands are relevant.

| ------------- | ----------------------------------------------------- |
| screen -d -r 	| reattach a session, and if necessary, detach it first |
| screen -d 	| detach session from terminal                          |
| screen -list 	| show status                                           |

Sometimes your screen may appear to freeze. 
You know the connection is still good, but key strokes do nothing. 
The problem is you accidentally pressed _Ctrl-S_, 
which causes the terminal window to stop. 
To resume, you need to press _Ctrl-Q_.

