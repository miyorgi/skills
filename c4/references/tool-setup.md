# Get the native tool

The current installation route is Homebrew on macOS (Apple silicon or Intel):

```sh
brew install --cask miyorgi/tap/c4
```

Resolve `miyorgi` on PATH and run `miyorgi --version`. The official getting-started page is https://miyorgi.com/c4#download. Do not offer Windows or Docker installation while those releases are unavailable. Do not ask users to compile the private implementation repository.

## Starting from the binary

From the user's project folder, run:

```sh
miyorgi c4 setup
```

Setup lets the user choose Codex or Claude Code and installs the C4 skill bundled with that binary. It also records the executable path in `references/runtime.json`, so the assistant can use it without changing PATH.

For a non-interactive setup, use `miyorgi c4 setup --agent codex` or `miyorgi c4 setup --agent claude`. The project destinations are `.agents/skills/c4` and `.claude/skills/c4`, respectively. An existing different skill is preserved unless replacement is explicitly requested with `--force`.

## Starting from the public skill

A user who already installed the skill only needs the tool to check and preview. Install the tool with Homebrew, then use `miyorgi` on PATH; reinstalling the skill is optional. Do not replace an existing project skill merely to locate the executable.

If installation is unavailable in the user’s environment, report that limitation and continue only the modelling work that does not need the tool. Never report a check or diagram preview as successful without running the executable.
