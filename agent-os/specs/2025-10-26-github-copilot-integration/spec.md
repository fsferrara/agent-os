# Specification: GitHub Copilot Migration

## Goal
Migrate Agent OS from Claude Code to GitHub Copilot by replacing Claude Code commands with GitHub Copilot prompts, Skills with instructions, and agents with chat modes. This is a complete replacement, removing all Claude Code support and multi-agent functionality.

## User Stories
- As a GitHub Copilot user, I want to use Agent OS workflows via prompts so that I can follow structured development processes
- As a developer, I want to use Agent OS with GitHub Copilot so that I can leverage agentic workflows in my preferred AI coding tool
- As a team lead, I want to install Agent OS with GitHub Copilot in my project so that my team can use standardized development workflows
- As a user, I want standards available as GitHub Copilot instructions so that the AI follows my coding conventions automatically
- As a developer, I want to use chat modes for specialized tasks so that I can get context-specific assistance

## Core Requirements
- Replace Claude Code configuration with GitHub Copilot configuration in `config.yml`
- Remove all multi-agent related code and configurations
- Create installation logic for GitHub Copilot prompts, instructions, and chat modes
- Migrate Claude Code commands to GitHub Copilot prompts
- Migrate Claude Code Skills to GitHub Copilot instructions
- Migrate Claude Code agents to GitHub Copilot chat modes
- Keep single-agent mode only, removing all multi-agent delegation logic
- Support standards injection via instructions or inline modes
- Remove all Claude Code-related directories and files
- Update documentation to reflect GitHub Copilot-only approach

## Visual Design
N/A - This is a backend/tooling feature with no UI

## Reusable Components

### Existing Code to Leverage
- **Scripts**: 
  - `project-install.sh` - Installation logic, configuration parsing, file copying
  - `project-update.sh` - Update logic, file comparison, overwrite handling
  - `common-functions.sh` - Compilation functions, conditional processing, workflow injection, standards replacement
- **Configuration System**: 
  - `config.yml` - YAML configuration format, flag handling
  - Configuration validation functions in `common-functions.sh`
- **Profile Structure**: 
  - `profiles/default/` - Template structure for commands and standards
  - Profile file resolution logic (`get_profile_file`, `get_profile_files`)
- **Compilation Pipeline**:
  - `process_conditionals` - Handle {{IF}}, {{UNLESS}} tags (adapt for new flags)
  - `process_workflows` - Inject workflow content (reuse as-is)
  - `process_standards` - Replace standards placeholders (reuse as-is)
  - `process_phase_tags` - Embed PHASE content for single-agent mode (reuse as-is)
  - `compile_command` - Full command compilation (adapt for prompts)
- **Directory Structure Logic**:
  - Single-agent mode: `agent-os/commands/` with embedded phases
  - Standards: `agent-os/standards/`

### New Components Required
- **GitHub Copilot Directories**:
  - `.github/prompts/agent-os/` - GitHub Copilot prompts (replacing `.claude/commands/`)
  - `.github/instructions/` - GitHub Copilot instructions (replacing `.claude/skills/`)
  - `.github/chat-modes/agent-os/` - GitHub Copilot chat modes (replacing `.claude/agents/`)
  - These use different directory structure than Claude Code
- **Installation Functions**:
  - `install_github_prompts()` - Install prompts from single-agent commands
  - `install_github_instructions()` - Install instructions from standards
  - `install_github_chat_modes()` - Install chat modes from agents
  - Replace all Claude Code installation functions
- **Configuration Variables**:
  - New flags in `config.yml` for GitHub Copilot
  - Remove all Claude Code-related flags
- **Conditional Compilation Updates**:
  - Remove `use_claude_code_subagents` conditional support
  - Remove `standards_as_claude_code_skills` conditional support
  - Add `standards_as_copilot_instructions` conditional support
  - Simplify conditionals since only single-agent mode exists

## Technical Approach

### Configuration Changes
Replace Claude Code flags in `config.yml` with:
```yaml
# GitHub Copilot Configuration
github_copilot_prompts: true                  # Install GitHub Copilot prompts
standards_as_copilot_instructions: true       # Use instructions for standards
agent_os_commands: true                       # Install tool-agnostic commands

# REMOVED (Claude Code - no longer supported):
# claude_code_commands
# use_claude_code_subagents
# standards_as_claude_code_skills
```

### Directory Structure
New GitHub Copilot structure:
- **Prompts**: `.github/prompts/agent-os/` (replaces `.claude/commands/`)
- **Instructions**: `.github/instructions/` (replaces `.claude/skills/`)
- **Chat Modes**: `.github/chat-modes/agent-os/` (replaces `.claude/agents/`)
- **Standards**: Continue using `agent-os/standards/` (shared)
- **Commands**: Continue using `agent-os/commands/` (tool-agnostic)

