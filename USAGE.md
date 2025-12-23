# Solodit MCP Usage Guide

This guide provides practical examples for using the Solodit MCP server across different AI coding assistants.

## Quick Start

### 1. Install Globally (Recommended)

```bash
cd solodit-mcp
npm install
npm run build

# Install globally (creates 'solodit-mcp' command)
npm install -g .
```

Verify installation:
```bash
which solodit-mcp  # Should show the global installation path
```

### 2. Get Your API Key

Visit [Cyfrin Solodit](https://solodit.cyfrin.io) to obtain your API key.

### 3. Configure Your Client

The Solodit MCP server works with:
- **Claude Desktop** - Anthropic's desktop app
- **Claude Code** - CLI tool for terminal usage
- **Cursor** - AI-first code editor
- **VS Code** - With GitHub Copilot Chat extension
- **Any MCP-compatible client**

> **Note**: The examples below show the **global installation** method (using `"command": "solodit-mcp"`). This is simpler and recommended. For local installation, use `"command": "node"` with `"args": ["/path/to/dist/index.js"]` instead.

#### Claude Desktop Setup

**macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
**Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

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

Restart Claude Desktop after saving.

#### Claude Code Setup

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

#### Cursor Setup

**macOS**: `~/Library/Application Support/Cursor/User/settings.json`
**Windows**: `%APPDATA%\Cursor\User\settings.json`
**Linux**: `~/.config/Cursor/User/settings.json`

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

Restart Cursor after saving.

#### VS Code + GitHub Copilot Setup

**macOS**: `~/Library/Application Support/Code/User/settings.json`
**Windows**: `%APPDATA%\Code\User\settings.json`
**Linux**: `~/.config/Code/User/settings.json`

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

Reload VS Code after saving (requires GitHub Copilot Chat v0.12.0+).

## Example Queries

Once configured, you can ask Claude questions like:

### General Search

> "Search Solodit for recent high severity vulnerabilities"

> "Find all reentrancy vulnerabilities reported in the last 30 days"

> "Show me the top quality oracle-related issues"

### Specific Filters

> "Search for flash loan attacks in DeFi protocols with quality score above 3"

> "Find all vulnerabilities discovered by Cyfrin in Solidity contracts"

> "Get HIGH and MEDIUM severity findings from Sherlock audits"

### Research Queries

> "What are the most common access control vulnerabilities in smart contracts?"

> "Show me recent price manipulation vulnerabilities"

> "Find vulnerabilities in lending protocols"

### Protocol-Specific

> "Search for vulnerabilities in Uniswap"

> "Find security findings for Aave protocol"

> "Show me all NFT marketplace vulnerabilities"

### Language-Specific

> "Find Rust smart contract vulnerabilities"

> "Show me Cairo language security findings"

> "Search for Vyper contract vulnerabilities"

## Tool Parameters Explained

### `search_findings`

#### Impact Levels
- `HIGH` - Critical vulnerabilities that could lead to loss of funds
- `MEDIUM` - Significant issues with potential impact
- `LOW` - Minor issues or best practice violations
- `GAS` - Gas optimization opportunities

#### Sort Options
- `Recency` - Most recently reported first (default)
- `Quality` - Highest quality findings first
- `Rarity` - Most unique/rare findings first

#### Time Filters
- `30` - Last 30 days
- `60` - Last 60 days
- `90` - Last 90 days
- `alltime` - All findings (default)

#### Quality/Rarity Scores
- Range: 0-5
- Higher scores indicate better quality or more unique findings
- Default minimum is 1

### Common Audit Firms

Use these in the `firms` filter:
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
- SlowMist
- PeckShield

### Common Tags

Use these in the `tags` filter:
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
- Governance
- Signature Verification
- Timestamp Dependence

### Protocol Categories

Use these in the `protocolCategory` filter:
- DeFi
- NFT
- Lending
- DEX
- Staking
- Governance
- Bridge
- Options Vault
- Yield Aggregator
- Liquid Staking
- Perpetuals

### Programming Languages

Use these in the `languages` filter:
- Solidity
- Rust
- Cairo
- Vyper
- Move
- Huff

## Advanced Examples

### Example 1: High-Quality DeFi Vulnerabilities

Ask Claude:
> "Search Solodit for DeFi protocol vulnerabilities with quality score above 4, focusing on HIGH impact findings"

This will use:
- `protocolCategory`: ["DeFi"]
- `qualityScore`: 4
- `impact`: ["HIGH"]
- `sortField`: "Quality"

### Example 2: Recent Reentrancy Attacks

Ask Claude:
> "Find all reentrancy vulnerabilities from the last 30 days, sorted by quality"

This will use:
- `tags`: ["Reentrancy"]
- `reportedDays`: "30"
- `sortField`: "Quality"
- `sortDirection`: "Desc"

### Example 3: Auditor-Specific Search

Ask Claude:
> "Show me all HIGH severity findings discovered by Trail of Bits in Rust contracts"

This will use:
- `firms`: ["Trail of Bits"]
- `impact`: ["HIGH"]
- `languages`: ["Rust"]

### Example 4: Multi-Tag Search

Ask Claude:
> "Find oracle and price manipulation vulnerabilities in lending protocols"

This will use:
- `tags`: ["Oracle", "Price Manipulation"]
- `protocolCategory`: ["Lending"]
- `impact`: ["HIGH", "MEDIUM"]

### Example 5: Rare Vulnerabilities

Ask Claude:
> "Show me the rarest vulnerabilities with high quality scores"

This will use:
- `rarityScore`: 4
- `qualityScore`: 4
- `sortField`: "Rarity"
- `sortDirection`: "Desc"

## Understanding Results

Each finding includes:

- **Title**: Brief description of the vulnerability
- **ID & Slug**: Unique identifiers
- **Impact**: Severity level (HIGH/MEDIUM/LOW/GAS)
- **Protocol**: Affected protocol name
- **Audit Firm**: Firm that conducted the audit
- **Quality Score**: Community-voted quality (0-5)
- **Rarity Score**: Uniqueness rating (0-5)
- **Finders**: Security researchers who found the issue
- **Tags**: Vulnerability categories and types
- **Source Link**: Link to full report
- **Content**: Detailed description and analysis

## Pagination

Results are paginated:
- Default: 20 results per page
- Maximum: 100 results per page
- You can request specific pages with the `page` parameter

Example:
> "Search for oracle vulnerabilities and show me page 2 with 50 results per page"

## Rate Limits

The API allows:
- **20 requests per 60 seconds**
- Rate limit info is shown in search results
- Plan your searches accordingly to avoid hitting limits

## Tips for Effective Searches

1. **Start Broad, Then Narrow**: Begin with general searches, then add filters
2. **Use Multiple Filters**: Combine impact, tags, and categories for precision
3. **Check Quality Scores**: Higher quality findings have more detailed analysis
4. **Review Recent Findings**: Use time filters to see latest vulnerabilities
5. **Learn from Patterns**: Search for specific vulnerability types to understand common issues
6. **Cross-Reference**: Search by protocol and by vulnerability type separately

## Troubleshooting

### No Results Found

- Try broader search terms
- Remove some filters
- Check spelling of firm names, tags, etc.
- Try `alltime` instead of time-limited searches

### Rate Limit Errors

- Wait for the rate limit window to reset
- Reduce frequency of searches
- Use larger page sizes to get more results per request

### API Key Issues

- Verify your API key is correct
- Ensure the environment variable is set
- Restart Claude Desktop after config changes
- Check the config file syntax is valid JSON

## Getting Help

- Check the [README](README.md) for setup instructions
- Review the [Solodit Documentation](https://docs.solodit.cyfrin.io)
- See the [API Specification](https://cyfrin.notion.site/Cyfrin-Solodit-Findings-API-Specification-299f46a1865c80bcaaf0d8672fece2d6)
