# pkgsrc

pkgsrc is a package management system for Unix-like systems, including NetBSD, Linux, and macOS. It allows users to easily install, update, and remove software packages from the command line.

## Installation

To install pkgsrc, first download the repository using `wget`:

```
wget https://cdn.netbsd.org/pub/pkgsrc/stable/pkgsrc.tar.gz
```

Then extract the downloaded tarball to the `/usr` directory:

```
tar -xzf pkgsrc.tar.gz -C /usr
```

Next, navigate to the `bootstrap` directory and run the `bootstrap` script:

```
cd /usr/pkgsrc/bootstrap ./bootstrap
```

This will set up the pkgsrc environment and install the necessary tools for managing packages.

## Updating pkgsrc

To update pkgsrc to the latest version, use the `cvs` utility:

```
cd /usr/pkgsrc && cvs update -dP
```

## Installing Packages

To install a package using pkgsrc, navigate to the package's directory and run `bmake`:

```
cd /usr/pkgsrc/*/<package>
bmake
bmake install
```

After the package has been installed, you can clean up the build files by running `bmake clean` and remove any unnecessary dependencies with `bmake clean-depends`.
