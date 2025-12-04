# Python-Pylance-Pyright-MCP-Server

Pylance MCP+ extends Microsoft's Pylance language server with professional-grade tooling for Python development. Get everything Microsoft offers PLUS ultra-fast Ruff linting, Bandit security scanning, Radon complexity analysis, and more. Perfect for professional developers who need enterprise-quality code analysis.

Free tier includes all core Pylance features. Professional tier ($19/mo) adds security scanning, linting, and code quality tools.

We do not collect data. 

## How AI Uses Pylance MCP+

### What is MCP (Model Context Protocol)?
MCP is a protocol that allows AI assistants (Claude, Cursor, GitHub Copilot, etc.) to call external tools. When you chat with an AI assistant that has Pylance MCP+ installed, the AI can automatically use Python language server capabilities without you explicitly asking.

### Example Conversations

**User:** "Rename the User class to UserAccount everywhere"

**Behind the scenes:**
1. AI sees your request
2. AI discovers it has a `rename_symbol` tool available via MCP
3. AI calls: `rename_symbol(file_path="src/models.py", line=5, character=6, new_name="UserAccount")`
4. Pylance MCP+ executes the rename across 47 locations
5. AI responds: "✅ Done! Renamed User → UserAccount in 47 locations across 12 files"

**You never see the MCP layer - it's completely transparent.**

---

**User:** "Are there any security issues in this file?"

**Behind the scenes:**
1. AI calls: `security_scan(file_path="src/api.py")`
2. Bandit scanner detects hardcoded password on line 23
3. AI responds: "⚠️ Found 1 HIGH severity issue: Hardcoded password on line 23. Recommendation: Use environment variables or a secrets manager."

---

### How AI Learns to Use Your Tools

AI assistants discover MCP tools through **tool schemas**. When Pylance MCP+ connects, it tells the AI:

```json
{
  "tool": "security_scan",
  "description": "Scan Python code for security vulnerabilities using Bandit",
  "parameters": {
    "file_path": "Path to Python file to scan",
    "content": "Optional: Code content if not saved to file"
  }
}
```

The AI reads these schemas and learns:
- **What** tools are available
- **When** to use them (based on description)
- **How** to call them (parameters required)

**You can enhance AI understanding by adding USAGE_GUIDE.md to your AI's instructions** (see below).

---

## Training Data Collection (Professional/AI-Enhanced Tiers)

### What Gets Collected
When you use Pylance MCP+ with AI assistants, we can collect anonymized training data:

**Logged automatically:**
- User prompts: "Rename this function", "Check for type errors"
- AI responses: "Renamed in 15 locations", "Found 3 type errors"
- Tool calls: Which MCP tools the AI used and why
- Success/failure rates: Did the AI choose the right tool?

**What we DON'T collect:**
- Your source code
- File contents
- Proprietary information
- Personal data

### Why Collect Training Data?

**1. Improve AI-MCP Integration**
- See which prompts work best
- Identify when AI chooses wrong tools
- Optimize tool descriptions for better AI understanding

**2. Build Custom Models (AI-Enhanced Tier)**
- Train models specific to your team's coding patterns
- AI learns your codebase terminology
- Personalized suggestions based on your history

**3. Aggregate Insights (Enterprise Tier)**
- Team-wide code quality trends
- Most common type errors
- Security vulnerabilities by project type

### How to Use Training Data

**Export your data:**
```bash
pylance-mcp export-training-data --format jsonl --output my_training_data.jsonl
```

**What you get:**
```jsonl
{"timestamp": "2025-12-03T10:30:00Z", "user_prompt": "Check for type errors", "tool_called": "type_check", "success": true}
{"timestamp": "2025-12-03T10:31:15Z", "user_prompt": "Scan for security issues", "tool_called": "security_scan", "result": "Found 2 issues"}
```

**Use cases:**
- Fine-tune your own LLM on Python development workflows
- Analyze team productivity patterns
- Generate reports on code quality improvements
- Train custom coding assistants for your company

### Privacy & Control

✅ **Opt-in only:** Training data collection is OFF by default  
✅ **Anonymized:** No source code or sensitive data collected  
✅ **You own it:** Export anytime, delete anytime  
✅ **Transparent:** See exactly what's logged before it's sent  
✅ **Compliant:** GDPR, CCPA, SOC 2 ready (Enterprise tier)

**To enable:**
```json
{
  "mcpServers": {
    "pylance-mcp": {
      "args": ["--api-key", "YOUR_KEY", "--enable-training-data"]
    }
  }
}
```

---

## Repository Topics/Tags
```
python
language-server
mcp
model-context-protocol
pylance
pyright
type-checking
linting
security-scanning
code-quality
ruff
bandit
radon
ai-assistant
claude
cursor
vscode
```
