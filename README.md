> **Fork context:** this repository is a fork of [SShadowS/al-lsp-for-agents](https://github.com/SShadowS/al-lsp-for-agents). The documentation below retains the original project's authorship. Use the parent repository for its published releases and support guidance; this audit does not establish a separate maintained distribution or promise synchronization.
>
> [Compare this fork with its parent](https://github.com/SShadowS/al-lsp-for-agents/compare/main...javiarmesto:al-lsp-for-agents:main). The comparison shows the current differences; fork-specific behavior must be assessed from those changes. Static documentation review: **6 October 2026**; no build, installation or service invocation performed.

# AL LSP for Agents

AL Language Server wrappers for AI-powered Business Central development. Works with Claude Code, OpenCode, and VS Code (GitHub Copilot agent mode).

## Checkout scope

This checkout supplies `al-language-server-go`, `al-language-server-go-windows` and `al-language-server-go-linux`. The Darwin and deprecated Python rows in the inherited table below describe other variants; those directories are not included here. Check the parent repository before following their installation paths. This review did not validate downloaded binaries or compilation.

## Available Wrappers

| Wrapper | Platform | Description |
|---------|----------|-------------|
| `al-language-server-go-windows` | Windows | **Recommended** - Go wrapper with Code Lens & Call Hierarchy |
| `al-language-server-go-linux` | Linux | **Recommended** - Go wrapper with Code Lens & Call Hierarchy |
| `al-language-server-go-darwin` | macOS | **Recommended** - Go wrapper with Code Lens & Call Hierarchy |
| `al-language-server-python` | Cross-platform | *Deprecated* - Basic LSP features only |

> **Migration Note:** If you're using the Python wrapper, switch to the Go wrapper for your platform to get Code Lens (reference counts) and Call Hierarchy features.

## Features

- **Hover** - Type information and documentation
- **Go to Definition** - Jump to symbol definitions (tables, codeunits, enums, procedures)
- **Document Symbols** - List all symbols in a file
- **Find References** - Find all references to a symbol
- **Call Hierarchy** - Find incoming and outgoing calls for procedures
- **Symbol Relations** - How an object relates to others (extends, implements, source table, extended-by)
- **Inspect Page** - A page's control tree or action tree, including dependency pages
- **Multi-project support** - Workspaces with multiple AL apps

## Prerequisites

1. **VS Code** with the [AL Language extension](https://marketplace.visualstudio.com/items?itemName=ms-dynamics-smb.al) installed

The Go wrappers are compiled binaries with no runtime dependencies. The wrapper automatically finds the newest AL extension version in your VS Code extensions folder.

## Installation

### OpenCode

One-command setup:

**Linux:**
```bash
curl -fsSL https://raw.githubusercontent.com/SShadowS/al-lsp-for-agents/main/scripts/setup-opencode.sh | bash
```

**Windows (PowerShell):**
```powershell
irm https://raw.githubusercontent.com/SShadowS/al-lsp-for-agents/main/scripts/setup-opencode.ps1 | iex
```

The script downloads the latest binaries, checks for the AL VS Code extension, and configures `opencode.json`. Run it again to update. Use `SCOPE=project` to write config to the current directory instead of globally.

### Claude Code

#### 1. Enable LSP Tool

```powershell
# PowerShell (current session)
$env:ENABLE_LSP_TOOL = "1"
claude

# PowerShell (permanent)
[Environment]::SetEnvironmentVariable("ENABLE_LSP_TOOL", "1", "User")
```

```bash
# Bash
export ENABLE_LSP_TOOL=1
claude
```

#### 2. Add Marketplace

```
/plugin marketplace add SShadowS/al-lsp-for-agents
```

#### 3. Install Plugin

1. Type `/plugins`
2. Tab to `Marketplaces`
3. Enter `al-lsp-for-agents` marketplace
4. Select the Go wrapper for your platform:
   - Windows: `al-language-server-go-windows`
   - Linux: `al-language-server-go-linux`
   - macOS: `al-language-server-go-darwin`
5. Press "i" to install
6. Restart Claude Code

## LSP Operations

Claude can use these LSP operations on AL files:

| Operation | Status | Description |
|-----------|--------|-------------|
| `goToDefinition` | Working | Go to symbol definition |
| `goToImplementation` | Working | Go to implementation |
| `hover` | Working | Get type/documentation info |
| `documentSymbol` | Working | List symbols in file |
| `findReferences` | Working | Find all references |
| `prepareCallHierarchy` | Working | Get call hierarchy item at position |
| `incomingCalls` | Working | Find callers of a procedure |
| `outgoingCalls` | Working | Find calls made by a procedure |
| `workspaceSymbol` | Working | Pass a `query`; needs Claude Code 2.1.x+ |
| `symbolRelations` | Working | Related symbols (extends/implements/source table/extended-by) |
| `inspectPage` | Working | Page control/action tree (requires nuget al tool) |

## Known Issues

### workspaceSymbol (resolved in Claude Code 2.1.x)

Older Claude Code versions hardcoded an empty `query` for `workspaceSymbol`, so it always returned 0 symbols ([anthropics/claude-code#17149](https://github.com/anthropics/claude-code/issues/17149)). Fixed client-side in Claude Code 2.1.x — pass a search term and it works.

On older clients, use `documentSymbol` (per-file symbols) or `Grep` as fallbacks. See [KnownIssues.md](KnownIssues.md) for full details.

## License

[GNU General Public License v3.0](https://choosealicense.com/licenses/gpl-3.0/)
