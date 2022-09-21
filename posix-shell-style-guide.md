# POSIX shell style guide

## Requirements for all projects

### Directory structure

**Requirement:** project directory has the following structure:

```
command/
├── build/
│   ├── command
│   └── command.1
├── docs/
│   └── index.md
├── src/
│   ├── command.1
│   └── command.sh
├── test/
│   └── test.sh
├── LICENSE
├── Makefile
└── README.md
```


### File extensions

**Requirement:** All files in the source code have a `.sh` extension except
the executable ones.

### Length

**Requirement:** shell programs are shorter than 100 lines. Proper programming
languages are preferred for longer programs (e.g. C, Go)

**Requirement:** functions are shorter than 50 lines.

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

### Shebang

**Requirement:** all scripts start with `#!/bin/sh`

### Layout

**Requirement:** layout is the following:

* Shebang
* Comment with LICENSE
* Comment with short explanation of file/tool.
* sets
* global definitions
* Sources (i.e. `. script`)
* Functions
* Main code

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

### Format

**Requirement:** use `shfmt -s -i 0 -w "$file"`.

Any piece of code that `shfmt` outputs is acceptable.

### Lint

**Requirement:** All files go through `shellcheck -x`.


### Sets

**Requirement:** all scripts use `set -ue` right after the shebang.

To avoid using variables that are not defined and dealing with errors manually
is no fun.

### Naming

**Requirement:** the following convention is used:

| **Type**      | **Local**        | **Global**         |
| ---       | ---                 | ---                |
| Functions | `_lower_with_under` | `lower_with_under` |
| Constants | `_CAPS_WITH_UNDER`   | `CAPS_WITH_UNDER`  |
| Variables | `_lower_with_under`  | `lower_with_under` |

### Documenting code

**Requirement:** all functions are documented as:

```bash
# ls args
# 
# ls lists information  about  the files (the current directory by default).
ls(){
	:
}
```

Document a type, variable, constant, function, or even a package, by writing a
regular comment directly preceding its declaration, with no intervening blank
line.

Notice this comment is a complete sentence that begins with the name of the
element it describes. This important convention allows us to generate
documentation in a variety of formats, from plain text to HTML to UNIX man
pages, and makes it read better when tools truncate it for brevity, such as
when they extract the first line or sentence.

### Local scoping

**Requirement:** functions that need to have local variables, are embeded in a
sub-shell:

```bash
my_function()
(
    echo Hello world
)
```

Avoid the use of `local`. Even though most of the shells support it, its
implementation may vary from shell to shell.

### Error messages

**Requirement:** all error messages go to "stderr".

Example:

```bash
_log_err(){
    printf '%s: %s\n' "$(basename "$0")" "$*" >&2
}

_log_err "could not find directory"
```


### Main

**Requirement:** `main "$@"` is not used

Avoid the use of:

```bash
main(){
    # ...
}

main "$@"
```

it complicates the code for no reason. Exceptions are when you might want to do
unit testing to load the functions without loading `main`.

### While loops

**Requirement:** `while true` is not used. Prefer `while :` instead.

Prefer `while :` over `while true` as `true` is not a builtin in every shell
while `:` is.

### Test

**Requirement:** `test` is not used, `[ ... ]` is prefered.

### Checking return values

**Requirement:** return values for critical actions are checked.

**Requirement:** there are no checks on `$?`, `err="$?"` is preferred.

* for short commands:

```bash
mv "$file" "$dest" || : error handling
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

### `echo` vs `printf`

**Requirement:** `printf` is used when variables are printed.
**Requirement:** `echo` is used when no variables are printed.

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

### Temporary files/directories

**Requirement:** temporary files use `mktemp`.

**Requirement:** temporary files are cleaned up with `trap`.

For temporary files/directories always use `mktemp` and `mktemp -d`
respectively.

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

### Multiline strings

**Requirement:** multi-line prints use `cat << 'EOF'` or `cat << EOF`.

If you find yourself escaping newlines and tabs or setting variables to
multi-line strings, consider using a here document instead.

```bash
cat << 'EOF'
This
is
a
multiline
string
EOF
```

### Be consisent

**Requirement:** new contributed code is consistent with that projects
conventions. This requirement overrides all of the previous.

Above all, if the code you are contributing to has a different style, follow
that style and not the one described here.


## Requirements for critical programs

**Requirement:** All loops have a fixed upper-bound (asserts).

**Requirement:** minimally N assertions for every function of more than M lines
are used. Typical values for N and M are N=2 and M=20.

```sh
if ! assert [ "$p" -gt 0 ]; then
	return 1
fi
```

with the assertion defined as follows:

```sh
assert(){
	if "$@"; then
		printf "%s: assertion '%s' failed\n" "$0" "$*"
		return 1
	fi
}
```

**Requirement:** Variables are declared at the smallest possible level of scope.

**Requirement:** return value of all functions and validity of all function
parameters is checked.

## Recommendations

### Debugging

The `:` operator is useful when debugging:

```bash
: HOME is "$HOME"
```

will output nothing. But when there is a `set -x`, it will output the following:

```bash
+ : HOME is /home/[USER]
```

### Unix principles

The following principles should always be honored:

* Make each program do one thing well. To do a new job, build afresh rather than
complicate old programs by adding new "features".

* Expect the output of every program to become the input to another, as yet
unknown, program. Don't clutter output with extraneous information. Avoid
stringently columnar or binary input formats. Don't insist on interactive input.
Make every program a filter.

* Make data complicated when required, not the program

* Avoid unnecessary output

* Write programs which fail in a way that is easy to diagnose. If an error
occurs, fail immediately and visibily. 

* Choose portability over efficiency.

* Store data in flat text files.

* Focus on MVP (Minimum Viable Product): a version of a product with just enough
features to be usable by early customers who can then provide feedback for
future product development.

* Separate mechanism from policy. Example:

*An everyday example of mechanism/policy separation is the use of card keys to
gain access to locked doors. The mechanisms (magnetic card readers, remote
controlled locks, connections to a security server) do not impose any
limitations on entrance policy (which people should be allowed to enter which
doors, at which times). These decisions are made by a centralized security
server, which (in turn) probably makes its decisions by consulting a database of
room access rules. Specific authorization decisions can be changed by updating a
room access database. If the rule schema of that database proved too limiting,
the entire security server could be replaced while leaving the fundamental
mechanisms (readers, locks, and connections) unchanged.*

*Contrast this with issuing physical keys: if you want to change who can open a
door, you have to issue new keys and change the lock. This intertwines the
unlocking mechanisms with the access policies. For a hotel, this is
significantly less effective than using key cards.*
