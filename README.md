# Solodit MCP Server

A Model Context Protocol (MCP) server that provides access to [Cyfrin Solodit](https://solodit.cyfrin.io), the world's largest database of smart contract security findings and vulnerabilities.

## Features

- **Search 49,000+ Security Findings**: Access comprehensive smart contract audit findings from major firms
- **Advanced Filtering**: Filter by impact, audit firms, tags, protocols, languages, and more
- **Quality Metrics**: Search by quality and rarity scores
- **Rate Limited API**: Respects Solodit's rate limits (20 requests per 60 seconds)
- **Universal MCP Support**: Works with Claude Desktop, Claude Code, Cursor, VS Code with GitHub Copilot, and any MCP-compatible client

## Prerequisites

- Node.js 18 or higher
- A Solodit API key (get one from [Cyfrin Solodit](https://solodit.cyfrin.io))

## Installation

### Recommended: Global Installation

Install the package globally to use it from anywhere:

```bash
# Clone or download this repository
cd solodit-mcp

# Install dependencies and build
npm install
npm run build

# Install globally (creates the 'solodit-mcp' command)
npm install -g .
```

After global installation, the `solodit-mcp` command will be available system-wide.

### Alternative: Local Development

For development or if you prefer not to install globally:

```bash
cd solodit-mcp
npm install
npm run build
```

Then use the full path to `dist/index.js` in your configuration.

## Configuration

### 1. Get Your API Key

Visit [Cyfrin Solodit](https://solodit.cyfrin.io) and obtain your API key.

### 2. Configure for Your MCP Client

Choose your preferred client below:

<details>
<summary><b>Claude Desktop</b></summary>

Edit the Claude Desktop config file:

**macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
**Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

**If installed globally** (recommended):

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

**If using local installation**:

```json
{
  "mcpServers": {
    "solodit": {
      "command": "node",
      "args": ["/ABSOLUTE/PATH/TO/solodit-mcp/dist/index.js"],
      "env": {
        "SOLODIT_API_KEY": "sk_your_api_key_here"
      }
    }
  }
}
```

After saving, restart Claude Desktop.

</details>

<details>
<summary><b>Claude Code (CLI)</b></summary>

Claude Code automatically discovers MCP servers configured in your settings.

Create or edit `~/.config/claude-code/settings.json`:

**If installed globally** (recommended):

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

**If using local installation**:

```json
{
  "mcpServers": {
    "solodit": {
      "command": "node",
      "args": ["/ABSOLUTE/PATH/TO/solodit-mcp/dist/index.js"],
      "env": {
        "SOLODIT_API_KEY": "sk_your_api_key_here"
      }
    }
  }
}
```

Or use environment variables:

```bash
export SOLODIT_API_KEY=sk_your_api_key_here
claude-code
```

</details>

<details>
<summary><b>Cursor</b></summary>

Cursor supports MCP through its settings configuration.

Edit the Cursor config file:

**macOS**: `~/Library/Application Support/Cursor/User/settings.json`
**Windows**: `%APPDATA%\Cursor\User\settings.json`
**Linux**: `~/.config/Cursor/User/settings.json`

**If installed globally** (recommended):

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

**If using local installation**:

```json
{
  "mcpServers": {
    "solodit": {
      "command": "node",
      "args": ["/ABSOLUTE/PATH/TO/solodit-mcp/dist/index.js"],
      "env": {
        "SOLODIT_API_KEY": "sk_your_api_key_here"
      }
    }
  }
}
```

Restart Cursor after making changes.

</details>

<details>
<summary><b>VS Code with GitHub Copilot</b></summary>

VS Code supports MCP servers through the GitHub Copilot extension (requires Copilot Chat).

Edit your VS Code settings:

**macOS**: `~/Library/Application Support/Code/User/settings.json`
**Windows**: `%APPDATA%\Code\User\settings.json`
**Linux**: `~/.config/Code/User/settings.json`

**If installed globally** (recommended):

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

**If using local installation**:

```json
{
  "github.copilot.chat.mcp.servers": {
    "solodit": {
      "command": "node",
      "args": ["/ABSOLUTE/PATH/TO/solodit-mcp/dist/index.js"],
      "env": {
        "SOLODIT_API_KEY": "sk_your_api_key_here"
      }
    }
  }
}
```

Alternatively, use the VS Code Command Palette:
1. Press `Cmd+Shift+P` (macOS) or `Ctrl+Shift+P` (Windows/Linux)
2. Type "Preferences: Open User Settings (JSON)"
3. Add the configuration above

Reload VS Code after configuration.

**Note**: MCP support in VS Code requires GitHub Copilot Chat extension v0.12.0 or later.

</details>

<details>
<summary><b>Other MCP Clients</b></summary>

For other MCP-compatible clients:

**If installed globally**:

```json
{
  "command": "solodit-mcp",
  "env": {
    "SOLODIT_API_KEY": "sk_your_api_key_here"
  }
}
```

**If using local installation**:

```json
{
  "command": "node",
  "args": ["/ABSOLUTE/PATH/TO/solodit-mcp/dist/index.js"],
  "env": {
    "SOLODIT_API_KEY": "sk_your_api_key_here"
  }
}
```

</details>

## Available Tools

### 1. `search_findings`

Search Solodit for smart contract security findings with advanced filtering options.

**Parameters:**

- `keywords` (string): Search keywords to find in title and content
- `impact` (array): Filter by severity - `["HIGH", "MEDIUM", "LOW", "GAS"]`
- `firms` (array): Filter by audit firm names (e.g., `["Cyfrin", "Sherlock", "Code4rena"]`)
- `tags` (array): Filter by vulnerability tags (e.g., `["Reentrancy", "Oracle", "Access Control"]`)
- `protocol` (string): Filter by protocol name (partial match)
- `protocolCategory` (array): Filter by protocol categories (e.g., `["DeFi", "NFT", "Lending"]`)
- `languages` (array): Filter by programming languages (e.g., `["Solidity", "Rust", "Cairo"]`)
- `user` (string): Filter by finder/auditor handle (partial match)
- `minFinders` (string): Minimum number of finders
- `maxFinders` (string): Maximum number of finders
- `reportedDays` (string): Time period - `"30"`, `"60"`, `"90"`, or `"alltime"`
- `qualityScore` (number): Minimum quality score (0-5)
- `rarityScore` (number): Minimum rarity score (0-5)
- `sortField` (string): Sort by `"Recency"`, `"Quality"`, or `"Rarity"`
- `sortDirection` (string): `"Desc"` or `"Asc"`
- `page` (number): Page number (default: 1)
- `pageSize` (number): Results per page (default: 20, max: 100)

**Example Usage:**

```
Search for high severity reentrancy vulnerabilities:
- keywords: "reentrancy"
- impact: ["HIGH"]
- sortField: "Quality"
- pageSize: 10
```

### 2. `get_finding_by_id`

Get detailed information about a specific finding by its ID or slug.

**Parameters:**

- `keywords` (string, required): The finding ID or slug to search for

**Example Usage:**

```
Get finding details by ID:
- keywords: "finding-id-12345"
```

## Usage Examples

### Example 1: Search for High Severity Findings

```
Use the search_findings tool with:
- impact: ["HIGH"]
- pageSize: 20
- sortField: "Recency"
```

### Example 2: Find Oracle-Related Issues in DeFi

```
Use the search_findings tool with:
- tags: ["Oracle"]
- protocolCategory: ["DeFi"]
- qualityScore: 3
```

### Example 3: Search Specific Audit Firm Reports

```
Use the search_findings tool with:
- firms: ["Cyfrin", "Trail of Bits"]
- impact: ["HIGH", "MEDIUM"]
- reportedDays: "30"
```

### Example 4: Search by Keywords

```
Use the search_findings tool with:
- keywords: "flash loan attack"
- sortField: "Quality"
- sortDirection: "Desc"
```

## Development

### Run in Development Mode

```bash
npm run dev
```

### Build

```bash
npm run build
```

### Watch Mode

```bash
npm run watch
```

## Rate Limiting

The Solodit API has a default rate limit of **20 requests per 60-second window**. The server includes rate limit information in responses:

- Total requests allowed
- Remaining requests in current window
- Time when the window resets

If you exceed the rate limit, you'll receive a `429 Too Many Requests` error.

## Error Handling

The server provides clear error messages for common issues:

- **Missing API Key**: "SOLODIT_API_KEY environment variable is not set"
- **Invalid API Key**: "Solodit API error (401): Invalid API key"
- **Rate Limit Exceeded**: "Solodit API error (429): Rate limit exceeded"
- **Network Errors**: Connection and timeout errors are properly reported

## Available Filters Reference

### Popular Audit Firms
- Cyfrin
- Sherlock
- Code4rena
- Trail of Bits
- OpenZeppelin
- Consensys Diligence
- Pashov Audit Group
- Spearbit
- Hacken
- Chainsecurity

### Common Vulnerability Tags
- Reentrancy
- Oracle
- Access Control
- Integer Overflow/Underflow
- Front-running
- Logic Error
- DOS
- Price Manipulation
- Flash Loan
- Griefing

### Protocol Categories
- DeFi
- NFT
- Lending
- DEX
- Staking
- Governance
- Bridge
- Options Vault
- Yield Aggregator

### Programming Languages
- Solidity
- Rust
- Cairo
- Vyper
- Move

For a comprehensive list of all available filter values, see the [Solodit API Documentation](https://cyfrin.notion.site/Cyfrin-Solodit-Findings-API-Specification-299f46a1865c80bcaaf0d8672fece2d6).

## Project Structure

```
solodit-mcp/
├── src/
│   └── index.ts          # Main MCP server implementation
├── dist/                 # Compiled JavaScript (generated)
├── package.json
├── tsconfig.json
├── README.md
└── .env.example
```

## Troubleshooting

### API Key Issues

If you get authentication errors:
1. Verify your API key is correct
2. Ensure the environment variable is set properly
3. Restart your MCP client after configuration changes

### Connection Issues

If the server fails to connect:
1. Check your internet connection
2. Verify the Solodit API is accessible: `curl https://solodit.cyfrin.io`
3. Check for any firewall or proxy issues

### Server Not Showing Up

If the MCP server doesn't appear in your client:
1. Verify the path to `dist/index.js` is absolute, not relative
2. Check that the build completed successfully (`npm run build`)
3. Ensure the config file JSON syntax is valid
4. Restart your MCP client completely
5. Check client logs for error messages

### Rate Limit Issues

If you're hitting rate limits:
1. Reduce the frequency of requests
2. Implement delays between searches
3. Use pagination wisely (larger page sizes for fewer requests)

## Contributing

Contributions are welcome! Please feel free to submit issues and pull requests.

## License

MIT

## Resources

- [Cyfrin Solodit](https://solodit.cyfrin.io)
- [Solodit Documentation](https://docs.solodit.cyfrin.io)
- [Solodit API Specification](https://cyfrin.notion.site/Cyfrin-Solodit-Findings-API-Specification-299f46a1865c80bcaaf0d8672fece2d6)
- [Model Context Protocol](https://modelcontextprotocol.io)
- [MCP TypeScript SDK](https://github.com/modelcontextprotocol/typescript-sdk)

## Support

For issues related to:
- **This MCP Server**: Open an issue in this repository
- **Solodit API**: Contact [Cyfrin Support](https://www.cyfrin.io)
- **MCP Protocol**: See [MCP Documentation](https://modelcontextprotocol.io)
