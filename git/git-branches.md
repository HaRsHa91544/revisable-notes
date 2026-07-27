# Git Branches
## Why Git Branches?
- Sometimes you need to make some changes or add some new code into your project and those changes may be buggy or incorrect and you don't want to directly modify the main code.
- In this scenario, without branches you need to create a new copy of the project folder then you can make experiments but it can take long time for large projects.
- So, to solve this problem git branches are introduced.

## What is actually a Git Branch?
- Before understanding what is a branch we need to understand how commits are tracked.
- The commits of the repo are stored as a tree and every new commit object has a pointer to its parent commit object as shown in the below figure.
- <img width="2391" height="792" alt="image" src="https://github.com/user-attachments/assets/bd386aa8-7717-4246-bca2-09aaeae0a6ca" />
- Github branch is just nothing but a `movable pointer` that is pointed to any one of those commits in a repo. The default branch name in Git is called `master`.

## My Misconceptions
- I thought braches would contain commits but the thing is different here.
- All commits form a tree and the branches are just labels placed on specific commits.

## What if 2 branches are pointing to same commit and how does git knows which branch you are in?
- Git uses a pointer called `HEAD` to know which branch you are in.
- We can check which branch HEAD is pointing to by running this command `git log --oneline --graph --decorate --all`.
- We can switch between branches by running this command `git switch branch-name`.

## Git commands related to branches
- `git branch` -> List of branches
- `git branch branch-name` -> Create a branch
- `git switch branch-name` -> Switch to given branch
- `git switch -c branch-name` -> Creates & switch to given branch
- `git branch -d branch-name` -> Deletes a given branch
- `git log --oneline --graph --decorate --all` -> HEAD pointer and commits graph
