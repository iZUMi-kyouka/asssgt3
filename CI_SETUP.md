# Cross-platform push checks

The GitHub Actions workflow configures and builds the CMake target on Linux, 64-bit Windows, Apple Silicon macOS, and Intel macOS. It verifies compilation and linking; it does not launch the interactive GLUT window.

## Enable the pre-push hook

Install GitHub CLI, authenticate with `gh auth login`, and run this once in the repository:

```bash
bash scripts/install-pre-push-hook.sh
```

Each `git push` submits the exact outgoing commit to a temporary `ci-validation/*` branch on the same GitHub remote. The hook waits for all four Actions jobs. A failed or unavailable check blocks the original push. Temporary branches are removed after the check; the Actions logs remain on GitHub. Uncommitted changes are not included.

The hook needs Git push credentials for the remote and GitHub CLI access to read workflow runs. If `gh` cannot infer the repository, set `GH_REPO=owner/repository` before pushing.

## Protect `main` without requiring pull requests

Create an active branch ruleset in **Settings → Rules → Rulesets** targeting `refs/heads/main`. Require these exact checks:

- `Build (Linux)`
- `Build (Windows x64)`
- `Build (macOS ARM64)`
- `Build (macOS Intel)`

Leave the pull-request requirement disabled. The hook submits the same commit SHA for hosted checks before the original push; the ruleset also prevents a bypassed local hook from updating `main` without passing checks. Do not configure bypass actors if all pushers must satisfy the checks.

Git hooks are local to each clone. Run the installer again after cloning on another machine.
