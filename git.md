# git

## dotfiles trick

**Warning:** If you want to give these dotfiles a try, you should first fork this repository, review the code, and remove things you don’t want or need. Don’t blindly use my settings unless you know what that entails. Use at your own risk!

``` bash
git clone --bare https://github.com/cristianrz/dotfiles.git "$HOME"/.dotfiles
alias dot='git --git-dir="$HOME"/.dotfiles/ --work-tree="$HOME"'
dot config --local status.showUntrackedFiles no
dot reset --hard
```

## Issue branches workflow

```bash
# Clone your fork to your local machine
git clone git@github.com:USERNAME/FORKED-PROJECT.git

# Add 'upstream' repo to list of remotes
git remote add upstream https://github.com/UPSTREAM-USER/ORIGINAL-PROJECT.git

# Verify the new remote named 'upstream'
git remote -v

# Fetch from upstream remote
git fetch upstream

# View all branches, including those from upstream
git branch -va

# Checkout your master branch and merge upstream
git checkout master
git merge upstream/master

# Create a new branch named newfeature (give your branch its own simple informative name)
git branch newfeature

# Switch to your new branch
git checkout newfeature

################
# YOUR CHANGES #
################

# Fetch upstream master and merge with your repo's master branch
git fetch upstream
git checkout master
git merge upstream/master

# If there were any new commits, rebase your development branch
git checkout newfeature
git rebase master

# Rebase all commits on your development branch
git checkout 
git rebase -i master

# Checkout the branch you're merging to in the target repo
git checkout master

# Pull the development branch from the fork repo where the pull request development was done.
git pull https://github.com/forkuser/forkedrepo.git newfeature

# Merge the development branch
git merge newfeature

# Push master with the new feature merged into it
git push origin master

# Now that you're done with the development branch, you're free to delete it.
git branch -d newfeature
```
