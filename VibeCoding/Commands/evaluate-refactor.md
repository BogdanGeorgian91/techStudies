# EVALUATE_REFACTOR

Thoroughly assess the codebase (or specified aspect) and critically evaluate whether it has reached a state where the benefits of a refactor would outweigh the time and risk of the work.

## Assessment Process

1. **Code Quality Analysis**
   - Analyze code complexity, maintainability metrics
   - Identify code smells, anti-patterns, and technical debt
   - Evaluate test coverage and quality
   - Review performance bottlenecks and inefficiencies

2. **Architecture Evaluation**
   - Assess current architecture patterns and adherence
   - Identify coupling issues and dependency problems
   - Evaluate scalability and extensibility concerns
   - Review consistency across the codebase

3. **Developer Experience Impact**
   - Measure development velocity and friction points
   - Identify areas causing frequent bugs or confusion
   - Evaluate onboarding difficulty for new developers
   - Assess debugging and troubleshooting complexity

4. **Business Impact Assessment**
   - Quantify maintenance burden and development costs
   - Evaluate risk of current technical debt
   - Assess impact on feature delivery timeline
   - Consider customer-facing performance implications

## Decision Framework

**Refactor is recommended when:**
- Technical debt significantly slows development (>20% velocity impact)
- Bug frequency is high due to code complexity
- Performance issues affect user experience
- Architecture prevents necessary features or scaling
- Developer onboarding takes >2x expected time
- Maintenance costs exceed 40% of development effort

**Refactor should be postponed when:**
- Current issues are manageable with incremental fixes
- Business priorities require immediate feature delivery
- Team lacks sufficient testing infrastructure
- Risk of regression outweighs current pain points
- Recent major changes need to stabilize first

## Refactor Planning (if recommended)

### 1. Scope Definition
- Define specific refactor boundaries and goals
- Identify critical vs. nice-to-have improvements
- Estimate effort and timeline
- Plan phased approach for large refactors

### 2. Pre-Refactor Regression Test Strategy
- **Behavioral Tests**: Create comprehensive end-to-end tests covering all critical user flows
- **API Contract Tests**: Document and test all public interfaces and their expected behavior
- **Performance Benchmarks**: Establish baseline metrics for response times, memory usage, and bundle size
- **Visual Regression Tests**: Capture screenshots of UI components before changes
- **Integration Tests**: Verify all external service integrations work correctly
- **Edge Case Tests**: Document and test error handling and boundary conditions

### 3. Execution Plan
- **Phase 1**: Infrastructure and tooling setup
- **Phase 2**: Core architecture changes
- **Phase 3**: Component refactoring
- **Phase 4**: Optimization and cleanup
- **Phase 5**: Documentation and knowledge transfer

### 4. Continuous Validation
- Run regression test suite after each change
- Monitor performance metrics throughout refactor
- Track bundle size changes and analyze impact
- Measure maintainability improvements (cyclomatic complexity, coupling metrics)
- Validate that original requirements are still met

### 5. Success Metrics
- **Performance**: Response times, memory usage, bundle size
- **Maintainability**: Code complexity scores, test coverage, documentation quality
- **Developer Experience**: Build times, debugging ease, onboarding time
- **Stability**: Bug frequency, test failure rates, production incidents

## Usage

```bash
claude -p "Evaluate the authentication system for refactoring needs"
claude -p "Assess whether the entire frontend architecture needs refactoring"
claude -p "Determine if the API layer should be refactored before adding new features"
```

## Output Format

The evaluation should provide:
1. Clear recommendation (refactor/postpone) with reasoning
2. Risk assessment and mitigation strategies
3. Detailed refactor plan with phases and timelines (if recommended)
4. Comprehensive regression testing strategy
5. Success metrics and validation checkpoints