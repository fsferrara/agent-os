# Tech Stack

## AI Coding Assistants

### Current
- **Claude Code** - Primary AI coding assistant with specialized agents and workflows

### Migration Target
- **GitHub Copilot** - Target platform for AI coding assistance
  - Chat participants for specialized agents
  - Slash commands for workflows
  - VS Code integration

## Framework & Architecture

- **Spec-Driven Development** - Structured approach with 3-layer context system
- **Multi-Agent System** - Specialized agents for different development phases:
  - product-planner
  - spec-initializer
  - spec-writer
  - spec-shaper
  - tasks-list-creator
  - implementer
  - implementation-verifier

## File System & Organization

- **Markdown-Based Documentation** - All specs, standards, and product docs in `.md` format
- **Dated Spec Folders** - Format: `YYYY-MM-DD-[name]` for chronological organization
- **Profile System** - Reusable configurations in `profiles/` directory
- **Standards Library** - Organized by domain (global, backend, frontend, testing)

## Development Tools

- **Shell Scripts** - Bash/Zsh scripts for installation and setup
  - `base-install.sh`
  - `project-install.sh`
  - `project-update.sh`
  - `create-profile.sh`
- **Git** - Version control for tracking changes across specs and implementations
- **VS Code** - Primary development environment (target for GitHub Copilot)

## Documentation System

- **Structured Markdown** - Templates for mission, roadmap, specs, tasks
- **Checkbox-Based Tasks** - `- [ ]` format for trackable implementation items
- **Acceptance Criteria** - Clear validation points in each spec
- **Effort Estimates** - XS, S, M, L, XL scale for task sizing

## Configuration

- **YAML** - Configuration files (`config.yml`)
- **Directory Conventions** - Standardized folder structure:
  - `agent-os/product/` - Product layer
  - `agent-os/specs/` - Specs layer
  - `agent-os/standards/` - Standards layer
  - `profiles/` - Reusable configurations

## Standards Categories

### Global
- Coding style
- Naming conventions
- Error handling
- Commenting practices
- Validation patterns

### Backend
- API design
- Database models
- Migrations
- Query optimization

### Frontend
- Component architecture
- CSS conventions
- Responsive design
- Accessibility

### Testing
- Test writing practices
- Coverage expectations (2-8 tests per component)

## Workflow Commands

- `/plan-product` - Product planning workflow
- `/shape-spec` - Spec initialization
- `/write-spec` - Spec writing
- `/create-tasks` - Task breakdown
- `/implement-tasks` - Implementation
- `/orchestrate-tasks` - Multi-agent coordination
- `/improve-skills` - Skills improvement

## Tech Stack Philosophy

- **Tool Agnostic** - Works with any AI coding tool, any language, any framework
- **Markdown First** - Human-readable, version-controllable documentation
- **Convention Over Configuration** - Standardized structure with customization options
- **Incremental Adoption** - Can be used for single features or entire products
- **Platform Neutral** - No vendor lock-in, portable across environments
