# Platform Systems

**Lifecycle · Access & Permissions · Internal Tools · Controls & Governance**

Product studies exploring the systems underneath customer-facing products: how account states propagate, how permissions are defined, how internal teams operate the platform, and how product rules remain consistent across interconnected services.

My professional experience includes 0→1 platform work spanning accounts, transactions, product data, rewards, internal admin tooling, and financial controls. I'm interested in the places where a simple customer-facing state depends on multiple systems agreeing on what that state actually means.

---

## 01 / Lifecycle

### "Suspended" Is Not One State

A customer or partner account may exist across commerce, billing, rewards, support tooling, and internal operations.

Changing the account to **Suspended** creates a product problem:

**What should actually stop?**

**Account state → Commerce → Billing → Rewards → Access → Operations**

A single label can imply very different behavior across the ecosystem.

| Surface | Product question |
| --- | --- |
| Commerce | Can new transactions be created? |
| Billing | Can existing obligations still be paid? |
| Rewards | Can value be earned, validated, or redeemed? |
| User access | Can users still sign in or view history? |
| Internal tools | Who can suspend or restore the account? |
| Audit | What reason, actor, and timestamp must be retained? |

**The decision principle:** define lifecycle states by their effects, not just their names.

A strong state model makes the downstream behavior explicit so each system does not invent its own interpretation of "suspended."

**What I'd measure:** state propagation failures, manual corrections, support escalations, and time to resolve lifecycle exceptions.

---

## 02 / Access & Permissions

### Account State ≠ User Authority

A business account can be active while an individual user should not have permission to perform every action.

That means product design has to separate:

**Account status → User role → Permission → Action**

Examples:

| Role | Example responsibility |
| --- | --- |
| Business owner | Account-level authority |
| Finance user | Billing and transaction responsibilities |
| Program operator | Rewards or program operations |
| Support user | Investigation and limited intervention |

The product problem is not simply creating roles.

It is defining **who can do what, under which account conditions, and with what consequences**.

**The decision principle:** model authority separately from account lifecycle so access rules remain understandable as the platform grows.

**What I'd measure:** permission-related support issues, unauthorized-action prevention, manual access overrides, and time to provision or change access.

---

## 03 / Internal Products

### External Experience ≠ Complete Product

Customer-facing features often depend on internal teams being able to operate, investigate, and correct the system safely.

An external action may require an internal counterpart:

**Customer action → System state → Operator visibility → Controlled intervention → Audit history**

For example, an internal admin experience may need to support:

- lifecycle changes
- eligibility overrides
- account investigation
- exception resolution
- role management
- reason capture
- audit history

The PM problem is deciding **which operational capabilities must exist for the external product to be supportable at scale**.

Building the customer experience without the operating model underneath it creates hidden manual work and inconsistent decisions.

**The decision principle:** treat internal operators as real product users with defined workflows, permissions, and failure states.

**What I'd measure:** manual work per account, exception resolution time, repeated escalations, and percentage of operational actions completed through supported workflows.

---

## 04 / Product Lifecycle

### Deprecation ≠ Deletion

Platform products often need to distinguish between something no longer available for new use and something that can safely disappear.

A simple lifecycle might be:

**Active → Deprecated → Inactive → Deleted**

Each transition affects different users and systems.

Questions include:

- Can existing customers continue using it?
- Can new customers select it?
- Should it remain visible in historical transactions?
- What happens to downstream references?
- When is deletion actually safe?

**The decision principle:** lifecycle states should preserve history and downstream integrity while still allowing the platform to evolve.

This is especially important for product data, financial records, entitlements, and other objects referenced across multiple systems.

**What I'd measure:** broken downstream references, lifecycle exceptions, manual cleanup, and migration completion.

---

## 05 / Guardrails

### Flexibility Needs Boundaries

Platform systems frequently need configurable behavior: pricing rules, FX handling, eligibility, account controls, or product configuration.

Flexibility without explicit boundaries can create inconsistent outcomes.

The product role is to define:

**Allowed range → Validation rule → Exception path → Ownership**

A guardrail should make invalid states difficult to create while still allowing legitimate operational flexibility.

**The decision principle:** encode important business constraints into the product wherever possible instead of relying on people to remember them.

**What I'd measure:** invalid configuration attempts, production corrections, exception volume, and incidents caused by unsupported states.

---

*The studies above draw from product patterns I've encountered professionally. Companies, systems, and implementation details are generalized or fictionalized.*
