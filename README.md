# Cube Buildathon · 05 · Recovery Manager

**Round 2 - Individual Build**

> Five agents, one unit, one record that follows it.

A physical product arrives, gets prepped, gets shipped, comes back, and creates operational events along the way. At every stage, a person may make a fast judgment that can be difficult to trace later.

Your job in **Round 2** is to build the **Recovery Manager**: the fifth agent in that chain.

Recovery Manager does not capture images or perform the upstream physical inspection itself. Instead, it reads the evidence produced by the other four Managers, matches that evidence to fee or reimbursement events, and determines whether a recovery claim is supported.

---

## Start Here

This is an **individual Round 2 build**.

You have already selected the Recovery Manager problem statement. Your task is to build one focused, working Recovery Manager agent and submit it as your individual solution.

### Your workflow

1. **Fork this repository** into your own GitHub account.
2. Clone your fork locally.
3. Read this `README.md` and [`RULES.md`](RULES.md).
4. Review the sample data in [`data/`](data/) and the upstream evidence files.
5. Design your Recovery Manager workflow.
6. Build and test your implementation inside your own fork.
7. Evaluate the system using an appropriate methodology.
8. Document your architecture, assumptions, limitations and results.
9. Deploy your solution where applicable.
10. Publish the mandatory LinkedIn post.
11. Submit your final repository and required links through the official submission form.

### Important Round 2 dates

| Date                                | Milestone                             |
| ----------------------------------- | ------------------------------------- |
| **25 September 2026 · 9:00 AM IST** | Round 2 build phase officially begins |
| **27 September 2026**               | Submission form opens                 |
| **1 October 2026 · 6:00 PM IST**    | Final submission deadline             |
| After the deadline                  | Submission form closes permanently    |

### Final submission rules

* The **1 October 2026, 6:00 PM IST** deadline is final.
* The submission form will **not be reopened** after the deadline.
* There is **no resubmission facility**.
* Once you submit, your submission is treated as final.
* All Round 2 code commits forming your submission must be made during the authorised build phase.
* Do not continue making Round 2 code changes after the build-phase cut-off.

---

# Your Problem Statement: Recovery Manager

## Position in the chain

**Step 5 of 5 — Money Back**

## Customer

Anyone being charged fees they may not actually owe, or a seller who has evidence that a reimbursement or recovery claim may be valid.

## What gets recorded

**Claim / recovery decision**

## Who consumes your output

The seller, operations team, finance/recovery team, or whoever reviews and submits the claim through the relevant channel.

---

## The Problem

Operational and fulfilment systems can generate costs when inventory is lost, damaged, incorrectly classified, mis-weighed or otherwise handled incorrectly.

At the same time, sellers may have evidence that supports a reimbursement or dispute but never actually use it.

The underlying problem is not simply finding a fee.

The problem is determining:

> **Does the available evidence support a recovery claim for this charge?**

Today, this kind of work can be handled manually, through agencies, or not at all. That creates delays, missed recoveries and unnecessary operational effort.

---

# What Your Agent Should Do

Your Recovery Manager should be able to:

1. **Ingest a fee, reimbursement or adjustment report.**
2. **Parse the relevant charge records.**
3. **Identify the unit and operational context associated with each charge.**
4. **Match each charge to relevant upstream evidence.**
5. Determine whether the evidence:

   * contradicts the charge,
   * supports the charge,
   * or is insufficient to reach a conclusion.
6. Decide whether a recovery/claim is supported.
7. Calculate or surface the relevant claim amount from the provided report/data.
8. Preserve the evidence supporting the decision.
9. Explicitly state when a claim cannot be supported and why.
10. Return a structured result that another system or operator can consume.

---

# What Recovery Manager Is Not

Recovery Manager is **not a vision agent**.

It does not need to reproduce the image classification work performed by:

* Receiving Manager
* Prep Manager
* Pack Manager
* Returns Manager

Instead, Recovery Manager should consume the evidence those agents produce.

Your value is in the **reasoning and joining layer**:

```text
Fee / Reimbursement Report
          │
          ▼
     Parse Charges
          │
          ▼
     Match by Unit
          │
          ▼
   Read Upstream Evidence
          │
          ▼
 Support / Contradict / Insufficient
          │
          ▼
    Recovery Decision
          │
          ▼
   Claim + Evidence Trail
```

---

# The Commerce Chain

Recovery Manager sits at the end of a five-stage operational chain:

