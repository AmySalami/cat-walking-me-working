# Workflow

Git workflow for Pomodoro Meadow. Followed even though the project is solo — for practice, safety, and portfolio quality.

## Quick rule

- **Tiny change** (typo, 1-line CSS) → push to `main` directly
- **Anything else** → feature branch → PR → merge

## Branch lifecycle

```bash
# 1. Start a branch
git checkout -b feature/keyboard-shortcuts

# 2. Work — commit as you reach logical checkpoints
git add . && git commit -m "feat: add S/R/Space keyboard shortcuts"

# 3. Push the branch
git push -u origin feature/keyboard-shortcuts

# 4. Open a Pull Request
gh pr create --title "Add keyboard shortcuts" --body "..."
# (or via GitHub UI: https://github.com/AmySalami/cat-walking-me-working)

# 5. Merge into main
gh pr merge --squash
# (or "Squash and merge" in GitHub UI)

# 6. Clean up
git checkout main
git pull
git branch -d feature/keyboard-shortcuts
```

## Branch naming

```
feature/<short-description>    e.g. feature/keyboard-shortcuts
fix/<short-description>        e.g. fix/embed-blocked-tracks
refactor/<short-description>   e.g. refactor/single-source-of-truth
docs/<short-description>       e.g. docs/establish-workflow
chore/<short-description>      e.g. chore/update-dependencies
```

- short kebab-case
- describes intent, not implementation

## Commit message style — Conventional Commits

```
<type>(<scope>): <short summary>

<optional longer body explaining what & why>

<optional footer e.g. Co-Authored-By:>
```

**Types**
- `feat` — new feature
- `fix` — bug fix
- `refactor` — restructure code (no behavior change)
- `style` — CSS / visual polish
- `docs` — documentation
- `chore` — maintenance, config, deps
- `perf` — performance

**Examples**
```
feat(music): replace Spotify with YouTube + fix SoundCloud stalls
fix(onboarding): correct spacing between section title and presets
docs: establish branch + PR workflow
```

## Versioning — SemVer

```
vMAJOR.MINOR.PATCH
```

- **MAJOR** — breaking change (rare)
- **MINOR** — new feature
- **PATCH** — bug fix or small polish

Tag a release after merging to `main`:

```bash
git tag -a v1.3.0 -m "Add keyboard shortcuts"
git push --follow-tags
```

Tags become GitHub Releases automatically.

## PR description template

Copy into the PR body:

```markdown
## What
Brief description of what changed.

## Why
The user-facing problem or technical motivation behind the change.

## How
Key implementation decisions, especially anything non-obvious.

## Test plan
- [ ] Tested locally via `http://localhost:8765/`
- [ ] Tested PiP mode (if visual/layout change)
- [ ] Tested onboarding flow if relevant (clear localStorage to retrigger)
- [ ] No console errors
- [ ] Works on both SoundCloud + YouTube sources if music-related

## Screenshots
(if visual change)
```

## Why this matters even for solo

1. **Portfolio quality** — recruiters look at branch + PR history
2. **Safety** — `main` auto-deploys to GitHub Pages. Feature branches let you test without breaking the live site
3. **Discipline** — same workflow you'd use at a job
4. **History** — PR descriptions become documentation for future-you

## Hard rules — never do these

- Push experimental code directly to `main`
- Skip the PR step "because it's solo"
- Tag untested commits as releases
- Force-push to `main`
- Commit secrets / API keys
- Test by opening `index.html` via `file://` (always use the local server)
