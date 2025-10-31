# Task Breakdown: Standards as GitHub Copilot Instructions

## Overview
Total Tasks: 4 major task groups
This feature adds support for installing Agent OS standards as GitHub Copilot instructions in `.github/instructions/` directory.

## Task List

### Configuration Layer

#### Task Group 1: Configuration Updates
**Dependencies:** None

- [x] 1.0 Complete configuration layer updates
  - [ ] 1.1 Write 2-8 focused tests for configuration changes
    - Test new `standards_as_copilot_instructions` flag parsing in config.yml
    - Test configuration validation with new flag
    - Test flag override via command-line argument
    - Test that invalid flag combinations are caught
    - Limit to critical configuration behaviors only
  - [x] 1.2 Update base config.yml schema
    - Add `standards_as_copilot_instructions: false` flag with comment explaining usage
    - Add comment about override flag: `--standards-as-copilot-instructions=true/false`
    - Position after `standards_as_claude_code_skills` section for clarity
  - [x] 1.3 Update configuration parsing in scripts
    - Modify `project-install.sh` to parse new `--standards-as-copilot-instructions` flag
    - Add flag to help text in `show_help()` function
    - Add variable initialization in defaults section
  - [x] 1.4 Update `validate_config()` function
    - Add `standards_as_copilot_instructions` parameter to function signature
    - Add validation logic for the new flag
    - Ensure no conflicts with existing flags
  - [x] 1.5 Update `write_project_config()` function
    - Add `standards_as_copilot_instructions` parameter to function signature
    - Include new flag in generated config.yml output
    - Add comment explaining the setting
  - [x] 1.6 Update configuration loading
    - Add `STANDARDS_AS_COPILOT_INSTRUCTIONS` variable in common-functions.sh
    - Add `EFFECTIVE_STANDARDS_AS_COPILOT_INSTRUCTIONS` variable
    - Update config loading to read new flag from config.yml
  - [ ] 1.7 Ensure configuration tests pass
    - Run ONLY the 2-8 tests written in 1.1
    - Verify new flag is recognized
    - Verify command-line override works

**Acceptance Criteria:**
- The 2-8 tests written in 1.1 pass
- config.yml contains `standards_as_copilot_instructions` flag
- Flag can be overridden via command-line
- Configuration validation works correctly
- All related functions accept new parameter

### Compilation Pipeline

#### Task Group 2: Update Compilation Functions
**Dependencies:** Task Group 1

- [x] 2.0 Complete compilation pipeline updates
  - [ ] 2.1 Write 2-8 focused tests for compilation functions
    - Test `process_conditionals()` handles `standards_as_copilot_instructions` flag
    - Test `{{UNLESS standards_as_copilot_instructions}}` blocks are removed when flag is true
    - Test `{{UNLESS standards_as_copilot_instructions}}` blocks are preserved when flag is false
    - Test `compile_instruction()` generates valid instruction files
    - Test instruction file naming convention (e.g., `standards/frontend/css.md` → `frontend-css.md`)
    - Limit to critical compilation behaviors only
  - [x] 2.2 Update `process_conditionals()` function
    - Add `standards_as_copilot_instructions` to conditional flag handling
    - Process `{{UNLESS standards_as_copilot_instructions}}` blocks
    - Process `{{IF standards_as_copilot_instructions}}` blocks if needed
    - Follow same pattern as existing `standards_as_claude_code_skills` handling
  - [x] 2.3 Create `compile_instruction()` function
    - Function signature: `compile_instruction(source, dest, base_dir, profile)`
    - Read source standards file content
    - Extract filename and create human-readable title
    - Apply GitHub Copilot instruction formatting
    - Reference `.github/copilot.instructions.md` for proper syntax
    - Write formatted instruction to destination
    - Return success/failure status
  - [x] 2.4 Implement instruction file naming logic
    - Create helper function to convert standards paths to instruction filenames
    - Example: `standards/frontend/css.md` → `frontend-css.md`
    - Example: `standards/global/coding-style.md` → `global-coding-style.md`
    - Preserve directory structure in filename using hyphens
    - Remove `standards/` prefix and keep `.md` extension
  - [ ] 2.5 Ensure compilation pipeline tests pass
    - Run ONLY the 2-8 tests written in 2.1
    - Verify conditionals process correctly
    - Verify instruction files are properly formatted

**Acceptance Criteria:**
- The 2-8 tests written in 2.1 pass
- `process_conditionals()` handles new flag correctly
- `compile_instruction()` creates valid GitHub Copilot instruction files
- Instruction file naming convention works as specified
- Conditional blocks are processed correctly based on flag state

### Installation Script

#### Task Group 3: Installation Logic
**Dependencies:** Task Group 2

