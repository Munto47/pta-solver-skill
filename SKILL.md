---
name: pta-solver
description: |
  Automated PTA (pintia.cn) problem set solver for Claude Code. Navigates problem
  sets, reads algorithm and data structure problem descriptions, writes C++ solutions
  locally, pastes code via clipboard into CodeMirror 6 editors, and submits answers.
  Handles login, language switching (C++ g++), and result verification. Supports
  brute force, divide & conquer, dynamic programming, greedy, graph (BFS DFS Dijkstra),
  string (KMP), math, backtracking, and bit manipulation algorithms. Triggered by
  user providing a PTA problem set URL.
metadata:
  trigger:
    - "pintia.cn/problem-sets"
    - "PTA"
    - "拼题A"
    - "完成这道题"
    - "提交代码"
    - "算法设计与分析"
    - "algorithm problem"
    - "competitive programming"
    - "自动刷题"
---

# PTA Problem Set Solver

Solves programming problems on PTA (pintia.cn) using Playwright MCP browser
automation for web interaction, plus local file tools for code authoring.

---

## Tool Inventory

### Core Browser Tools (Playwright MCP)

| Tool | Purpose |
|------|---------|
| `mcp__playwright__browser_navigate` | Open any PTA page (login, problem set, problem detail, submissions) |
| `mcp__playwright__browser_snapshot` | Capture the accessibility tree of the current page to locate elements |
| `mcp__playwright__browser_evaluate` | Execute arbitrary JS in the page: extract text, write clipboard, find elements |
| `mcp__playwright__browser_click` | Click buttons/links by ref or role |
| `mcp__playwright__browser_press_key` | Keyboard shortcuts: `Control+a`, `Backspace`, `Control+v`, `ArrowDown`, `Enter` |
| `mcp__playwright__browser_run_code_unsafe` | Run Playwright API code (Page object) for multi-step interactions atomically |
| `mcp__playwright__browser_type` | Type text into an element (fallback, rarely works for CodeMirror) |
| `mcp__playwright__browser_wait_for` | Wait N seconds for page load or evaluation result |
| `mcp__playwright__browser_handle_dialog` | Accept/dismiss `beforeunload` dialogs |
| `mcp__playwright__browser_fill_form` | Fill login form fields |

### Local Code Tools

| Tool | Purpose |
|------|---------|
| `Write` | Create new `.cpp` solution file |
| `Read` | Read back `.cpp` file content for pasting |
| `Edit` | Fix bugs in existing solution files |
| `Grep` | Search existing solutions for patterns |

### Reasoning / Planning Tools

| Tool | Purpose |
|------|---------|
| `mcp__sequential-thinking__sequentialthinking` | Break down complex algorithm problems step by step |
| `EnterPlanMode` / `ExitPlanMode` | Design implementation approach before coding complex problems |

---

## Complete Workflow

### Phase 0: Initial Setup — Login

1. **Navigate to problem set URL** — PTA will redirect to `/auth/login` if not authenticated.
   ```
   browser_navigate("https://pintia.cn/problem-sets/{PROBLEM_SET_ID}/exam/problems/type/7")
   ```
2. **Fill login form** using `browser_fill_form`:
   - Email textbox → user's email
   - Password textbox → user's password
3. **Click login button** → redirected to home page.
4. **Re-navigate** to the problem set URL. Now logged in.

### Phase 1: Reconnaissance — Understand What Needs Doing

1. **Get problem list snapshot** to see the sidebar progress (e.g., "编程题 1/30").
2. **Go to submission list** (`/exam/submissions`) to identify:
   - Which problems already have submissions
   - Status of each submission (AC / WA / CE / partial)
   - Only re-submit problems that are NOT "答案正确"

### Phase 2: Read & Solve a Single Problem

#### 2a. Navigate to the Problem

Use the URL pattern:
```
https://pintia.cn/problem-sets/{PROBLEM_SET_ID}/exam/problems/type/7?problemSetProblemId={PROBLEM_ID}
```
Problem IDs are visible in the sidebar links on the problem list page.

#### 2b. Extract Problem Description

**Method 1 — browser_evaluate (fastest):**
```js
() => {
    const main = document.querySelector('main');
    return main ? main.innerText.substring(0, 3000) : 'not found';
}
```
This extracts the full problem text including title, description, input/output format,
constraints, and sample I/O.

**Method 2 — browser_snapshot (for precise element targeting):**
Use when you need to locate specific buttons (language selector, submit button).

#### 2c. Write the Solution Locally

1. **Analyze** the problem: constraints, time limits, memory limits.
2. **Design** the algorithm. Use `sequentialthinking` for complex problems.
3. **Write** the `.cpp` file locally with the `Write` tool.

File naming convention: `solution_{keyword}.cpp` (e.g., `solution_24point.cpp`).

**Critical C++ guidelines for PTA:**
- Use `<cstdio>` and `scanf`/`printf` exclusively — avoid `<iostream>` and `cin`/`cout`
- Never mix `scanf` with `cin`/`cout` (unsynchronized buffers cause WA)
- Avoid `using namespace std;` when unnecessary
- Use `std::sort` with `<algorithm>`, `std::fabs` with `<cmath>`
- For large integers, PTA's GCC supports `__int128`
- Global arrays are zero-initialized; use `memset` for local arrays
- `puts(str)` auto-appends `\n`; `printf` requires explicit `\n`
- PTA uses C++17/g++; C++20 features like `cin >> (char*)` are removed

#### 2d. Paste Code into PTA Editor

PTA uses **CodeMirror 6** editors. Direct `.fill()` or `.type()` does NOT work.
The clipboard-based method is the only reliable approach:

**Step 1: Ensure C++ (g++) is selected.**

