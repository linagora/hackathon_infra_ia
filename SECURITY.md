# Security and Privacy Guide

## 🔴 Critical Warnings

### Never Send to LLMs
- **Passwords, API keys, tokens** - These should never leave your secure environment
- **Personal Identifiable Information (PII)** - Emails, phone numbers, addresses, IDs
- **Proprietary/confidential code** - Company secrets or intellectual property
- **Medical or financial information** - HIPAA, GDPR protected data
- **Private keys and certificates** - SSH keys, SSL certificates, encryption keys

### Dangerous Commands to Review
When OpenCode suggests bash commands, always review for:

| Command Pattern | Risk Level | Why Dangerous |
|----------------|------------|---------------|
| `rm -rf /` | 🔴 Critical | Deletes entire filesystem |
| `curl \| bash` | 🔴 Critical | Executes remote code without verification |
| `curl \| sh` | 🔴 Critical | Same as above, different shell |
| `sudo rm -rf` | 🔴 Critical | Deletes with elevated privileges |
| `> /dev/sda` | 🔴 Critical | Overwrites disk directly |
| `mkfs.ext4 /dev/sdX` | 🔴 Critical | Formats disk |
| `:(){ :\|:& };:` | 🔴 Critical | Fork bomb (denial of service) |
| `chmod -R 777 /` | 🟡 High | Makes everything world-writable |
| `dd if=/dev/zero of=/dev/sda` | 🔴 Critical | Overwrites disk with zeros |
| `mv ~ /dev/null` | 🔴 Critical | Deletes home directory |

## 🛡️ Secure Configuration

### Minimal Secure Setup

```json
{
  "$schema": "https://opencode.ai/config.json",
  "permission": "ask",
  "provider": {
    "local": {
      "npm": "@ai-sdk/ollama",
      "name": "Local Ollama",
      "options": {
        "baseURL": "http://localhost:11434"
      },
      "models": {
        "llama3": {
          "name": "Llama 3",
          "context": 8192
        }
      }
    }
  },
  "agent": {
    "build": {
      "permission": {
        "bash": "ask",
        "edit": "ask",
        "write": "ask",
        "apply_patch": "ask"
      }
    }
  }
}
```

### Recommended Secure Workflow

1. **Start with "permission": "ask" mode**
   ```bash
   opencode
   # Inside OpenCode:
   # Every command will require your confirmation
   ```

2. **Review every proposed command**
   - Read carefully what the command does
   - Check for dangerous patterns listed above
   - Verify paths are correct
   - Ensure no sensitive data exposure

3. **Use Ctrl+C to interrupt if unsure**
   ```bash
   # If a command looks suspicious or you're not sure:
   Press Ctrl+C immediately
   ```

4. **Create backups before major changes**
   ```bash
   # Before major refactoring:
   git commit -m "Backup before OpenCode changes"
   # or
   cp -r myproject myproject.backup.$(date +%Y%m%d)
   ```

## 🔐 Provider Security Comparison

| Provider | Data Location | GDPR Compliant | Data Retention | Recommendation |
|----------|--------------|----------------|----------------|----------------|
| **Local (Ollama)** | Your machine | ✅ Yes | None | 🔒 **Most Secure** |
| **Mistral AI** | France 🇫🇷 | ✅ Yes | Check policy | 🔒 **Recommended** |
| **OVHcloud AI** | France 🇫🇷 | ✅ Yes | Check policy | 🔒 **Recommended** |
| **Aleph Alpha** | Germany 🇩🇪 | ✅ Yes | Check policy | 🔒 **Recommended** |
| **Anthropic** | US 🇺🇸 | ⚠️ No | Yes | ⚠️ Review carefully |
| **OpenAI** | US 🇺🇸 | ⚠️ No | Yes | ⚠️ Review carefully |
| **Google** | US 🇺🇸 | ⚠️ No | Yes | ⚠️ Review carefully |

## 🛠️ Tool-Specific Security

### Bash Tool
```json
{
  "agent": {
    "build": {
      "permission": {
        "bash": "ask"
      }
    }
  }
}
```
**Why**: Prevents accidental execution of dangerous commands

### Edit/Write Tools
```json
{
  "agent": {
    "build": {
      "permission": {
        "edit": "ask",
        "write": "ask"
      }
    }
  }
}
```
**Why**: Prevents unwanted file modifications

### MCP Servers
Before connecting to any MCP server:
1. **Verify the source** - Only use trusted servers
2. **Check permissions** - What data does it access?
3. **Review code** - If open source, audit the implementation
4. **Test in isolation** - Use Docker or VM first

```json
{
  "mcp": {
    "servers": {
      "trusted-server": {
        "url": "https://trusted-source.com/mcp",
        "permissions": ["read-only"]  // Limit permissions
      }
    }
  }
}
```

### Web Tools
```json
{
  "agent": {
    "build": {
      "permission": {
        "webfetch": "ask",
        "websearch": "ask"
      }
    }
  }
}
```
**Why**: Prevents sending sensitive data to search engines

## 🔍 Security Audit Checklist

Before using OpenCode in production:

- [ ] **Configuration Review**
  - [ ] `"permission": "ask"` is set
  - [ ] No hardcoded credentials in config
  - [ ] Provider is GDPR compliant (if required)

- [ ] **MCP Servers Audit**
  - [ ] All servers are from trusted sources
  - [ ] Permissions are minimal necessary
  - [ ] Code reviewed if open source

- [ ] **File Permissions**
  - [ ] `.opencode/` directory has restricted permissions
  - [ ] No world-readable files with sensitive data
  - [ ] SQLite database is secured

- [ ] **Network Security**
  - [ ] HTTPS only for remote providers
  - [ ] VPN used for sensitive operations
  - [ ] No sensitive data in HTTP headers

- [ ] **Data Handling**
  - [ ] No PII in prompts
  - [ ] No secrets in code
  - [ ] Regular session cleanup

## 🚨 Incident Response

### If You Accidentally Sent Sensitive Data

1. **Immediately revoke the credential**
   ```bash
   # For API keys:
   - Log into provider dashboard
   - Revoke/rotate the key immediately
   - Generate new key if needed
   ```

2. **Check access logs**
   ```bash
   # Review provider logs if available
   # Look for unauthorized access
   ```

3. **Document the incident**
   ```bash
   # What was sent?
   # When?
   # To which provider?
   ```

4. **Notify relevant parties**
   - Your security team
   - Affected users (if applicable)
   - Compliance officer (if regulated industry)

## 📚 Additional Resources

- [OpenCode Security Best Practices](https://opencode.ai/security)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [GDPR Compliance Guide](https://gdpr.eu/)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)

## 🆘 Getting Help

If you encounter a security issue:
1. **Immediate**: `Ctrl+C` to stop current operation
2. **Report**: Create issue at https://github.com/linagora/hackathon_infra_ia/issues
3. **Emergency**: Contact your security team

---

**Remember**: Security is everyone's responsibility. When in doubt, ask!

*This guide is part of the OpenCode Starter Kit by LINAGORA*  
*Version 1.0 - March 2026*  
*License: CC BY 4.0*
