# Implement Plan Command

**Purpose**: Autonomously implement a detailed plan from a markdown file, working through each step programmatically while documenting unclear points for later user clarification.

## How to Use

```bash
claude -p "Implement the plan outlined in [plan-file.md]. Follow the implement-plan command approach - work autonomously through each step, evaluate before implementing, and document any unclear points in a separate tracking file. Only stop to consult me if issues would block other planned work or require major changes that affect the overall implementation."
```

## Command Behavior

### Core Approach
1. **Read and parse the plan** - Understand the overall structure and dependencies
2. **Work through steps sequentially** - Implement each step in order
3. **Evaluate before implementing** - Check if there's a better approach for each step
4. **Document unclear points** - Create/update a tracking file for minor clarifications
5. **Stop for blocking issues** - Only consult user when uncertainty affects other planned work

### Implementation Guidelines

#### When to Continue Autonomously
- Minor implementation details that don't affect the overall architecture
- Non-blocking clarifications that can be addressed with basic placeholders
- Small variations in approach that don't impact other steps
- Questions about styling, naming conventions, or similar details

#### When to Stop and Consult
- The plan is fundamentally unclear or incomplete
- A step would require major changes that affect subsequent steps
- Uncertainty about core architecture decisions
- Conflicts between plan steps that can't be resolved
- Missing critical information that blocks multiple future steps

#### Documentation Pattern for Unclear Points
Create/update `implementation-questions.md` with entries like:

```markdown
## Issue #X: [Brief Description]

**Context**: [What part of the plan this relates to]

**Current Implementation**: [What was done as placeholder]

**Question**: [Specific question or clarification needed]

**Options**: 
- Option A: [Description with pros/cons]
- Option B: [Description with pros/cons]

**Recommendation**: [Your suggested approach if applicable]

**Impact**: [How this affects other parts of the implementation]
```

### Evaluation Criteria
Before implementing each step, consider:
- Is there a more efficient/maintainable approach?
- Does this align with existing codebase patterns?
- Are there potential security or performance implications?
- Would a different approach better serve the overall plan?

### Success Criteria
- Maximum implementation completed without assumptions
- Clear documentation of remaining questions
- Working code that follows the plan's intent
- Minimal blocking issues for user review

## Example Usage Scenarios

### Scenario 1: Clear Plan
```
User provides: "Add user authentication system with login/logout"
Plan has: Clear steps, tech stack, file structure
Action: Implement fully, document minor styling questions
```

### Scenario 2: Partially Clear Plan
```
User provides: "Refactor data layer for better performance"
Plan has: General approach but vague on specific optimizations
Action: Implement what's clear, document optimization strategy questions
```

### Scenario 3: Unclear Plan
```
User provides: "Make the app better"
Plan has: Vague goals, no specific steps
Action: Stop immediately and request clarification
```

## File Management
- Never create unnecessary files
- Prefer editing existing files over creating new ones
- Only create `implementation-questions.md` if there are actual questions to document
- Follow existing codebase patterns and conventions

## Quality Assurance
- Run relevant linting/typechecking commands after implementation
- Verify code follows existing conventions
- Ensure no security issues are introduced
- Test implementation if testing framework is available