# Get the native tool

The official download page is https://miyorgi.com/c4#download. Choose the archive matching the user's operating system and processor. Public native targets are macOS Apple Silicon, macOS Intel, and Windows x64. Published release metadata is at https://downloads.miyorgi.com/c4/latest.json and includes archive URLs and SHA-256 checksums. Availability is shown on the download page; do not invent release URLs or claim an unpublished release is available.

Extract the archive. The executable is `bin/miyorgi` on macOS or `bin/miyorgi.exe` on Windows. Use that path directly, or a `miyorgi` already on PATH. When downloading programmatically, use the official published release metadata and verify its checksum before executing the downloaded file.

## Starting from the binary

From the user's project folder, run:

```sh
miyorgi c4 setup
```

Setup lets the user choose Codex or Claude Code and installs the C4 skill bundled with that binary. It also records the executable path in `references/runtime.json`, so the assistant can use it without changing PATH.

For a non-interactive setup, use `miyorgi c4 setup --agent codex` or `miyorgi c4 setup --agent claude`. The project destinations are `.agents/skills/c4` and `.claude/skills/c4`, respectively. An existing different skill is preserved unless replacement is explicitly requested with `--force`.

## Starting from the public skill

A user who already installed the skill only needs the tool to check and preview. Use the downloaded executable's path directly; reinstalling the skill is optional. Prefer an official binary download over asking the user to compile the private implementation repository.

If no matching download is published yet, report that limitation and continue only the modelling work that does not need the tool. Never report a check or diagram preview as successful without running the executable.
