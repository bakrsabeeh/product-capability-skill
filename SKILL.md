---
name: product-capability-professional
description: Professional-grade skill for translating product intent, PRDs, roadmap items, and product discussions into explicit implementation-ready capability plans. Exposes constraints, invariants, interfaces, architectural decisions, and unresolved questions before multi-service work starts. Use when you need a durable, reusable capability contract that survives across harnesses, sessions, and teams.
origin: ECC
tier: professional
version: 2.0
---

# Product Capability Skill (Professional Edition v2.0)

Turn vague product intent into explicit engineering constraints and implementation-ready capability plans.

---

## When to Activate

**Product Planning Triggers:**
- A PRD, roadmap item, discussion, or founder note exists but implementation constraints are still implicit
- Product intent is clear but architecture, data, lifecycle, or policy implications are fuzzy
- A feature crosses multiple services, repos, or teams and needs a capability contract before coding
- Senior engineers keep restating the same hidden assumptions during reviews
- You need a reusable artifact that survives across harnesses and sessions

**Team Scenarios:**
- Starting a new product initiative or feature
- Handoff from product to engineering
- Architecture review before implementation
- Multi-team coordination on a shared capability
- Scope negotiation with stakeholders
- Migration or breaking change planning

**Problem Signals:**
- Implementation drifts from product intent mid-PR
- Engineering review reveals hidden constraints
- Meetings keep circling back to "what does this really mean?"
- Different teams interpret the requirement differently
- Architects argue about whether it's a product or engineering decision

---

## Non-Negotiable Rules

### 1. **Do Not Invent Product Truth**
- Mark unresolved questions explicitly with [OPEN]
- Source every constraint to a document, conversation, or decision
- If something is assumption, label it [ASSUMPTION]
- If something conflicts with existing policy, call it out clearly

### 2. **Separate User-Visible Promises from Implementation Details**
- User-facing behavior ≠ technical implementation
- Same capability can be built multiple ways
- Be explicit about what is a promise and what is a design choice

### 3. **Distinguish Between Fixed Policy, Architecture Preference, and Open Decisions**
- Fixed policy: Non-negotiable, business-driven (e.g., "SOC2 compliance required")
- Architecture preference: Chosen based on constraints but could be different (e.g., "uses async job queue")
- Open decision: Deliberately unresolved pending implementation or further discovery

### 4. **If the Request Conflicts with Existing Constraints, Say So Clearly**
- Don't smooth over conflicts
- State: "Requested capability conflicts with existing [constraint] because [reason]"
- Offer options: "Could proceed if we [option 1], [option 2], or [option 3]"

### 5. **Prefer One Reusable Capability Artifact Over Scattered Ad Hoc Notes**
- Single source of truth
- Survives across sessions and team changes
- Can be linked from PRs, RFCs, design docs
- Enables handoff without tribal knowledge

---

## Canonical Artifact Location

### Durable Home

If the repo has an existing product-context file:
- `PRODUCT.md` (root level)
- `docs/product/` (directory)
- `docs/capabilities/` (capability-specific)
- `docs/architecture/capabilities.md` (architecture-focused)

If no capability manifest exists:
- Create one using the template below
- Place at: `docs/product/[capability-name].md`
- Link from main README

### Version Control & Updates

- Store in git alongside code
- Link from relevant PRs and design docs
- Update when product scope changes
- Review alongside architecture decisions

---

## Core Workflow

### Stage 1: Gather Context (Read-Only Phase)

Read only what you need:

**Product Intent:**
- PRD or RFC (if exists)
- Product brief or feature description
- Founder/PM notes or discussions
- Roadmap context
- User stories or use cases

**Current Architecture:**
- Relevant service contracts
- Database schemas
- API surfaces and routes
- Existing workflows and state machines
- Data ownership and boundaries

**Existing Capability Context:**
- PRODUCT.md or equivalent
- Design docs and RFCs
- Migration notes or operating-model docs
- Compliance and policy docs
- Performance and reliability docs

**Delivery Constraints:**
- Authentication and authorization requirements
- Billing/monetization implications
- Compliance and regulatory requirements
- Rollout strategy (gradual, feature flag, etc.)
- Backwards compatibility requirements
- Performance SLAs
- Code review and approval policies

### Stage 2: Restate the Capability (Clarity Phase)

Compress the entire ask into one precise statement:

**Template:**
```
[Actor] can now [action], which enables them to [outcome].
This changes [what was previously hard] to [what is now possible].
```

**Examples:**

BAD (vague):
"Users can now manage their team's access"