- [x] 3.0 Complete installation logic
  - [ ] 3.1 Write 2-8 focused tests for installation functions
    - Test `install_github_instructions()` creates `.github/instructions/` directory
    - Test instruction files are created from standards when flag is true
    - Test instruction files are NOT created when flag is false
    - Test instruction files have correct naming (e.g., `frontend-css.md`)
    - Test instruction files have valid GitHub Copilot format
    - Limit to critical installation behaviors only
  - [x] 3.2 Create `install_github_instructions()` function
    - Function signature: `install_github_instructions(base_dir, profile, project_dir, verbose)`
    - Check if `standards_as_copilot_instructions` is enabled
    - Create `.github/instructions/` directory in project
    - Find all standards files in profile
    - For each standards file:
      - Determine target instruction filename
      - Call `compile_instruction()` to create instruction file
      - Log progress if verbose enabled
    - Return success/failure status
  - [x] 3.3 Integrate into installation flow
    - Call `install_github_instructions()` after standards installation
    - Pass required parameters (base_dir, profile, project_dir, verbose)
    - Handle errors gracefully
    - Skip if flag is disabled
  - [x] 3.4 Update installation status display
    - Add "Installing GitHub Copilot instructions..." message when enabled
    - Add "Skipping GitHub Copilot instructions..." message when disabled
    - Show count of instruction files created
    - Update final installation summary
  - [x] 3.5 Update verbose logging
    - Log each instruction file being created
    - Log source → destination mapping
    - Log any errors during instruction compilation
    - Follow existing verbose logging patterns
  - [ ] 3.6 Ensure installation tests pass
    - Run ONLY the 2-8 tests written in 3.1
    - Verify `.github/instructions/` directory is created
    - Verify instruction files are created correctly

**Acceptance Criteria:**
- The 2-8 tests written in 3.1 pass
- `install_github_instructions()` function works correctly
- `.github/instructions/` directory is created when flag is enabled
- Instruction files are created from standards files
- Installation status displays correctly
- Verbose logging provides helpful feedback

### Update Script & Documentation

#### Task Group 4: Update Script & Documentation
**Dependencies:** Task Group 3

- [x] 4.0 Complete update script and documentation
  - [ ] 4.1 Write 2-8 focused tests for update functions
    - Test `update_github_instructions()` updates existing instruction files
    - Test instruction files are created if missing when flag is enabled
    - Test instruction files are removed when flag is disabled
    - Test update preserves file permissions
    - Test cleanup logic prompts for confirmation
    - Limit to critical update behaviors only
  - [x] 4.2 Create `update_github_instructions()` function
    - Function signature: `update_github_instructions(base_dir, profile, project_dir, verbose)`
    - Check current and new flag values
    - If enabled: Update or create all instruction files
    - If disabled: Prompt to remove `.github/instructions/` directory
    - Handle transition between enabled/disabled states
    - Log all actions if verbose enabled
  - [x] 4.3 Add instruction file cleanup logic
    - Create cleanup prompt when flag changes from true to false
    - Ask user confirmation before deleting `.github/instructions/`
    - If confirmed, remove directory and all instruction files
    - If declined, warn that files remain but won't be updated
    - Log cleanup actions
  - [x] 4.4 Integrate update logic
    - Call `update_github_instructions()` in main update flow
    - Position after `update_standards()` call
    - Handle errors gracefully
    - Update status messages
  - [x] 4.5 Update README.md
    - Add section explaining `standards_as_copilot_instructions` flag
    - Document when to use instructions vs inline standards
    - Add example usage: `./project-install.sh --standards-as-copilot-instructions=true`
    - Clarify relationship with `standards_as_claude_code_skills`
    - Add note about `.github/instructions/` directory structure
  - [x] 4.6 Update CHANGELOG.md
    - Add entry for new GitHub Copilot instructions feature
    - Document new configuration flag
    - Note that this is an additive feature (non-breaking)
    - Include version number (e.g., v2.1.0)
  - [ ] 4.7 Ensure update tests pass
    - Run ONLY the 2-8 tests written in 4.1
    - Verify updates work correctly
    - Verify cleanup logic works
    - Verify documentation is accurate


### Update Script Support

#### Task Group 4: Update Script and Documentation
**Dependencies:** Task Group 3