```text
Supplier Delivery
      │
      ▼
┌──────────────────────┐
│ 01 · Receiving       │
│ Condition on arrival │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ 02 · Prep            │
│ Compliance / proof   │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ 03 · Pack            │
│ Contents / seal      │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ 04 · Returns         │
│ Condition / outcome  │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ 05 · Recovery        │
│ Evidence → claim     │
└──────────────────────┘
```

The first four Managers generate operational evidence.

Recovery Manager turns that evidence into a financial/recovery decision.

---

# Important: Round 2 Is Individual

Although Recovery Manager is part of a larger five-agent system, **Round 2 is not a group build**.

You are responsible for your own:

* implementation,
* agent logic,
* evaluation,
* evidence handling,
* documentation,
* deployment,
* demo,
* repository,
* and submission.

Do not wait for another participant to complete your build.

The cross-manager evidence contract for Round 2 is provided by the organisers. Use the official contract as the baseline rather than negotiating or inventing a different contract.

---

# Reference Data

The `data/` directory contains synthetic sample data for development and testing.

The primary Recovery dataset includes:

```text
data/
├── fee_report_sample.csv
└── upstream/
    ├── receiving_sample.csv
    ├── prep_sample.csv
    ├── pack_sample.csv
    └── returns_sample.csv
```

The sample data is provided to help you understand how Recovery connects to the other four stages.

## The data is synthetic

The following are invented:

* SKUs
* ASINs
* FNSKUs
* orders
* suppliers
* operators
* requirement flags
* fee amounts
* reimbursement amounts

Do **not** treat the sample values as real-world Amazon rules or real Amazon fee schedules.

The sample data exists for engineering and development purposes.

Where an authoritative external rule or fee schedule is required, use the relevant authoritative source.

---

# The `unit_id` Connection

All five buildathon repositories use the same conceptual `unit_id` values so that a unit can be followed across the operational chain.

For example:

```text
UNIT-0007

Receiving
   ↓
Prep OR Pack
   ↓
Returns
   ↓
Recovery
```

A unit follows its applicable operational path.

For the supplied sample, a unit is represented through either:

* **FBA / prep flow**, or
* **merchant-fulfilled / 3PL flow**

so a sample unit may have a Prep record or a Pack record rather than both.

Recovery should use these identifiers and the relevant upstream evidence to establish the correct relationship between a fee/charge and the unit involved.

---

# Fee Report Structure

The sample fee report contains fields such as:

| Field             | Meaning                                        |
| ----------------- | ---------------------------------------------- |
| `line_id`         | One line in the channel report                 |
| `report_type`     | Type of report/event                           |
| `unit_id`         | Unit identifier used to join upstream evidence |
| `org_id`          | Organisation identifier                        |
| `sku`             | SKU                                            |
| `fnsku`           | FNSKU                                          |
| `fba_shipment_id` | Shipment identifier where applicable           |
| `order_id`        | Order identifier where applicable              |
| `charge_type`     | Type of charge                                 |
| `quantity`        | Quantity covered by the line                   |
| `amount_usd`      | Synthetic amount in the sample                 |
| `posted_date`     | Date the line was posted                       |

Example charge types in the sample include:

* `inbound_defect_fee`
* `lost_inbound`
* `damaged_in_warehouse`
* `fulfilment_fee_weight_tier`
* `refund_issued_item_not_returned`

These sample values are for development and evaluation design. They are not authoritative external fee definitions.

---

# Upstream Evidence

The `data/upstream/` directory contains copies of the other Managers' sample outputs so you can develop and test your joining logic before Round 3.

The core Recovery question is:

```text
Charge
  +
Matching Unit
  +
Upstream Evidence
  =
Recovery Decision
```

The sample data contains cases where evidence may:

* support a charge,
* contradict a charge,
* or be insufficient/silent.

You must determine the outcome through your own reasoning and implementation.

The supplied data does not contain a ready-made answer key.

---

# Evidence Contract

Recovery Manager is a downstream consumer of structured evidence.

Use the official evidence contract supplied for the Buildathon.

A recovery decision should be traceable to the upstream records that were used to produce it.

Relevant evidence concepts include:

* `record_id`
* `schema_version`
* `organization_id`
* `client_id`
* `agent`
* `subject`
* `captured_at`
* `operator_label`
* `images`
* `checks`
* `outcome`
* `overrides`
* `status`
* `content_hash`

Each check should preserve useful information such as:

