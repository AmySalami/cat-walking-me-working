# Workflow

Git workflow for Pomodoro Meadow. Followed even though the project is solo — for practice, safety, and portfolio quality.

## Quick rule

- **Tiny change** (typo, 1-line CSS) → push to `main` directly
- **Anything else** → feature branch → PR → merge

## Who does what

| Step | Where | Who |
|---|---|---|
| Branch + commits | Local (terminal) | Claude (with my approval) |
| Push branch | Local (terminal) | **Me** |
| Open PR | GitHub web UI | **Me** |
| Merge PR | GitHub web UI | **Me** |
| Pull main + cleanup | Local (terminal) | **Me** |
| Tag release | Local (terminal) | **Me** |

Claude prepares the branch + drafts a PR description but does not push, open PRs, or merge. I handle all the GitHub-side actions through the web UI.

## Branch lifecycle

```bash
# 1. Claude creates the branch + commits locally
git checkout -b feature/keyboard-shortcuts
# ... Claude makes changes + commits ...

# 2. I push (after reviewing what Claude did)
git push -u origin feature/keyboard-shortcuts

# 3. I open the PR on GitHub
#    → https://github.com/AmySalami/cat-walking-me-working
#    → "Compare & pull request" banner shows up after push
#    → Paste the PR description Claude drafted

# 4. I merge on GitHub
#    → "Squash and merge" (or regular merge if I want commit history preserved)
#    → "Delete branch" button after merge

# 5. I sync local main + clean up
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

## Versioning — SemVer (required for every behavior-changing merge)

```
vMAJOR.MINOR.PATCH
```

| Commit type in PR | Bump | Example |
|---|---|---|
| `feat:` — new functionality | **MINOR** — `v1.3.0 → v1.4.0` | new character, new music source |
| `fix:` — user-visible bug fix | **PATCH** — `v1.4.0 → v1.4.1` | broken playback, layout glitch |
| `feat:` with breaking change | **MAJOR** — `v1.4.0 → v2.0.0` | rename of localStorage key, removed feature |
| Mixed `feat:` + `fix:` | Highest bump (MINOR) | |
| `refactor:` / `style:` / `perf:` | No bump required | optional checkpoint tag if I want |
| `docs:` / `chore:` only | **No tag** | these don't ship behavior |

**Tag every release-worthy merge.** `main` is what GitHub Pages deploys, so every merge ships to users — every shipped change deserves a version record.

```bash
# After merging the PR + git pull on main:
git tag -a v1.4.1 -m "Fix playlist auto-skip on region-blocked tracks

Some YouTube tracks region-block embed silently — the player would stall
instead of advancing. Now the onError handler treats 101/150 as a skip
signal in playlist mode.
"
git push --tags
```

Tags become GitHub Releases automatically — clean portfolio history.

Claude must:
- Track the current latest tag and suggest the next version number
- Draft the annotation message (user-facing summary, not implementation detail)
- Include the tag command in the wrap-up handoff so I can paste + run

If I forget to tag a release, Claude will notice on the next session ("v1.4.0 was merged but never tagged, want to add it now?").

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
- **Amend a commit that's already been merged to `main`.** `git commit
  --amend` rewrites the SHA; once the original is in `main`, the
  amended branch creates a conflict on the same lines on the next
  push. For follow-up work: sync `main`, create a NEW branch, make a
  NEW commit.

## After every merge

The moment a PR lands on `main`, run **before any further work**:

```bash
git checkout main && git pull
```

This guarantees the next branch starts from the actual deployed state
and prevents the amend-after-merge trap above. If Claude is the one
doing the next change, it should run this itself the moment you say
"merged" / "PR ปิดแล้ว" / equivalent — and refuse to amend any commit
whose content is now part of `main`.
