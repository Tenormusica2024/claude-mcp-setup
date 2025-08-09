# Claude Code MCP Servers Installation Guide

## Prerequisites

### Required Software
- **Claude Code** (latest version)
- **Node.js** (for npx-based servers)
- **Python 3.10+** with **uv package manager**
- **Git** (with SSH configuration)
- **Docker Desktop** (for container-based operations)

### Pre-Installation Setup

1. **Configure SSH for GitHub**
   ```bash
   ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_nopass -C "your_email@example.com" -N ""
   # Add public key to GitHub: ~/.ssh/id_ed25519_nopass.pub
   ```

2. **Install uv package manager**
   ```bash
   pip install uv
   ```

## Step-by-Step Installation

### 1. Core MCP Servers

#### GitHub Integration
```bash
claude mcp add github -s user -- npx -y @modelcontextprotocol/server-github
```

#### Sequential Thinking (Official Anthropic)
```bash
claude mcp add sequential-thinking -s user -- npx -y @modelcontextprotocol/server-sequential-thinking
```

#### Web Automation
```bash
claude mcp add puppeteer -s user -- npx -y @modelcontextprotocol/server-puppeteer
```

#### Filesystem Access
```bash
claude mcp add filesystem -s user -- npx -y @modelcontextprotocol/server-filesystem ~/Documents ~/Desktop ~/Downloads
```

#### Persistent Memory
```bash
npm install -g @movibe/memory-bank-mcp
claude mcp add memory-bank -s user -- npx @movibe/memory-bank-mcp --mode code
```

#### Optimized Web Fetching
```bash
claude mcp add fetch -s user -- uvx mcp-server-fetch
```

### 2. Advanced Servers (Pre-configured)

#### Serena (Semantic Code Analysis)
*Already configured via previous setup*
- **Path**: `C:\Users\Tenormusica\.local\bin\serena-mcp-server.exe`
- **Config**: `C:\Users\Tenormusica\.serena\project.yml`

#### Cipher (AI Memory Layer)
*Already configured*
- **Command**: `cipher-mcp-server`

#### Context7 (API Documentation)
*Already configured*
- **Command**: `npx --yes @upstash/context7-mcp`

### 3. Permission Configuration

#### Maximum Permissions Setup
Create/update `~/.claude/settings.local.json`:
```json
{
  "permissions": {
    "allow": ["*"],
    "deny": []
  },
  "dangerously-skip-permissions": true
}
```

#### Environment Variable
```bash
setx CLAUDE_DEFAULT_ARGS "--dangerously-skip-permissions"
```

### 4. Verification

Check all servers are connected:
```bash
claude mcp list
```

Expected output should show all servers with **✓ Connected** status.

## Configuration Details

### Filesystem Server Paths
The filesystem server is configured to access:
- `~/Documents` - User documents
- `~/Desktop` - Desktop files  
- `~/Downloads` - Download folder

### Memory Bank Settings
- **Mode**: `code` (optimized for development)
- **Storage**: Local JSON file
- **Features**: Cross-session persistence

### Security Considerations

⚠️ **Important Security Notes:**
- `dangerously-skip-permissions` removes all safety prompts
- Only use in trusted environments
- Consider Docker containers for isolation
- Regular backups recommended

## Troubleshooting

### Common Issues

#### Server Connection Failed
```bash
# Remove and re-add problematic server
claude mcp remove [server-name]
claude mcp add [server-name] -s user -- [command]
```

#### Permission Denied
- Ensure Claude Code has necessary system permissions
- Run as administrator if required
- Check file/directory permissions

#### SSH Issues
```bash
# Test SSH connection
ssh -T git@github.com
# Should return: "Hi [username]! You've successfully authenticated..."
```

### Server-Specific Issues

#### Puppeteer
- Requires Chrome/Chromium installation
- May need `--no-sandbox` flag in restricted environments

#### Memory Bank
- Check write permissions to user directory
- Verify Node.js version compatibility

#### Fetch Server
- Requires network connectivity
- May need proxy configuration in corporate environments

## Advanced Configuration

### Custom Server Paths
Modify server commands in `~/.claude.json` for custom configurations:
```json
{
  "mcpServers": {
    "custom-server": {
      "command": "custom-command",
      "args": ["--custom-args"]
    }
  }
}
```

### Performance Optimization
- Use SSD storage for Memory Bank persistence
- Ensure adequate RAM for Puppeteer operations
- Configure Docker with sufficient resources

## Maintenance

### Regular Updates
```bash
# Update all npm-based servers
npm update -g @movibe/memory-bank-mcp

# Check for server updates
claude mcp list --check-updates
```

### Health Monitoring
```bash
# Regular health check
claude mcp list
```

### Backup Configuration
Backup these files regularly:
- `~/.claude.json`
- `~/.claude/settings.local.json`
- `~/.serena/project.yml`

---

**Last Updated:** 2025-08-09  
**Version:** 1.0  
**Support:** Check GitHub issues for community support