* check key,
* verdict,
* confidence,
* detail,
* model/version where applicable,
* latency where applicable.

Your implementation should not merely return:

```json
{
  "claim": true
}
```

A useful Recovery result should make it possible to answer:

```text
Which charge?
Which unit?
Which evidence?
What did the evidence say?
What decision was made?
Why?
What amount is involved?
Was the evidence sufficient?
```

---

# PASS · FAIL · UNCERTAIN

The evidence model uses three important reasoning states.

## PASS

The available evidence supports the condition being evaluated.

## FAIL

The available evidence supports that the condition is not met.

## UNCERTAIN

The available evidence does not support a reliable judgment.

`UNCERTAIN` is not simply a low-confidence PASS.

For Recovery Manager, insufficient or contradictory evidence should result in an appropriate review/uncertain outcome rather than forcing an unsupported claim.

---

# Expected Recovery Decision

Your exact implementation can vary, but the output should make the decision understandable.

A conceptual output could contain:

```json
{
  "charge_id": "LINE-0042",
  "unit_id": "UNIT-0017",
  "charge_type": "inbound_defect_fee",
  "amount_usd": 125.00,
  "decision": "CLAIM",
  "confidence": 0.91,
  "evidence_status": "CONTRADICTED",
  "evidence_records": [
    "EV-0017-REC",
    "EV-0017-PREP"
  ],
  "reason": "Upstream evidence indicates the unit met the relevant requirement.",
  "review_required": false
}
```

This is only a conceptual example.

You may design your own structured schema as long as it is clear, reproducible and traceable to the required evidence contract.

---

# Recommended System Flow

A strong Recovery Manager can be structured around the following flow:

```text
1. Ingest
   ↓
2. Validate report
   ↓
3. Parse charge lines
   ↓
4. Resolve identifiers
   ↓
5. Match unit → evidence records
   ↓
6. Inspect upstream decisions
   ↓
7. Apply recovery reasoning
   ↓
8. Determine:
      CLAIM
      DO NOT CLAIM
      REVIEW / UNCERTAIN
   ↓
9. Attach evidence
   ↓
10. Produce structured output
   ↓
11. Persist decision / trace
```

Think about each stage explicitly instead of hiding everything inside one large model call.

---

# Engineering Expectations

The goal is not simply to make an LLM produce convincing text.

You are building an operational system.

## 1. Tenancy isolation

If your solution stores persistent data, keep organisation/client data properly isolated.

A second organisation should not be able to access another organisation's records simply by guessing an identifier or resource key.

The sample data includes multiple organisation identifiers specifically to encourage this type of testing.

---

## 2. Efficient model usage

If you use an AI model, avoid unnecessary repeated calls.

Group related reasoning when appropriate.

Do not create one expensive model call for every tiny field if those decisions can be handled together safely.

---

## 3. Fail open

A temporary model failure, timeout or dependency failure should not silently discard incoming information.

Persist what you can and move the item into an appropriate pending/review state.

For example:

```text
Input received
      ↓
Model unavailable
      ↓
Persist input/evidence
      ↓
Status = pending / review
```

The system should remain operational rather than losing the record.

---

## 4. UNCERTAIN is a first-class outcome

Do not force every case into:

```text
CLAIM
or
DO NOT CLAIM
```

There will be situations where the evidence is insufficient.

The system should be able to say:

```text
Evidence insufficient → Review required
```

That is a valid operational outcome.

---

## 5. Use authoritative rules

Do not rely on a language model remembering a fee policy from memory.

Where an external channel publishes a relevant requirement or rule, use the authoritative source.

The sample CSV is synthetic and must not be treated as the source of truth.

---

# Evaluation — Recovery Is Different

Recovery Manager is evaluated differently from the vision-oriented Managers.

The primary question is:

> **When Recovery Manager recommends a claim, is that claim actually supported by the available evidence?**

Your evaluation should therefore focus on **claim correctness and precision**.

You are not primarily being judged on image-level classification accuracy.

---

# What Your Evaluation Should Measure

At minimum, evaluate:

### 1. Report parsing

Can your system correctly interpret the incoming report?

### 2. Charge identification

Can it correctly identify the charge type, amount and relevant identifiers?

### 3. Evidence matching

Can it correctly connect the charge to the appropriate unit and upstream evidence?

### 4. Evidence interpretation

Can it determine whether the evidence:

* supports,
* contradicts,
* or is insufficient for the charge?

### 5. Claim reasoning

