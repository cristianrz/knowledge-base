# Linux processes

## Fork/exec

* When you run a command on the shell if first forks the process and then
  execs the command
* `exec` does not fork
* `ps f` shows processes as a tree
* a child inherits stdin, stdout and stderr

## bg/fg

* Ctrl + z suspends the process
* `jobs` shows jobs running
* `fg` brings a process to the foreground (if there is only one)
* `fg %3` brings process no. 3 to the foreground
* T means stopped, S means sleep
* + is last job and - is second to last

## kill

* `ps ax` show all processes
* `kill -l` list of signals
* `kill -15` sigterm
* `kill` sigterm, kills gracefully
* `kill -2` sigint
* `kill -1` sighup
* `kill -9` sigkill, brute force kill
