# `tmate`

`tmate` is a tool that allows us to share our terminal with others.  It is an essential tool for peer learning during the lab sessions.

To run `tmate`, type

```
$ tmate
```

on the command prompt on any of the PE nodes.

This command clears the screen and displays a bar at the bottom with the word `[tmux]` as well as the following message:

```
Tip: if you wish to use tmate only for remote access, run: tmate -F                                                                                                                                                               [0/0]
To see the following messages again, run in a tmate session: tmate show-messages
Press <q> or <ctrl-c> to continue
---------------------------------------------------------------------
Connecting to tmate.comp.nus.edu.sg...
Note: clear your terminal before sharing readonly access
ssh session read only: ssh -p2200 ro-XmKETJTna9Rapc3hmqP2BLt9N@tmate.comp.nus.edu.sg
ssh session: ssh -p2200 eLdTHTzshFhduQMj7JJbfBt5x@tmate.comp.nus.edu.sg
```

Follow the instructions given.

## Sharing Your Terminal With Someone

The most important lines in the message above are the last two lines.  

- If you want to show your terminal to someone else, but do not want to give them the control to type into your terminal, you can share the information about the read-only sessions on the second last line.   This is useful, for instance, if your tutor wants to share your screen with the class.
- If you want to let someone else type into your terminal, share the last line with them.  Doing so with your tutors might be helpful during the lab session for debugging or demonstration purposes.

!!! warning "Keep your tmate sessions private"
    Do not at any time, make these links public -- only share them with people you trust.

## Quitting `tmate`

To exit from `tmate`, you just need to hit ++control+d++ on your `tmate` command prompt.  This should bring you back to your usual command prompt (the bar labeling `[tmux]` should disappear).
