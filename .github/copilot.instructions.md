# Copilot Instructions for Agent OS

## What is Agent OS?

Agent OS is a spec-driven development framework that transforms AI coding from guesswork into structured, reliable development.

## Core Concept: 3-Layer Context System

Agent OS gives AI agents structured context through three layers:

### 1. Standards Layer - How You Build
- Location: `profiles/default/` and `standards/`
- Contains: Coding conventions, patterns, best practices
- Purpose: Train AI agents to code consistently

### 2. Product Layer - What & Why
- Location: `agent-os/product/`
- Contains: Vision, roadmap, tech stack decisions
- Purpose: Provide strategic context

### 3. Specs Layer - What's Next
- Location: `agent-os/specs/[YYYY-MM-DD]-[name]/`
- Contains: Detailed requirements, tasks, acceptance criteria
- Purpose: Guide specific implementations

## Specialized Agents

- **product-planner**: Creates mission, roadmap, tech stack
- **spec-initializer**: Sets up spec folder structure
- **spec-writer**: Writes detailed specifications
- **tasks-list-creator**: Breaks specs into implementable tasks
- **implementer**: Executes development tasks
- **implementation-verifier**: Validates completed work

## Key Workflows

### Planning (`/plan-product`)
Establishes product foundation:
- `product/mission.md` - Vision and goals
- `product/roadmap.md` - Development phases
- `product/tech-stack.md` - Technology choices

### Specification
- `/shape-spec` - Initialize spec structure
- `/write-spec` - Generate detailed specs
- `/create-tasks` - Break into implementable tasks

### Implementation
- `/implement-tasks` - Execute development
- `/orchestrate-tasks` - Multi-agent coordination
- Verification - Validate against all 3 layers

## File Structure

```
agent-os/
├── product/              # What & why you're building
│   ├── mission.md
│   ├── roadmap.md
│   └── tech-stack.md
├── specs/                # What you're building next
│   └── [YYYY-MM-DD]-[name]/
│       ├── spec.md
│       ├── tasks.md
│       └── planning/
└── standards/            # How you build
    ├── global/
    ├── backend/
    ├── frontend/
    └── testing/
```

## Development Process

1. **Setup Standards**: Define your coding conventions
2. **Plan Product**: Create mission, roadmap, tech stack
3. **Write Specs**: Document requirements with acceptance criteria
4. **Create Tasks**: Break specs into actionable items (checkboxes)
5. **Implement**: Execute tasks following all 3 layers
6. **Verify**: Validate against standards, product vision, and specs

## Task Format

- Use checkboxes: `- [ ]` (incomplete) → `- [x]` (complete)
- Group logically (e.g., Database, API, UI)
- Mark dependencies clearly
- Limit tests to 2-8 focused tests per component

## Core Standards

- **DRY Principle**: Don't repeat yourself
- **Meaningful Names**: Clear, descriptive identifiers
- **Small Functions**: Focused, single-purpose
- **Minimal Tests**: 2-8 tests per component
- **Clean Code**: Remove unused code and imports

## Success Metrics

- First-try working code
- Consistent patterns across codebase
- Complete traceability from vision to implementation
- Automated validation
- Maintainable, production-ready results

## Philosophy

Agent OS eliminates AI coding trial-and-error by providing structured context. Your standards, product vision, and specifications guide AI agents to reliably produce code that matches your requirements and conventions.
