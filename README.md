# codebase-cartographer

> Navigate any codebase like Google Maps — zoom, search, and explore dependencies interactively.

A [Claude Code](https://claude.ai/code) skill that scans a code repository and generates an interactive D3.js architecture map showing module dependencies, circular dependency warnings, hub modules, and isolated files.

## Features

- **Multi-Language Import Parsing** — Python, JavaScript/TypeScript, Go, Java
- **Interactive D3.js Force Graph** — Zoom, pan, drag, hover, click for details
- **Circular Dependency Detection** — Tarjan's SCC algorithm, highlighted in red
- **Module Classification**
  - **Hubs** — High in-degree (core dependencies everyone imports)
  - **Orchestrators** — High out-degree (coordinators importing many modules)
  - **Islands** — Zero connections (potential dead code)
- **Project Detection** — Auto-detects framework (Next.js, Django, Flask, etc.) and package manager
- **Dark Theme** — Sidebar with search, filter, and statistics panel

## Installation

```bash
claude skill add daizhouchen/codebase-cartographer
```

## How It Works

1. **Scan** — `scripts/scan.py` recursively walks the directory, classifies files by language
2. **Analyze** — `scripts/analyze_deps.py` parses imports, builds dependency graph, detects cycles
3. **Render** — Generates an interactive HTML map using the D3.js template

## Manual Usage

```bash
# Scan a project directory
python3 scripts/scan.py /path/to/project

# Analyze dependencies and generate map
python3 scripts/analyze_deps.py
# Output: codemap.html
```

## Trigger Phrases

- "代码架构" / "项目结构" / "依赖关系"
- "帮我看懂这个项目"
- "这个仓库好大我不知道从哪里看起"

## Project Structure

```
codebase-cartographer/
├── SKILL.md                   # Skill definition and workflow
├── scripts/
│   ├── scan.py                # Directory scanner and file classifier
│   └── analyze_deps.py        # Dependency graph builder + Tarjan SCC
├── assets/
│   └── map_template.html      # D3.js interactive map template
└── README.md
```

## Supported Languages

| Language | Import Patterns |
|----------|----------------|
| Python | `import X`, `from X import Y` |
| JavaScript/TypeScript | `import ... from '...'`, `require('...')` |
| Go | `import "..."` |
| Java | `import ...` |

## Map Interactions

| Action | Effect |
|--------|--------|
| Scroll | Zoom in/out |
| Drag background | Pan the view |
| Drag node | Reposition module |
| Hover node | Show file path and degree |
| Click node | Open info sidebar |
| Search box | Filter by filename |

## Requirements

- Python 3.8+ (no external packages)
- A modern browser for viewing the HTML map

## Limitations

- Only parses import statements (not dynamic imports or reflection)
- Large repos (>5000 files) use sampling strategy
- Does not read file contents beyond import lines (privacy-friendly)

## License

MIT
