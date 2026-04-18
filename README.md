# Product Capability Skill (Professional Edition v2.0)

**Turn vague product intent into explicit engineering constraints and implementation-ready capability plans.**

Master the art of translating PRDs, roadmap items, and product discussions into durable capability contracts that survive across harnesses, sessions, and teams.

---

## What's Inside

This professional-grade skill framework covers everything needed to bridge product and engineering:

### Core Concepts
- **When to Use** — Product planning triggers, team scenarios, problem signals
- **Non-Negotiable Rules** — How to avoid common pitfalls in capability planning
- **Canonical Artifact** — Where to store and maintain capability plans
- **Core Workflow** — 4-stage process from intent to implementation

### 4-Stage Workflow
1. **Gather Context** — Read product intent, architecture, constraints
2. **Restate Capability** — Compress into one precise statement
3. **Resolve Constraints** — Extract business rules, invariants, trust boundaries
4. **Define Contract** — Create implementation-ready SRS-style plan

### Detailed Constraint Categories
- **Business Rules** — Non-negotiable, policy-driven requirements
- **Scope Boundaries** — What this does NOT include
- **Invariants** — What must always be true
- **Trust Boundaries** — Who can do what
- **Data Ownership & Lifecycle** — Who owns data, how long retained
- **State Transitions** — What states and allowed transitions
- **Rollout & Migration** — How to deploy without breaking things
- **Failure & Recovery** — How to handle edge cases

### Implementation Contract Details
- **Actors & Surfaces** — Who interacts with this capability
- **States & Transitions** — State machine and lifecycle
- **Interfaces & Data** — APIs, schemas, data structure changes
- **Security, Billing & Policy** — Non-functional constraints
- **Observability & Operations** — Metrics, dashboards, alerts, operator actions
- **Non-Goals** — What this explicitly does NOT do
- **Open Questions** — Blockers still requiring decisions

### Common Patterns & Examples
- Feature flag rollout strategy
- Breaking API change handling
- Data migration without downtime
- Multi-service coordination

### Quality Checklist
- 13-point verification checklist
- Ready-for-implementation signals
- When to escalate for architecture review
- When to ask for product clarification

---

## Quick Start

### The Problem

You have a PRD but implementation keeps diverging:
- Hidden constraints discovered mid-PR
- Different teams interpret requirements differently
- Senior engineers keep restating assumptions
- No durable artifact for handoff

### The Solution

Create a capability plan:

```markdown
# Team Member Management Capability Plan

## CAPABILITY
Team owners can now invite new members via email instead of manual admin intervention. 
This enables self-service onboarding, reducing setup time from days to minutes.

## CONSTRAINTS

### Business Rules
- Teams can have max 1000 members
- Access revocation takes effect within 5 minutes

### Invariants
- Only team owners can invite
- Invited users must accept before getting access
- Team isolation: Can't see other teams' members

### Data Ownership
- Team owner owns membership data
- Audit logs retained 90 days for compliance

## IMPLEMENTATION CONTRACT

### Actors
- Team owner: Can invite/remove members
- Team member: Can accept/decline invitations
- Admin: Can force-remove, view all logs

### Interfaces
```
POST /api/v1/teams/{id}/members
  Input: {email, role}
  Output: {invitation_id, expires_at}

DELETE /api/v1/teams/{id}/members/{user_id}
  Auth: Requires team.admin
```

### Non-Goals
- This does NOT implement full RBAC (Phase 2)
- This does NOT include SSO provisioning
- This does NOT support bulk upload

## OPEN QUESTIONS
- [OPEN] Should existing members be grandfathered or asked to accept?
- [OPEN] Should pending invitations clear on member removal?

## HANDOFF
Ready for: Architecture review → Implementation
Dependencies: Auth service (existing), Audit logs
```

---

## Key Improvements from v1.0

✅ **Detailed constraint categories** (was brief)  
✅ **Complete 4-stage workflow** (with examples)  
✅ **Implementation contract details** (APIs, data, security)  
✅ **Common patterns section** (feature flags, migrations, breaking changes)  
✅ **Quality checklist** (13 points)  
✅ **Anti-patterns guide** (what NOT to do)  
✅ **Handoff signals** (when ready to code)  
✅ **Connected lanes section** (integration with other workflows)  
✅ **624 lines** (was ~200)

