# Publishing Your Skill to skills.sh

This guide explains how the [skills.sh](https://skills.sh) ecosystem works and
how to get your skill listed on the leaderboard.

---

## How skills.sh Works

Based on the official [skills.sh FAQ](https://skills.sh/docs/faq):

> **Skills appear on the leaderboard automatically through anonymous telemetry
> when users run `npx skills add <owner/repo>`. Once your skill is installed by
> users, it will be tracked and appear in the rankings based on its installation
> count.**

There is **no manual submission or approval process**. The leaderboard is
entirely driven by install data.

The flow:
1. You host a skill in a public GitHub repository with proper structure
2. Users install it via `npx skills add <owner/repo>`
3. Anonymous telemetry records the install
4. Your skill appears on the [skills.sh leaderboard](https://skills.sh)
   ranked by total installs

> **Privacy note:** The telemetry is completely anonymous — only aggregate
> installation counts are collected. No personal information, usage patterns,
> or identifying data is stored.

---

## Getting Your First Installs

Since the leaderboard is telemetry-driven, the critical step is getting users
to install your skill. Here's how to seed adoption:

### 1. Self-Install to Bootstrap Telemetry

```bash
npx skills add https://github.com/Munto47/pta-solver-skill -g -y
```

This registers the first install in the telemetry system.

### 2. Share the Install Command

The primary call-to-action for your skill:

```bash
npx skills add https://github.com/Munto47/pta-solver-skill -g -y
```

Once enough installs are recorded, the shorthand format may also work:

```bash
npx skills add Munto47/pta-solver-skill -g
```

### 3. Promote Through These Channels

| Channel | Action |
|---------|--------|
| **GitHub README** | Add install badge and command (already done) |
| **GitHub topics** | Add `claude-code`, `skill`, `pta`, `algorithm` tags |
| **PTA community** | Share in PTA/pintia.cn user forums, QQ groups |
| **Claude Code community** | Post in Claude Code Discord/Slack channels |
| **Social media** | Share on Twitter/X, Reddit (r/ClaudeCode, r/algorithms) |
| **Chinese platforms** | 知乎, CSDN, 掘金 articles about the skill |
| **Word of mouth** | Tell classmates/colleagues who use PTA |

### 4. Optimize for `npx skills find`

The `npx skills find` command searches skill names and descriptions. To maximize
discoverability, your `SKILL.md` description should include high-value keywords:

```yaml
description: |
  Automated PTA (pintia.cn) problem set solver for Claude Code. Navigates
  problem sets, reads algorithm problems, writes C++ solutions, pastes code
  into CodeMirror 6 editors, and submits. Handles login, language switching,
  and result verification. Supports brute force, dynamic programming, greedy,
  graph algorithms, and more.
```

Keywords covered: PTA, pintia.cn, problem set, solver, Claude Code, C++,
CodeMirror, algorithm, brute force, dynamic programming, greedy, graph.

---

## Repository Checklist

Before promoting, verify your repo meets these standards:

- [ ] **Public GitHub repository** — private repos don't count
- [ ] **`SKILL.md` at repo root** — with valid YAML frontmatter
- [ ] **Skill definition file** — e.g., `pta-solver.md` with complete workflow
- [ ] **README.md** — clear install instructions, feature list, troubleshooting
- [ ] **Open-source license** — MIT recommended
- [ ] **GitHub topics** — `claude-code`, `skill`, `pta`, `competitive-programming`
- [ ] **GitHub description** — concise, keyword-rich (shown in search results)

---

## Monitoring Your Growth

After seeding installs, track your skill's performance:

1. **Leaderboard rank** — Visit https://skills.sh and search for your skill
2. **GitHub stars** — Social proof that drives more installs
3. **GitHub traffic** — Repo Insights → Traffic shows visitor counts
4. **Issue activity** — Community engagement signals health

The skills.sh leaderboard ranks by **all-time installs**, so early promotion
is key to building momentum.

---

## FAQ

### How long until my skill appears on the leaderboard?
Skills appear as soon as the first install telemetry is recorded. There may
be a short delay (minutes to hours) for the leaderboard to refresh.

### Do I need to re-submit after updating my skill?
No. skills.sh tracks installs, not code. Updates to your repo are picked up
when users run `npx skills update`.

### Can I have multiple skills in one repo?
Yes. Use `SKILL.md` as the package entry point describing all skills. Users
install individual skills with `--skill <name>`.

### What if my skill doesn't appear?
Run a self-install to bootstrap telemetry:
```bash
npx skills add https://github.com/Munto47/pta-solver-skill -g -y
```

### Is there any cost?
No. skills.sh is free. The ecosystem is built by Vercel and open-source.

---

## References

- [skills.sh — The Agent Skills Directory](https://skills.sh)
- [skills.sh FAQ](https://skills.sh/docs/faq)
- [Vercel Labs Skills Repository](https://github.com/vercel-labs/skills)
