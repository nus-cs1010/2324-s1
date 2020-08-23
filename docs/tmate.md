# `tmate`

`tmate` is a tool that allows us to share our terminal with one another.  It is an essential tool for us to run our labs online.

To run `tmate`, type

```
$ tmate
```

on the command prompt on any of the PE nodes.

You should see that the screen is cleared, and a bar appears at the bottom of your screen with the word `[tmate]`.

## First Run

The first time you run `tmate`, you will be asked to generate an SSH key.  You can do this by running `ssh-keygen` and follow the instructions therein.

## Sharing Your Terminal With Someone

After you started `tmate`, you can run:

```
$ tmate show-messages
```

or
```
$ tmate showmsgs
```

This will display a message similar to the following:

```
Sun Aug 23 10:37:06 2020 [tmate] Connecting to ssh.tmate.io...
Sun Aug 23 10:37:06 2020 [tmate] Note: clear your terminal before sharing readonly access
Sun Aug 23 10:37:06 2020 [tmate] web session read only: https://tmate.io/t/ro-fhVJJmuNzpy2qBmpaB6n6v3Cb
Sun Aug 23 10:37:06 2020 [tmate] ssh session read only: ssh ro-fhVJJmuNzpy2qBmpaB6n6v3Cb@sgp1.tmate.io
Sun Aug 23 10:37:06 2020 [tmate] web session: https://tmate.io/t/SDgPyXE9juTRJpb6ghpCvVrxE
Sun Aug 23 10:37:06 2020 [tmate] ssh session: ssh SDgPyXE9juTRJpb6ghpCvVrxE@sgp1.tmate.io
Sun Aug 23 10:37:26 2020 [tmate] tmate can be upgraded to 2.4.0. See https://tmate.io for a list of new features
```

The most important lines are the Lines 3-6.  If you want to show your terminal with someone else, but do not want to give them the control to type into your terminal, you can share the information about the read-only sessions on Line 3-4.

If you want to let someone else type into your terminal, share Line 5-6 with them.  Doing so with your tutors would be very helpful during the tutorial session.

!!! warning "Keep your tmate sessions private"
    Do not at any time, make these links public -- only share them with people you trust.

## Quitting `tmate`

To exit from `tmate`, you just need to hit ++control+d++ on your `tmate` command prompt.  This should bring you back to your usual command prompt (the bar labeling `[tmate]` should disappear).
