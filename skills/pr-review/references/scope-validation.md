# Scope Validation

Ensure PR implementation aligns with ticket requirements and acceptance criteria.

## Table of Contents

- [Process](#process)
  - [Extract Ticket Requirements](#1-extract-ticket-requirements)
  - [Map Implementation to Requirements](#2-map-implementation-to-requirements)
  - [Validate Against Requirements, Not Code Patterns](#3-validate-against-requirements-not-code-patterns)
  - [Identify Scope Boundaries](#4-identify-scope-boundaries)
  - [Categorize Implementation Completeness](#5-categorize-implementation-completeness)
  - [Document Scope Assessment](#6-document-scope-assessment)
  - [Clarify with Stakeholders](#7-clarify-with-stakeholders)
- [Examples](#examples)
- [Anti-Patterns](#anti-patterns)
- [Success Criteria](#success-criteria)
- [Integration with Core Review](#integration-with-core-review)

## Process

### 1. Extract Ticket Requirements

From the ticket, identify and document:

**Core Requirements:**
- **Title/Summary**: What is being built/fixed?
- **Description**: What problem is being solved? Why?
- **Acceptance Criteria**: Specific, testable requirements
- **Subtasks**: If part of an epic, which subtask does this address?
- **Definition of Done**: Any project-specific completion criteria

**Context:**
- **Epic/Parent**: Is this part of a larger feature?
- **Dependencies**: Does this depend on or block other work?
- **Priority**: Is this must-have or nice-to-have?

**Check beyond the AC text:**

AC frequently diverges from actual decisions made during implementation. Before treating AC as ground truth, also check:
- **Ticket comments**: Decisions, clarifications, or scope changes recorded after the ticket was written
- **Acceptance comments**: Notes left when closing or accepting related tickets — these often document why AC was modified or superseded
- **Linked/parent tickets**: If AC was copied from a parent or related ticket, check that ticket's comments for decisions that weren't propagated back

A criterion in the AC may have been superseded by a decision recorded only in a comment elsewhere.

### 2. Map Implementation to Requirements

**Principle: Validate explicitly, don't infer from code patterns**

When requirements specify behavior, validate the implementation matches those requirements. Don't assume correctness based on consistency with existing code - existing code may have the same bug.

Create a checklist mapping each acceptance criterion to implementation:

```markdown
ACCEPTANCE CRITERIA CHECKLIST:

✅ [Criterion 1]: Fully implemented and tested
   - Implementation: [file/method reference]
   - Tests: [test file reference]
   
⚠️ [Criterion 2]: Partially implemented
   - What's done: [description]
   - What's missing: [description]
   - Reason: [why partial]
   
❌ [Criterion 3]: Not implemented
   - Reason: [out of scope / future work / changed requirements]
   
➖ [Criterion 4]: Not applicable
   - Reason: [why no longer relevant]
```

### 3. Validate Against Requirements, Not Code Patterns

Validate all implementation explicitly against requirements. Don't infer expected behavior from code consistency.

**Don't rely on:**
- Code consistency alone - existing code may have the same bug
- Pattern matching - wrong patterns can be repeated
- Inference from similar code - requirements may differ

**Do verify:**
- What requirements actually specify for each scenario
- Implementation produces the specified behavior
- Edge cases and special states are handled per requirements

**This applies to all behavior, including:**
- Authorization/access control - Who can access what?
- Data visibility - What data is returned to different users?
- Error handling - What errors are returned in what scenarios?
- State transitions - What states are valid and when?
- Business logic - What operations are performed?
- Validation rules - What inputs are accepted/rejected?

### 4. Identify Scope Boundaries

**Questions to ask:**

**About missing criteria:**
- Are there acceptance criteria not addressed in this PR?
- Should missing criteria be:
  - Added to this PR before merge?
  - Tracked in follow-up tickets?
  - Removed as no longer relevant?
- Is there a clear plan for completing missing work?

**About implemented work:**
- Does the PR implement things NOT in the ticket?
- Is additional scope justified and documented?
- Has scope expansion been approved?

**About epic/subtask relationship:**
- If this is part of an epic, which subtask does it address?
- Does it fully complete that subtask?
- Are there dependencies on other subtasks?

### 5. Categorize Implementation Completeness

Determine which category the PR falls into:

**Category A: Complete Implementation** ✅
- All acceptance criteria met
- All tests passing
- Documentation updated
- Ready to merge

**Category B: Intentional Partial Implementation** ⚠️
- Implements defined subset of requirements
- Part of phased approach
- Follow-up work planned and tracked
- Clear boundaries documented
- Acceptable to merge as-is

**Category C: Incomplete Implementation** ❌
- Missing required acceptance criteria
- No plan for completing missing work
- Unclear scope boundaries
- Should not merge until complete or scope adjusted

**Category D: Scope Mismatch** 🔄
- Implements different functionality than ticket describes
- Requirements have changed
- Ticket needs updating to match implementation
- Requires stakeholder alignment

### 6. Document Scope Assessment

Create clear summary for PR review:

```markdown
## Scope Validation

**TICKET**: [ID] - [Title]
**SUBTASK**: [If applicable, which subtask of larger epic]
**CATEGORY**: [A/B/C/D from above]

### Implemented ✅
- Core functionality X
- Test coverage for Y  
- Error handling for Z

### Not Implemented ❌
- Retry logic with exponential backoff
- Caching with 5-minute TTL
- Warning logging for empty results

### Rationale for Partial Implementation
[Explain why certain criteria aren't met]
- This PR implements the **query layer** only
- Retry/caching will be added in follow-up ticket [LINK]
- Phased approach approved by [stakeholder]

### Follow-up Work Required
- [ ] Create ticket for retry logic implementation
- [ ] Create ticket for caching layer
- [ ] Update epic to reflect phased approach

### Recommendation
- ✅ Merge this PR as foundational work
- 📋 Create follow-up tickets for missing criteria
- 📝 Update ticket description to reflect actual scope
```

### 7. Clarify with Stakeholders

If scope is unclear, ask specific questions:

**Good questions:**
```
❓ "This PR implements the query layer but not retry logic. 
   Is this intentional phase 1, or should retry be added before merge?"

❓ "The ticket mentions caching with 5-min TTL, but implementation has none.
   Should caching be added here, or tracked separately?"

❓ "This PR adds feature X which isn't in the ticket.
   Is this intentional scope expansion, or should it be removed?"
```

**Bad questions:**
```
❌ "Is this PR complete?" (too vague)
❌ "Why didn't you implement everything?" (accusatory)
❌ "Should I approve this?" (asking for decision you should make)
```

## Examples

### Example 1: Complete Implementation

**Ticket**: "Add user authentication endpoint"

**Acceptance Criteria:**
> * POST /api/auth endpoint accepts username/password
> * Returns JWT token on success
> * Returns 401 on invalid credentials
> * Rate limiting: 5 attempts per minute
> * Unit tests with 90%+ coverage

**PR Implementation:**
- ✅ POST endpoint implemented
- ✅ JWT token generation
- ✅ 401 error handling
- ✅ Rate limiting middleware
- ✅ 95% test coverage

**Assessment**: Category A - Complete ✅  
**Recommendation**: Approve and merge

---

### Example 2: Intentional Partial (Acceptable)

**Ticket**: "Implement resource visibility service"

**Acceptance Criteria:**
> * Service returns accurate lists of accessible resources
> * Service retries up to 3 times on transient errors
> * Caching implemented with 5-minute TTL
> * Unit tests achieve 90% coverage

**PR Implementation:**
- ✅ Returns accurate lists (tested)
- ❌ No retry logic
- ❌ No caching
- ✅ 95% test coverage

**Assessment**: Category B - Intentional Partial ⚠️

**Documented Rationale**:
> This PR implements the core query layer for resolving
> resource visibility. Retry logic and caching will be added in
> phase 2 (follow-up ticket) after validating the query design
> with production data.

**Recommendation**: 
- ✅ Approve for merge (foundational work)
- 📋 Verify follow-up ticket exists for retry/caching
- 📝 Update ticket to reflect phased approach

---

### Example 3: Incomplete (Not Acceptable)

**Ticket**: "Fix security vulnerability in auth flow"

**Acceptance Criteria:**
> * CSRF token validation on all POST endpoints
> * Session timeout after 30 minutes
> * Audit logging of auth failures
> * Security test suite

**PR Implementation:**
- ✅ CSRF token validation
- ❌ No session timeout
- ❌ No audit logging
- ❌ No security tests

**Assessment**: Category C - Incomplete ❌

**Issues**:
- Critical security features missing
- No tests for security requirements
- No explanation for missing work

**Recommendation**:
- ❌ Do not merge
- 📋 Complete missing security features
- 🧪 Add security test coverage
- OR justify why criteria can be deferred

---

### Example 4: Scope Mismatch

**Ticket**: "Add pagination to user list endpoint"

**Acceptance Criteria:**
> * Support page and per_page query params
> * Return total count in response
> * Default to 20 items per page

**PR Implementation:**
- ❌ No pagination added
- ✅ Added user filtering by role
- ✅ Added user sorting by name
- ✅ Tests for filtering and sorting

**Assessment**: Category D - Scope Mismatch 🔄

**Issues**:
- PR doesn't implement what ticket requested
- PR adds features not in ticket
- Unclear if requirements changed

**Recommendation**:
- 🔄 Clarify with author and stakeholders
- Either:
  - Update ticket to match implementation (if requirements changed)
  - Update PR to match ticket (if scope drift occurred)
  - Split into separate tickets (pagination + filtering)

## Anti-Patterns

### ❌ Assuming partial implementation is wrong

```
BAD: "This PR is incomplete, it's missing retry logic. Request changes."

GOOD: "This PR implements the query layer but not retry logic from 
       acceptance criteria. Is this intentional phase 1, or should 
       retry be added before merge?"
```

### ❌ Accepting scope creep without discussion

```
BAD: Silently approving PR that does more than ticket asked

GOOD: "This PR adds feature X which wasn't in the ticket. Is this
       intentional scope expansion? Should it be tracked separately?"
```

### ❌ Blocking on aspirational acceptance criteria

```
BAD: Blocking merge because ticket mentioned "future caching capability"

GOOD: "Ticket mentions caching. Is this required now or planned for later?
       If later, should we update ticket to reflect current scope?"
```

### ❌ Ignoring missing critical requirements

```
BAD: Approving security fix that's missing test coverage

GOOD: "Security fixes require test coverage per our standards. 
       Can we add tests before merge?"
```

## Success Criteria

- [ ] All acceptance criteria mapped to implementation
- [ ] Scope boundaries clearly defined
- [ ] Missing work identified and tracked
- [ ] Stakeholder alignment on what's in/out of scope
- [ ] Clear recommendation: merge / needs work / needs clarification
- [ ] Follow-up tickets created if needed

## Integration with Core Review

Use this skill with:
- **[PR Review](../SKILL.md)** - Use as step 2 after initializing review
- **[Agent Issue Tracking](../../agent-issue-tracking/SKILL.md)** - Track scope findings and follow-up work

In core review process, add scope validation as first step:

```
1. Initialize Review Tracking
2. Validate Scope (use this add-on)
   - Extract ticket requirements
   - Map to implementation
   - Document gaps and boundaries
3. Understand Technical Scope
4. Create Review Categories
...
```

This ensures you're reviewing the right thing before diving into technical details.
