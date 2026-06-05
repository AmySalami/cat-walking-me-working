# Pomodoro Meadow — Claude Context

> Cozy single-file Pomodoro timer with a walking cat, scrolling meadow, and ambient music (SoundCloud + YouTube). One HTML file, no build step.

## Project

- **Repo:** `AmySalami/cat-walking-me-working`
- **Live:** https://amysalami.github.io/cat-walking-me-working/
- **Stack:** Single `index.html` — HTML5, vanilla CSS/JS, no build, no deps beyond Google Fonts + SoundCloud Widget API + YouTube IFrame Player API
- **Owner:** solo project (no team)

## Git Workflow — Always Follow

This project uses a **feature-branch + PR workflow** even though it's solo, because:
1. Portfolio context — recruiters look at branch/PR history
2. Safety — `main` is live (GitHub Pages auto-deploys)
3. Practice — same workflow used in real teams

### When to branch

| Change | Branch? |
|---|:---:|
| Typo, 1-line CSS tweak, color change | ❌ push `main` OK |
| Bug fix that needs verification | ✅ |
| New feature (2+ commits) | ✅ |
| Refactor | ✅ |
| Documentation changes | ✅ |

### Reminders Claude must give

**Split of responsibility:**
- **Claude does (locally):** branch, code, commits.
- **User does (on GitHub UI):** push, open PR, merge, optional tag.

This means **don't suggest `gh pr ...` commands** in wrap-up messages — the user prefers the GitHub web UI for review/merge. Hand off cleanly with the branch ready and a PR description drafted; the user takes it from there.

**Before starting any non-trivial work:**

```
🌱 Branch setup needed:
  Type:    [feature | fix | refactor | docs | chore]
  Name:    <type>/<short-kebab-description>

  Run:     git checkout -b <branch-name>
```

**After work is complete — what Claude hands to the user:**

```
🚀 Ready for PR.
  Branch:  <branch-name>
  Commits: <count>, +<adds> / -<dels>
  Bump:    MINOR | PATCH | MAJOR | none  (see Versioning section)
  Next:    vX.Y.Z  (current latest tag: vA.B.C)

  Next (user does these on GitHub UI):
    1. Push:      git push -u origin <branch-name>
    2. Open PR    on https://github.com/<owner>/<repo>/pulls
                  (Compare & pull request banner appears after push)
    3. Paste this PR description:
       <ready-to-paste markdown body>
    4. Merge      via "Squash and merge" or "Merge pull request"
    5. Tag        (REQUIRED if Bump is MINOR/PATCH/MAJOR — skip only if "none"):
       git checkout main && git pull
       git tag -a vX.Y.Z -m "..."
       git push --tags
```

Claude does NOT run push / PR / merge / tag-push commands. The branch + draft PR description + suggested tag command are the deliverable.

### Branch naming

- `feature/<name>` — new functionality
- `fix/<name>` — bug fix
- `refactor/<name>` — code restructure (no behavior change)
- `docs/<name>` — documentation only
- `chore/<name>` — config, deps, tooling

Use short kebab-case descriptions. Examples:
- `feature/keyboard-shortcuts`
- `fix/embed-blocked-tracks`
- `refactor/single-source-of-truth`
- `docs/establish-workflow`

### Commit message style (Conventional Commits)

```
<type>(<scope>): <short summary>

<optional longer body explaining what/why>

<optional footer>
```

Types: `feat`, `fix`, `refactor`, `style`, `docs`, `chore`, `perf`

Include scope when useful: `feat(music): ...`, `fix(onboarding): ...`

### Versioning (SemVer) — REQUIRED after every release-worthy merge

Every merged PR that contains a `feat:` or `fix:` commit MUST be tagged with a new
SemVer version. This is non-negotiable — `main` is what GitHub Pages deploys, so
every merge ships to users, and every shipped change deserves a version record.

**Bump rules:**

| Commit type in PR | Bump | Example |
|---|---|---|
| `feat:` (new functionality) | **MINOR** — `v1.3.0 → v1.4.0` | new character, new music source, new UI affordance |
| `fix:` (user-visible bug fix) | **PATCH** — `v1.4.0 → v1.4.1` | broken playback, layout bug, wrong color |
| `feat:` with breaking change | **MAJOR** — `v1.4.0 → v2.0.0` | rename of settings key, removal of feature, URL format change |
| Mixed `feat:` + `fix:` in one PR | Take the highest bump (MINOR wins over PATCH) | |
| `refactor:` / `style:` / `perf:` (no behavior change) | No version bump required | tag only if the user wants a checkpoint |
| `docs:` / `chore:` only | **No tag** | these don't ship behavior |

**Workflow Claude must follow:**

After commits land on a feature branch, Claude's wrap-up message must include the
next version number + a `git tag` command line, ready for the user to run after
they merge the PR:

```
🏷  Suggested next tag (run after merging the PR):
  Bump:    MINOR (feat:)
  Version: vX.Y.Z  (current: vA.B.C)

  git tag -a vX.Y.Z <merge-commit-sha> -m "..."
  git push --tags
```

Claude is responsible for:
1. Reading the current tag with `git tag -l --sort=-v:refname | head -1`.
2. Picking the correct bump based on commit types in the PR.
3. Drafting an annotation message that summarises the user-facing change
   (not the implementation detail). Same tone as a GitHub release note.

If a release-worthy change ships without a tag, that's a process failure — flag it
on the next session: "noticed v1.4.0 → v1.5.0 was merged but never tagged, tag it now?"

**Tag annotation style** (always annotated, never lightweight):

```bash
git tag -a v1.5.0 <sha> -m "<one-line summary>

<2-4 bullet highlights of what's new, written for the end user>

<optional: notes on architecture changes if relevant>"
```

Current state: check `git tag -l --sort=-v:refname` and `git log --oneline --decorate -5` before suggesting next steps.

## File structure

```
.
├── index.html        # The entire app
├── README.md         # Public-facing project intro
├── CLAUDE.md         # This file — AI assistant context
├── WORKFLOW.md       # Human-readable workflow reference
├── LICENSE           # MIT
└── .gitignore
```

## Local development

```bash
python3 -m http.server 8765
# open http://localhost:8765/
```

**Critical:** Never test by opening `file://` — SoundCloud + YouTube iframes require HTTP origin or they fail with cryptic embed errors.

## What NOT to do

- ❌ Push experimental code straight to `main` without a branch
- ❌ Skip the PR step "because it's solo" — defeats the purpose
- ❌ Tag a release without testing on a branch first
- ❌ Use `--no-verify` to skip hooks unless explicitly asked
- ❌ Force-push to `main` without explicit user request
- ❌ Commit secrets / API keys (none should exist in this project)
- ❌ Open `index.html` via `file://` to verify — always use the local server
