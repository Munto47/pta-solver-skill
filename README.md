# PTA Solver Skill for Claude Code

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://claude.ai/code)

Automated PTA (pintia.cn) problem set solver skill for Claude Code. Uses **Playwright MCP**
browser automation to navigate problem sets, paste solutions into CodeMirror 6 editors,
and submit — all through natural language conversation.

## Quick Install

In Claude Code, just run:

```
/find-skills pta-solver
```

That's it. Claude Code will discover and install the skill automatically.

## Alternative Install Methods

**Method 1: Git clone (recommended for contributors)**

```bash
git clone https://github.com/Munto47/pta-solver-skill.git
# The skill file pta-solver.md goes into your project's .claude/skills/ directory
# Copy it to any Claude Code project you want to use it in:
cp pta-solver.md /path/to/your-project/.claude/skills/
```

**Method 2: Direct download**

Download `pta-solver.md` from the [releases page](https://github.com/Munto47/pta-solver-skill/releases)
and place it in your project's `.claude/skills/` directory.

**Method 3: Manual placement**

```bash
# Per-project install
mkdir -p .claude/skills
cp pta-solver.md .claude/skills/

# Or global install (available in all projects)
mkdir -p ~/.claude/skills
cp pta-solver.md ~/.claude/skills/
```

## Requirements

- **Claude Code** (any recent version)
- **Playwright MCP** server configured in your Claude Code settings

If you haven't set up Playwright MCP yet, see the
[Playwright MCP setup guide](https://github.com/anthropics/claude-code/tree/main/mcp).

## What This Skill Does

Automates solving algorithm programming problems on [PTA (pintia.cn)](https://pintia.cn/):

- Auto-login to PTA
- Read problem descriptions
- Write C++ solutions locally
- Paste code into CodeMirror 6 editors (clipboard + keyboard method)
- Submit and verify results
- Batch-solve entire problem sets (30+ problems)

## Supported Problem Types

Brute force, divide & conquer, dynamic programming, greedy, graph algorithms
(BFS, DFS, Dijkstra, Floyd, MST, bipartite matching), string algorithms (KMP),
math & probability, backtracking, bit manipulation, and more.

## Contributing

Community contributions are welcome! Here's how you can help:

### Report Bugs

Found an issue? Open a [GitHub Issue](https://github.com/Munto47/pta-solver-skill/issues)
with:
- The PTA problem set URL you were working on
- What went wrong (error messages, screenshots)
- Claude Code version and OS

### Submit Improvements

1. Fork the repo
2. Create a feature branch (`git checkout -b feature/your-improvement`)
3. Make your changes
4. Test the skill on a real PTA problem set
5. Submit a Pull Request

### What We Need Help With

- New problem-type templates
- Better CodeMirror 6 interaction patterns
- Language support beyond C++ (Java, Python)
- Login flow improvements for different auth methods
- Documentation and translations

## Troubleshooting

### Skill not found via `/find-skills`
The skill registry may need time to index. As a workaround, use the manual install methods above.

### "Playwright MCP not configured"
This skill requires the Playwright MCP server. Install it via:
```bash
npm install -g @anthropic/mcp-playwright
```
Then add it to your Claude Code MCP configuration.

### Code doesn't paste into the editor
PTA uses CodeMirror 6 which blocks programmatic text insertion. The skill uses
a clipboard + keyboard shortcut approach that handles this reliably. If it still
fails, ensure the browser window has focus during execution.

### Wrong answer on submissions
- Verify the problem constraints — many PTA problems have subtle edge cases
- Check integer overflow (`long long` vs `int`)
- Ensure output format matches exactly (trailing newlines, spaces)

## Project Structure

```
pta-solver-skill/
  pta-solver.md          # Main skill definition
  SKILL.md               # Skill registry entry point
  README.md              # This file
  LICENSE                # MIT
  .claude/skills/        # Bundled install target
```

## Author

Created and maintained by [Munto47](https://github.com/Munto47).

## License

MIT — use it, modify it, share it. See [LICENSE](LICENSE).