GOOD (precise):
"A team owner can now invite new team members via email and revoke individual access without leaving the team settings page. This changes team access from manual admin intervention to self-service, reducing onboarding time from days to minutes and enabling teams to manage membership without engineering involvement."

BAD (missing outcome):
"API now supports pagination"

GOOD (with outcome):
"The list-users API now supports cursor-based pagination with configurable page size up to 1000. This allows clients to efficiently retrieve large result sets without loading everything into memory, enabling the web dashboard to handle teams with 10,000+ members without browser lag."

**Clarity Check:**
If you can't restate it in 2-3 sentences, the implementation will drift.

### Stage 3: Resolve Capability Constraints (Constraint Extraction Phase)

Extract the constraints that must hold before implementation. These live in senior-engineer memory and cause issues when rediscovered mid-PR.

**Categories of Constraints:**

**Business Rules (Non-Negotiable)**
- Pricing/quota/rate-limit rules
- Data retention or deletion policies
- Compliance or regulatory requirements
- SLA and uptime commitments
- User-facing behavior guarantees

**Scope Boundaries (What This Does NOT Include)**
- Related features explicitly excluded
- Future extensibility assumptions
- Phase 2 / future work
- Adjacent systems intentionally not touched

**Invariants (Must Always Be True)**
- Consistency guarantees (strong, eventual, ???)
- Data ownership rules
- Team isolation or multi-tenancy boundaries
- Audit or compliance logging
- Identity and permission invariants

**Trust Boundaries (Who Can Do What)**
- Authentication: How is the user verified?
- Authorization: What permissions are required?
- Cross-team or cross-org boundaries
- Service-to-service trust model
- Delegation rules (can user A act on behalf of user B?)

**Data Ownership & Lifecycle**
- Who owns the data?
- How long is it retained?
- How is it deleted or archived?
- What triggers archival or purge?
- Can it be exported or migrated?

**State Machine & Lifecycle Transitions**
- What states can the entity be in?
- What transitions are allowed?
- What triggers each transition?
- Can transitions be undone?
- What happens on failure or timeout?

**Rollout & Migration**
- Can old and new versions coexist?
- Is there a feature flag?
- How are existing data migrated?
- What's the rollback plan?
- Are there backwards-compatibility requirements?

**Failure & Recovery**
- What happens if the service fails mid-operation?
- Can operations be retried safely?
- What is idempotent and what isn't?
- How do we recover from partial failures?
- What are the reliability/SLA commitments?

**Example Constraint Extraction:**

```
BUSINESS RULES:
- Teams can have max 1000 members (quota enforcement required)
- Access revocation must take effect within 5 minutes (consistency SLA)
- Audit logs must retain member access history for 90 days

INVARIANTS:
- Only team owners can invite new members
- Invited users must accept before getting access
- Team isolation: Members of team A cannot see team B's members

DATA OWNERSHIP:
- Team owner owns the membership data
- Members cannot be unilaterally removed (must self-remove or owner removes)
- Access logs owned by compliance team, 90-day retention

ROLLOUT:
- Feature flag: team-member-management-v2
- Gradual rollout: 10% → 50% → 100% over 1 week
- Existing team APIs continue to work during transition
```

### Stage 4: Define Implementation-Facing Contract (SRS Phase)

Produce an implementation-ready capability plan with all details needed to code without rediscovery:

#### **Actors & Surfaces**

Who interacts with this capability and how?

```
ACTORS:
- Team owner: Can invite/remove members, view audit logs
- Team member: Can accept/decline invitations, manage own settings
- Admin: Can force-remove members, view all audit logs

SURFACES:
- Web UI: Team settings → Members tab
- API: POST /teams/{id}/members, DELETE /teams/{id}/members/{user_id}
- Webhook: team.member_invited, team.member_removed
- Admin console: Access log viewer
```

#### **Required States & Transitions**

What states can entities be in? What transitions are allowed?

```
MEMBER LIFECYCLE:
Invited → (accept) → Active → (remove/leave) → Removed
         → (decline) → Declined → [no further transitions]

TRANSITIONS:
- Invited → Active: Member accepts invitation via email link
- Invited → Declined: Member declines via email link
- Active → Removed: Owner removes or member self-removes
- Invited expires after 7 days (auto-decline)

INVARIANTS:
- Transition to Active requires identity verification
- Transition to Removed is permanent for audit purposes
- Only one Invited state per email address at a time
```

#### **Interface / Data Implications**

What APIs, data structures, or contract changes are needed?

