---
name: sync-upstream
description: "Sync the repository and user fork (origin: vmellos/supoclip) with latest features and hotfixes from upstream (FujiwaraChoki/supoclip). Trigger whenever user says 'Sincronize com o projeto original', 'Puxe as novidades do upstream', 'atualizar fork', or similar."
---

# Sync Upstream

Synchronize the local repository and the user fork with updates from the original repository (`FujiwaraChoki/supoclip`), preserving all user customizations.

## Trigger Phrases
Activate this skill whenever the user asks:
- *"Sincronize com o projeto original"*
- *"Puxe as novidades do upstream"*
- *"Atualizar fork"*
- *"Sync upstream"*

---

## Remotes Configuration

- `origin`: `git@github.com:vmellos/supoclip.git` (write destination - user fork)
- `upstream`: `git@github.com:FujiwaraChoki/supoclip.git` (read-only upstream - original repository)

Verify with `git remote -v`.

---

## Protocol & Execution Workflow

Follow this 2-stage execution protocol to keep `main` clean and `custom` updated:

### 1. Pre-flight Check
1. Run `git status` to verify the working tree is clean.
2. If there are uncommitted changes, stash them with `git stash` before proceeding.

### 2. Fetch Upstream Changes
```bash
git fetch upstream
```

### 3. Review Incoming Updates
Compare `main` against `upstream/main`:
```bash
git log main..upstream/main --oneline
```
If there are no incoming commits, inform the user that the repository is already 100% up to date.

### 4. Etapa 1: Atualizar a `main`
Checkout `main`, merge upstream changes cleanly, and push to origin:
```bash
git checkout main
git merge upstream/main
git push origin main
```
*Note: `main` must remain a 1:1 mirror of `upstream/main`. Never commit customizations directly to `main`.*

### 5. Etapa 2: Selecionar a `custom` e incorporar as novidades
Checkout the user's custom branch (`custom`), merge the updated `main`, and push to origin:
```bash
git checkout custom
git merge main
git push origin custom
```

### 6. Conflict Resolution (if applicable)
If git reports conflicts when merging into `custom`:
1. Identify conflicting files with `git status`.
2. Inspect conflicting blocks. Always protect and preserve user customizations, personal configurations, and custom rules.
3. Test resolved files (`git diff`).
4. Finalize the merge: `git commit -m "chore: sync with upstream/main and preserve custom adjustments"`.
5. Push to origin: `git push origin custom`.

### 7. Completion
Ensure the repository is left on branch **`custom`** so the user can immediately resume working. Report a concise summary of the incoming commits integrated.
