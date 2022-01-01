# POSIX shell style guide

## File extensions

All files in the source code need to have a `.sh` extension but not the
executable ones.

## Format

Use `shfmt` with the following flags:

```sh
shfmt -s -i 0 -w
```

Any piece of code that `shfmt` outputs is acceptable.

## Lint

Use `shellcheck -x` over your code.

## Local scoping

If functions need to have local variables, they can be embeded in a sub-shell:

```bash
my_function()
(
    echo Hello world
)
```

Avoid the use of `local`. Even though most of the shells support it, its
implementation may vary from shell to shell.

## Sets

It is recommended to use `set -ue` right after the shebang to avoid using
variables that are not defined and dealing with errors manually is no fun.

## Naming

| **Type** | **Internal** | **Public** |
| --- | --- | --- |
| Functions | `_lower_with_under` | `lower_with_under` |
| Constants | `CAPS_WITH_UNDER` | `CAPS_WITH_UNDER` |
| Variables | `lower_with_under` | `lower_with_under` |

## Main

Generally, avoid the use of:

```bash
main(){
    # ...
}

main "$@"
```

it complicates the code for no reason. Exceptions are when you might want to do
unit testing to load the functions without loading `main`.

## Function length

Prefer small and focused functions.

Long functions are sometimes appropriate, so no hard limit is placed on
function length. If a function exceeds about 40 lines, think about whether it
can be broken up without harming the structure of the program.

Even if your long function works perfectly now, someone modifying it in a
few months may add new behavior. This could result in bugs that are hard to
find. Keeping your functions short and simple makes it easier for other people
to read and modify your code.

You could find long and complicated functions when working with some code.
Do not be intimidated by modifying existing code: if working with such a
function proves to be difficult, you find that errors are hard to debug, or you
want to use a piece of it in several different contexts, consider breaking up
the function into smaller and more manageable pieces.

## Shebang

Always `#!/bin/sh`

## While loops

Prefer `while :` over `while true` as `true` is not a builtin in every shell
while `:` is.

## Layout

* Shebang
* Comment with LICENSE and possibly short explanation of file/tool.
* Global sets and traps
* Sources
* Global variables
* Functions
* Main

```bash
#!/bin/sh
#
# LICENSE
#
# Short description of this file's purpose 

set -eu

. [FILE]

HELLO="Hello, world!"

_my_function(){
	printf '%s\n' "$HELLO"
}

date
_my_function
```

## Debugging

The `:` operator is useful when debugging:

```bash
: HOME is "$HOME"
```

will output nothing. But when there is a `set -x`, it will output the following:

```bash
+ : HOME is /home/[USER]
```

## Error messages

All error messages should go to `STDERR`:

```bash
_log_err(){
    printf '%s: %s\n' "$(basename "$0")" "$*" >&2
}

_log_err "could not find directory"
```

## Documenting code

Document a type, variable, constant, function, or even a package, by writing a
regular comment directly preceding its declaration, with no intervening blank
line.

```bash
# ls lists information  about  the FILEs (the current directory by default).
# ls(flags)
ls(){
```

Notice this comment is a complete sentence that begins with the name of the
element it describes. This important convention allows us to generate
documentation in a variety of formats, from plain text to HTML to UNIX man
pages, and makes it read better when tools truncate it for brevity, such as
when they extract the first line or sentence.

## Test

`[ ... ]` is prefered over `test`.

## Checking return values

Always check return values and give informative return values.

* for short commands:

```bash
mv "$file" "$dest" && : error handling
```

* for other commands

```bash
if ! mv "$file" "$dest"; then
	: error handling
fi
```

avoid checking for `$?` as it is easy to lose track of which command `$?`
actually corresponds to. If you insist to use `$?` store its value somewhere
else:

```bash
rm file.txt
err="$?"
# ...
if [ "$err -ne 0 ]; then
	: error handling
fi
```

## Be consisent

Above all, if the code you are contributing to has a different style, **follow
that style** and not the one described here.

## `echo` vs `printf`

Interpolating variables within strings passed to `echo` can have unexpected
results: `printf` is preferred. Where no variables are going to be printed,
`echo` is preferred.

```bash
who=world

echo Hello, world # This is OK
printf 'Hello, %s\n' "$who" # This is OK

# echo "Hello $who" / This is not OK
# echo Hello "$who" / This is not OK
```

## Temporary files/directories

For temporary files/directories always use `mktemp` and `mktemp -d` respectively.

## Cleaning up

Use bash traps to clean up any temporary files created by your script, or
operate on any remote resources that need to be destroyed when your scrip
texits.

```bash
_cleanup() {
    rm -f "$_tmp"
}

_tmp="$(mktemp)"
trap '_cleanup' EXIT

# rest of your code\
```

## Multiline strings

If you find yourself escaping newlines and tabs or setting variables to
multiline strings, consider using a here document instead.

```bash
cat << 'EOF'
This
is
a
multiline
string
EOF
```

## Unix principles

The following principles should always be honored:

* Make each program do one thing well. To do a new job, build afresh rather than complicate old programs by adding new "features".
* Expect the output of every program to become the input to another, as yet unknown, program. Don't clutter output with extraneous information. Avoid stringently columnar or binary input formats. Don't insist on interactive input. Make every program a filter.
* Separate mechanism from policy. Mechanisms must, by necessity, be under the control of people with considerable programming or systems expertise, while policy should be controllable by people who have no such expertise.
* Make data complicated when required, not the program
* Avoid unnecessary output
* Write programs which fail in a way that is easy to diagnose. If an error occurs, fail immediately and visibily. 
* Value developer time over machine time
* Choose portability over efficiency.
* Store data in flat text files.

## Function comments

Function comments should have the following form:

```bash
# Returns the sum of a and b
# my_func(a, b)
my_func(){
    printf '%s+%s' "$a" "$b" | bc -l
}
```
