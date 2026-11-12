# WorkBuddy adapter

How to install this skill into WorkBuddy.

**Read the verification section before repeating any claim from this file.**
WorkBuddy's skill and MCP handling changes between versions, and an unverified
install instruction wastes more time than no instruction at all.

## Do not assume the Vercel Skills CLI works here

`npx skills add banmu123/brewphase-life-path` targets repositories that follow
the `skills/<name>/SKILL.md` convention and an installer that knows where to put
them. **It has not been verified that this CLI installs into WorkBuddy.** Do not
state or imply that it does until someone has actually run it and confirmed
where the files landed.

## Installation

### Option A — drop the skill into the skills directory

WorkBuddy discovers skills from a user-level directory (and a project-level one):

| Scope | Path |
|---|---|
| User (all projects) | `~/.workbuddy/skills/` |
| Project only | `<workspace>/.workbuddy/skills/` |

Copy or link the canonical skill directory there:

```bash
# macOS / Linux
ln -s "$(pwd)/skills/brewphase-life-path" ~/.workbuddy/skills/brewphase-life-path
```

```powershell
# Windows, Developer Mode on
New-Item -ItemType SymbolicLink `
  -Path   "$env:USERPROFILE\.workbuddy\skills\brewphase-life-path" `
  -Target "$PWD\skills\brewphase-life-path"
```

A copy also works, but then it is a fork — when the upstream changes, yours does
not. Prefer the link.

### Option B — the app's own import or marketplace flow

Some WorkBuddy builds expose a skill import or marketplace entry point. If yours
does, use it and point at this repository. The behaviour differs by build, so it
is not documented here as a guarantee.

## Verify it is loaded

Ask WorkBuddy something that should match on intent, without naming the skill:

> 我在现在这份工作里待了六年，说不上差，但也不觉得自己在往哪里走。

If the skill is active, the answer should follow this repository's method:
coverage across regions, conditions and costs for each example, no similarity
percentages, no predicted dates. If you get generic advice, the skill is not
being selected — check the directory and restart the app.

## Connect BrewPhase MCP (optional)

The skill works standalone. To let it read your own confirmed direction and work
context:

1. In **BrewPhase Desktop** → 设置 → AI 连接, create a client and grant
   `direction:read` (add `work_context:read` for work contexts). Copy the token
   once — it is not shown again.
2. In WorkBuddy's MCP configuration, add a stdio server:

```json
{
  "mcpServers": {
    "brewphase": {
      "command": "node",
      "args": ["<path-to-BrewPhase>/Desktop/out/mcp/stdio.mjs"],
      "env": {
        "BREWPHASE_MCP_TOKEN": "<token from step 1>",
        "BREWPHASE_MCP_USER_DATA_DIR": "<your BrewPhase user data directory>"
      }
    }
  }
}
```

3. Trust / enable the new server in WorkBuddy's connector settings — a newly
   written MCP entry does not activate by itself.

**Never commit the token, the user-data path, or your `mcp.json` to a
repository.** They are local credentials.

Only grant `direction:propose` if you want the agent to be able to write drafts
for you to confirm in the app. It cannot confirm them — by design, the confirm
action does not exist on the MCP side.

## Verification status

**Observed to be true** in a current WorkBuddy desktop build:

- skills are discovered from a `skills/` directory at user and project level
- MCP servers are configured through a JSON file and require explicit trust
  before they run
- the `SKILL.md` frontmatter shape (`name` + `description`) is the right shape

**Not verified:**

- whether `npx skills add` installs into WorkBuddy
- whether every build uses the same directory names and the same MCP config
  location
- whether relative symlinks inside a skill directory are followed on Windows
- whether a marketplace/import UI exists in your build

If you verify any of these, correct this file and say which build you tested.
