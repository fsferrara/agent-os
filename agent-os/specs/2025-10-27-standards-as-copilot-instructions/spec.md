# Specification: Standards as GitHub Copilot Instructions

## Goal
Enable users to install Agent OS standards as GitHub Copilot instructions in the `.github/instructions/` directory, providing an alternative to the Claude Code Skills approach for making standards accessible to AI coding assistants.

## User Stories
- As a developer using GitHub Copilot, I want to have my coding standards automatically available as Copilot instructions so that GitHub Copilot follows my project's conventions
- As a team lead, I want to control whether standards are delivered as instructions or inline references so that I can choose the best approach for my team's workflow
- As a user migrating from Claude Code, I want a similar experience with GitHub Copilot so that my standards are consistently applied across different AI tools

## Core Requirements
- Add `standards_as_copilot_instructions` configuration flag to `config.yml`
- Create `install_github_instructions()` function to install standards as Copilot instructions
- Generate instruction files in `.github/instructions/` directory from standards files
- Maintain existing conditional compilation logic (`{{UNLESS standards_as_copilot_instructions}}`)
- Support command-line flag `--standards-as-copilot-instructions=true/false` for installation overrides
- Preserve existing standards installation in `agent-os/standards/` directory
- Ensure standards can be consumed either as instructions OR inline file references, not both simultaneously

## Visual Design
N/A - This is a backend/scripting feature with no UI components

## Reusable Components

### Existing Code to Leverage
- **Configuration System**: Use existing config.yml parsing and validation from `common-functions.sh` (similar to `standards_as_claude_code_skills`)
- **File Processing**: Leverage `get_profile_files()` and file iteration patterns from `install_standards()` function
- **Directory Creation**: Reuse `ensure_dir()` utility for creating `.github/instructions/` directory
- **Conditional Compilation**: Extend existing `process_conditionals()` function that handles `{{UNLESS standards_as_claude_code_skills}}` tags
- **Standards Installation Pattern**: Model after `install_claude_code_skills()` function structure in `project-install.sh` (lines 1390-1420)
- **Update Script Logic**: Follow patterns from `update_standards()` in `project-update.sh` for handling instruction file updates

### New Components Required
- **`compile_instruction()` Function**: New function to transform standards markdown into GitHub Copilot instruction format
  - Why: GitHub Copilot instructions use a different format than Claude Code Skills (single file vs SKILL.md in directory)
  - Must apply proper frontmatter and formatting for Copilot's instruction syntax
  - Should preserve markdown content from original standards files
  
- **Instruction File Naming Convention**: New logic to convert standards paths to instruction filenames
  - Why: `.github/instructions/` uses flat structure, unlike nested `.claude/skills/` directories
  - Example: `standards/frontend/css.md` → `.github/instructions/frontend-css.md`

## Technical Approach

### Configuration Layer
- Add new config flag `standards_as_copilot_instructions: false` (default) to base `config.yml`
- Update `validate_config()` to accept the new parameter
- Add flag parsing in `project-install.sh` (similar to `--standards-as-claude-code-skills`)
- Update `write_project_config()` to include the new setting

### Compilation Pipeline
- Extend `process_conditionals()` in `common-functions.sh`:
  - Add handling for `standards_as_copilot_instructions` flag alongside existing `standards_as_claude_code_skills`
  - Ensure conditional blocks `{{UNLESS standards_as_copilot_instructions}}` are processed
  
- Create `compile_instruction()` function:
  - Input: standards file path, destination path, base directory, profile
  - Process: Read standards file, apply GitHub Copilot instruction formatting
  - Output: Formatted instruction file in `.github/instructions/`
  - Reference existing `.github/copilot.instructions.md` for proper syntax

### Installation Logic
- Create `install_github_instructions()` function in `project-install.sh`:
  - Only run when `standards_as_copilot_instructions: true` 
  - Source files from `profiles/default/standards/**/*.md`
  - Target directory: `.github/instructions/`
  - Use `compile_instruction()` for each standards file
  - Preserve directory structure in filenames (e.g., `global-coding-style.md`)
  
- Integrate into main installation flow (around line 455 in `project-install.sh`):
  - Call after `install_standards()` 
  - Conditional on flag being enabled
  - Similar placement to `install_claude_code_skills()`

### Update Script Support
- Add instruction file handling to `project-update.sh`:
  - Check for existing `.github/instructions/` directory
  - Compare instruction files with source standards
  - Handle overwrite prompts similar to standards updates
  - Remove instruction files if flag is disabled

## Out of Scope
- Converting commands to GitHub Copilot prompts (covered in roadmap item #12)
- Removing Claude Code support (that was the previous spec)
- Multi-agent orchestration with GitHub Copilot
- Automatic instruction file optimization or consolidation
- GitHub Copilot workspace extensions or plugins
- Testing or validation of Copilot instruction effectiveness
- Migration scripts from Claude Code Skills to Copilot instructions

## Success Criteria
- User can set `standards_as_copilot_instructions: true` in `config.yml`
- Running `./scripts/project-install.sh` creates instruction files in `.github/instructions/`
- Each standards file generates one corresponding instruction file with proper naming
- Conditional compilation removes inline standards when flag is true
- Flag can be overridden via `--standards-as-copilot-instructions=true/false` command line argument
- Update script properly handles adding/updating/removing instruction files
- Documentation clearly explains when to use instructions vs inline standards
- No conflicts between `standards_as_claude_code_skills` and `standards_as_copilot_instructions` flags
- Existing installations continue working without changes