Can it correctly decide when a claim is justified?

### 6. Uncertainty handling

Can it correctly avoid making unsupported claims?

### 7. Failure modes

Can you explain where and why the system fails?

---

# Primary Metric: Claim Precision

A key metric for Recovery Manager is **claim precision**.

Conceptually:

```text
Claim Precision
=
Correctly Supported Claims
--------------------------
All Claims Recommended
```

Also report, where measurable:

* total charges evaluated,
* claims recommended,
* correctly supported claims,
* incorrectly recommended claims,
* missed recoverable claims,
* review/uncertain rate,
* important failure modes,
* latency,
* cost.

Do not report an impressive-looking accuracy number without explaining how it was calculated.

---

# Evaluation Dataset

Build an evaluation set that is separate from the examples you use while developing the system.

Do not use your final evaluation set as a tuning playground.

Your evaluation should contain a meaningful mix of cases, including:

* clearly supportable claims,
* clearly unsupported claims,
* contradictory evidence,
* missing evidence,
* ambiguous cases,
* different charge types,
* different operational paths.

Where applicable, use unseen/held-out cases for the final measurement.

For Recovery Manager, focus on **charge-level evaluation**, not simply image-level evaluation.

---

# Human Evaluation

Human review is valuable when creating your evaluation set.

For ambiguous or evidence-dependent cases:

1. Have humans independently review the relevant evidence.
2. Record their judgments before running the final agent evaluation.
3. Compare agent results against the human-reviewed outcome.
4. Document disagreement and ambiguity rather than hiding it.

If multiple human reviewers are involved, report their agreement where practical.

---

# What Good Evaluation Looks Like

A good evaluation report lets an evaluator understand:

```text
How many charges?
How many claims?
How many correct?
How many incorrect?
How many uncertain?
How many missed?
What failed?
Why did it fail?
```

A report saying:

> “The model works well.”

is not enough.

A report saying:

> “42 of 48 recommended claims were supported, giving 87.5% claim precision. Most errors came from incomplete upstream evidence matching.”

is much more useful.

---

# Round 2 Evaluation Rubric — 100 Points

Your individual Recovery Manager submission is evaluated using five criteria.

| Criterion                                    |  Points |
| -------------------------------------------- | ------: |
| Problem Understanding & Solution Relevance   |  **15** |
| Agent Functionality & Decision Quality       |  **25** |
| Evaluation, Accuracy & Uncertainty Handling  |  **25** |
| Evidence, Traceability & Engineering Quality |  **20** |
| UX, Demo & Documentation                     |  **15** |
| **TOTAL**                                    | **100** |

---

## 1. Problem Understanding & Solution Relevance — 15 Points

Evaluators look for:

* Clear understanding of the Recovery Manager problem.
* Understanding of the real operational workflow.
* Appropriate scope.
* Sensible assumptions.
* A solution that directly addresses recovery/claim reasoning.

---

## 2. Agent Functionality & Decision Quality — 25 Points

Evaluators look for:

* Working Recovery Manager.
* Correct report parsing.
* Charge identification.
* Evidence matching.
* Claim reasoning.
* Structured outputs.
* Appropriate handling of missing/contradictory evidence.
* Good decision quality.
* Useful edge-case handling.

---

## 3. Evaluation, Accuracy & Uncertainty Handling — 25 Points

Evaluators look for:

* Credible evaluation methodology.
* Measured performance.
* Claim precision.
* Correctness of evidence interpretation.
* False claim analysis.
* Missed claim analysis where measurable.
* Appropriate use of `UNCERTAIN` / review states.
* Reproducible results.
* Clearly documented failure modes.

---

## 4. Evidence, Traceability & Engineering Quality — 20 Points

Evaluators look for:

* Evidence supporting decisions.
* Clear connection between charge and unit.
* Traceability to upstream evidence records.
* Confidence and relevant metadata.
* Decision history/overrides where applicable.
* Clean architecture.
* Reliability and error handling.
* Security and data isolation.
* Maintainability.
* Appropriate AI/model usage.

---

## 5. UX, Demo & Documentation — 15 Points

Evaluators look for:

* Clear workflow.
* Easy-to-understand decision display.
* Evidence visibility.
* Useful human review flow.
* Working demo.
* Clear README.
* Clear architecture documentation.
* Working links.
* Good submission quality.

---

# What You Must Submit

Your final Round 2 submission should include:

## 1. GitHub Repository

Your **own fork** of this repository containing the complete Recovery Manager implementation.

