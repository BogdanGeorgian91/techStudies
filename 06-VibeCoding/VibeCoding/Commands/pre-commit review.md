# Pre-Commit Review

Perform a comprehensive review of uncommitted changes in the working branch before they are published for a PR.

## Instructions

You are tasked with reviewing uncommitted changes in the current working branch. Perform a thorough analysis and provide a detailed report covering the following areas:

### 1. Artifact Cleanup
- Identify any temporary files, debug code, console logs, or other artifacts that are no longer needed
- Look for commented-out code that should be removed
- Check for unused imports, variables, or functions introduced by the changes

### 2. Pattern Adherence
- Analyze if the new code follows existing patterns and conventions in the codebase
- Check for consistency in naming conventions, code structure, and architectural patterns
- Identify any deviations from established coding standards

### 3. Breaking Changes
- Identify any changes that could break existing functionality
- Look for modified APIs, changed function signatures, or altered data structures
- Check for removed or renamed public interfaces

### 4. Scope and Focus
- Assess whether the changes stay focused on the intended goals
- Identify any over-reaching modifications that extend beyond the commit's purpose
- Look for unrelated changes that should be in separate commits

### 5. Documentation and Comments
- Review if comments and documentation are current and accurate
- Check if new functionality has appropriate documentation (only if the project already maintains thorough docs)
- Ensure code comments are clean and meaningful

### 6. Test Coverage (Conditional)
- **Only if the project already has comprehensive tests**: Check if new functionality has corresponding tests
- Identify existing tests that might be broken by the changes but haven't been updated
- Look for missing test cases for new features or modified behavior

## Process

1. First, examine the git status and diff to understand the scope of changes
2. Read the modified files to understand the context and existing patterns
3. Analyze each area systematically
4. Provide a structured report with findings and recommendations

## Important Notes

- **Do not make any changes** - this is a review-only process
- **Assume all functionality and tests are working** - all tests and functionality have been tested (either through automatic tests or manual tests) and should be assumed to be working correctly
- Focus on providing actionable feedback
- Be specific about file locations and line numbers when identifying issues
- Distinguish between critical issues and suggestions for improvement
- If the project lacks comprehensive documentation or tests, do not request them to be added

Provide your findings in a clear, structured report that helps the developer improve their changes before committing.