---

## Use Cases

### Feature Planning
- Translate PRD into engineering constraints
- Expose hidden assumptions before coding
- Create durable handoff artifact

### Architecture Review
- Evaluate cross-service implications
- Identify data migration complexity
- Surface performance/reliability concerns

### Multi-Team Coordination
- Define capability contract all teams follow
- Prevent scope drift across teams
- Enable parallel work with clear boundaries

### Scope Negotiation
- Be explicit about what is included/excluded
- Quantify effort and impact
- Guide prioritization discussions

### Migration Planning
- Map old and new systems during transition
- Minimize downtime and risk
- Guide rollout and rollback strategy

---

## Why This Matters

### Without a Capability Plan
- ❌ Implementation drifts from product intent
- ❌ Engineering review reveals hidden constraints mid-PR
- ❌ Different teams interpret the requirement differently
- ❌ Knowledge lives only in people, not documents
- ❌ Handoff is tribal and error-prone

### With a Capability Plan
- ✅ Everyone knows what success looks like
- ✅ Implementation is faithful to intent
- ✅ Reviews focus on execution, not rediscovering scope
- ✅ New team members understand quickly
- ✅ Future changes are scoped against single source of truth

---

## Output Format

Every capability plan includes:

1. **Capability** — One-paragraph restatement
2. **Constraints** — All business rules and invariants
3. **Implementation Contract** — Actors, interfaces, data, security
4. **Non-Goals** — What this explicitly does NOT do
5. **Open Questions** — Blockers requiring decisions
6. **Handoff** — What happens next and who owns it

---

## When to Escalate

### Ready for Direct Implementation
- All constraints explicit and resolvable
- No [OPEN] questions
- APIs and data fully specified
- No architectural concerns

**Next:** Architecture review → Implementation

### Needs Architecture Review
- Cross-service implications unclear
- Data migration complexity not understood
- Performance concerns
- Architectural decision not finalized

**Next:** Senior architect → RFC → Implementation

### Needs Product Clarification
- [OPEN] questions about user behavior
- Scope boundaries unclear
- Conflicting stakeholder requirements
- Business rule not agreed

**Next:** Product team → Updated PRD → Back to capability planning

---

## Quality Checklist

- [ ] Capability statement is concrete enough to implement
- [ ] All business rules and invariants are explicit
- [ ] Trust boundaries and auth/authz are clear
- [ ] Data ownership and lifecycle specified
- [ ] State transitions and edge cases documented
- [ ] API/data structure changes detailed
- [ ] Non-goals are explicit
- [ ] Security, billing, compliance constraints listed
- [ ] Observability and operator requirements specified
- [ ] Open questions marked with [OPEN]
- [ ] Conflicts with existing constraints called out
- [ ] Document stored in version control
- [ ] Implementation path is clear

---

## Anti-Patterns to Avoid

❌ **Mixing capability levels** — Too vague (improve performance vs specific metric)  
❌ **Deferring hard decisions** — TBD on critical behavior  
❌ **Assuming existing knowledge** — Don't assume readers know your systems  
❌ **Skipping non-goals** — Be explicit about scope boundaries  
❌ **Missing observability** — No way to debug or operate  

---

## Connected Workflows

This skill feeds into:
- **project-flow-ops** — Operations setup
- **workspace-surface-audit** — Security audit
- **api-connector-builder** — Integration building
- **dashboard-builder** — User-facing features
- **tdd-workflow** — Test-driven implementation
- **verification-loop** — Verify capability meets intent

---

## Learn More

- **Pragmatic Marketing:** https://www.pragmaticmarketing.com
- **System Design:** https://github.com/donnemartin/system-design-primer
- **Architecture Patterns:** https://docs.anthropic.com

---

## License

MIT License — Use, modify, share freely with attribution.

---

## Version History

**v2.0** (Current) — Enhanced with detailed constraints, workflow, patterns, 624 lines  
**v1.0** — Original Product Capability skill by ECC (~200 lines)

---

**Bridge the gap between product intent and engineering execution.**

