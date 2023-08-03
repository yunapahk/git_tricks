# 13 Advanced Git Techniques and Shortcuts 
Credits to Fireship's 
[YouTube Video](https://www.youtube.com/watch?v=ecK3EnyGD8o)


## Aliases
Automatically add all the files in the current working directory
```
git commit -am "easy peasy"
```

## Amend 
Create custom command
```
git config --global alias.custom-command "commit -am"
git custom-command "piece of cake"
```
 
Update the latest commit 
```
git commit --amend -m "no sweat"
```

Update last commit with new files with the same message 
```
git commit --amend --no-edit
```
This only works if you haven't already pushed your code to a remote repo. Unless you use:
```
git push origin master --force
```
This will override the remote commit with the state of your local code. If there are commits on the remote branch that you don't have, you will lose them forever!


## Revert
Undo a commit with a new commit
Allows you to take one commit and go back to the state that was there previously

This does not remove the original commit, but instead goes back to the original state 
```
git revert better-days 
```

## Stash/Pop
Remove the changes from your current working directory and save them for later use without committing them to the repo
```
git stash
```
Use after you're ready to add those changes back into your code
```
git pop
```
If you use the command a lot you can use this command followed by a name to reference it later
```
git stash save coolstuff
```

Then when you're ready to work on it again, use:

```
git stash list 
```

When you're ready to apply, use with the corresponding index like so:
```
git stash apply 0
```

Change the branch
```
git branch -M Yuna
```

## Log
View a history of the commits

Use with caution as the output is harder and harder to read as your project grows in complexity 
```
git log
```

For readability, and a more concise breakdown/view of how different branches connect use:
```
git log --graph --oneline --decorate
```

## Bisect
Allows you to start from a commit that is known to have a bug (likely the most recent commit)

Point bisect to the last known working commit to perform a binary search to walk you through each commit in between
```
git bisect start
```
then 
```
git bisect bad
```
then (if the commit looks good) to move on to the next commit
```
git bisect good 
```
Eventually you will find the bad one and know exactly which code needs to be fixed

## Squash
Image you're working on a feature branch that has 3 different commits and you're ready to merge it on the main. All these commit messages are a little pointless and it would be better if it was just one single commit

This can be done from our feature branch by using an interactive option for the main branch 

This will pull up a file with a list of commits on this branch
```
git rebase master --interactive 
```

Should look something like:
```
pick 62079a8 feature complete
pick b95ba00 1
pick e9d38a4 2
pick 63bc7fd 3
```

If you want to use a commit use the pick command

Can also change a message using the reword command or you can combine/squash everything into the original commit using:
```
pick 62079a8 feature complete
squash b95ba00 1
squash e9d38a4 2
squash 63bc7fd 3
```
Save the file and close it. Git will pull up another file prompting you to update the commit message.

By default this will automatically be a combination of all the messages that you just squashed. 

If you don't like all the messages to be combined, you can use 
```
git commit --fixup fb2f677
```
and 
 ```
 git commit --squash fc2f677
 ```
When making commits on your branch. This tells git in advance that you want to squash them so when you go to do a rebase with the auto squash flag, it can handle all the squashing automatically.

## Hooks
Whenever you perform an operation with git like a commit for example, it creates an event 

Hooks allow you to run some code either before or after that event happens 

If you look in the hidden git directory, you'll see a directory of hooks and inside that, a bunch of different scripts that can be configured to run when different events in git happen

Install NPM husky to make it easier to configure git hooks
* This will create a script that will validate or link your code before each commit 
* This can help improve your overall code quality 

## Destruction
* Image you have a remote repo on github then a local version on your machine that you've been making changes to that haven't been going to well

* If you just want to go back to the original state and the remote repo, use:

(catches the latest code in the remote repo)
```
git fetch origin 
```
then (override local code with remote code)
```
git reset --hard origin/master 
```
* Be careful, your local changes witll be lost forever, but you might still be left with some random untracked files or build artifacts here and there.

* To remove those files as well, use:
```
git clean -df
```

* If you're sick of git at this point and want to get rid of them all together, Apache subversion will help you change things up a bit

* Just delete that hidden git folder or use command:
```
rm -rf .git 
```

## Checkout
* Go directly back to the previous branch 
```
git checkout - 
```