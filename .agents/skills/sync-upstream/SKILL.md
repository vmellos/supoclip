---
name: sync-upstream
description: "Sync the repository and user fork (origin: vmellos/supoclip) with latest features, hotfixes, and commits from upstream (FujiwaraChoki/supoclip)."
---

# Sync Upstream

Sync the local repository and the user fork with updates from the original repository (`FujiwaraChoki/supoclip`), preserving all user customizations.

## Remotes Configuration

Ensure remotes match the fork configuration:
- `origin`: `git@github.com:vmellos/supoclip.git` (write destination)
- `upstream`: `git@github.com:FujiwaraChoki/supoclip.git` (read-only upstream)

Verify with `git remote -v`. If missing, configure with:
```bash
git remote add upstream git@github.com:FujiwaraChoki/supoclip.git
```

## Standard Daily Workflow

```bash
# 1. No dia a dia, você trabalha e roda o projeto na branch custom:
git checkout custom

# 2. Quando quiser atualizar com novidades do criador original:
git checkout main
git merge upstream/main
git push origin main

# 3. Incorpora as novidades na sua branch de ajustes:
git checkout custom
git merge main
git push origin custom
```

## Detailed Sync Procedure

### 1. Pre-flight Check
1. Run `git status` to verify the working tree is clean.
2. If there are uncommitted changes, ask the user or stash them (`git stash`) before proceeding.

### 2. Fetch Upstream Changes
```bash
git fetch upstream
```

### 3. Review Incoming Updates
Compare current branch or `main` against `upstream/main`:
```bash
git log HEAD..upstream/main --oneline
```
Identify which areas were modified (e.g. backend, frontend, dependencies).

### 4. Integrate Updates
- **If updating `main`**:
  ```bash
  git checkout main
  git merge upstream/main
  ```
- **If updating a custom branch (`custom`, `feat/*`)**:
  ```bash
  git checkout <custom-branch>
  git merge upstream/main
  ```

### 5. Conflict Resolution
If git reports conflicts:
1. Identify conflicting files with `git status`.
2. Inspect conflicting blocks. Always preserve local customizations and adaptations made by the user.
3. Test resolved files to ensure syntax and build integrity (`make check`).
4. Complete merge: `git commit -m "chore: sync with upstream/main and resolve conflicts"`.

### 6. Verification
Run sanity checks to confirm the codebase remains healthy:
```bash
make check
```

### 7. Push to User Fork
Push updated branches strictly to `origin`:
```bash
git push origin <branch-name>
```
*(Never attempt to push to `upstream`)*