- [ ] 4.0 Complete update script and documentation
  - [ ] 4.1 Write 2-8 focused tests for update script
    - Test `update_github_instructions()` handles instruction file updates
    - Test file comparison for changed instructions
    - Test overwrite handling for instruction files
    - Test removal of instructions when flag is disabled
    - Limit to critical update behaviors only
  - [ ] 4.2 Create `update_github_instructions()` function in project-update.sh
    - Follow pattern from `update_standards()` function
    - Check if `.github/instructions/` directory exists
    - Iterate through standards files from profile
    - Compare with existing instruction files
    - Prompt for overwrites using `should_skip_file()` pattern
    - Handle `OVERWRITE_ALL` flag
    - Track updated/skipped/new counts
    - Print summary of updates
  - [ ] 4.3 Add instruction file cleanup logic
    - Create function to remove instruction files when flag is disabled
    - Check `EFFECTIVE_STANDARDS_AS_COPILOT_INSTRUCTIONS` flag
    - If false and `.github/instructions/` exists, prompt for removal
    - Similar to Claude Code cleanup patterns
  - [ ] 4.4 Integrate update logic into main flow
    - Add call to `update_github_instructions()` in main update sequence
    - Position after `update_standards()` call
    - Add instruction cleanup check where appropriate
  - [ ] 4.5 Update README.md documentation
    - Add section explaining `standards_as_copilot_instructions` flag
    - Document when to use instructions vs inline standards
    - Add example usage with flag enabled
    - Clarify relationship with `standards_as_claude_code_skills`
    - Add to configuration options section
  - [ ] 4.6 Update CHANGELOG.md
    - Add entry for new GitHub Copilot instructions feature
    - Document new configuration flag
    - Note that this is additive (not breaking)
    - Mention compatibility with existing installations
  - [ ] 4.7 Ensure update script tests pass
    - Run ONLY the 2-8 tests written in 4.1
    - Verify update script handles instruction files correctly
    - Verify cleanup logic works properly

**Acceptance Criteria:**
- The 2-8 tests written in 4.1 pass
- Update script properly handles instruction file updates
- Overwrite prompts work correctly
- Instruction cleanup works when flag disabled
- README.md documents the new feature clearly
- CHANGELOG.md includes version notes
- Documentation explains when to use each approach

### Integration Testing

#### Task Group 5: End-to-End Testing & Validation
**Dependencies:** Task Groups 1-4

- [ ] 5.0 Complete integration testing and validation
  - [ ] 5.1 Review tests from Task Groups 1-4
    - Review the 2-8 tests written for configuration (Task 1.1)
    - Review the 2-8 tests written for compilation pipeline (Task 2.1)
    - Review the 2-8 tests written for installation script (Task 3.1)
    - Review the 2-8 tests written for update script (Task 4.1)
    - Total existing tests: approximately 8-32 tests
  - [ ] 5.2 Analyze integration test coverage gaps
    - Identify critical end-to-end workflows that lack coverage
    - Focus on complete installation scenarios with flag enabled/disabled
    - Focus on update scenarios and flag toggling
    - Prioritize multi-flag combination testing
    - Skip unit test gaps (covered in earlier groups)
  - [ ] 5.3 Write up to 10 additional integration tests maximum
    - Test full installation flow with `standards_as_copilot_instructions=true`
    - Test full installation flow with `standards_as_copilot_instructions=false`
    - Test installation with both copilot instructions AND claude skills enabled
    - Test update script with flag toggling (true → false, false → true)
    - Test that conditional compilation works in actual commands
    - Test that instruction files are valid GitHub Copilot format
    - Test edge cases: empty standards directory, special characters in filenames
    - Focus on end-to-end user workflows
    - Skip individual function testing (covered in earlier groups)
  - [ ] 5.4 Manual validation checklist
    - Run `./scripts/project-install.sh --standards-as-copilot-instructions=true` on clean project
    - Verify `.github/instructions/` contains all standards as instruction files
    - Verify instruction file naming matches specification
    - Verify instruction file format is valid for GitHub Copilot
    - Test with `standards_as_copilot_instructions=false` flag
    - Verify conditional blocks in commands work correctly
    - Test `./scripts/project-update.sh` with flag enabled
    - Verify existing installations work without changes
    - Test flag override via command-line works
  - [ ] 5.5 Run all feature tests
    - Run ALL tests from groups 1-5 (approximately 18-42 tests total)
    - Verify no test failures
    - Fix any issues discovered during testing

**Acceptance Criteria:**
- All feature tests pass (approximately 18-42 tests total)
- Manual validation checklist completed successfully
- No more than 10 additional tests added for integration gaps
- Installation works correctly with flag enabled/disabled
- Update script works properly
- No regression in existing functionality
- Documentation accurate and complete

## Execution Order

Recommended implementation sequence:
1. Configuration Layer (Task Group 1) - Add new flag and config handling
2. Compilation Pipeline (Task Group 2) - Create instruction compilation logic
3. Installation Script (Task Group 3) - Implement instruction installation
4. Update Script & Documentation (Task Group 4) - Complete update support
5. Integration Testing & Validation (Task Group 5) - Verify everything works end-to-end

## Important Notes

- **Non-Breaking Change**: This feature is additive and doesn't break existing installations
- **Flag Independence**: `standards_as_copilot_instructions` can coexist with `standards_as_claude_code_skills`
- **Test Strategy**: Each group writes 2-8 focused tests, with max 10 additional integration tests at the end
- **Directory Structure**: GitHub Copilot instructions use flat file structure in `.github/instructions/`
- **Conditional Compilation**: Existing conditional logic (`{{UNLESS standards_as_copilot_instructions}}`) needs to be supported
- **Standards Preservation**: Original standards files in `agent-os/standards/` are always installed regardless of flags