## 2. README.md

Your README should explain:

* problem understanding,
* solution overview,
* setup,
* usage,
* architecture summary,
* assumptions,
* limitations,
* evaluation approach.

## 3. ARCHITECTURE.md

Document:

* system architecture,
* major components,
* data flow,
* evidence flow,
* model/agent usage,
* decision logic,
* important engineering decisions.

## 4. Working Agent

The Recovery Manager should be demonstrable and runnable.

## 5. Evaluation Results

Include:

* evaluation methodology,
* test setup,
* metrics,
* claim precision,
* uncertainty/review handling,
* failure modes,
* relevant measurements.

## 6. Demo Video

Demonstrate the actual working solution.

Show enough of the flow that an evaluator can understand:

```text
Report
  ↓
Charge
  ↓
Evidence matching
  ↓
Reasoning
  ↓
Decision
  ↓
Evidence trail
```

## 7. Deployment URL

Provide the working URL if your implementation is deployed.

## 8. LinkedIn Post

A LinkedIn post is **mandatory** for Round 2.

Your post should:

* mention your selected track,
* explain what you built,
* describe the problem,
* share a meaningful engineering detail/result,
* tag **CodeQuesters**,
* tag **Sydon.AI**.

The official LinkedIn post template will be shared by the organisers.

Include the live LinkedIn post URL in the submission form.

---

# GitHub Workflow

Round 2 uses a **fork-based workflow**.

## Step 1 — Fork

Fork this repository into your own GitHub account.

## Step 2 — Clone

```bash
git clone https://github.com/<your-github-username>/cube-05-recovery-manager.git
cd cube-05-recovery-manager
```

## Step 3 — Build

Make your changes inside your own fork.

## Step 4 — Commit

Use meaningful commit messages.

```bash
git add .
git commit -m "Build Recovery Manager"
git push origin main
```

You may use branches inside your own fork if that helps your development workflow, but the final repository submitted for evaluation must contain the final Round 2 implementation.

## Step 5 — Submit

Submit your fork through the official Cube Buildathon submission form.

You do **not** need to:

* work inside a central shared organiser branch,
* create `submissions/<username>/` in the organiser repository,
* open a PR into the organiser's `main`,
* or wait for organisers to merge your individual implementation.

---

# Commit Rules

Your repository history matters.

### During the build phase

You may:

* build,
* test,
* refactor,
* document,
* deploy,
* commit,
* push.

### After the build phase

Do not continue making Round 2 code changes.

The submitted implementation should reflect work completed during the authorised build phase.

Do not intentionally make post-build changes and submit an earlier state.

---

# No Secrets

Never commit:

* API keys,
* passwords,
* private tokens,
* credentials,
* `.env` files containing secrets,
* private certificates.

Use environment variables and secret-management practices instead.

If a credential is accidentally exposed, revoke it immediately.

---

# Recommended API Interface

The repository does not force a particular application framework.

You are free to choose your implementation architecture.

For solutions exposed as an HTTP API, a simple evaluator-friendly interface is recommended:

```text
POST /agent
```

for the main Recovery Manager operation.

A health endpoint is also recommended:

```text
GET /health
```

These are engineering recommendations, not a substitute for the submission requirements. Your actual interface should be documented clearly in your README and demo.

---

# Suggested Local Development Workflow

A practical build sequence is:

```text
1. Understand the problem
        ↓
2. Inspect sample data
        ↓
3. Define inputs and outputs
        ↓
4. Implement charge parsing
        ↓
5. Implement evidence matching
        ↓
6. Implement claim reasoning
        ↓
7. Add structured decision output
        ↓
8. Add evidence/traceability
        ↓
9. Add error and uncertainty handling
        ↓
10. Evaluate
        ↓
11. Document
        ↓
12. Demo and deploy
        ↓
13. Submit
```

Do not wait until the end to test.

Get a small end-to-end path working early.

---

# What You Should Build First

A sensible minimum flow is:

```text
Input fee report
       ↓
Parse one charge
       ↓
Resolve unit_id
       ↓
Find upstream evidence
       ↓
Evaluate evidence
       ↓
Return:
   CLAIM
   DO NOT CLAIM
   REVIEW / UNCERTAIN
       ↓
Show evidence and reasoning
```

Once that works, expand the system.

---

# Minimum Working Quality

A strong minimum implementation should be able to:

