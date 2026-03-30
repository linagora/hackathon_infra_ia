# Privacy Guide

## 🌍 Data Privacy Overview

When using OpenCode, your code, prompts, and conversations are sent to configured LLM providers. Understanding what data goes where is crucial for maintaining privacy and compliance.

## 📊 Data Sent to LLM Providers

### What Gets Sent

| Data Type | Sent to Provider | Risk Level | Mitigation |
|-----------|-----------------|------------|------------|
| **Code files** | ✅ Yes | 🟡 Medium | Use local provider for sensitive code |
| **File contents** | ✅ Yes | 🟡 Medium | Review before sending |
| **Prompts/questions** | ✅ Yes | 🟢 Low | Avoid including PII |
| **Error messages** | ✅ Yes | 🟡 Medium | May contain paths/info |
| **Git history** | ⚠️ Sometimes | 🟡 Medium | Check what's exposed |
| **Configuration** | ❌ No | 🟢 Low | Stored locally |
| **Session history** | ❌ No | 🟢 Low | SQLite local database |

### Provider-Specific Data Handling

#### 🇫🇷 European Providers (GDPR Compliant)

| Provider | Location | Data Retention | GDPR | HIPAA | SOC2 |
|----------|----------|----------------|------|-------|------|
| **Mistral AI** | France | Check policy | ✅ Yes | ⚠️ No | ✅ Yes |
| **OVHcloud AI** | France | Check policy | ✅ Yes | ⚠️ No | ✅ Yes |
| **Aleph Alpha** | Germany | Check policy | ✅ Yes | ⚠️ No | ✅ Yes |

**Advantages:**
- GDPR compliance guaranteed
- Data stays in EU jurisdiction
- Stronger privacy protections

#### 🇺🇸 US-Based Providers

| Provider | Location | Data Retention | GDPR | HIPAA | SOC2 |
|----------|----------|----------------|------|-------|------|
| **Anthropic** | US | 30 days default | ⚠️ DPF* | ⚠️ BAA required | ✅ Yes |
| **OpenAI** | US | Check policy | ⚠️ DPF* | ⚠️ BAA required | ✅ Yes |
| **Google** | US | Check policy | ⚠️ DPF* | ⚠️ BAA required | ✅ Yes |

*DPF = Data Privacy Framework (EU-US adequacy decision)

**Considerations:**
- Data subject to US laws (CLOUD Act)
- May be used for model training (opt-out available)
- Cross-border data transfer

#### 🏠 Local Providers (Most Private)

| Provider | Location | Data Retention | Best For |
|----------|----------|----------------|----------|
| **Ollama** | Your machine | None | 🔒 Most sensitive code |
| **vLLM** | Your server | None | 🔒 Enterprise use |
| **TGI** | Your server | None | 🔒 High-security environments |

**Advantages:**
- ✅ No data leaves your infrastructure
- ✅ Complete control
- ✅ No network dependency
- ✅ Zero latency

**Requirements:**
- Sufficient hardware (GPU recommended)
- Technical setup knowledge
- Model downloads and updates

## 🔒 Privacy by Design Configuration

### Tier 1: Maximum Privacy (Sensitive Code)

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
        "codellama": {
          "name": "Code Llama",
          "context": 16384
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
        "webfetch": "deny",
        "websearch": "deny"
      }
    }
  }
}
```

**Use Case:**
- Proprietary company code
- Financial applications
- Healthcare software
- Government projects

### Tier 2: GDPR Compliance (European Projects)

```json
{
  "$schema": "https://opencode.ai/config.json",
  "permission": "ask",
  "provider": {
    "mistral": {
      "name": "Mistral AI",
      "options": {
        "baseURL": "https://api.mistral.ai/v1",
        "apiKey": "${MISTRAL_API_KEY}"
      },
      "models": {
        "mistral-large": {
          "name": "Mistral Large",
          "context": 32768
        }
      }
    }
  },
  "agent": {
    "build": {
      "permission": {
        "bash": "ask",
        "edit": "ask"
      }
    }
  }
}
```

**Use Case:**
- EU-based companies
- GDPR compliance required
- European customer data

### Tier 3: Standard Use (Open Source / Public Code)

```json
{
  "$schema": "https://opencode.ai/config.json",
  "permission": "auto",
  "provider": {
    "anthropic": {
      "name": "Anthropic",
      "options": {
        "apiKey": "${ANTHROPIC_API_KEY}"
      },
      "models": {
        "claude-3-opus": {
          "name": "Claude 3 Opus",
          "context": 200000
        }
      }
    }
  }
}
```

**Use Case:**
- Open source projects
- Public repositories
- Non-sensitive code
- Learning/experimentation

## 🚫 Data Never to Send

### 🔴 Critical - Never Include in Prompts

```bash
# ❌ WRONG - Never do this
"Fix this code: api_key = 'sk-1234567890abcdef'"
"Help me debug: password = 'MyS3cr3tP@ss'"
"Review: const dbPassword = 'production_db_pass'"

