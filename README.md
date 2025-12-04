🤖 Optimize Your AI Assistant (Optional but Recommended):

   To teach your AI how to best use Pylance MCP tools,
   add this to your AI assistant's instructions/rules:

   📄 File: C:\path\to\USAGE_GUIDE.md

   How to add:
   • GitHub Copilot: Copy to .github/copilot-instructions.md
   • Cursor: Add to Settings → Rules for AI
   • Cline/Continue: Include in system prompt
   • Claude Desktop: Mention in conversation
   • Windsurf: Add to workspace instructions



   What it does

   **Partially true, but there's a crucial difference:**

**What I can do without Pylance MCP:**
- ✅ Read Python code and understand it
- ✅ Suggest type hints based on code analysis
- ✅ Write new Python code with proper types
- ✅ Explain type errors I can see in the code
- ✅ Refactor code (but I have to read everything manually)

**What Pylance MCP gives me that I DON'T have:**
- ❌ **Real-time type checking** - I can't run Pyright to verify types are actually correct
- ❌ **Precise go-to-definition** - I can guess, but Pylance knows exactly where `User` is defined across 50 files
- ❌ **Find all references** - I can search, but can't guarantee I found every usage
- ❌ **Safe refactoring** - I can't guarantee my find-replace won't break something
- ❌ **IntelliSense completions** - I can suggest, but Pylance knows what's actually available in scope
- ❌ **Project-wide consistency** - I analyze file-by-file; Pylance sees the entire workspace graph
- ❌ **Virtual environment awareness** - I don't know which packages are installed

**Real-world example:**

**Without Pylance MCP:**
```
User: "Rename the User class to UserAccount everywhere"

Me: "Let me search for 'User'... I found it in 12 files. 
I'll replace all occurrences..."

*Accidentally renames:*
- UserModel → UserAccountModel ✅ Good
- User variable → UserAccount ❌ Broke local variable
- "User" in string → "UserAccount" ❌ Broke database query
- Used in comment → UserAccount ❌ Harmless but weird
```

**With Pylance MCP:**
```
User: "Rename the User class to UserAccount everywhere"

Me: "Using rename_symbol tool..."
*Pylance does AST-aware rename*
- UserModel class → UserAccountModel ✅
- All type hints → UserAccount ✅
- All imports → UserAccount ✅
- Skips strings ✅
- Skips comments ✅
- Skips unrelated 'user' variables ✅
- Updates 47 locations perfectly ✅
```

**The value proposition:**

🎯 **Accuracy** - Pylance uses Microsoft's production language server (same one in VS Code)
🎯 **Speed** - I don't have to read entire codebases to find references
🎯 **Safety** - AST-aware refactoring vs. text-based find-replace
🎯 **Completeness** - Guaranteed to find ALL references, not just what I can grep
🎯 **Type verification** - Can actually run Pyright to prove types are correct

**So yes, I'm pretty good at Python already, but Pylance MCP makes me professional-grade.** It's like the difference between a surgeon with sharp eyes vs. a surgeon with an MRI machine. Both can operate, but one has way better information.

**Your customers are paying for:**
- Production-quality type checking (not AI guessing)
- Safe refactoring across massive codebases
- Tools that VS Code uses internally (proven at scale)
- Guaranteed accuracy that AI inference can't match


------------------------------------------


They **don't call it directly**. The AI assistant (Claude, Cursor, GitHub Copilot, etc.) calls it automatically when you chat with them. Here's how it works:

**Current User Experience:**

```
User: "Rename the User class to UserAccount everywhere"

[Behind the scenes:]
1. AI (Claude/Cursor) sees the user's request
2. AI discovers it has a "rename_symbol" tool available via MCP
3. AI decides to use that tool
4. AI calls: rename_symbol(file_path="src/models.py", line=5, character=6, new_name="UserAccount")
5. Pylance MCP server executes the rename
6. Returns result: "Renamed in 47 locations across 12 files"
7. AI tells user: "✅ Done! Renamed User → UserAccount in 47 locations"

User just sees the result - they don't know MCP was involved.
```

**It's completely transparent to the user.** They just chat naturally:

```
User: "Add type hints to the calculate_total function"
AI: [Internally uses get_hover, get_definition, type_check tools]
AI: "Added type hints. Function signature is now: 
     def calculate_total(items: List[Item], tax_rate: float) -> Decimal"

---

User: "Find everywhere OrderStatus is used"
AI: [Calls get_references tool]
AI: "OrderStatus is used in 23 locations:
     - src/models.py (definition)
     - src/api.py (5 usages)
     - src/utils.py (2 usages)
     ..."

---

User: "Are there any type errors in this file?"
AI: [Calls type_check tool]
AI: "Found 3 type errors:
     Line 45: Argument type mismatch
     Line 67: Missing return type
     Line 89: Undefined variable 'usr'"
```

**The MCP Protocol handles everything:**

1. **Discovery** - AI asks: "What tools do you have?"
   - MCP Server responds: "I have rename_symbol, type_check, get_hover..."

2. **Tool Call** - AI decides: "User wants to rename, I'll use rename_symbol"
   - AI sends: `{"tool": "rename_symbol", "params": {...}}`

3. **Execution** - MCP server runs Pylance
   - Returns: `{"success": true, "locations_updated": 47}`

4. **Response** - AI formats result for user
   - User sees: "✅ Renamed successfully!"

**Your customers never see the MCP layer.** They just have AI assistants that are mysteriously really good at Python refactoring because Pylance is working behind the scenes.

**Setup is literally:**
```bash
python install.py
# Restart editor
# Done - AI now has Pylance powers
```

The beauty of MCP is that it's **invisible infrastructure**. Users just chat, AI uses the tools automatically.