* accept or load a fee/reimbursement report,
* identify charge records,
* match them to units,
* retrieve upstream evidence,
* reason over that evidence,
* produce a structured decision,
* preserve supporting evidence,
* handle insufficient evidence,
* and produce a measurable evaluation result.

A simple working system with strong evidence and evaluation is more valuable than a large system that cannot be demonstrated reliably.

---

# Honesty Rules

## Say what you built

Describe capabilities accurately.

Do not claim:

* immutable records,
* tamper-proof records,
* production-grade systems,
* autonomous recovery,
* guaranteed accuracy,

unless you actually implemented and demonstrated those properties.

---

## Overrides are data

If a human reviewer disagrees with an agent decision, preserve the original decision, the revised decision and the reason where your system supports overrides.

Do not silently erase disagreement.

---

## Numbers matter

Do not write:

> “Very accurate.”

Write the actual result and methodology.

For example:

```text
48 charges evaluated
44 claims recommended
38 correctly supported
6 incorrectly recommended
Claim precision = 86.36%
```

Then explain the failure modes.

---

## Contradictions are findings

If the supplied documentation or datasets contain inconsistencies, identify them.

Do not silently choose whichever version is convenient.

---

# Common Mistakes to Avoid

* Building a generic chatbot instead of a Recovery Manager.
* Re-running image classification instead of consuming upstream evidence.
* Ignoring the relationship between charges and `unit_id`.
* Treating synthetic fee values as real-world fee rules.
* Making unsupported claims.
* Forcing every case into CLAIM or DO NOT CLAIM.
* Ignoring missing evidence.
* Ignoring contradictory evidence.
* Reporting only positive examples.
* Skipping evaluation.
* Building a sophisticated UI before the core reasoning works.
* Committing secrets.
* Making code changes outside the authorised build phase.
* Forgetting the mandatory LinkedIn post.
* Submitting without checking all required links.
* Assuming a submitted entry can be replaced later.

---

# Round 2 → Round 3

Round 2 is about your **individual Recovery Manager**.

Round 3 is about **system integration**.

Selected participants will form five-person Pods containing:

```text
Receiving Manager
       +
Prep Manager
       +
Pack Manager
       +
Returns Manager
       +
Recovery Manager
```

The Pod's challenge is to integrate the five specialised agents into one connected end-to-end commerce system.

That means the quality of your Round 2 implementation matters beyond this repository.

Good evidence structures, predictable outputs, clear interfaces and understandable engineering decisions make the Round 3 integration easier.

---

# Why Round 2 Matters

Round 2 is scored out of **100 points**.

For participants selected for Round 3:

```text
Round 2 Score
      +
Round 3 Score
      =
Final Combined Score
```

Your Round 2 performance therefore contributes directly to the final result.

Build your individual solution as though another engineer will need to integrate it later.

---

# Submission Deadline

## 1 October 2026 · 6:00 PM IST

The submission form closes permanently at this time.

### There is:

* no reopening,
* no extension window,
* no resubmission.

Before submitting, verify every required field and link.

---

# Final Submission Checklist

Before submitting, confirm:

* [ ] Correct track: Recovery Manager
* [ ] Working implementation
* [ ] GitHub fork contains final code
* [ ] Required code commits were made during the authorised build phase
* [ ] README.md complete
* [ ] ARCHITECTURE.md complete
* [ ] Evaluation methodology documented
* [ ] Evaluation results documented
* [ ] Claim precision reported
* [ ] Failure modes documented
* [ ] UNCERTAIN/review handling demonstrated
* [ ] Evidence traceability demonstrated
* [ ] Demo video works
* [ ] Deployment URL works, if applicable
* [ ] LinkedIn post published
* [ ] CodeQuesters tagged
* [ ] Sydon.AI tagged
* [ ] LinkedIn URL copied correctly
* [ ] All submission-form fields completed
* [ ] Final repository checked
* [ ] No missing links/files
* [ ] Submission will be completed before **1 October 2026 · 6:00 PM IST**

---

# Final Principle

The goal is not to build the biggest Recovery Manager.

The goal is to build one that can answer a simple operational question reliably:

> **Should this charge be recovered, and can you prove why?**

Build the reasoning carefully.

Trace every important decision.

Measure the result.

Document the failures.

Then submit with confidence.

---

**Cube Buildathon · 05 · Recovery Manager**

**Round 2 — Individual Build**

**Build → Test → Measure → Document → Publish → Submit**

*Don’t just build AI. Engineer it.*
