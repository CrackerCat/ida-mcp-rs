# ida-mcp

Headless IDA Pro MCP server for AI-powered reverse engineering.

## Prerequisites

- IDA 9.2+ (stable) or IDA 9.3+ (beta) with valid license

## Getting Started

### Install

Via [Homebrew](https://brew.sh) (macOS only)
```bash
# Stable (IDA 9.2)
brew install blacktop/tap/ida-mcp

# Beta (IDA 9.3)
brew install blacktop/tap/ida-mcp@beta
```

**Download binary**

Grab the latest release for your platform from [GitHub Releases](https://github.com/blacktop/ida-mcp-rs/releases).

**Build from source**

See [docs/BUILDING.md](docs/BUILDING.md).

### Platform Setup

#### macOS

```bash
# Add to your MCP config
claude mcp add ida -- ida-mcp
```

If using IDA Home/Essential or non-standard path:
```bash
claude mcp add ida -s user -e DYLD_LIBRARY_PATH='/Applications/IDA Essential 9.2.app/Contents/MacOS' -- ida-mcp
```

Common macOS IDA paths:
- `/Applications/IDA Professional 9.2.app/Contents/MacOS`
- `/Applications/IDA Professional 9.3.app/Contents/MacOS` (beta)
- `/Applications/IDA Home 9.2.app/Contents/MacOS`
- `/Applications/IDA Essential 9.2.app/Contents/MacOS`

#### Linux

```bash
# Set IDADIR to your IDA installation
export IDADIR=/opt/idapro-9.2

# Add to your MCP config
claude mcp add ida -s user -e IDADIR='/opt/idapro-9.2' -- ida-mcp
```

Common Linux IDA paths:
- `/opt/idapro-9.2`
- `/home/<user>/idapro-9.2`
- `/usr/local/idapro-9.2`

#### Windows

```powershell
# Set IDADIR environment variable (System Properties > Environment Variables)
# Or in PowerShell profile:
$env:IDADIR = "C:\Program Files\IDA Professional 9.2"
$env:PATH = "$env:IDADIR;$env:PATH"

# Add to MCP config
# Note: You may need to set IDADIR globally in System Environment Variables
claude mcp add ida -- ida-mcp
```

Common Windows IDA paths:
- `C:\Program Files\IDA Professional 9.2`
- `C:\IDA Professional 9.2`
- `C:\Program Files\IDA Home 9.2`

### Runtime Requirements

The binary links against IDA's libraries at runtime:

| Platform | Library | How to Configure |
|----------|---------|------------------|
| macOS | `libidalib.dylib` | `DYLD_LIBRARY_PATH` or baked RPATH |
| Linux | `libidalib.so` | `IDADIR` env var (RPATH baked at build) |
| Windows | `idalib.dll` | Add `IDADIR` to `PATH` |

### Configure your AI agent

#### [Claude Code](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/overview)
```bash
claude mcp add ida -- ida-mcp
```

#### [Codex CLI](https://platform.openai.com/docs/guides/mcp)
```bash
cursor mcp add ida -- ida-mcp
```

#### [Gemini CLI](https://github.com/google/gemini-cli)

```bash
gemini mcp add ida ida-mcp
```

### Usage

Once configured, you can analyze binaries through your AI agent:

```
# Open a binary (IDA analyzes raw binaries automatically)
open_idb(path: "~/samples/malware")

# Discover available tools
tool_catalog(query: "find callers")

# List functions
list_functions(limit: 20)

# Disassemble by name
disasm_by_name(name: "main", count: 20)

# Decompile (requires Hex-Rays)
decompile(address: "0x100000f00")
```

The default tool list includes all tools. Use `tool_catalog`/`tool_help` to discover capabilities and avoid dumping the full list into context.

## Docs

- [docs/TOOLS.md](docs/TOOLS.md) - Tool catalog and discovery workflow
- [docs/TRANSPORTS.md](docs/TRANSPORTS.md) - Stdio vs Streamable HTTP
- [docs/BUILDING.md](docs/BUILDING.md) - Build from source
- [docs/TESTING.md](docs/TESTING.md) - Running tests

## License

MIT Copyright (c) 2026 **blacktop**
