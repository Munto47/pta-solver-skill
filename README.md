# PTA Solver Skill for Claude Code

Automated PTA (pintia.cn) problem set solver skill. Uses **Playwright MCP** for browser
automation to navigate problems, paste code into CodeMirror 6 editors, and submit solutions.

## Installation

### Prerequisites

1. **Claude Code** with MCP support
2. **Playwright MCP server** configured in your Claude Code settings

### Install the Skill

**Option 1: Clone to project**
```bash
# In your project directory
mkdir -p .claude/skills
cp pta-solver.md .claude/skills/
```

**Option 2: Global install**
```bash
mkdir -p ~/.claude/skills
cp pta-solver.md ~/.claude/skills/
```

**Option 3: Via Claude Code**
In Claude Code, run:
```
/find-skills pta-solver
```

## What This Skill Does

Automates solving algorithm programming problems on [PTA (pintia.cn)](https://pintia.cn/):

- 🔑 Auto-login to PTA
- 📋 Read problem descriptions
- ✍️ Write C++ solutions locally
- 📋 Paste code into CodeMirror 6 editors (clipboard + keyboard method)
- 🚀 Submit and verify results
- 🔄 Batch-solve entire problem sets (30+ problems)

## Key Tools Used

| Tool | Purpose |
|------|---------|
| `mcp__playwright__browser_navigate` | Page navigation |
| `mcp__playwright__browser_evaluate` | JS execution (extract text, clipboard) |
| `mcp__playwright__browser_run_code_unsafe` | Atomic multi-step submission |
| `mcp__playwright__browser_press_key` | Keyboard shortcuts |
| `Write` / `Read` / `Edit` | Local code file management |

## Supported Problem Types

- Brute force, divide & conquer, decrease & conquer
- Dynamic programming, greedy algorithms
- Graph algorithms (BFS, DFS, Dijkstra, Floyd, MST, bipartite matching)
- String algorithms (KMP)
- Math & probability problems
- Backtracking (N-Queens)
- XOR / bit manipulation

## Author

Created by [Munto47](https://github.com/Munto47)

## License

MIT
