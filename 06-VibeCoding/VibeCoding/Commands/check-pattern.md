# Check Pattern Command

## Purpose
Analyze existing codebase patterns for a specific feature or implementation approach. This command helps understand how similar functionality is already implemented before adding new code.

## Usage
When the user provides a feature or implementation they want to add, analyze the codebase to:

1. **Search for existing patterns** - Look for similar implementations, naming conventions, and architectural approaches
2. **Identify common patterns** - Present all existing patterns found for implementing that type of functionality
3. **Assess consistency** - Note if there are multiple different approaches and their frequency
4. **Provide recommendations** - If no patterns exist, suggest potential patterns based on the codebase structure

## Analysis Steps

1. **Feature Identification**
   - Understand what feature/implementation the user wants to add
   - Identify key concepts and functionality involved

2. **Pattern Search**
   - Search for similar features in the codebase
   - Look for related files, functions, classes, and modules
   - Examine naming conventions and file organization
   - Check for existing abstractions or utilities

3. **Pattern Analysis**
   - Document all found patterns with specific examples
   - Note file locations and line numbers for reference
   - Identify common architectural approaches
   - Look for shared utilities, base classes, or common patterns

4. **Recommendation**
   - If patterns exist: Present them with pros/cons and usage examples
   - If no patterns exist: Propose potential approaches based on codebase structure and best practices
   - Consider consistency with existing code style and architecture

## Output Format

### When Patterns Exist:
```
## Existing Patterns Found

### Pattern 1: [Name/Description]
- **Location**: file_path:line_number
- **Usage**: Brief description
- **Example**: Code snippet or reference

### Pattern 2: [Name/Description]
- **Location**: file_path:line_number
- **Usage**: Brief description
- **Example**: Code snippet or reference

## Recommendation
Based on the analysis, I recommend using [Pattern X] because [reasoning].
```

### When No Patterns Exist:
```
## No Existing Patterns Found

No similar implementations were found in the codebase for [feature/functionality].

## Proposed Patterns

### Option 1: [Approach Name]
- **Structure**: Proposed file/class organization
- **Rationale**: Why this fits the codebase
- **Example**: Basic structure outline

### Option 2: [Approach Name]
- **Structure**: Alternative organization
- **Rationale**: Alternative reasoning
- **Example**: Basic structure outline

## Recommendation
I recommend [Option X] because it aligns with [existing codebase patterns/conventions].
```

## Important Notes
- **READ-ONLY**: This command should never modify any files
- **Comprehensive Search**: Use multiple search strategies (grep, glob, file reading)
- **Specific References**: Always include file paths and line numbers
- **Context Awareness**: Consider the broader codebase architecture and conventions
- **No Implementation**: Focus on analysis and recommendations, not actual code changes