# `.codex/skills`

This directory is meant to hold a **relative symlink** to the canonical skill:

```
.codex/skills/brewphase-life-path -> ../../skills/brewphase-life-path
```

Not committed in this checkout — see
[`.agents/skills/README.md`](../../.agents/skills/README.md) for the reason and
the exact command for your platform. They are identical; the link is created in
both places because different loaders read different directories.

## Create it

macOS / Linux:

```bash
ln -s ../../skills/brewphase-life-path .codex/skills/brewphase-life-path
```

Windows (Developer Mode on, or an elevated shell):

```powershell
New-Item -ItemType SymbolicLink `
  -Path   .codex\skills\brewphase-life-path `
  -Target ..\..\skills\brewphase-life-path
```

## Note

`.codex` here follows the repository-level skills convention. If your Codex
version reads a user-level directory instead (`~/.codex/skills` or similar),
point that at `skills/brewphase-life-path` — the canonical path is the same file
either way. Do not copy; link.
