# Different  git rebase approaches

Started playing around different ways to approach rebasing since I got tired having to runthrough each conflict per commit.

## Recursive -Xours and 3 way

The idea is to pull everything from main. Then do merge conflict through patch.

Good

* This preserves commit messages
* All your changes are in the patch file

Bad

* Still have some issues from time to time may need to manually do rebase

```bash
# make sure you're in A-XXX branch
git checkout A-XXX

# fetch refs from origin/main
git fetch origin main

# pulls all changes from main and apply the ours strategy, mean will always prioritize changes coming from main
git rebase origin/main -s recursive -Xours

# alternative is to git reset --hard
git reset --hard origin/A-XXX

# --binary will compare binary changes like images
git diff origin/main...origin/A-XXX --binary > changes.patch

# Theirs here would mean "changes in A-XXX"
Ours here would mean "changes in main"
git apply --3way changes.patch

# ---force-with-lease will stop push if it sees new changes in remote
git push --force-with-lease
```

## Rebase -i squash itself

This will combine all commits you made into a single one so we only have to do one merge conflict during rebase.

Good

* All commits message will be consolidated to a single commit
* More consistent

Bad

* Can't preserve commits

```bash
# get the list of commits you have
# git rev-list --count origin/main..origin/A-XXX
# rebase interactive our own branch
git rebase -i HEAD~$(git rev-list --count origin/main..origin/A-XXX)

# press i assuming vi is the default editor
# squash the rest except for the first one
Enter

# this replaces everything with squash and stop at #

:2./^#/s/pick/squash/g
:wq

# will then show preview of the commits/ may skip if there is only one
:wq

# resolve the conflicts this should happen only once sine we have collapsed every commits we had before
git push --force-with-lease
```

## Copilot with git reset hard

This uses reset hard to just accept everything from main. Then get create a patch and do a 3 way to fix any merge conflicts. Then let copilot create commit message

Good

* Easy to remember

Bad

* Commits are gone
* Copilot (alternative is to just write one yourself)

```bash
git switch A-XXX
git reset --hard origin/main
git diff origin/main...origin/A-XXX --binary > changes.patch
git apply --3way changes.patch
git add .

# use copilot to generate the commit messages and commit
git push --force-with-lease
```

## Git log with git reset hard

This uses reset hard to just accept everything from main. Then get create a patch and do a 3 way to fix any merge conflicts. Then let git log create the commit message

Good

* Easy to remember
* All commits message will be consolidated to a single commit

Bad

* Can't preserve commits

```bash
git switch A-XXX
git reset --hard origin/main
git diff origin/main...origin/A-XXX --binary > changes.patch
git apply --3way changes.patch
git add .

# use git log to generate the commit messages and commit
git commit -m "$(git log origin/main..origin/A-XXX --oneline)"
git push --force-with-lease
```

## References

* <https://blog.bigfont.ca/git-rebase-strategy-with-xours/>
* <https://git-scm.com/docs/git-rebase#_merge_strategies>
