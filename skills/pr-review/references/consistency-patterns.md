# Consistency Checking

Verify that new code follows existing codebase patterns and conventions.

## Process

### 1. Identify What's Being Added

Determine the type of change:
- New feature or component
- Modification to existing code
- New test coverage
- Documentation or configuration

### 2. Find the Closest Existing Analog

Before evaluating new code for internal correctness, identify the closest existing analog in the codebase.

**For new classes:**
1. Find the existing class that serves the most similar purpose
2. Map new methods against existing methods explicitly — don't rely on a high-level impression
3. If substantial logic already exists in the analog (query building, data access, business rules), flag it as a duplication finding
4. Don't assume a design pattern (composite, decorator, adapter) justifies duplicated logic — the pattern and the duplication are separate concerns

**For new infrastructure components** (HTTP clients, service wrappers, external API clients, config readers, etc.):
1. Ask first: "Does an established pattern for this component type already exist in the codebase?"
2. Find that pattern before evaluating anything else
3. Compare the new component against the established pattern — not just for structure, but for behavior (error handling, timeouts, retries, SSL config, dependency injection)
4. Flag deviations as consistency findings even if the new component is internally correct

**Finding analogs:**
- Search by component type (e.g., other HTTP clients, other service wrappers)
- Search by dependency (e.g., other classes that use the same external library)
- Look in relevant directories for similar naming patterns
- Check recent commits for similar work

### 3. Compare Patterns

Check for consistency with existing code:

#### Code Structure
- [ ] Inherits from correct base classes/interfaces
- [ ] Follows naming conventions
- [ ] Uses established error handling patterns
- [ ] Implements required methods/interfaces
- [ ] Follows file/directory structure conventions
- [ ] Uses consistent coding style

#### Architectural Patterns
- [ ] Respects existing layered architecture (if present)
- [ ] Business logic in appropriate layer
- [ ] Follows separation of concerns
- [ ] Uses established abstractions
- [ ] Matches dependency injection patterns

**Example - Pattern Comparison:**

```
1. Find existing similar code:
   - Search for similar functionality
   - Identify the pattern used

2. Compare structure:
   - Does new code follow the same approach?
   - Are responsibilities organized the same way?
   - Does it use the same abstractions?

3. Document differences:
   - Note any deviations
   - Assess if justified or should be aligned
```

#### Tests
- [ ] Test organization and structure
- [ ] Shared examples/helpers usage
- [ ] Assertion patterns
- [ ] Test data setup
- [ ] Naming conventions

### 4. Document Deviations

For each deviation from patterns, create a tracking item with:
- Component and pattern name
- Existing pattern (with example)
- New code showing deviation
- Impact assessment
- Recommendation (align or justify)

### 5. Check Architectural Patterns

Verify the code respects the codebase's architectural approach:

- [ ] Follows established layering (if present)
- [ ] Respects separation of concerns
- [ ] Uses appropriate abstractions
- [ ] Matches error handling approach
- [ ] Follows dependency patterns
- [ ] Clear boundaries between components

### 6. Generate Consistency Report

Document findings in structured format:
- **Components Checked**: List all reviewed components
- **Patterns Followed**: List correct patterns ✅
- **Deviations Found**: List deviations with references ⚠️
- **Architectural Alignment**: Overall assessment ✅/⚠️
- **Recommendation**: Overall consistency assessment

## Quick Checklist

For any new component:
- [ ] Similar to existing components in structure
- [ ] Follows established naming conventions
- [ ] Uses consistent error handling
- [ ] Respects architectural boundaries
- [ ] Tests follow existing patterns
- [ ] Uses shared helpers/utilities where applicable

## Common Issues

### 🚩 Violating Architectural Boundaries
New code places logic in the wrong layer or violates separation of concerns established in the codebase.

### 🚩 Different Error Handling Pattern
New code uses different error handling approach than existing code without justification.

### 🚩 Reinventing Existing Patterns
New code implements functionality that existing helpers/utilities already provide. This is easiest to miss when the new code is internally correct — correctness does not imply consistency.

### 🚩 Design Pattern Accepted as Justification for Duplication
A design pattern (composite, decorator, adapter) is correctly identified and described, but used as justification to stop checking for duplication. The pattern and the duplication are independent concerns — a composite class can still duplicate logic wholesale from the class it wraps.

### 🚩 Inconsistent Naming
New code doesn't follow established naming conventions (e.g., `CreateUser` vs existing pattern of `UserCreate`).

### 🚩 Not Using Shared Test Helpers
New tests manually implement patterns that shared examples/helpers already cover.

### 🚩 Skipping Established Abstractions
New code bypasses established abstractions (base classes, mixins, interfaces) without reason.

## Success Criteria
- All components follow existing patterns
- Deviations documented with justification
- Architectural alignment verified
- Consistency report generated
- Tracking items created for misalignments

## Integration with Core Review

In core review process, add consistency as a review category with:
- Clear scope (which components to check)
- Link to main review
- Specific patterns to verify
