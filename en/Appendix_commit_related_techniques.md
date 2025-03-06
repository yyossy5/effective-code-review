- [Check Commit Messages and Change History for Any Line](#check-commit-messages-and-change-history-for-any-line)
  - [Use VSCode GitLens Extension](#use-vscode-gitlens-extension)
  - [git blame](#git-blame)
- [Edit Commits](#edit-commits)
  - [amend commit](#amend-commit)
    - [Add or Modify Latest Commit](#add-or-modify-latest-commit)
  - [interactive rebase](#interactive-rebase)
    - [When You Want to Include Changes in a Commit Before the Latest One](#when-you-want-to-include-changes-in-a-commit-before-the-latest-one)
    - [Modify Commit Message of a Commit Before the Latest One](#modify-commit-message-of-a-commit-before-the-latest-one)
    - [Combine Multiple Existing Commits](#combine-multiple-existing-commits)
  - [If Something Goes Wrong During Rebase](#if-something-goes-wrong-during-rebase)
- [How to Use Rebase to Incorporate Changes from Target Branch](#how-to-use-rebase-to-incorporate-changes-from-target-branch)


# Check Commit Messages and Change History for Any Line

## Use VSCode GitLens Extension
- Inline commit message display feature

![inline_commit_message](../assets/vscode_gitlens_inline_commit_msg.png)


- Line History feature

![line_history](../assets/vscode_gitlens_line_hist.png)


## git blame
- Check using command or browser (GitHub, Bitbucket) git blame

# Edit Commits

General precautions for commit editing:

- Generally not recommended to rewrite commit history when others might have pulled the changes
    - Especially not recommended after PR submission (commit hashes change, making PR comment links outdated, and others might have pulled)
- Force push needed after editing commits that are already pushed to remote
    - For shared branches, --force-with-lease is recommended over --force
    - --force: Forces push unconditionally, risking accidentally overwriting others' work on shared branches
    - --force-with-lease: Checks remote branch state before pushing. Push fails if someone else has pushed new commits to the same branch. Therefore safer than --force for shared branches
    - For shared branches, better to notify team members even when using --force-with-lease


## amend commit

### Add or Modify Latest Commit

1. Stage the content you forgot to include
2. Commit using git commit --amend
3. git push (force push needed if previous commit was already pushed to remote)

## interactive rebase

### When You Want to Include Changes in a Commit Before the Latest One

Use interactive rebase.

1. Say you have this history and want to include changes in the "Add subtraction feature" commit

```text
(HEAD -> feature/xxx)docs: Update documentation
feat: Add multiplication feature
feat: Add subtraction feature
feat: Add addition feature
```

2. Commit the changes you want to include with a temporary message (say "tmp")

```text
(HEAD -> feature/xxx) tmp
docs: Update documentation
feat: Add multiplication feature
feat: Add subtraction feature
feat: Add addition feature
```

3. Start interactive rebase

Counting from latest commit as 1, "Add subtraction feature" is 4th, so specify HEAD~4

```bash
git rebase -i HEAD~4
```

4. Editor opens showing something like this

```text
pick ghi9101 Add subtraction feature
pick def5678 Add multiplication feature
pick abc1234 Update documentation
pick edf6789 tmp
```

5. Move tmp commit right after "Add subtraction feature" commit, change pick to fixup (or f), and save and exit (in vim, :wq)

```text
pick ghi9101 Add subtraction feature
fixup edf6789 tmp
pick def5678 Add multiplication feature
pick abc1234 Update documentation
```

6. Rebase complete (commit message remains same as original commit)

Commit hashes from "Add subtraction feature" to "Update documentation" have changed.

```text
(HEAD -> feature/xxx)docs: Update documentation
feat: Add multiplication feature
feat: Add subtraction feature
feat: Add addition feature
```

7. When pushing, force push needed if rebased on commits already pushed to remote

```text
git push --force origin feature/xxx
```

By the way, can also use edit or squash instead of fixup.
- edit(e): Modify files during rebase
- squash(s): Like fixup combines multiple commits but allows editing new commit message

### Modify Commit Message of a Commit Before the Latest One
- In rebase screen, change pick to reword for commit you want to modify, save, and editor will open again to modify commit message. Save and exit after modifying.
- For latest commit, can modify using git commit --amend without staging anything

### Combine Multiple Existing Commits

- Same as "When You Want to Include Changes in a Commit Before the Latest One" example above. Combine using fixup or squash

## If Something Goes Wrong During Rebase

Can cancel ongoing rebase process and return to pre-rebase state with this command:

```bash
git rebase --abort
```

Use when:
- Conflicts occur during rebase and resolution is difficult
- Rebase result unexpected and want to start over
- Accidentally started rebase

# How to Use Rebase to Incorporate Changes from Target Branch

Explaining how to use rebase instead of merge.

In this example, development branch is feature/xxx and target branch is develop.

1. Update local develop repository

```bash
git checkout develop
git pull
```

2. Execute rebase on feature/xxx

```bash
git checkout feature/xxx
git rebase develop
```

3. Resolve conflicts if any exist

- Edit files to resolve conflicts
- Stage resolved files with git add
- Continue rebase with git rebase --continue

4. Force push after rebase completion if needed

```bash
git push --force origin feature/xxx
```

References:
- [Merging vs rebasing - Atlassian](https://www.atlassian.com/ja/git/tutorials/merging-vs-rebasing)
- [03_coding_phase_tips.md#consider-rebase-instead-of-merge-when-incorporating-changes-from-target-branch](./03_coding_phase_tips.md#consider-rebase-instead-of-merge-when-incorporating-changes-from-target-branch)
