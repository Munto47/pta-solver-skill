# Publishing Your Skill to skills.sh Registry

This guide explains how to publish the PTA Solver skill (or any skill) to the
[skills.sh](https://skills.sh) registry so it becomes discoverable via
`/find-skills <name>` and `npx skills add <package>`.

---

## How skills.sh Discovery Works

skills.sh maintains a registry of skills scraped from GitHub repositories.
When a user runs `/find-skills pta-solver` in Claude Code, the CLI queries
the skills.sh registry, which indexes repositories that contain properly
formatted skill definitions.

The key mechanism:
1. Your GitHub repository contains a `SKILL.md` at the root (this is the **registry
   entry point**)
2. The skill definition files (e.g., `pta-solver.md`) reside in the same repo
3. skills.sh scans the repo, reads `SKILL.md`, and adds your skill to the index
4. Users discover and install via `npx skills add <owner/repo>` or the shorthand
   `npx skills add https://github.com/<owner>/<repo>`

---

## Prerequisites

Before submitting, ensure your repository meets these requirements:

- [ ] **Public GitHub repository** — skills.sh cannot scan private repos
- [ ] **`SKILL.md` at the repository root** — acts as the entry point
- [ ] **Valid frontmatter** in `SKILL.md` with `name` and `description` fields
- [ ] **Skill definition file(s)** referenced by or alongside `SKILL.md`
- [ ] **MIT or other open-source license** (recommended)
- [ ] **README.md** with installation and usage instructions

---

## Step 1: Verify Your SKILL.md

Your repository already has a `SKILL.md` at the root. Let's verify it's properly
formatted.

Open `SKILL.md` and confirm it has valid YAML frontmatter:

```markdown
---
name: pta-solver
description: |
  Automated PTA (pintia.cn) problem set solver. Navigates problem sets, reads
  problem descriptions, writes C++ solutions locally, pastes code via clipboard
  into CodeMirror 6 editors, and submits. Handles login, language switching,
  and result verification.
metadata:
  trigger:
    - "pintia.cn/problem-sets"
    - "PTA"
    - "拼题A"
    - "完成这道题"
    - "提交代码"
    - "算法设计与分析"
---
```

### Key Fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Unique skill identifier (kebab-case) |
| `description` | Yes | What the skill does (shown in search results) |
| `metadata.trigger` | No | Keywords that trigger auto-suggestion |

---

## Step 2: Ensure the Repository is Well-Structured

A discoverable skill repository should have this layout:

```
your-repo/
  SKILL.md               # ← Registry entry point (REQUIRED)
  pta-solver.md          # ← Main skill definition
  README.md              # ← User-facing documentation
  README.zh-CN.md        # ← Optional: translated docs
  docs/                  # ← Optional: detailed guides
    playwright-mcp-setup.md
    publish-to-skills-sh.md
  LICENSE                # ← Open-source license
```

The current `pta-solver-skill` repo already follows this structure.

---

## Step 3: Submit to skills.sh

There are two ways to get your skill listed:

### Method A: Submit via the Website (Easiest)

1. Go to [https://skills.sh](https://skills.sh)
2. Look for a **"Submit Skill"** or **"Add Skill"** button (top-right or in the
   navigation)
3. Enter your GitHub repository URL:
   ```
   https://github.com/Munto47/pta-solver-skill
   ```
4. The site will validate your repo's `SKILL.md` and list your skill
5. Once approved, your skill appears on the leaderboard and in `npx skills find`
   results

### Method B: Submit via CLI

Some versions of the `npx skills` CLI support direct publishing:

```bash
# Navigate to your skill repository
cd pta-solver-skill

# Publish (if supported by your CLI version)
npx skills publish
```

If the `publish` subcommand is not available, use Method A.

### Method C: Contact via GitHub

If the skills.sh website doesn't have a submission form:

1. Open an issue at [skills.sh's GitHub repository](https://github.com/vercel-labs/skills)
2. Request that your skill be added to the registry
3. Include your repo URL and a brief description

---

## Step 4: Verify Publication

After submission, verify your skill is discoverable:

```bash
# Search for your skill
npx skills find pta-solver

# Install directly from GitHub
npx skills add https://github.com/Munto47/pta-solver-skill -g -y

# Or install using the shorthand (once registered)
npx skills add Munto47/pta-solver-skill -g -y
```

Check the skills.sh leaderboard for your skill:
```
https://skills.sh
```

> **Note:** Indexing may take a few hours after submission. If your skill doesn't
> appear immediately, check back later.

---

## Step 5: Promote Your Skill

Once published, help users find it:

### Add Install Badges to README

```markdown
[![Install with skills.sh](https://img.shields.io/badge/install-skills.sh-blue)](https://skills.sh)
```

### Share the Install Command

```bash
npx skills add Munto47/pta-solver-skill -g
```

### Add Claude Code Trigger Hint

In your README, tell users to run in Claude Code:
```
/find-skills pta-solver
```

---

## Keeping Your Skill Updated

skills.sh periodically re-scans indexed repositories. To update your skill:

1. Push changes to your GitHub repository (update `SKILL.md` or skill files)
2. skills.sh will pick up changes on the next scan cycle
3. Users can update with: `npx skills update pta-solver`

There's no need to re-submit after the initial publication — the registry
automatically tracks your repo's `main` branch.

---

## Tips for Better Discoverability

1. **Descriptive `description` field** — Include keywords users would search for
   (e.g., "PTA", "pintia.cn", "algorithm", "competitive programming", "C++")

2. **Trigger keywords** — The `metadata.trigger` list helps Claude Code auto-suggest
   your skill when users mention related topics

3. **Rich README** — A well-documented skill gets more installs. Include:
   - Clear one-line description
   - Quick install command
   - What problem it solves
   - Screenshots or demos

4. **GitHub topics** — Add relevant topics to your GitHub repo:
   - `claude-code`
   - `skill`
   - `pta`
   - `competitive-programming`
   - `algorithm`

5. **Star your own repo** — It signals legitimacy and helps with initial visibility

---

## Troubleshooting

### "Skill not found" after submission

- skills.sh may take up to 24 hours to index new submissions
- Verify your repository is **public** (not private)
- Check that `SKILL.md` exists at the repository root (not in a subdirectory)
- Validate the YAML frontmatter syntax — a syntax error causes indexing to fail

### "Invalid SKILL.md format"

Run a YAML validator on your SKILL.md frontmatter:

```bash
npx yaml-validate SKILL.md
```

Or paste the frontmatter into an online YAML validator.

### Skill appears but `/find-skills` doesn't find it

Claude Code's skill search may have its own caching layer. Try:

```bash
npx skills update
```

Then restart Claude Code and try `/find-skills pta-solver` again.

### Multiple skills in one repo

If your repo contains multiple skill files (e.g., `skill-a.md`, `skill-b.md`),
the root `SKILL.md` should describe all of them. skills.sh will index them as
a skill **package**. Users install individual skills with:

```bash
npx skills add owner/repo --skill pta-solver
```

---

## References

- [skills.sh — The Agent Skills Directory](https://skills.sh)
- [Vercel Labs Skills Repository](https://github.com/vercel-labs/skills) —
  Reference implementation of skills.sh ecosystem
