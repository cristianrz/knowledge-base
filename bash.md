# Bash

## Filename and extension

```shell
$ filename=$(basename -- "$fullfile")
$ extension="${filename##*.}"
$ filename="${filename%.*}"
```

## Octal/numeric permissions

```shell
$ stat -c "%a %n" *
```

## Get n'th line

```shell
$ sed '1q;d' [FILE]
# or
$ awk 'NR == n' [FILE]
```

