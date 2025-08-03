# Task List Management - One-Shot Execution

## Execution Mode
- Execute all tasks sequentially without user intervention
- Update task list in real-time as work progresses
- Document all decisions and blockers for human review

## MCP Tool Usage Rules
- **Before every task:** Use `zen:planner` to strategize approach
- **Important decisions:** Use `zen:consensus` to evaluate options
- **Bugs/problems:** Use `zen:debugger` to analyze and solve issues

## TDD Process (Mandatory for Coding)
1. **RED:** Write failing tests first → commit `test: add failing tests for [feature]`
2. **GREEN:** Write minimal code to pass → commit `feat: implement [feature]`
3. **REFACTOR:** Improve code quality → commit `refactor: improve [feature]`

## Commit Protocol
**After EVERY sub-task completion:**
```bash
git add . && git commit -m "type: description" -m "- Detail 1" -m "- Detail 2" -m "Sub-task: [identifier]"
```
Types: `feat|fix|test|refactor|docs|style|perf|chore`

## After-Tasks-List Documentation
Create `after-tasks-list.md` as an interactive review document for generating follow-up tasks:

### Format as Questions for LLM Processing
Structure each item as a question that can be answered to generate new tasks:
**Note:** Use `zen:consensus` before documenting architecture decisions or major trade-offs

```markdown
# After-Tasks Review - Interactive Questions

## Architecture Decisions
1. **Authentication Method**
   - Current: Implemented OAuth2 flow with Google provider
   - Question: Should we also support JWT tokens for API access?
   - Options: [Keep OAuth2 only | Add JWT | Replace with JWT]
   - Impact: Auth middleware and user session handling

2. **Database Choice**  
   - Current: Used PostgreSQL-specific JSON columns
   - Question: Should we abstract to support MySQL/SQLite?
   - Options: [Keep PostgreSQL only | Add abstraction layer | Use ORM]
   - Impact: Query builders and migration files

## Mocked Services - Integration Needed
1. **Payment Processing**
   - Current: Mock returns success with fake transaction ID
   - Question: Which payment provider should we integrate?
   - Options: [Stripe | PayPal | Square | Other: ___]
   - Files to update: `payment_service.js`, `checkout_controller.js`

2. **Email Service**
   - Current: Console.log outputs email content
   - Question: Which email service should we use?
   - Options: [SendGrid | AWS SES | SMTP | Other: ___]
   - Files to update: `mailer.js`, `notification_service.js`

## Blocked Tasks - Information Required
1. **API Rate Limiting**
   - Question: What should the rate limits be per endpoint?
   - Needed: [Requests per minute | Daily limits | User tiers]
   - Blocking: Rate limiter middleware implementation

2. **Deployment Configuration**
   - Question: What is the target deployment environment?
   - Needed: [AWS | GCP | Azure | Self-hosted]
   - Blocking: CI/CD pipeline setup

## Follow-up Task Generation
Based on answers above, generate new task list with:
- [ ] Integration tasks for selected services
- [ ] Refactoring tasks for architecture changes
- [ ] Configuration tasks for deployment
```

This format enables the LLM to:
1. Present each item as a clear question
2. Show current implementation and options
3. Identify impacted files/components
4. Generate specific follow-up tasks from answers

## Task Implementation
1. Use `zen:planner` before starting each parent task
2. Complete tasks sequentially in order
3. Mark sub-tasks `[x]` immediately upon completion
4. Run tests after each sub-task
5. Commit after each sub-task (not just parent tasks)
6. Mark parent `[x]` only after ALL sub-tasks complete
7. Use `zen:debugger` if tests fail or errors occur

## Auto-Compaction Protocol
When conversation is auto-compacted:
1. Re-read this entire document
2. List and verify all MCP servers (especially zen:planner, zen:consensus, zen:debugger)
3. Review current task list state
4. Check after-tasks-list for pending items
5. Resume from last completed task

## Task List Format
```
- [ ] Parent task
  - [ ] Sub-task 1 → commit
  - [ ] Sub-task 2 → commit
```
Maintain "Relevant Files" section with all created/modified files + descriptions.

## AI Instructions
- Use `zen:planner` before starting each parent task
- Start with first incomplete task, continue until all done
- Never skip TDD cycle for coding tasks
- Use `zen:consensus` for architecture decisions and trade-offs
- Use `zen:debugger` when encountering bugs or test failures
- Document ANY assumption or workaround in after-tasks-list
- If blocked, document why and continue with next possible task
- Only stop when all tasks are completed or documented as blocked
