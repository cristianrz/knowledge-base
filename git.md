# git

## dotfiles trick

**Warning:** If you want to give these dotfiles a try, you should first fork this repository, review the code, and remove things you don’t want or need. Don’t blindly use my settings unless you know what that entails. Use at your own risk!

``` bash
git clone --bare https://github.com/cristianrz/dotfiles.git "$HOME"/.dotfiles
alias dot='git --git-dir="$HOME"/.dotfiles/ --work-tree="$HOME"'
dot config --local status.showUntrackedFiles no
dot reset --hard
```