```
NEW ENDPOINTS:
POST /api/v1/teams/{id}/members
  Input: {email, role: "owner"|"member"}
  Output: {invitation_id, expires_at, sent_at}
  Auth: Requires team.admin

DELETE /api/v1/teams/{id}/members/{user_id}
  Input: {reason?: string}
  Output: {user_id, removed_at, archived: true}
  Auth: Requires team.admin or is self

GET /api/v1/teams/{id}/members
  Output: {members: [{user_id, email, role, joined_at, status}]}
  Auth: Requires team.member

DATA SCHEMA CHANGES:
- Add teams.member_count (denormalized for quota checking)
- Add team_members.status enum: invited|active|declined|removed
- Add team_members.invited_at, accepted_at, removed_at timestamps
- Add team_audit_logs table for compliance

BACKWARDS COMPATIBILITY:
- Existing teams.members JSON field continues to work
- New endpoints are alongside old ones during transition
- Feature flag gates new behavior
```

#### **Security & Policy Constraints**

What security, billing, or policy constraints apply?

```
SECURITY:
- Email invitations sent over TLS only
- Invitation tokens are single-use and time-limited (7 days)
- Removed members cannot rejoin without owner invite
- Audit logs immutable once written
- Rate limit: 100 invitations per hour per team

BILLING:
- Team member count affects pricing tier
- Billing updated when member state changes (not invited, actual)
- Retroactive seat pricing if adding many members at once

COMPLIANCE:
- Access changes logged for SOC2 audit trail
- PII (emails) encrypted at rest
- Removed members' data available for GDPR deletion request
- Data retention: 90 days for audit logs, 180 days for removed-member records
```

#### **Observability & Operator Requirements**

What must operators see and be able to do?

```
METRICS:
- team_member_count (gauge, per team)
- member_invitation_rate (rate, per team)
- member_acceptance_rate (ratio)
- invitation_expiry_rate (rate)

DASHBOARDS:
- Team membership status (invite pending, active, etc.)
- Member activity feed (who joined/left when)
- Quota usage (approaching 1000 member limit)

ALERTS:
- Invitation acceptance rate drops below 50%
- Team approaches 900+ members
- Audit log write errors

OPERATOR ACTIONS:
- View all members of a team
- Force-remove a member
- Resend invitation
- Clear pending invitations
- View audit logs
```

#### **Non-Goals (What This Does NOT Do)**

Be explicit about scope boundaries:

```
NON-GOALS:
- This does not implement role-based access control (RBAC) beyond owner/member
  → Full RBAC is planned for Q3 as separate capability
- This does not support SSO provisioning
  → Manual invitations only; SSO sync is future work
- This does not include bulk member upload
  → Single-user invitation only; bulk import planned for Phase 2
- This does not implement nested teams or team hierarchies
  → Flat team structure only
- This does not auto-revoke access on domain leave
  → Manual revocation required; domain linking is future work
```

---

## Complete Output Format

When producing a capability plan, use this structure:

```markdown
# [Capability Name] Capability Plan

## CAPABILITY

[One paragraph restatement with actor, action, and outcome]

## CONSTRAINTS

### Business Rules
- [constraint 1]
- [constraint 2]

### Invariants
- [invariant 1]
- [invariant 2]

### Data Ownership & Lifecycle
- [ownership rule]
- [retention policy]

## IMPLEMENTATION CONTRACT

### Actors & Surfaces
[Who interacts with this and how]

### Required States & Transitions
[State machine]

### Interfaces & Data
[APIs, schemas, data structure changes]

### Security, Billing & Policy
[Security constraints, billing implications, compliance requirements]

### Observability & Operations
[Metrics, dashboards, alerts, operator actions]

## NON-GOALS

[What this explicitly does not include]

## OPEN QUESTIONS

[Issues blocking implementation]

## HANDOFF

- Ready for: [implementation, architecture review, product clarification]
- Next lane: [project-flow-ops, workspace-surface-audit, etc.]
- Dependencies: [services/teams needed for implementation]
- Estimated effort: [T-shirt sizing]
```

---

## Common Patterns & Examples

### Pattern: Feature Flag Rollout

```
CAPABILITY:
Teams can now manage member access via self-service UI instead of manual admin process.

CONSTRAINT (Rollout):
- Feature must be rolled out gradually using feature flag
- Phase 1 (10%): Private beta with 5 pilot teams
- Phase 2 (50%): Staged rollout over 1 week
- Phase 3 (100%): Full release
- Rollback plan: Disable flag, revert to old UI

INVARIANT:
- Old and new systems must coexist during transition
- No data migration required (both read/write same tables)
- Users may see either UI depending on phase
```

