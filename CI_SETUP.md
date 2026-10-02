# Cross-platform push checks

The public GitHub repository is [iZUMi-kyouka/asssgt3](https://github.com/iZUMi-kyouka/asssgt3). The Actions workflow configures and builds the CMake target on Linux, 64-bit Windows, Apple Silicon macOS, and Intel macOS. It verifies compilation and linking; it does not launch the interactive GLUT window.

## Enable the pre-push hook

Install GitHub CLI, authenticate with `gh auth login`, and run this once in the repository:

```bash
bash scripts/install-pre-push-hook.sh
```

Each `git push` submits the exact outgoing commit to a temporary `ci-validation/*` branch on the same GitHub remote. The hook waits for all four Actions jobs. A failed or unavailable check blocks the original push. Temporary branches are removed after the check; the Actions logs remain on GitHub. Uncommitted changes are not included.

The hook needs Git push credentials for the remote and GitHub CLI access to read workflow runs. If `gh` cannot infer the repository, set `GH_REPO=owner/repository` before pushing.

## `main` protection

The repository already has an active ruleset named **Cross-platform checks on main**, targeting `refs/heads/main`. It requires these checks:

- `Build (Linux)`
- `Build (Windows x64)`
- `Build (macOS ARM64)`
- `Build (macOS Intel)`

The ruleset allows direct pushes and does not require pull requests. The hook submits the same commit SHA for hosted checks before the original push; the ruleset also prevents a bypassed local hook from updating `main` without passing checks. It has no bypass actors.

Git hooks are local to each clone. Run the installer again after cloning on another machine.
