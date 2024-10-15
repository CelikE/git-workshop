# Understanding `git stash`

`git stash` is a powerful command in Git that allows you to temporarily save changes in your working directory without committing them. This is especially useful when you need to switch branches, pull updates, or fix something else without losing your current work. Essentially, it creates a "stash" of your uncommitted changes, allowing you to retrieve and continue working on them later.

In this guide, we will explore what `git stash` is, how it works, and some practical examples of using it in various workflows.

## What is `git stash`?

`git stash` saves your local modifications (changes in tracked files and newly created files) in a stack called the stash stack. This allows you to revert your working directory to a clean state, where there are no uncommitted changes, without discarding your changes.

The syntax to stash changes is:

```bash
git stash
```

This command takes all changes in tracked files and saves them into the stash stack while leaving the working directory clean. By default, untracked files are not stashed unless explicitly mentioned.

## Basic Usage of `git stash`

### Stashing Changes

To save your changes in the stash, simply run:

```bash
git stash
```

This saves all changes (modified files and staged files) and resets your working directory to match the latest commit.

If you also want to stash untracked files, you can use:

```bash
git stash -u
```

The `-u` flag will include untracked files in the stash.

### Viewing Stashed Changes

To view the list of stashed changes, use:

```bash
git stash list
```

This will give you an output similar to:

```
stash@{0}: WIP on feature-branch: a1b2c3d Update feature X
stash@{1}: WIP on main: d4e5f6g Refactor API logic
```

Each stash entry is named as `stash@{index}`. The index starts from `0` and increments for each stash.

### Applying Stashed Changes

To reapply the most recent stash, use:

```bash
git stash apply
```

This will apply the stashed changes back to your working directory. Note that the stash itself will remain in the stash stack after applying.

If you want to apply a specific stash, for example, `stash@{1}`, you can use:

```bash
git stash apply stash@{1}
```

### Popping Stashed Changes

If you want to apply the changes and remove the stash from the stash stack, use:

```bash
git stash pop
```

This will apply the changes and delete the stash entry from the stash stack.

### Deleting a Stash

To remove a specific stash without applying it, use:

```bash
git stash drop stash@{0}
```

Or, to remove all stashed changes:

```bash
git stash clear
```

## Practical Examples of Using `git stash`

### Example 1: Stashing Changes to Switch Branches

Imagine you are working on a feature and need to switch to another branch to fix a bug, but you aren't ready to commit your current changes. You can stash your changes, switch branches, fix the bug, and then reapply your changes.

1. Stash your changes:
   ```bash
   git stash
   ```

2. Switch to the branch that needs fixing:
   ```bash
   git checkout bugfix-branch
   ```

3. Fix the bug, commit the changes, and return to your feature branch:
   ```bash
   git commit -m "Fix critical bug"
   git checkout feature-branch
   ```

4. Reapply your stashed changes:
   ```bash
   git stash pop
   ```

### Example 2: Stashing Only Staged Changes

If you only want to stash staged changes (those that have been added to the index with `git add`), you can use the following:

```bash
git stash --staged
```

This will only stash the staged changes, leaving other modifications in your working directory.

### Example 3: Creating a Named Stash

You can create a named stash to make it easier to identify later. For instance:

```bash
git stash save "WIP - Add unit tests for login"
```

The stash will now appear in the list with the description you provided:

```bash
git stash list
```

```
stash@{0}: WIP - Add unit tests for login
```

### Example 4: Applying a Stash to a Different Branch

You might want to apply stashed changes to a different branch than the one you were working on. To do this:

1. Stash your changes on the current branch:
   ```bash
   git stash
   ```

2. Switch to the desired branch:
   ```bash
   git checkout different-branch
   ```

3. Apply the stashed changes:
   ```bash
   git stash apply
   ```

## Summary

- `git stash` is used to temporarily save your local changes without committing them, allowing you to switch contexts or work on something else while keeping your modifications safe.
- Use `git stash apply` to reapply stashed changes and `git stash pop` to apply and delete the stash simultaneously.
- Stashes are stored in a stack, and you can manage them with commands like `git stash list`, `git stash drop`, and `git stash clear`.

`git stash` is a versatile command that helps keep your workflow smooth, allowing you to handle interruptions or switch contexts without losing any work in progress.
