# `.agents/skills`

This directory is meant to hold a **relative symlink** to the canonical skill:

```
.agents/skills/brewphase-life-path -> ../../skills/brewphase-life-path
```

The link is not committed in this checkout because Windows refuses to create
symbolic links without Developer Mode or elevation, and a directory junction
would be expanded into a duplicate copy by Git — which would break the rule that
`skills/brewphase-life-path/` is the single canonical source.

## Create it

macOS / Linux:

```bash
ln -s ../../skills/brewphase-life-path .agents/skills/brewphase-life-path
```

Windows (Developer Mode on, or an elevated shell):

```powershell
New-Item -ItemType SymbolicLink `
  -Path   .agents\skills\brewphase-life-path `
  -Target ..\..\skills\brewphase-life-path
```

Windows without Developer Mode — use the canonical path directly instead of a
link, or enable Developer Mode once and run the command above.

## Why a link and not a copy

An agent loader that reads `.agents/skills/` should see exactly the same files as
one that reads `skills/`. With a copy, the two drift the moment anyone edits one
of them, and there is no way to tell which is authoritative.

## Verify

```bash
ls -l .agents/skills/brewphase-life-path/SKILL.md
```

On Windows, `Read-Item` should report `LinkType: SymbolicLink`.
