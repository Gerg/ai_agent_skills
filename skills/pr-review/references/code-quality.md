# Code Quality

Evaluate code quality beyond functional correctness: clarity, maintainability, and adherence to project conventions.

## Table of Contents

- [Comment Quality](#comment-quality)
- [Naming Clarity](#naming-clarity)
- [Redundancy Detection](#redundancy-detection)
- [Test Pressure](#test-pressure)
- [Project Conventions](#project-conventions)
- [Success Criteria](#success-criteria)

## Comment Quality

### Principle

Comments should explain **why**, not **what**. Code should be self-explanatory through clear naming and structure.

### Red Flags

**Comments that restate code:**
- Comment describes what the code obviously does
- Comment adds no information beyond reading the code
- Comment in conditional that just restates the condition

**Examples:**
```
❌ BAD: Comment restates code
// Set the value to x
value = x

// Loop through items
for item in items

// Check if empty
if list.isEmpty()

// Neither A nor B provided
else
```

**Outdated or misleading comments:**
- Comment doesn't match current code
- Comment describes old behavior
- Comment contradicts implementation

### Good Comments

**Explain non-obvious intent:**
- Why this approach was chosen over alternatives
- Trade-offs or constraints
- Performance considerations
- Business rules or domain logic

**Document invariants:**
- Assumptions that must hold
- Preconditions or postconditions
- Thread safety requirements

**Provide context:**
- Link to tickets, RFCs, or documentation
- Reference external systems or APIs
- Explain workarounds or temporary solutions

### Questions to Ask

- Can this code be understood without the comment?
- Does the comment add information not in the code?
- Would better naming eliminate the need for this comment?
- Is the comment explaining "what" or "why"?

### Validation

Check project documentation for comment philosophy:
- Does the project prefer minimal comments?
- Are there specific comment requirements (public APIs, complex algorithms)?
- What's the balance between comments and self-documenting code?

**Important:** Finding the same bad comment pattern elsewhere in the codebase does not make it acceptable. Consistency with a bad pattern is still a bad pattern - flag it regardless.

## Naming Clarity

### Principle

Names should follow **existing conventions** in the codebase. Consistency with established patterns matters more than any specific naming philosophy.

### Red Flags

**Inconsistent with existing patterns:**
- New code uses different naming style than similar existing code
- Related components don't follow same naming pattern
- Name doesn't match what existing code calls similar concepts

**Breaks established layer conventions:**
- Controller doesn't follow controller naming pattern
- Model doesn't follow model naming pattern
- New code uses different convention than existing code in same layer

**Unclear or misleading:**
- Name doesn't convey purpose or role
- Name suggests wrong abstraction or relationship
- Name is ambiguous or confusing

**Note:** Different layers using different names for related concepts can be intentional (e.g., controller uses domain term, model uses table name). This is only a red flag if it breaks the established pattern for that codebase.

### Good Naming

**Follow existing patterns:**
- Check how existing similar code names things
- Use same conventions as related components
- Match the style and terminology already established

**Match the level of abstraction:**
- Identify whether existing code is domain-centric, implementation-centric, or hybrid
- Use the same abstraction level for similar components
- If different layers use different levels, follow the pattern for that layer
- Examples:
  - Domain-centric: `Customer`, `Order`, `Payment`
  - Implementation-centric: `CustomerTable`, `OrderRecord`, `PaymentDTO`
  - Hybrid: `CustomerService` (domain) + `CustomerRepository` (implementation)

**Be consistent across layers:**
- Same concept should have same name in all layers (unless layer-specific conventions differ)
- Related components should follow same naming pattern
- Check what existing code calls this concept in each layer

**When conventions vary by layer:**
- Controllers might use domain terms while models use table names
- Services might use business terms while DAOs use technical terms
- Understand the pattern and follow it

### Questions to Ask

- What do existing similar components call this?
- What naming conventions exist in the codebase?
- Is this name consistent across all layers?
- Does this match the established pattern?
- Would someone familiar with the codebase expect this name?

### Validation

1. **Find similar existing code** - Look for components with similar roles or responsibilities

2. **Identify the naming pattern** - What conventions exist?
   - What words or terms are used?
   - What structure or format (e.g., VerbNoun, NounVerb)?
   
3. **Determine the level of abstraction** - What does "consistent" mean here?
   - **Domain-centric**: Names reflect business concepts (e.g., `Order`, `Customer`, `Invoice`)
   - **Implementation-centric**: Names reflect technical details (e.g., `OrderTable`, `CustomerRecord`, `InvoiceDTO`)
   - **Hybrid**: Mix of both (e.g., `OrderService` uses domain term, `OrderRepository` uses implementation term)
   - **Layer-specific**: Different abstraction levels per layer (e.g., controllers use domain terms, models use table names)

4. **Match the abstraction level** - Use the same level as existing code
   - If existing code uses domain terms, use domain terms
   - If existing code uses implementation terms, use implementation terms
   - If existing code mixes levels, understand the pattern for when to use which

5. **Verify consistency across all layers** - Does each layer follow its own convention?

6. **When in doubt, ask** - "What naming convention should I follow for this type of component?"

## Redundancy Detection

### Principle

Avoid multiple mechanisms doing the same thing. Prefer established patterns over ad-hoc solutions.

### Common Redundancies

**Dead code:**
- Commented-out blocks left behind after refactoring
- Unused imports, variables, or methods
- Abandoned experiments or feature flags that never shipped

**ID/Key generation:**
- Global mechanism (framework plugin, base class)
- Local mechanism (model hook, initializer)
- Explicit setting (in calling code)
- Check if multiple are present - which one actually runs?

**Validation logic:**
- Duplicated across layers (API, service, model)
- Duplicated when extending base classes
- Check if base class already validates

**Authorization checks:**
- Duplicated across layers (middleware, controller, service)
- Check if framework provides global mechanism

**Error handling:**
- Duplicated rescue blocks
- Check if framework provides global error handling

### Detection Process

**1. Find global mechanisms:**
- Check for framework plugins or middleware
- Check for base classes that provide functionality
- Look for configuration that enables automatic behavior

**2. Find local mechanisms:**
- Check for hooks (before/after callbacks)
- Check for explicit code in methods
- Look for duplicated patterns across files

**3. Trace execution:**
- What runs first?
- What actually does the work?
- What's redundant?

**4. Compare with existing patterns:**
- How does existing code handle this?
- Is there an established pattern?
- Why is this different?

### Questions to Ask

- Is there a global mechanism handling this?
- How do existing similar components handle this?
- What's the execution order?
- Which mechanism actually does the work?
- Can we remove any without breaking functionality?
- Why was this duplicated instead of using existing mechanism?

### Validation

**Check for dead code:**
- If explicit setting always runs, hooks are dead code
- If global mechanism runs, local mechanism may be redundant
- Test by removing suspected redundancy

**Verify with existing patterns:**
- Do other similar components have this redundancy?
- Or do they rely on single mechanism?
- What's the established pattern?

## Test Pressure

### Principle

Every piece of implementation logic should be driven by test requirements, not speculation about future needs. Code without test pressure is likely over-engineered.

### What Is Test Pressure?

**Test pressure** is when tests require specific implementation logic to pass. If you can remove logic without breaking tests, that logic lacks test pressure.

**Benefits:**
- **Regression safety** - Tests verify the logic actually works
- **Refactoring confidence** - Can change implementation knowing tests will catch breaks
- **Avoid over-engineering** - Only implement what's needed
- **Keep code simple** - No speculative complexity

### Red Flags

**Logic without corresponding tests:**
- Conditional branches not exercised by tests
- Error handling paths not tested
- Edge case handling without edge case tests
- Configuration options not tested

**Speculative features:**
- "We might need this later"
- "This makes it more flexible"
- "This handles a case that could happen"
- Without tests demonstrating the need

**Complex logic without test justification:**
- Multiple conditional branches
- Nested logic
- Special case handling
- But tests only cover happy path

### Questions to Ask

- Is there a test that requires this logic?
- What breaks if I remove this code?
- Is this handling a real scenario or a hypothetical one?
- Do tests exercise all branches/paths in this logic?
- Is this complexity driven by test requirements?

### Validation Process

**1. Identify implementation logic:**
- Conditionals (if/else, switch/case)
- Error handling (try/catch, error checks)
- Edge case handling
- Configuration-based behavior

**2. Find corresponding tests:**
- Is there a test that exercises this branch?
- Is there a test that requires this error handling?
- Is there a test for this edge case?
- Is there a test for each configuration option?

**3. Check if logic is justified:**
- Remove the logic mentally - what test would fail?
- If no test would fail → logic lacks test pressure
- If test would fail → logic has test pressure

**4. Evaluate necessity:**
- Is this logic required by current tests?
- Or is it speculative "just in case" code?
- If speculative, should it be removed or should tests be added?

### Examples

**Example 1: Logic with test pressure ✅**

```
Implementation:
  if input.empty?
    raise ValidationError, "Input cannot be empty"
  end
  
Test:
  it 'rejects empty input' do
    expect { process('') }.to raise_error(ValidationError, /cannot be empty/)
  end
```

**Test pressure:** Test requires the empty check. Removing it breaks the test.

---

**Example 2: Logic without test pressure ❌**

```
Implementation:
  if input.empty?
    raise ValidationError, "Input cannot be empty"
  elsif input.length > 1000
    raise ValidationError, "Input too long"
  end
  
Tests:
  it 'processes valid input' do
    expect(process('valid')).to eq(result)
  end
```

**No test pressure:** No test exercises the empty or length checks. Could remove both without breaking tests.

---

**Example 3: Speculative complexity ❌**

```
Implementation:
  def process(input, options = {})
    format = options[:format] || 'json'
    timeout = options[:timeout] || 30
    retry_count = options[:retry_count] || 3
    
    case format
    when 'json' then process_json(input)
    when 'xml' then process_xml(input)
    when 'yaml' then process_yaml(input)
    end
  end
  
Tests:
  it 'processes json input' do
    expect(process('{"key": "value"}')).to eq(result)
  end
```

**No test pressure:** Only JSON format is tested. XML, YAML, timeout, and retry options have no test pressure - likely over-engineered.

### Test Data Should Reflect Production Invariants

If the system enforces a prerequisite (e.g., a create action requires an org role before a space role can be assigned), test setup should include that prerequisite — not bypass it. Tests that violate a production-enforced invariant in their setup can pass vacuously: the scenario they test may never actually occur in production, meaning the test proves nothing.

**Check:** Does the test setup reflect constraints the production system enforces? If setup bypasses a system-enforced prerequisite, the test may be validating an impossible state.

### When Speculative Code Is Acceptable

**Rare cases where logic without immediate test pressure is justified:**
- Framework requirements (must implement interface method)
- Defensive programming for external data (validate untrusted input)
- Backward compatibility (maintain old behavior)
- Migration paths (support both old and new)

**But even then:**
- Document why it's needed
- Consider adding tests for completeness
- Mark as intentional (comment or ticket)

### Integration with Test Coverage

**Test coverage** measures what code is executed by tests.
**Test pressure** measures what code is required by tests.

**Difference:**
```
High coverage, low pressure:
- Tests execute the code
- But tests would pass if code was removed
- Code is not justified by tests

High coverage, high pressure:
- Tests execute the code
- Tests would fail if code was removed
- Code is justified by tests
```

**Both matter:**
- Coverage ensures tests exercise code
- Pressure ensures code is necessary

## Project Conventions

### Principle

Understand project-specific patterns before reviewing. Don't assume conventions from other projects.

### What to Learn

**Documentation philosophy:**
- Minimal comments vs verbose documentation
- Where should documentation live (code, README, wiki)
- What requires comments (public APIs, complex algorithms, all code)

**Architectural patterns:**
- How to extend existing components
- Where different types of code belong
- Naming conventions for different layers

**Global mechanisms:**
- What's handled automatically by framework
- What plugins or middleware are active
- What base classes provide

**Code style:**
- Formatting preferences
- Idiom preferences
- Testing patterns

### How to Learn

**Read project documentation:**
- Look for CONTRIBUTING, ARCHITECTURE, or similar docs
- Check for coding standards or style guides
- Review README for project philosophy

**Study existing code:**
- Find similar components
- See how they handle common concerns
- Identify patterns and conventions

**Check configuration:**
- Look for framework configuration
- Check for enabled plugins or middleware
- Review base classes and mixins

### Apply to Review

**When evaluating comments:**
- Do they match project philosophy?
- Are they consistent with existing code?

**When checking naming:**
- Do names follow project conventions?
- Are they consistent with existing patterns?

**When spotting redundancy:**
- What's handled globally in this project?
- What's the established pattern?

**When assessing architecture:**
- Does this match project patterns?
- Is this how existing code handles this?

## Success Criteria

- [ ] Comments explain "why" not "what"
- [ ] Names follow existing codebase conventions
- [ ] No unnecessary redundancy
- [ ] Implementation logic has test pressure (justified by tests)
- [ ] Follows project conventions
- [ ] Consistent with existing codebase patterns
- [ ] Code is self-explanatory where possible
