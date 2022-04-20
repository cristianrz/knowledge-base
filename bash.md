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

