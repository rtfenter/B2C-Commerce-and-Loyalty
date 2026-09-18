# B2C Commerce & Loyalty

### Purchase ≠ Final Transaction

A purchase can look simple to a customer while creating multiple connected forms of value underneath.

When loyalty value is earned, redeemed, and then involved in a return or exchange, the product has to answer a harder question:

> **How should rewards and money unwind when the transaction that connected them changes?**

---

## The Scenario

Consider a fictional loyalty program where:

**1,000 points = $5 reward**

A customer starts with **1,000 points**.

They redeem those points for a $5 reward and use it toward a $20 purchase.

| Purchase | Value |
| --- | ---: |
| Item | $20 |
| Reward applied | $5 |
| Customer pays | $15 |

The customer also earns points from the qualifying purchase.

Later, they return the $20 item and purchase a different item for $10.

From the customer's perspective, this is a straightforward return and new purchase.

Underneath, several forms of value now have to be reconciled.

---

## The Product Problem

The original transaction connected:

**Money paid**

**Loyalty value redeemed**

**New loyalty value earned**

Returning the item means all three need deterministic behavior.

Simply refunding $20 would ignore how the original purchase was funded.

Simply removing earned points would ignore the reward that was redeemed.

Allowing the resulting loyalty balance to become artificially negative pushes system complexity onto the customer.

The product needs to unwind the transaction while preserving where each form of value came from.

---

## Follow the Value

### 1 / Redeem

**1,000 points → $5 reward**

The points become a different form of loyalty value, but their origin still matters if that reward later needs to be reversed.

### 2 / Purchase

**$5 reward + $15 paid → $20 purchase**

The transaction also creates new earned points based on the qualifying purchase.

### 3 / Return

The original purchase is reversed.

That means:

- refund the $15 paid
- reverse the $5 reward
- restore the **1,000 points** that created that reward
- reverse the points earned from the returned purchase

### 4 / New Purchase

The replacement $10 purchase is treated as its own qualifying transaction.

It earns points according to the rules for that purchase rather than inheriting the reward state of the transaction that was returned.

---

## The Product Decision

**Reverse value back to its originating form.**

The $5 reward originated as 1,000 points.

When the transaction using that reward reverses, the loyalty value returns to **1,000 points** rather than becoming an artificial negative balance or disappearing as an absorbed loss.

Likewise, points earned from the returned transaction are reversed because the qualifying activity that created them no longer exists.

The new purchase then creates its own earning event.

---

## Customer Experience vs. System State

The underlying reconciliation is complicated.

The customer experience should not be.

### What the customer needs to understand

**You returned the original item.**  
Your payment was refunded.

**You got your reward value back.**  
The 1,000 points used to create the $5 reward were restored.

**Points from the returned purchase were adjusted.**

**Your new purchase earned points normally.**

### What the product resolves underneath

**Reward lifecycle**  
1,000 points → $5 reward → Applied → Reversed → 1,000 points restored

**Original transaction**  
$20 purchase → Returned → Payment refunded

**Original earning**  
Points earned → Reversed

**New transaction**  
$10 purchase → New earning event

The customer should be able to understand **what changed and why** without needing to understand the ledger mechanics required to make it correct.

---

## Try the Transaction

*[Interactive demo will be linked here.]*

The demo follows the same customer through redemption, purchase, return, and replacement purchase.

At each step it shows:

**Customer View** — what the member sees and needs to understand.

**Product View** — the value transformations and reconciliation rules happening underneath.

The purpose isn't to simulate a loyalty program. It's to make the product decisions behind one transaction visible.

---

## Rules That Need to Stay Deterministic

The same model has to remain predictable beyond the happy path.

| Situation | Product behavior |
| --- | --- |
| Reward used on returned purchase | Restore value to its originating points |
| Points earned on returned purchase | Reverse the earning |
| Partial return | Reverse only value attributable to returned items |
| Promotional earning | Reverse using the earning rules of the original transaction |
| New purchase during return | Treat as a new earning event |
| Refund still processing | Don't represent unresolved value as final |

These rules allow the financial and loyalty states to reconcile without requiring the customer to understand how those systems interact.

---

## Measurement

### Primary

**Reconciliation accuracy**

Returned and adjusted transactions should resolve to the correct financial and loyalty state without manual correction.

### Supporting measures

- Reward adjustment accuracy
- Manual reconciliation rate
- Return-related loyalty support contacts
- Time to final reward state

### Guardrails

- Incorrect customer balances
- Duplicate reward restoration
- Earned value remaining after qualifying activity is reversed
- Loyalty value lost during transaction reversal

---

## What I'd Validate

The system can be financially correct and still create a bad customer experience.

I'd validate:

- Can customers understand why their point balance changed?
- Do they understand that a redeemed reward was restored as points?
- Which adjustments need to be surfaced versus handled silently?
- Does the return experience clearly distinguish the old transaction from the new purchase?
- Where do customers interpret a correct adjustment as lost value?

The goal isn't to expose the reconciliation system.

It's to make the outcome **predictable, explainable, and correct**.

---

*This is a fictionalized product study based on product patterns I've encountered professionally. Point conversion, transactions, companies, and implementation details are illustrative.*