Old Claude Code directories to remove:
- `.claude/commands/agent-os/`
- `.claude/agents/agent-os/`
- `.claude/skills/`

### Profile Organization
Keep existing structure, remove multi-agent folders:
```
profiles/default/
  commands/
    plan-product/
      single-agent/    # Source for both prompts and agent-os commands
    shape-spec/
      single-agent/
    write-spec/
      single-agent/
    create-tasks/
      single-agent/
    implement-tasks/
      single-agent/
    orchestrate-tasks/
      orchestrate-tasks.md  # Single file, no subfolders
  agents/
    *.md              # Source for chat modes
  standards/
    */                # Source for instructions or inline injection
  claude-code-skill-template.md  # REMOVE - no longer needed
```

### Installation Script Changes
Modify `project-install.sh`:
1. Remove all Claude Code installation functions:
   - Remove `install_claude_code_commands_with_delegation()`
   - Remove `install_claude_code_commands_without_delegation()`
   - Remove `install_claude_code_agents()`
   - Remove `install_claude_code_skills()`
   - Remove `install_improve_skills_command()`
2. Add new GitHub Copilot functions:
   - `install_github_prompts()` - Install from single-agent commands
   - `install_github_instructions()` - Install from standards
   - `install_github_chat_modes()` - Install from agents
3. Remove multi-agent related flags and logic
4. Update configuration parsing for new flags
5. Simplify validation (no multi-agent dependencies)

### Compilation Pipeline Updates
Modify `common-functions.sh`:
1. Update `process_conditionals()`:
   - Remove `use_claude_code_subagents` flag handling
   - Remove `standards_as_claude_code_skills` flag handling  
   - Add `standards_as_copilot_instructions` flag handling
   - Remove `compiled_single_command` parameter (always single-agent now)
2. Reuse existing functions:
   - `process_workflows()` - No changes needed
   - `process_standards()` - No changes needed
   - `process_phase_tags()` - No changes needed (always in "embed" mode)
3. Adapt compilation functions:
   - `compile_command()` - Retarget to `.github/prompts/` when needed
   - Create `compile_instruction()` - Generate instruction files from standards
   - Rename `compile_agent()` to `compile_chat_mode()` - Target `.github/chat-modes/`

### Prompt Generation
Convert single-agent commands to GitHub Copilot prompts:
- Source: `profiles/default/commands/*/single-agent/*.md`
- Target: `.github/prompts/agent-os/*.md`
- Processing:
  - Apply workflow injection (reuse existing)
  - Apply standards replacement with new conditional
  - Apply PHASE embedding (reuse existing)
  - Format for GitHub Copilot prompt syntax

### Instructions Generation  
Convert standards to GitHub Copilot instructions:
- Source: `profiles/default/standards/**/*.md`
- Target: `.github/instructions/*.md`
- Two modes based on `standards_as_copilot_instructions`:
  - **true**: Create instruction files in `.github/instructions/`
  - **false**: Inject standards inline in prompts using existing replacement logic

### Chat Modes Generation
Convert agents to GitHub Copilot chat modes:
- Source: `profiles/default/agents/*.md`
- Target: `.github/chat-modes/agent-os/*.md`
- Processing: Similar to current agent compilation but different output directory

## Out of Scope
- Maintaining Claude Code support (complete removal)
- Supporting multi-agent orchestration (removing entirely)
- Migration tools or scripts from Claude Code to GitHub Copilot
- Supporting other AI tools beyond GitHub Copilot (already supported via `agent_os_commands`)
- Creating new commands or workflows
- Modifying standards content or organization
- Visual tooling or GUI for configuration
- Backward compatibility with Claude Code installations

## Success Criteria
- User can run `project-install.sh --github-copilot-prompts=true` successfully
- GitHub Copilot prompts appear in `.github/prompts/agent-os/`
- All existing commands (plan-product, shape-spec, write-spec, create-tasks, implement-tasks, orchestrate-tasks) work as GitHub Copilot prompts
- GitHub Copilot instructions are created in `.github/instructions/` when `standards_as_copilot_instructions: true`
- GitHub Copilot chat modes appear in `.github/chat-modes/agent-os/`
- Standards are properly injected based on `standards_as_copilot_instructions` setting
- Conditional compilation correctly processes new flags and removes old flags
- All Claude Code-related code is removed from scripts
- All multi-agent code is removed from scripts and profiles
- Configuration validation prevents invalid combinations
- Update script properly handles GitHub Copilot files
- Documentation clearly explains GitHub Copilot installation and usage
- No `.claude/` directories are created
- `claude-code-skill-template.md` is removed from profiles
- All references to `use_claude_code_subagents` are removed
- All references to `standards_as_claude_code_skills` are removed
