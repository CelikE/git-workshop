# Understanding Git Hooks

Git hooks are scripts that run automatically at specific points in the Git workflow. They allow you to automate tasks, enforce policies, and streamline development processes. Git hooks can be used for a wide range of purposes, such as verifying code quality before a commit, sending notifications after a push, or enforcing commit message formats.

In this guide, we will explain the different types of Git hooks, provide examples of common hooks, and demonstrate how to set up and use them effectively.

## Types of Git Hooks

Git hooks are divided into two main categories:

1. **Client-Side Hooks**: These run on the local repository and can be used to trigger actions at key events like commits, merges, or rebase.
2. **Server-Side Hooks**: These run on the remote repository and are commonly used to enforce policies during operations like pushing or receiving updates.

### Client-Side Hooks

Some common client-side hooks include:

- **`pre-commit`**: Runs before a commit is created. This hook can be used to check the code for syntax errors, lint issues, or perform other code quality checks.
- **`prepare-commit-msg`**: Runs after the default commit message is created but before the editor opens. It is useful for modifying the default commit message.
- **`commit-msg`**: Runs after the commit message is created. It can be used to verify the format of commit messages.
- **`post-commit`**: Runs after a commit has been made. You can use this hook to perform actions such as sending notifications or triggering follow-up tasks.
- **`pre-rebase`**: Runs before a rebase starts, allowing you to prevent a rebase or make modifications beforehand.

### Server-Side Hooks

Server-side hooks are useful for ensuring that rules are followed for all contributors to the repository:

- **`pre-receive`**: Runs on the remote server before any updates are applied. It can be used to enforce security policies or reject commits.
- **`update`**: Runs after `pre-receive` and before any references are updated. It checks each branch separately and can be used to enforce additional checks.
- **`post-receive`**: Runs after updates are made to the repository. It is often used to trigger deployment scripts, notifications, or continuous integration tasks.

## Setting Up Git Hooks

Git hooks are stored in the `.git/hooks` directory within a Git repository. By default, Git provides sample hooks with a `.sample` extension that can be customized and renamed.

### Creating a Hook

1. **Navigate to the Hooks Directory**:
   ```bash
   cd .git/hooks
   ```

2. **Create or Modify a Hook Script**:
   To create a `pre-commit` hook, for instance:
   ```bash
   touch pre-commit
   chmod +x pre-commit
   ```

3. **Add Hook Logic**:
   Edit the `pre-commit` file and add your custom logic. For example, to ensure there are no `TODO` comments left in the code before committing:
   ```bash
   #!/bin/sh
   if grep -r 'TODO' .; then
     echo "Commit aborted: TODO comments found."
     exit 1
   fi
   ```

   This script will prevent the commit if any `TODO` comments are found.

### Common Examples of Git Hooks

#### Example 1: `pre-commit` Hook to Run Linter

A `pre-commit` hook can be used to run a linter like ESLint before a commit is created to ensure code quality.

```bash
#!/bin/sh

# Run ESLint
eslint .

if [ $? -ne 0 ]; then
  echo "Linting errors found. Please fix them before committing."
  exit 1
fi
```

This hook prevents a commit from proceeding if the linter finds any errors.

#### Example 2: `commit-msg` Hook to Enforce Commit Message Format

This hook ensures that commit messages follow a specific format, such as including a ticket ID or a specific prefix:

```bash
#!/bin/sh

TICKET_PATTERN="^[A-Z]+-[0-9]+:"

if ! grep -qE "$TICKET_PATTERN" "$1"; then
  echo "Error: Commit message must start with a ticket ID (e.g., PROJ-123: Description)"
  exit 1
fi
```

This script ensures that each commit message starts with a ticket identifier (e.g., `PROJ-123:`).

#### Example 3: `post-receive` Hook for Deployment

A `post-receive` hook can be set up on a server to automatically deploy changes when they are pushed to the `main` branch:

```bash
#!/bin/sh

while read oldrev newrev ref
do
  if [ "$ref" = "refs/heads/main" ]; then
    echo "Deploying to production..."
    # Add your deployment commands here
    ./deploy.sh
  fi
done
```

This script will automatically trigger a deployment whenever updates are pushed to the `main` branch.

## Best Practices for Git Hooks

1. **Make Hooks Shareable**: By default, hooks are not shared with others when you clone a repository. To share hooks with your team, include them in your repository (e.g., in a `hooks` directory) and provide a script for setting them up.

2. **Use a Hook Manager**: Consider using tools like [Husky](https://typicode.github.io/husky/#/) for managing Git hooks, especially in a collaborative environment. It helps automate the setup and ensures consistency across teams.

3. **Keep Hooks Fast**: Hooks like `pre-commit` and `commit-msg` are run frequently, so make sure they execute quickly to avoid slowing down the development workflow.

4. **Avoid Breaking Changes**: Hooks that prevent actions (such as `pre-commit` or `pre-receive`) should be carefully written to avoid blocking legitimate workflow. Consider providing bypass options if needed.

## Summary

Git hooks are a powerful way to automate actions and enforce standards at various points in the Git workflow. Whether you are ensuring code quality before commits, enforcing commit message formats, or automating deployment after updates, Git hooks provide a flexible mechanism for streamlining and enforcing your team's development practices.

To effectively use hooks:

- Understand the different types of hooks: client-side and server-side.
- Set up hooks by creating or modifying scripts in the `.git/hooks` directory.
- Follow best practices to ensure that hooks are efficient and do not disrupt the workflow.

With well-implemented hooks, you can improve consistency, automate repetitive tasks, and enhance the quality of your development workflow.
