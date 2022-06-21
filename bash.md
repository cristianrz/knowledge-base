# Bash

## Filename and extension

```shell
$ filename=$(basename -- "$fullfile")
$ extension="${filename##*.}"
$ filename="${filename%.*}"
```

## Get n'th line

```shell
$ sed '1q;d' [FILE]
# or
$ awk 'NR == n' [FILE]
```

## Stuff that creates a subshell
* Parent gnome-terminal 61801
* Bash process = 61697
* `eval` keeps same shell
* `{ }` keeps subshell
* `bash -c` makes a subshell
* `( )` makes a subshell
* `|` makes a subshell
