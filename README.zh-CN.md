# PTA 自动答题技能 · Claude Code 插件

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://claude.ai/code)
[![English Docs](https://img.shields.io/badge/Docs-English-blue)](README.md)

> [Switch to English →](README.md)

PTA（pintia.cn）算法题自动求解技能。基于 **Playwright MCP** 浏览器自动化，实现
浏览题目、将代码粘贴到 CodeMirror 6 编辑器、提交答案 —— 全程通过自然语言对话完成。

## 快速安装

在 Claude Code 中直接输入：

```
/find-skills pta-solver
```

即可自动发现并安装。也可以通过 `npx skills` 命令行安装：

```bash
npx skills add https://github.com/Munto47/pta-solver-skill -g -y
```

## 其他安装方式

**方式一：Git 克隆（推荐贡献者使用）**

```bash
git clone https://github.com/Munto47/pta-solver-skill.git
# 将技能文件 pta-solver.md 复制到你的 Claude Code 项目 .claude/skills/ 目录下
cp pta-solver.md /path/to/your-project/.claude/skills/
```

**方式二：直接下载**

从 [Releases 页面](https://github.com/Munto47/pta-solver-skill/releases) 下载
`pta-solver.md`，放入项目的 `.claude/skills/` 目录。

**方式三：手动放置**

```bash
# 按项目安装
mkdir -p .claude/skills
cp pta-solver.md .claude/skills/

# 或全局安装（所有项目可用）
mkdir -p ~/.claude/skills
cp pta-solver.md ~/.claude/skills/
```

## 环境要求

- **Claude Code**（任意较新版本）
- **Playwright MCP** 服务器已在 Claude Code 设置中配置

如果尚未配置 Playwright MCP，请参考
[Playwright MCP 配置指南](docs/playwright-mcp-setup.md)。

## 功能概览

自动化完成 [PTA（拼题A）](https://pintia.cn/) 上的算法编程题：

- 自动登录 PTA
- 读取题目描述与约束
- 本地编写 C++ 题解
- 将代码粘贴到 CodeMirror 6 编辑器（剪贴板 + 键盘方案）
- 自动提交并验证结果
- 批量完成整组题单（30+ 题）

## 支持题型

暴力枚举、分治法、减治法、动态规划、贪心算法、图论算法
（BFS、DFS、Dijkstra、Floyd、最小生成树、二分图匹配）、字符串算法（KMP）、
数学与概率、回溯法（N 皇后）、位运算 / XOR 等。

## 参与贡献

欢迎社区贡献！以下是参与方式：

### 报告 Bug

发现问题？请在 [GitHub Issues](https://github.com/Munto47/pta-solver-skill/issues)
提交，包含以下信息：
- 正在处理的 PTA 题单链接
- 出错时的具体表现（错误信息、截图）
- Claude Code 版本和操作系统

### 提交改进

1. Fork 本仓库
2. 创建功能分支（`git checkout -b feature/你的改进`）
3. 修改代码
4. 在真实的 PTA 题单上测试
5. 提交 Pull Request

### 社区求助方向

- 新题型模板
- 更稳定的 CodeMirror 6 交互方案
- C++ 以外的语言支持（Java、Python）
- 多种登录方式的适配
- 文档完善与多语言翻译

## 常见问题排查

### `/find-skills` 找不到此技能
技能注册表可能需要时间同步。可以先用上面的手动安装方式，或直接用 `npx skills add` 从 GitHub 安装。

### "Playwright MCP 未配置"
此技能依赖 Playwright MCP 服务器。详见
[Playwright MCP 配置指南](docs/playwright-mcp-setup.md)。快速安装：
```bash
npm install -g @playwright/mcp
npx playwright install chromium
```
然后将其添加到 Claude Code 的 MCP 配置中。

### 代码无法粘贴到编辑器
PTA 使用 CodeMirror 6 编辑器，会阻止程序化文本插入。本技能使用剪贴板 + 快捷键方案
来绕过此限制。如果仍然失败，请确保浏览器窗口在运行期间保持焦点。

### 提交后答案错误
- 仔细复核题目约束 —— PTA 很多题目有隐蔽的边界条件
- 检查整数溢出（需要时用 `long long` 代替 `int`）
- 确认输出格式完全匹配（末尾换行、空格等）

## 项目结构

```
pta-solver-skill/
  pta-solver.md                       # 主技能定义文件
  SKILL.md                            # skills.sh 注册表入口
  README.md                           # 英文文档
  README.zh-CN.md                     # 中文文档（本文件）
  LICENSE                             # MIT 许可证
  docs/
    playwright-mcp-setup.md           # Playwright MCP 配置指南
    publish-to-skills-sh.md           # skills.sh 发布指南
  .claude/skills/                     # 内置安装目录
```

## 发布到 skills.sh

要让此技能通过 `/find-skills` 和 `npx skills find` 被搜索到，
请参考 [发布到 skills.sh 指南](docs/publish-to-skills-sh.md)。

## 作者

由 [Munto47](https://github.com/Munto47) 创建并维护。

## 许可证

MIT — 自由使用、修改、分发。详见 [LICENSE](LICENSE)。
