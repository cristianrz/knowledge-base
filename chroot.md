## Accessing graphical applications inside the chroot

You can run graphical applications within a chroot, but you need to provide an
X server for them to run in first. The easiest way to do this is to set the
display of the chroot system to be identical to the root display of your
system\'s main X server and provide access to it.

In other words, in the chroot shell type

```sh
export DISPLAY=:0.0
```

And in the system shell type

```sh
xhost +
```

Any X command you type will now get its own window as you\'re used to, but as it
is running inside the chroot jail it will not be able to see your normal file
system.

You don\'t have to enter the chroot shell to access its commands. Suppose you
want to run Firefox in a chroot jail in order to avoid security problems with
signed Java applets and other components which otherwise would have access to
your personal files. You can do this by running the command

```sh
gksudo chroot /var/chroot firefox -DISPLAY=:0.0
```

This command can also be invoked from the menu, or a panel applet or desktop
shortcut. 
