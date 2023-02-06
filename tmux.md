# Tmux

## Start new session

```terminal
$ tmux new -s [session_name]
```

## Detach from session

`ctrl + B`, `d`

## List sessions

```bash
tmux ls
```

## Re-attach session

```term
tmux attach -t [session_name]
```

## Re-attach to last session, otherwise new one

```bash
tmux attach || tmux new
```

## Start command in tmux

```bash
tmux new-session -s abc 'my_command'
```

## Start command in tmux in the bg

```bash
tmux new-session -d -s abc 'my_command'
```

## Using tabs

- `ctrl + b c`: new tab
- `ctrl + b NUMBER`: go to tab NUMBER