### Pattern: Breaking API Change

```
CAPABILITY:
The users API now returns {user_id, email, ...} instead of {id, email_address, ...}

CONSTRAINT (Backwards Compatibility):
- Old field names must still work during deprecation period
- Both old and new names returned in response
- Documentation updated first (1 week before change)
- Deprecation warning sent to API clients (2 weeks notice)
- Old names removed after 3-month deprecation window

INVARIANT:
- No request data changes required (backwards compatible)
- Response includes both old and new names
- Breaking removal happens only after public notice period
```

### Pattern: Data Migration

```
CAPABILITY:
Team metadata migrated from teams.config JSON to structured team_metadata table.

CONSTRAINT (Migration):
- Background migration runs continuously
- Old and new schemas both updated during transition
- Feature flag gates new schema reads/writes
- Rollback: Always readable from old schema
- No downtime required

INVARIANT:
- Data consistency: Old and new schemas stay in sync
- Migration is idempotent (safe to re-run)
- All team operations work before, during, and after migration
```

---

## Quality Checklist

Before finalizing a capability plan, verify:

- [ ] Capability statement is concrete enough to implement without questions
- [ ] All business rules and invariants are explicit
- [ ] Trust boundaries and authentication/authorization are clear
- [ ] Data ownership and lifecycle are specified
- [ ] State transitions and edge cases are documented
- [ ] API/data structure changes are detailed
- [ ] Non-goals are explicit (what this does NOT do)
- [ ] Security, billing, and compliance constraints are listed
- [ ] Observability and operator requirements are specified
- [ ] Open questions are marked with [OPEN]
- [ ] Conflicts with existing constraints are called out
- [ ] Document is stored in version control, not ephemeral
- [ ] Implementation path is clear (ready to code, or needs more work)

---

## Handoff Signals

### Ready for Direct Implementation

- All constraints are explicit and resolvable
- No [OPEN] questions blocking the work
- APIs and data structures are fully specified
- Team has necessary access and permissions
- No architectural concerns remaining
- Estimated effort is clear

**Handoff to:** Architecture review → Implementation lane (e.g., `project-flow-ops`)

### Needs Architecture Review First

- Cross-service implications not fully thought through
- Data migration complexity unclear
- Performance or reliability concerns
- Architectural decision not finalized
- Potential for technical debt or rework

**Handoff to:** Senior architect → Architecture RFC → Implementation

### Needs Product Clarification First

- [OPEN] questions about user behavior
- Scope boundaries unclear
- Conflicting requirements from stakeholders
- Product decision not finalized
- Business rule not agreed upon

**Handoff to:** Product team → Updated PRD → Back to capability planning

---

## Connected ECC Lanes

This skill feeds into and connects with:

- **project-flow-ops** — Operations and infrastructure setup after capability plan
- **workspace-surface-audit** — Audit and secure the capability once built
- **api-connector-builder** — Build integrations for the capability
- **dashboard-builder** — Build user-facing features for the capability
- **tdd-workflow** — Test-driven implementation of the capability
- **verification-loop** — Verify the capability meets the original intent

---

## Anti-Patterns to Avoid

### ❌ Mixing Capability Levels

Don't write a capability plan for "improve performance" (too vague).  
Write one for "reduce team member list load time from 3s to <500ms by implementing pagination" (specific).

### ❌ Deferring Hard Decisions

Don't write "TBD how we handle concurrent member removal" (unresolved).  
Write "concurrent removals use last-write-wins; audit logs show all removals in order" (explicit).

### ❌ Assuming Existing Knowledge

Don't assume reviewers know "how our auth system works."  
Write "authentication via JWT in Authorization header; team membership checked via admin role" (explicit).

### ❌ Skipping Non-Goals

Don't assume what you won't do is obvious.  
Write "This does NOT include SSO or bulk import" (explicit scope boundary).

### ❌ Missing Observability

Don't build features with no way to debug or operate them.  
Include metrics, dashboards, alerts, and operator actions.

---

## Why This Matters

Without a capability plan:
- ❌ Implementation drifts from product intent
- ❌ Engineering review reveals hidden constraints mid-PR
- ❌ Different teams interpret the requirement differently
- ❌ Knowledge lives only in people, not documents
- ❌ Handoff is tribal and error-prone

With a capability plan:
- ✅ Everyone knows what success looks like
- ✅ Implementation is faithful to intent
- ✅ Reviews focus on execution, not rediscovering scope
- ✅ New team members can understand quickly
- ✅ Future changes are scoped against a single source of truth