The language selector defaults to "C (gcc)" when navigating to a new problem.
Switch it by:

```
1. Click the language combobox (browser_click on the combobox ref)
2. Press ArrowDown to open the dropdown
3. Click "C++ (g++)" in the dropdown list
```

**Step 2: Paste code using clipboard + keyboard.**

Use `browser_run_code_unsafe` for an atomic multi-step operation:

```js
async (page) => {
    const code = `#include <cstdio>
// ...your solution code...`;

    // 1. Write code to clipboard
    await page.evaluate((text) => navigator.clipboard.writeText(text), code);

    // 2. Locate the code editor (always the LAST .cm-editor)
    const editors = page.locator('.cm-editor');
    const codeEditor = editors.nth((await editors.count()) - 1);

    // 3. Click to focus
    await codeEditor.click();
    await page.waitForTimeout(150);

    // 4. Select all → Delete → Paste
    await page.keyboard.press('Control+a');
    await page.waitForTimeout(50);
    await page.keyboard.press('Backspace');
    await page.waitForTimeout(50);
    await page.keyboard.press('Control+v');
    await page.waitForTimeout(150);

    // 5. Trigger CodeMirror change detection
    //    (The submit button stays disabled until the editor fires a change event)
    await page.keyboard.press('End');
    await page.keyboard.press('Space');
    await page.waitForTimeout(50);
    await page.keyboard.press('Backspace');
    await page.waitForTimeout(100);

    // 6. Click submit
    const btn = page.getByRole('button', { name: '提交本题作答' });
    if (!(await btn.isDisabled())) {
        await btn.click();
        await page.waitForTimeout(2500);
    }
}
```

**Why the "Space + Backspace" trick is needed:**

CodeMirror 6 does not fire a change event when content is pasted via clipboard
API + Ctrl+V. The submit button's `disabled` state is bound to the editor's
change event. Typing a character (Space) then deleting it (Backspace) forces
a change event, enabling the submit button.

**Why the last `.cm-editor` is the code editor:**

PTA pages have a variable number of CodeMirror editors:
- Sample input textboxes (`.cm-editor` instances)
- Sample output textboxes (`.cm-editor` instances)
- The main code editor (always the **last** `.cm-editor`)

The count varies by problem (some have 3 editors, some have 5), so always
pick `editors.nth(count - 1)`.

#### 2e. Verify the Result

After clicking submit, wait ~2.5 seconds, then evaluate:

```js
() => {
    const txt = document.body.innerText;
    if (txt.includes('答案正确')) return 'AC';
    if (txt.includes('答案错误')) return 'WA';
    if (txt.includes('编译错误')) return 'CE';
    if (txt.includes('排队')) return 'QUEUED';
    return 'UNKNOWN';
}
```

If QUEUED, close the result dialog (click "确认" button) and move to the next
problem. The sidebar counter updates immediately on submission, even if
evaluation is pending.

### Phase 3: Iterate Through Remaining Problems

1. Navigate to the next problem by URL (incrementing `problemSetProblemId`).
2. Repeat Phase 2 for each problem.
3. Skip problems that already show AC in the submission list.

**Pro tip:** When a submission is queued (`排队中`), immediately close the
result dialog and move to the next problem. Don't waste time waiting.

---

## Code Editor Interaction Reference

| Action | How |
|--------|-----|
| Focus editor | `codeEditor.click()` (last `.cm-editor`) |
| Select all | `page.keyboard.press('Control+a')` |
| Delete | `page.keyboard.press('Backspace')` |
| Paste from clipboard | `page.keyboard.press('Control+v')` |
| Trigger change event | Type Space then Backspace at end of document |
| Submit | `page.getByRole('button', { name: '提交本题作答' }).click()` |
| Switch to C++ | Click combobox → ArrowDown → Click "C++ (g++)" |

---

## Common Problems & Solutions

### 1. `printf` format wrong
- `puts("YES")` auto-adds newline — preferred for single strings
- `printf("%d\n", x)` — always include explicit `\n`
- In JS template literals for clipboard: use `\\n` for literal `\n` in C code

### 2. Compile Error
- Check for C++20-incompatible syntax (`cin >> (char*)` removed)
- Check for mixed `scanf`/`cin` usage
- Check for missing `#include` directives
- View compiler output in the result dialog

### 3. Wrong Answer — Debug Strategy
- Read the problem statement again for subtle constraints
- Check integer overflow (use `long long` or `__int128`)
- Check output format (trailing spaces, newlines, exact string match)
- For "unofficial 24-point game": numbers stay in order, standard precedence,
  no parentheses (the example `(9*3)-(6/2)` just illustrates precedence)
- For XOR problems: check if `__int128` is needed for large results

### 4. Submit Button Disabled
- Ensure code was actually pasted (check `.cm-content` textContent)
- Ensure language is C++ (g++) not C (gcc)
- Trigger change event (Space + Backspace)
- If still disabled: re-click the editor, re-paste, re-trigger

### 5. Dialog / Modal Blocks Interaction
- `browser_handle_dialog` for `beforeunload` prompts
- Click "确认" button to close result dialogs
- If overlay blocks click: use `page.evaluate(() => btn.click())` to bypass

---

## Example: Solving a Problem End-to-End

```
1. browser_navigate → problem page
2. browser_evaluate → extract problem text
3. Analyze: "This is a Josephus problem, k=3, n≤100"
4. Write → solution_kill.cpp with recurrence formula
5. browser_evaluate → navigator.clipboard.writeText(code)
6. browser_run_code_unsafe → click editor, Ctrl+A, Backspace, Ctrl+V,
   Space, Backspace, click submit
7. browser_evaluate → check result: "答案正确" → AC!
8. browser_navigate → next problem
```

Repeat 28 more times.
