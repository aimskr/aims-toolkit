---
name: sequential-thinking
description: "순차적 사고, 분석, 문제 해결, 복잡한 문제, 단계별 분석 - Use when solving complex problems. Systematic thinking workflow using Sequential Thinking MCP for multi-step analysis, trade-off evaluation, and architectural decisions."
---

# Sequential Thinking Workflow

## Use Scenarios

1. **Complex Design Decisions**: Architecture selection, tech stack decisions
2. **Multi-step Problem Solving**: Bug root cause analysis, performance optimization strategy
3. **Uncertain Requirements**: Requirements interpretation, approach exploration
4. **Trade-off Analysis**: Comparing pros and cons of multiple options
5. **Refactoring Planning**: Code improvement strategy

## Thinking Process

```
1. Problem Decomposition (estimate total_thoughts)
   ↓
2. Step-by-step Thinking (analyze with each thought)
   ↓
3. Verification/Revision (is_revision: true)
   ↓
4. Hypothesis Formation and Verification
   ↓
5. Final Answer (next_thought_needed: false)
```

## Checklist

**Before Starting:**
- [ ] Is the problem truly complex? (Answer simple questions directly)
- [ ] Would step-by-step thinking help?
- [ ] Estimate initial total_thoughts (5-15)

**During Process:**
- [ ] Does each thought have a clear purpose?
- [ ] Is it logically connected to previous thoughts?
- [ ] When stuck: use revises_thought to reconsider
- [ ] When exploring alternatives: use branch_from_thought
- [ ] If more needed: needs_more_thoughts: true

**At Completion:**
- [ ] Has a satisfactory answer been reached?
- [ ] Has the hypothesis been sufficiently verified?
- [ ] Set next_thought_needed: false

## Effective Usage Tips

- Add thoughts as needed, don't be constrained by initial estimates
- Express uncertainty honestly when things aren't clear
- Reconsider previous decisions using revises_thought
- Use branching for alternative exploration (branch_from_thought, branch_id)
- Ignore information irrelevant to the current step

## What to Avoid

- Using excessive thinking for simple questions
- Prematurely ending the thinking process
- Lack of logical connection to previous thoughts
- Pretending certainty when uncertain

## Integration with Serena MCP

For complex code work:
1. **Sequential Thinking**: Analyze requirements and establish approach strategy
2. **Serena MCP**: Explore code structure and current state
3. **Sequential Thinking**: Analyze exploration results and design modifications
4. **Serena MCP**: Perform actual code modifications
