## man

- `manpath` shows equivalent of `PATH` for man pages
- `export MANPATH=/home/share/localman:/usr/share/man`

## bash

- `fc` to send last command to `$EDITOR`
- `[ -w file ]` and `[ -r file ]` to check for readable and writable files
- `file {-nt|-ot} file` to compare file dates

## Regexp

- `[char]` any character from the set
- `[^char]` any character not in the set
- `\w` any word
- `\s` any white space char
- `\d` any digit
- `a|b` either a or b
- `(expr)` limits scope and captures
- `?`: 0 or 1 of the preceding element
- `*`: 0+ of the preceding element
- `+`: 1+ of the preceding
- `{n}` exactly n instances
- `{min,}` at least min instances
- `{min,max}` matches any number of instances from min to max
- `*?` and `+?` are lazy versions, they catch the first instead of the last
- `.*` is generally dangerous

## /proc

- `cmd` Command or program the process is executing
- `cmdline` Complete command line of the process (null-separated)
- `cwd` Symbolic link to the process’s current directory
- `environ` The process’s environment variables (null-separated)
- `exe` Symbolic link to the file being executed
- `fd` Subdirectory containing links for each open file descriptor
- `maps` Memory mapping information (shared segments, libraries, etc.)
- `root` Symbolic link to the process’s root directory (set with chroot)
- `stat` General process status information (best decoded with ps)
- `statm` Memory usage information

`cmdline` and `environ` files are separated by null characters, filter their contents
through `tr "\000" "\n"`

## strace

- `-f` follow forks
- `-e file` only file operations 

## find

- `find -print0 | xargs -0`  for files with spaces

## mount

- `fuser -c mountpoint` prints the PID of every process that’s using a file or directory on that filesystem
- Then check with `ps -fp "157 315 5049"`

## chmod

- Copy permissions: `chmod --reference=fileA fileB`
- 