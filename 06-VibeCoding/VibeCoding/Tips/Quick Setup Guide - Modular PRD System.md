## 🚀 Initial Setup (5 minutes)

### Step 1: Create Directory Structure

```bash
# Run this in your project root
mkdir -p docs/prd/{features,user-stories,technical,api,data-models,design/wireframes,metrics}
mkdir -p docs/{config,standards}
mkdir -p claude/{tasks,history}

# Create essential files
touch docs/prd/PRD.md
touch docs/prd/CHANGELOG.md
touch claude/project_manifest.md
touch Claude.md
```

### Step 2: Create Minimal PRD Template

Start with this minimal `docs/prd/PRD.md`:

```markdown
# Product Requirements Document

## Product Overview
[Your product in 2 sentences]

## Core Features (MVP)
1. **[Feature 1]** → Details: `docs/prd/features/feature1.md`
2. **[Feature 2]** → Details: `docs/prd/features/feature2.md`

## Technical Stack
→ See: `docs/prd/technical/tech-stack.md`

## Quick Links
- [Recent Changes](./CHANGELOG.md)
- [Technical Architecture](./technical/architecture.md)
- [API Documentation](./api/)
```

### Step 3: Create Your First Feature Spec

Example `docs/prd/features/feature1.md`:

```markdown
# Feature: [Feature Name]

## Overview
[What this feature does]

## Requirements
- [ ] Requirement 1
- [ ] Requirement 2

## Technical Notes
[Any technical considerations]

## UI/UX
[Link to designs or describe interface]
```

## 📝 Instructions for Claude Code

Add this to your `Claude.md`:

```markdown
## PRD Navigation Rules

1. **Always start with**: `docs/prd/PRD.md`
2. **Follow references** only as needed for current task
3. **When updating**: Log changes in `docs/prd/CHANGELOG.md`
4. **Ask for clarification** on which PRD section to review

## Document Hierarchy
- Level 1: PRD.md (overview)
- Level 2: Feature specs (details)
- Level 3: Technical/API/Design docs (implementation)
```

## 🎯 Usage Examples

### Example 1: Asking Claude to implement a feature

```
Claude, please:
1. Read the PRD overview at docs/prd/PRD.md
2. Review the authentication feature at docs/prd/features/authentication.md
3. Create an implementation plan in claude/tasks/auth_implementation.md
```

### Example 2: Reviewing specific requirements

```
Claude, review only the API specifications in docs/prd/api/
Don't read the UI/UX or user story documents for now.
```

### Example 3: Updating after implementation

```
Claude, I've approved your implementation. Please:
1. Update docs/prd/features/authentication.md with actual behavior
2. Add to docs/prd/CHANGELOG.md
3. Update status in PRD.md to "Complete"
```

## 🔄 Progressive Enhancement

### Start Simple (Week 1)

- Main PRD with bullet points
- 2-3 feature files
- Basic CHANGELOG

### Add Structure (Week 2)

- Technical specifications
- API documentation
- User stories

### Full System (Month 1)

- Complete design docs
- Data models
- Metrics tracking
- Risk analysis

## 💡 Pro Tips

### 1. Use Status Badges

In your PRD.md:

```markdown
#### Authentication ![Status: Complete](https://img.shields.io/badge/Status-Complete-green)
#### Dashboard ![Status: In Progress](https://img.shields.io/badge/Status-In%20Progress-yellow)
```

### 2. Version Your Specs

In each feature file:

```markdown
> Version: 1.0.0
> Last Updated: 2024-01-20
> Author: @username
```

### 3. Create Templates

Save templates in `docs/prd/templates/`:

- `feature-template.md`
- `api-endpoint-template.md`
- `user-story-template.md`

### 4. Link Validation

Periodically ask Claude:

```
Claude, validate all links in docs/prd/PRD.md and report any broken references
```

## 🎓 Best Practices

### DO ✅

- Keep main PRD under 200 lines
- Use consistent naming conventions
- Update CHANGELOG immediately
- Link bidirectionally (parent ↔ child docs)
- Use relative links for portability

### DON'T ❌

- Put implementation details in PRD
- Create specs for undefined features
- Mix requirements with code documentation
- Forget to update status indicators
- Use absolute file paths

## 📊 Example CHANGELOG.md

```markdown
# PRD Change Log

## 2024-01-20
- **Added**: OAuth support to authentication spec
- **Changed**: Dashboard layout from grid to flex
- **Removed**: Legacy API endpoints
- **Updated**: Performance requirements (100ms → 50ms)

## 2024-01-15
- **Added**: Initial authentication specification
- **Added**: Dashboard feature specification
```

## 🔍 Verification Checklist

Before starting development, ensure:

- [ ] PRD.md exists and is readable
- [ ] All referenced files exist
- [ ] No broken links in documentation
- [ ] CHANGELOG.md is up to date
- [ ] Feature specs have clear requirements
- [ ] Claude.md includes PRD navigation rules

## 🚦 Quick Start Command for Claude

```
Claude, please run a PRD system check:
1. Verify docs/prd/PRD.md exists
2. List all feature files in docs/prd/features/
3. Check for broken references in the main PRD
4. Confirm you understand the navigation rules
```

---

_This setup takes ~10 minutes initially and saves hours of confusion later!_