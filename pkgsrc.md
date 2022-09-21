# pkgsrc

## Install

Get the repositories:

```sh
wget https://cdn.netbsd.org/pub/pkgsrc/stable/pkgsrc.tar.gz
tar -xzf pkgsrc.tar.gz -C /usr
```

Get the building tools:

```sh
wget http://cdn.netbsd.org/pub/pkgsrc/packages/Linux/x86_64/Ubuntu-18.04_head/bootstrap.tar.gz
tar -xzf bootstrap.tar.gz -C /
```

## Update

```sh
cd /usr/pkgsrc && cvs update -dP
```

## Installing

```sh
cd /usr/pkgsrc/*/<package>
bmake
bmake install
bmake clean
bmake clean-depends
```