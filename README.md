# openclaw-configs

OpenClaw configuration files and audit trail. **Version control for gateway tokens, channel configs, and deployment settings.**

## 🃄 Repository Structure

```
openclaw-configs/
├── .github/workflows/
│   └── audit.yml                 # Automated config audit workflow
├── configs/
│   ├── gateway.yaml             # OpenClaw gateway configuration
│   ├── telegram.yaml            # Telegram bot integration
│   ├── cost-optimization.yaml   # Cost optimization settings
│   ├── .env.example             # Environment variables template
│   └── .env.production          # Production-specific settings (secrets)
├── docs/
│   ├── VERSIONING.md           # Git versioning guide
│   ├── AUDIT.md                # Audit trail documentation
│   └── SECURITY.md             # Security best practices
├── README.md
└── .gitignore
```

## 🔐 GitHub Secrets Configuration

Store sensitive tokens and keys in GitHub Secrets:

```
OPENCLAW_GATEWAY_TOKEN   - Gateway authentication token
TELEGRAM_BOT_TOKEN       - Telegram bot token
BYTEROVER_API_KEY        - ByteRover persistent memory API key
TS_AUTHKEY               - Tailscale VPN auth key
```

**NEVER commit secrets to the repository.** Use environment variable substitution (`${VAR_NAME}`) in YAML files instead.

## 🔁 Automatic Audit Workflow

Every configuration change triggers an automated audit:

1. **Sensitive Data Detection** - Scans for exposed tokens, passwords
2. **Audit Log Generation** - Git history with detailed change tracking
3. **Issue Creation** - Creates GitHub issue for manual review
4. **Version Tracking** - Maintains full audit trail

### Workflow Triggers
- `configs/**` files modified
- `.env*` files modified  
- `clawdbot.json` modified
- Scheduled daily at 4:00 AM UTC

## 📋 Configuration Files

### gateway.yaml
OpenClaw gateway status, health checks, and feature configuration.

```yaml
gateway:
  environment: production
  region: eu-central-1
  status: connected
```

### telegram.yaml
Telegram bot integration, polling mode, and connection details.

```yaml
telegram:
  bot_name: OpenClaw-ChatBot
  configured: true
  running: false
```

### cost-optimization.yaml
97% cost savings configuration based on Matt Ganzac's guide:

- **Heartbeat Optimization** (80% savings) - Local-only checks
- **Multi-Model Distribution** (10% savings) - Use cheaper models
- **Session Compression** (5% savings) - Reduce token bloat
- **Prompt Caching** (2% savings) - Cache expensive contexts

## 🔗 Versioning Guide

All configuration changes are automatically versioned in Git:

### Making Configuration Changes

1. **Clone repository**
   ```bash
   git clone https://github.com/egoriklok/openclaw-configs.git
   cd openclaw-configs
   ```

2. **Edit configuration files**
   ```bash
   # Edit any YAML in configs/ folder
   vim configs/gateway.yaml
   ```

3. **Create descriptive commit**
   ```bash
   git add configs/gateway.yaml
   git commit -m "Update gateway heartbeat configuration"
   git push origin main
   ```

4. **Automatic audit triggers**
   - GitHub Actions workflow runs
   - Scans for sensitive data
   - Generates audit log
   - Creates GitHub issue for review

### Viewing Audit Trail

```bash
# View all commits
git log --oneline --all

# View detailed changes
git log --format="%h %aD %an: %s" --name-status

# View specific file history
git log --follow -p configs/gateway.yaml

# Show differences between versions
git diff v1.0.0 v1.0.1
```

## ✅ Security Best Practices

### DO
- ✅ Use GitHub Secrets for all sensitive tokens
- ✅ Include `.env.production` in `.gitignore`
- ✅ Review GitHub Actions audit logs
- ✅ Use signed commits for production changes
- ✅ Tag releases with version numbers

### DON'T
- ❌ Don't commit `.env` files with real tokens
- ❌ Don't hardcode API keys in YAML
- ❌ Don't push to main without review
- ❌ Don't delete audit history

## 📊 Audit Trail Access

### GitHub Web Interface
1. Go to repository `Code` tab
2. View commit history with file changes
3. Click on commits to see diffs
4. Check "Issues" tab for audit reports

### Command Line
```bash
# View who changed what and when
git log --format="fuller" --all

# See all commits that touched configs
git log -- "configs/"

# Find commits by specific person
git log --author="egoriklok"
```

## 🚀 Deployment Integration

Configuration updates are automatically deployed via GitHub Actions:

1. **Push** configuration changes to main branch
2. **Audit workflow** validates changes
3. **GitHub issue** created for review
4. **Manual approval** (optional branch protection)
5. **Deployment** to CloudRun via linked pipeline

## 📋 Documentation

- `docs/VERSIONING.md` - Detailed versioning workflow
- `docs/AUDIT.md` - Understanding audit logs
- `docs/SECURITY.md` - Security policies and procedures

## 🔓 Secrets Management

### Add a New Secret

1. Go to GitHub repository Settings
2. Click "Secrets and variables" → "Actions"
3. Click "New repository secret"
4. Name: `OPENCLAW_GATEWAY_TOKEN`
5. Value: Paste the token
6. Click "Add secret"

### Rotate a Secret

1. Generate new token in OpenClaw Control UI
2. Update GitHub Secret with new value
3. Restart CloudRun container
4. Verify connectivity in logs
5. Document change in AUDIT.md

## 📆 Last Updated

- **Version**: 2026.2.3
- **Updated**: February 6, 2026
- **Region**: EU (eu-central-1)
- **Status**: Production

## 🔍 Support & Troubleshooting

### Config Validation Failed
- Check YAML syntax: `yamllint configs/`
- Verify no secrets in files
- Review GitHub Actions audit logs

### Deployment Not Updating
- Verify changes pushed to main branch
- Check GitHub Actions workflow status
- Confirm CloudRun container restarted

## 📝 License

Configuration management system for OpenClaw AI gateway.
Keep this repository private for security.
