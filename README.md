<img width="1280" height="640" alt="agent-os-og" src="https://github.com/user-attachments/assets/f70671a2-66e8-4c80-8998-d4318af55d10" />

## Your system for spec-driven agentic development.

[Agent OS](https://buildermethods.com/agent-os) transforms AI coding agents from confused interns into productive developers. With structured workflows that capture your standards, your stack, and the unique details of your codebase, Agent OS gives your agents the specs they need to ship quality code on the first try—not the fifth.

Use it with:

✅ Claude Code, Cursor, or any other AI coding tool.

✅ New products or established codebases.

✅ Big features, small fixes, or anything in between.

✅ Any language or framework.

---

### Documentation & Installation

Docs, installation, usage, & best practices 👉 [It's all here](https://buildermethods.com/agent-os)

---

### Configuration Options

#### GitHub Copilot Instructions

Agent OS can install your coding standards as [GitHub Copilot instructions](https://docs.github.com/en/copilot/customizing-copilot/adding-custom-instructions-for-github-copilot), making them available to GitHub Copilot when you code.

Enable this feature during installation:

```bash
./scripts/project-install.sh --standards-as-copilot-instructions=true
```

Or configure it in your `agent-os/config.yml`:

```yaml
standards_as_copilot_instructions: true
```

When enabled, Agent OS will:
- Create instruction files in `.github/instructions/` directory
- Convert each standards file into a properly formatted Copilot instruction
- Name files using the pattern: `standards/frontend/css.md` → `frontend-css.md`

**When to use Copilot Instructions:**
- You want GitHub Copilot to reference your standards during coding
- You prefer having standards available to Copilot without manual prompting
- You're using GitHub Copilot as your primary AI coding assistant

**Relationship with Claude Code Skills:**
- GitHub Copilot instructions (`standards_as_copilot_instructions`) and Claude Code Skills (`standards_as_claude_code_skills`) are independent features
- They can be enabled simultaneously or separately
- Choose based on which AI coding tools you primarily use

---

### Follow updates & releases

Read the [changelog](CHANGELOG.md)

[Subscribe to be notified of major new releases of Agent OS](https://buildermethods.com/agent-os)

---

### Created by Brian Casel @ Builder Methods

Created by Brian Casel, the creator of [Builder Methods](https://buildermethods.com), where Brian helps professional software developers and teams build with AI.

Get Brian's free resources on building with AI:
- [Builder Briefing newsletter](https://buildermethods.com)
- [YouTube](https://youtube.com/@briancasel)

Join [Builder Methods Pro](https://buildermethods.com/pro) for official support and connect with our community of AI-first builders:
