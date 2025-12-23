# Solodit MCP - Quick Start Guide

## Installation (Recommended: Global)

### 1. Build and Install Globally
```bash
cd solodit-mcp
npm install
npm run build

# Install globally (creates 'solodit-mcp' command)
npm install -g .
```

### 2. Get Your API Key
Visit [Cyfrin Solodit](https://solodit.cyfrin.io) to obtain your API key.

### 3. Configure Your MCP Client

Choose your client:

## Claude Desktop

**macOS**: Edit `~/Library/Application Support/Claude/claude_desktop_config.json`
**Windows**: Edit `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "solodit": {
      "command": "solodit-mcp",
      "env": {
        "SOLODIT_API_KEY": "sk_your_api_key_here"
      }
    }
  }
}
```

**Restart Claude Desktop** after saving.

---

## Claude Code (CLI)

Edit `~/.config/claude-code/settings.json`:

```json
{
  "mcpServers": {
    "solodit": {
      "command": "solodit-mcp",
      "env": {
        "SOLODIT_API_KEY": "sk_your_api_key_here"
      }
    }
  }
}
```

Or set environment variable:

```bash
export SOLODIT_API_KEY=sk_your_api_key_here
claude-code
```

---

## Cursor

**macOS**: Edit `~/Library/Application Support/Cursor/User/settings.json`
**Windows**: Edit `%APPDATA%\Cursor\User\settings.json`
**Linux**: Edit `~/.config/Cursor/User/settings.json`

```json
{
  "mcpServers": {
    "solodit": {
      "command": "solodit-mcp",
      "env": {
        "SOLODIT_API_KEY": "sk_your_api_key_here"
      }
    }
  }
}
```

**Restart Cursor** after saving.

---

## VS Code + GitHub Copilot

**macOS**: Edit `~/Library/Application Support/Code/User/settings.json`
**Windows**: Edit `%APPDATA%\Code\User\settings.json`
**Linux**: Edit `~/.config/Code/User/settings.json`

```json
{
  "github.copilot.chat.mcp.servers": {
    "solodit": {
      "command": "solodit-mcp",
      "env": {
        "SOLODIT_API_KEY": "sk_your_api_key_here"
      }
    }
  }
}
```

**Reload VS Code** after saving.

**Note**: Requires GitHub Copilot Chat v0.12.0+

---

## Alternative: Local Installation (No Global Install)

If you prefer not to install globally:

```bash
cd solodit-mcp
npm install
npm run build
```

Then use the full path in your config:

```json
{
  "mcpServers": {
    "solodit": {
      "command": "node",
      "args": ["/FULL/PATH/TO/solodit-mcp/dist/index.js"],
      "env": {
        "SOLODIT_API_KEY": "sk_your_api_key_here"
      }
    }
  }
}
```

---

## Important Notes

- **Global install recommended**: Simpler configuration with just `"command": "solodit-mcp"`
- **Replace API key**: Use your actual Solodit API key
- **Restart required**: Always restart your client after config changes
- **Verify installation**: Run `which solodit-mcp` to confirm global install

## Verify It's Working

Try asking:
> "Search Solodit for recent high severity vulnerabilities"

## Common Commands

### Search Examples

```
"Find reentrancy vulnerabilities from the last 30 days"
"Show HIGH severity findings from Cyfrin"
"Search for oracle issues in DeFi protocols"
"Find flash loan vulnerabilities with quality score above 4"
"Get findings for Uniswap protocol"
```

## Available Tools

1. **search_findings** - Search and filter 49,000+ security findings
2. **get_finding_by_id** - Get details for a specific finding

## Key Features

- **Impact Levels**: HIGH, MEDIUM, LOW, GAS
- **49,000+ Findings**: From major audit firms
- **Advanced Filters**: Tags, protocols, languages, firms, dates
- **Quality Scores**: Community-rated findings (0-5)
- **Rarity Scores**: Unique vulnerability ratings (0-5)

## Rate Limits

- 20 requests per 60 seconds
- Plan your searches accordingly

## Need Help?

- Full documentation: [README.md](README.md)
- Usage examples: [USAGE.md](USAGE.md)
- API docs: https://cyfrin.notion.site/Cyfrin-Solodit-Findings-API-Specification-299f46a1865c80bcaaf0d8672fece2d6

## Troubleshooting

**Server not connecting?**
1. Verify global install: `which solodit-mcp`
2. Reinstall if needed: `npm install -g .`
3. Check API key is correct
4. Ensure you restarted your client
5. Check config JSON syntax is valid

**Command not found?**
- Make sure you ran `npm install -g .` from the solodit-mcp directory
- Check npm global bin directory: `npm bin -g`
- Add to PATH if needed

**No results?**
1. Try broader search terms
2. Remove some filters
3. Check filter values match available options

**Rate limit errors?**
- Wait 60 seconds between heavy searches
- Use pagination wisely

## Uninstalling

To remove the global installation:

```bash
npm uninstall -g solodit-mcp
```

## Client-Specific Tips

### Claude Desktop
- Config changes require full app restart
- Check Console logs for MCP server errors

### Claude Code
- MCP servers load on CLI startup
- Use `--verbose` flag to see MCP server logs

### Cursor
- Enable MCP in Settings → Features
- Check Cursor Developer Tools for errors

### VS Code
- Open Output panel → GitHub Copilot Chat to see MCP logs
- Ensure Copilot Chat extension is v0.12.0+
- Use Command Palette: "Developer: Reload Window" to reload