# ✅ CORRECT - Remove secrets first
"Fix this code: api_key = process.env.API_KEY"
"Help me debug: [password removed]"
"Review: const dbPassword = config.db.password"
```

### Personal Data (PII)
- Names, emails, phone numbers
- Addresses, postal codes
- ID numbers (SSN, passport, driver's license)
- Financial information (credit cards, bank accounts)
- Health information (medical records, conditions)

### Company Secrets
- API keys and tokens
- Database credentials
- Internal IP addresses and infrastructure details
- Proprietary algorithms
- Unreleased product features
- Security vulnerabilities

## 🔍 Privacy Checklist

Before each session, verify:

- [ ] **No secrets in code**
  - [ ] API keys removed
  - [ ] Passwords externalized
  - [ ] Tokens in environment variables

- [ ] **No PII in prompts**
  - [ ] Real names replaced
  - [ ] Emails anonymized
  - [ ] Phone numbers removed

- [ ] **Provider selection**
  - [ ] Appropriate for data sensitivity
  - [ ] GDPR compliant if needed
  - [ ] Local provider for sensitive code

- [ ] **Session cleanup**
  - [ ] Old sessions deleted
  - [ ] SQLite database secured
  - [ ] No sensitive data in history

## 📋 Data Retention Policies

### Understanding Provider Policies

| Provider | Retention Period | Training Use | Opt-Out |
|----------|-----------------|--------------|---------|
| **Ollama** | N/A (local) | ❌ No | N/A |
| **Mistral** | Check ToS | ⚠️ Possible | ✅ Yes |
| **Anthropic** | 30 days | ⚠️ Possible | ✅ Yes |
| **OpenAI** | Check ToS | ⚠️ Possible | ✅ Yes |

### How to Opt-Out

```json
{
  "provider": {
    "openai": {
      "options": {
        "apiKey": "${OPENAI_API_KEY}",
        "headers": {
          "OpenAI-Beta": "assistants=v2",
          "User-Agent": "OpenCode/1.0"
        }
      }
    }
  }
}
```

For enterprise/teams:
- Contact provider for data processing agreements
- Enable zero data retention (if available)
- Use dedicated instances (Enterprise plans)

## 🛡️ Privacy Best Practices

### 1. Environment Variables for Secrets

```bash
# .env file (never commit this!)
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
MISTRAL_API_KEY=...-

# .gitignore
.env
.opencode/
```

### 2. Prompt Sanitization

```bash
# Before asking OpenCode:
# 1. Remove all secrets
# 2. Anonymize data
# 3. Replace real values with placeholders

# ❌ BAD
"Debug: User john.doe@company.com failed auth with password 'Secret123'"

# ✅ GOOD
"Debug: User [REDACTED]@company.com failed auth with password [REDACTED]"
```

### 3. Regular Session Cleanup

```bash
# Compact sessions regularly
# Inside OpenCode:
/compact

# Or manually clean old sessions:
rm ~/.opencode/sessions/*.db
```

### 4. Use .opencodeignore

```bash
# .opencodeignore
.env
*.key
*.pem
secrets/
config/production.yml
```

## 🌍 Regional Compliance

### GDPR (Europe)
- ✅ Use Mistral, OVHcloud, or Aleph Alpha
- ✅ Data stays in EU
- ✅ Right to erasure supported
- ⚠️ Avoid US providers without DPA

### CCPA (California)
- ⚠️ Check provider's CCPA compliance
- ✅ Right to know what data is collected
- ✅ Right to delete personal information

### HIPAA (Healthcare US)
- ⚠️ BAA required with provider
- ⚠️ Use local provider or dedicated instance
- ❌ Standard APIs not HIPAA compliant

### PCI DSS (Payment Cards)
- ❌ Never send card numbers
- ❌ Never send payment data
- ✅ Use tokenization

## 🆘 Privacy Incident Response

### If You Accidentally Sent PII

1. **Document what was sent**
   ```bash
   # Record:
   # - What data was exposed
   # - Which provider received it
   # - When it happened
   ```

2. **Contact provider immediately**
   - Request data deletion
   - File incident report
   - Get confirmation

3. **Assess impact**
   - Who is affected?
   - What is the risk level?
   - Regulatory obligations?

4. **Notify affected parties**
   - Internal stakeholders
   - Affected users (if applicable)
   - Regulatory authorities (if required)

5. **Prevent recurrence**
   - Update procedures
   - Train team members
   - Implement technical controls

## 📚 Additional Resources

- [GDPR Official Text](https://gdpr.eu/)
- [CCPA Overview](https://oag.ca.gov/privacy/ccpa)
- [Privacy International](https://privacyinternational.org/)
- [EFF Privacy Guide](https://www.eff.org/issues/privacy)

## 🆘 Getting Help

Privacy questions or concerns?
- **LINAGORA Team**: https://www.linagora.com/contact
- **OpenCode Issues**: https://github.com/linagora/hackathon_infra_ia/issues
- **Privacy Officer**: Your organization's DPO

---

**Remember**: Privacy is not just compliance—it's respect for your users and yourself.

*This guide is part of the OpenCode Starter Kit by LINAGORA*  
*Version 1.0 - March 2026*  
*License: CC BY 4.0*
