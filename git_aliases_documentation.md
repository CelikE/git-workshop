# Git Aliases and Functions for Productivity

This document provides a list of custom Git aliases and functions that can be used to boost productivity and simplify common tasks when using Git. These aliases are designed to minimize repetitive typing and speed up common workflows.

## Custom Git Functions

### `git_squash_to_first`

This function allows you to squash all commits on the current branch into the first commit. It is useful for cleaning up the commit history before merging a feature branch.

```bash
# Function to squash all commits on the current branch into the first commit
git_squash_to_first() {
    # Get the name of the master branch
    master_branch=$(git remote show origin | sed -n '/HEAD branch/s/.*: //p')
    
    # Find the commit where the current branch diverged from the master branch
    merge_base=$(git merge-base HEAD $master_branch)
    
    # Count the number of commits on the current branch including the first one
    commit_count=$(git rev-list --count $merge_base..HEAD)
    
    if [ $commit_count -le 1 ]; then
        echo "Not enough commits to squash. You need at least 2 commits."
        return 1
    fi
    
    # Perform the interactive rebase
    git rebase -i $merge_base
}
```

### `git_squash_fixups_to_first`

This function allows you to squash all commits, including `fixup!` commits, into the first commit on the branch. It helps maintain a clean and organized commit history.

```bash
# Function to autosquash fixup commits into the first commit of the current branch
git_squash_fixups_to_first() {
    # Get the name of the default branch (e.g., 'main', 'master')
    master_branch=$(git remote show origin | sed -n '/HEAD branch/s/.*: //p')

    # Find the commit where the current branch diverged from the master branch
    merge_base=$(git merge-base HEAD $master_branch)

    # Count the number of commits on the current branch, including the first one
    commit_count=$(git rev-list --count $merge_base..HEAD)

    if [ "$commit_count" -le 1 ]; then
        echo "Not enough commits to squash. You need at least 2 commits."
        return 1
    fi

    # Perform the interactive rebase with autosquash
    git rebase -i --autosquash $merge_base
}
```

## Custom Git Aliases

### General Aliases

- **`jf`**: Runs a Java formatting script located at `./bin/javafmtw`.
  ```bash
  alias jf='./bin/javafmtw'
  ```

- **`gith`**: Lists all branches sorted by the most recent commit date.
  ```bash
  alias gith='git for-each-ref --sort=committerdate refs/heads/'
  ```

- **`gcan`**: Amends the most recent commit without changing its message.
  ```bash
  alias gcan='git commit --amend --no-edit'
  ```

- **`gfwl`**: Force pushes the current branch to origin using `--force-with-lease` to avoid overwriting unintended changes.
  ```bash
  alias gfwl='git push origin $(git rev-parse --abbrev-ref HEAD) --force-with-lease'
  ```

- **`gsq`**: Calls the `git_squash_to_first` function to squash all commits into the first commit on the branch.
  ```bash
  alias gsq='git_squash_to_first'
  ```

- **`gasq`**: Calls the `git_squash_fixups_to_first` function to squash all commits (including fixups) into the first commit on the branch.
  ```bash
  alias gasq='git_squash_fixups_to_first'
  ```

- **`g.`**: Adds all changes in the working directory to the staging area.
  ```bash
  alias g.='git add .'
  ```

- **`gfu`**: Creates a fixup commit for the most recent commit.
  ```bash
  alias gfu='git commit --fixup=HEAD'
  ```

### Combined Workflow Aliases

- **`jag`**: Formats Java code, stages all changes, amends the last commit, and force pushes with lease.
  ```bash
  alias jag='jf && g. && gcan && gfwl'
  ```

- **`jagg`**: Formats Java code, stages all changes, creates a new commit, and pushes the changes to the current branch.
  ```bash
  alias jagg='jf && g. && git commit && git push origin $(git rev-parse --abbrev-ref HEAD)'
  ```

## Summary

These custom Git aliases and functions are designed to improve efficiency in your Git workflow. Whether you are frequently committing, formatting code, squashing commits, or force-pushing, these shortcuts help minimize repetitive tasks and streamline development processes.

Feel free to add more aliases that suit your workflow, and remember to share them with your team to ensure consistency and increase productivity across projects.
