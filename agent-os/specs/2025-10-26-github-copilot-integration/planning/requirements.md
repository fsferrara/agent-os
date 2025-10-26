# Requirements: GitHub Copilot Migration

## Overview
Complete migration from Claude Code to GitHub Copilot by replacing all Claude Code-specific functionality. This includes removing multi-agent support and replacing Claude Code commands, Skills, and agents with GitHub Copilot prompts, instructions, and chat modes.

## Current State Analysis

Agent OS currently supports:
- Claude Code commands in `.claude/commands/agent-os/` → **REMOVE**
- Claude Code agents in `.claude/agents/agent-os/` → **REMOVE**
- Claude Code Skills for standards in `.claude/skills/` → **REMOVE**
- Multi-agent orchestration with delegation → **REMOVE**
- Single-agent workflow with embedded phases → **KEEP**
- Conditional compilation based on tool preferences → **UPDATE**

Configuration options in `config.yml` to remove:
- `claude_code_commands`: Install Claude Code commands → **DELETE**
- `use_claude_code_subagents`: Use multi-agent delegation → **DELETE**
- `standards_as_claude_code_skills`: Use Skills feature for standards → **DELETE**

Configuration option to keep:
- `agent_os_commands`: Install tool-agnostic commands → **KEEP**

## User Requirements

### 1. GitHub Copilot as Primary Tool
- Replace all Claude Code functionality with GitHub Copilot equivalents
- Implement prompts (replacing commands)
- Implement instructions (replacing Skills)
- Implement chat modes (replacing agents)
- Use `.github/` directories instead of `.claude/`

### 2. Remove Multi-Agent Support
- Delete all multi-agent delegation code
- Keep only single-agent workflow
- Remove all multi-agent profile folders
- Simplify command structure (no delegation logic)

### 3. Remove Claude Code Support
- Delete all Claude Code installation functions
- Remove all Claude Code configuration flags
- Clean up all Claude Code conditionals
- Remove `.claude/` directory creation
- No backward compatibility needed

### 4. Configuration Simplification
- Replace Claude Code flags with GitHub Copilot flags
- Remove multi-agent related flags
- Keep simple, single-agent configuration
- Update validation logic

### 5. Installation Script Overhaul
- Remove all Claude Code installation logic from `project-install.sh`
- Remove all Claude Code update logic from `project-update.sh`
- Remove multi-agent logic from both scripts
- Add GitHub Copilot installation logic
- Reuse compilation pipeline where possible

### 6. Documentation Updates
- Update README to reflect GitHub Copilot only
- Update CHANGELOG with breaking changes
- Remove all Claude Code references
- Document new directory structure

## Technical Requirements

### Configuration Changes
Replace in `config.yml`:
```yaml
# NEW GitHub Copilot Configuration
github_copilot_prompts: true
standards_as_copilot_instructions: true
agent_os_commands: true

# REMOVED (no longer supported)
# claude_code_commands
# use_claude_code_subagents  
# standards_as_claude_code_skills
```

### Directory Structure
New GitHub Copilot structure:
- Prompts: `.github/prompts/agent-os/` (instead of `.claude/commands/`)
- Instructions: `.github/instructions/` (instead of `.claude/skills/`)
- Chat modes: `.github/chat-modes/agent-os/` (instead of `.claude/agents/`)
- Standards: `agent-os/standards/` (unchanged)
- Commands: `agent-os/commands/` (unchanged)

### Profile Structure Cleanup
Remove from `profiles/default/`:
- `commands/*/multi-agent/` folders → **DELETE ALL**
- `claude-code-skill-template.md` → **DELETE**

Keep:
- `commands/*/single-agent/` folders → **KEEP** (source for prompts)
- `agents/*.md` → **KEEP** (source for chat modes)
- `standards/` → **KEEP** (source for instructions or inline)

### Script Modifications
Delete from scripts:
- All `install_claude_code_*` functions
- All `use_claude_code_subagents` conditional logic
- All `standards_as_claude_code_skills` conditional logic
- All multi-agent delegation logic

Add to scripts:
- `install_github_prompts()` function
- `install_github_instructions()` function
- `install_github_chat_modes()` function
- New conditional support for `standards_as_copilot_instructions`

Reuse from scripts:
- `process_workflows()` - No changes needed
- `process_standards()` - No changes needed
- `process_phase_tags()` - No changes needed (always embed mode)
- File copying and validation logic

## Out of Scope
- Maintaining any Claude Code support
- Supporting multi-agent workflows
- Providing migration tools or scripts
- Creating backward compatibility layer
- Supporting tools other than GitHub Copilot (via agent_os_commands)
- Changing workflow logic or standards content

## Success Criteria
- All Claude Code references removed from codebase
- All multi-agent code removed from codebase
- GitHub Copilot prompts successfully installed
- GitHub Copilot instructions successfully created
- GitHub Copilot chat modes successfully installed
- All existing workflows work with GitHub Copilot
- Configuration is simplified and clear
- Documentation reflects GitHub Copilot-only approach
- Installation and update scripts work correctly
- No `.claude/` directories created
- Conditional compilation works with new flags

## Constraints
- Must maintain the 3-layer architecture (standards, product, specs)
- Must preserve existing workflow content and logic
- Must keep single-agent embedded PHASE pattern
- Installation scripts must remain shell-based
- Profile system must continue to work
- Tool-agnostic commands (`agent_os_commands`) must still work
