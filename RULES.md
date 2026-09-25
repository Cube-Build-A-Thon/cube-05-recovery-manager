# Rules

These rules define how you should build, evaluate and submit your **Recovery Manager** for **Cube Buildathon — Round 2**.

Round 2 is an **individual build**. Your implementation will be evaluated on the quality of the solution, engineering practices, evidence handling, evaluation methodology and final submission.

---

# 1. Round 2 Participation Rules

## R1 — Individual Build

Round 2 is an **individual build**.

Each participant must independently build the **Recovery Manager** for the problem statement they selected.

Do not build or submit another participant's solution.

---

## R2 — Work in Your Own Fork

Participants must **fork the official Recovery Manager repository** into their own GitHub account.

Your fork is your individual development and submission repository for Round 2.

The intended workflow is:

```text
Official Repository
        ↓
      Fork
        ↓
   Your GitHub Repo
        ↓
   Build + Test
        ↓
 Commit + Push
        ↓
   Final Submission
```

You do not need to work inside the organiser's central repository.

---

## R3 — No Shared-Repository Workflow

The Round 2 process does **not** require:

* creating a participant branch in the official organiser repository,
* creating `submissions/<your-github-username>/`,
* opening a pull request into the organiser's `main`,
* waiting for organiser approval or merge,
* or using a shared participant branch.

Your own fork is where you build your solution.

---

## R4 — Build Only the Selected Problem

Your Round 2 submission must focus on:

**05 · Recovery Manager**

Do not dilute the solution by attempting to build all five Managers.

Recovery Manager is the downstream reasoning layer that works with fee/recovery events and upstream operational evidence.

---

## R5 — Build-Phase Commits Only

All code commits that form your Round 2 submission must be made during the **authorised Round 2 build phase**.

Once the build phase ends:

* do not continue making Round 2 code changes,
* do not add new implementation features,
* do not silently replace the submitted implementation with a later version.

Your submitted repository should reflect work completed during the authorised build phase.

---

## R6 — Final Submission Deadline

The final Round 2 submission deadline is:

**1 October 2026 · 6:00 PM IST**

The submission form closes permanently at this time.

There will be:

* no reopening of the form,
* no extension through the submission form,
* no resubmission facility.

Plan your build accordingly.

---

## R7 — No Resubmission

Once you submit the official Round 2 submission form, your submission is **final**.

You cannot replace the submission with another repository, updated codebase, revised video or new links after submission.

Verify everything before clicking Submit.

---

## R8 — No Secrets

Never commit sensitive credentials to the repository.

This includes:

* API keys,
* access tokens,
* passwords,
* private credentials,
* secret keys,
* `.env` files containing secrets.

Use environment variables and appropriate secret-management practices.

If a secret is accidentally exposed, revoke it immediately.

---

# 2. Recovery Manager Engineering Rules

These rules are part of the technical assessment.

## 1. Tenancy Isolation

If your application stores persistent data, organisation and client data must be logically isolated.

A user belonging to one organisation must not be able to access another organisation's records simply by guessing an identifier or resource key.

Test the isolation behavior in your implementation.

The sample data includes multiple organisation identifiers to encourage this type of testing.

---

## 2. Efficient Model Usage

Use model calls efficiently.

Avoid unnecessary repeated model calls when the same reasoning can be performed safely within a smaller number of well-structured calls.

For example, do not create multiple independent model requests for information that can be evaluated together when doing so adds unnecessary latency or cost.

Your architecture should make the model's role clear.

---

## 3. Fail Open

A model failure, timeout or temporary dependency failure must not silently discard the incoming report or evidence.

Where possible:

```text
Input Received
      ↓
Dependency Failure
      ↓
Persist Available Information
      ↓
Pending / Review State
```

The system should preserve the available information and allow the case to be handled later.

Do not silently lose records because an external dependency failed.

---

## 4. `UNCERTAIN` Is a Valid Outcome

`UNCERTAIN` is a first-class reasoning state.

It should be used when the available evidence does not support a reliable decision.

Do not force every case into:

```text
CLAIM
or
DO NOT CLAIM
```

When evidence is missing, contradictory or insufficient, an appropriate review/uncertain state may be the correct result.

`UNCERTAIN` is not simply a low-confidence claim.

---

## 5. Use Authoritative Rules

Where an external channel publishes an authoritative requirement, rule or fee policy, retrieve and use the authoritative information.

Do not rely on a model's memory of a rule.

Do not infer official rules from the sample CSV files.

The sample fee amounts, flags and other values in this repository are **synthetic development data**.

They must not be treated as authoritative external fee schedules.

---

# 3. Recovery Manager Evidence Rules

Recovery Manager is a downstream consumer of evidence generated by the other Managers.

## 1. Use the Official Evidence Contract

For Round 2, use the **official evidence contract provided by the organisers** as the baseline interoperability contract.

Do not invent a completely different cross-manager contract for your Round 2 implementation.

The evidence structure should make the upstream decision traceable.

---

## 2. Preserve Evidence

A Recovery decision should be traceable back to the evidence used to reach that decision.

A reviewer should be able to understand:

```text
Which charge?
      ↓
Which unit?
      ↓
Which upstream evidence?
      ↓
What did the evidence indicate?
      ↓
What decision was made?
      ↓
Why was that decision made?
```

---

## 3. Important Evidence Fields

The official evidence contract includes concepts such as:

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

Where applicable, checks may contain:

* `check_key`
* `verdict`
* `confidence`
* `detail`
* `model_version`
* `latency_ms`

Use the relevant fields needed to make your implementation traceable.

---

## 4. Overrides Are Data

If an operator disagrees with the agent's decision and your system supports an override, preserve:

* the original decision,
* the updated decision,
* and the reason for the override.

Do not silently erase the original decision.

A human disagreement is useful operational information.

---

# 4. Recovery Decision Rules

Recovery Manager should reason about whether a charge is supported by the available evidence.

A useful conceptual outcome is:

```text
CLAIM
DO NOT CLAIM
REVIEW / UNCERTAIN
```

The exact implementation and naming may vary, but the meaning must be clear.

## CLAIM

Use when the available evidence supports the recovery claim.

## DO NOT CLAIM

Use when the available evidence supports that the charge should not be recovered.

## REVIEW / UNCERTAIN

Use when the evidence is insufficient, contradictory or otherwise does not support a reliable automated conclusion.

The system should clearly explain the evidence behind the selected state.

---

# 5. Charge-to-Evidence Matching

Recovery Manager should correctly connect:

```text
Fee / Charge
      ↓
Relevant Unit
      ↓
Upstream Evidence
      ↓
Recovery Reasoning
```

Where a `unit_id` or another identifier is available, use it appropriately to establish the relationship.

Do not assume that every upstream record applies to every charge.

The implementation should account for:

* missing records,
* mismatched identifiers,
* multiple records,
* contradictory evidence,
* incomplete evidence,
* and ambiguous cases.

---

# 6. Evaluation Rules

Evaluation is part of the assessment.

Do not rely only on screenshots or a small collection of examples that make the system appear successful.

Your evaluation should measure whether Recovery Manager actually makes useful recovery decisions.

---

## 1. Evaluate Charge-Level Performance

Recovery Manager is different from the vision-oriented Managers.

The primary evaluation focus should be:

**claim correctness and claim precision**

rather than image-level classification accuracy.

---

## 2. Primary Metric — Claim Precision

A key metric is:

```text
Claim Precision
=
Correctly Supported Claims
--------------------------
All Claims Recommended
```

Report the calculation methodology clearly.

---

## 3. Recommended Evaluation Measures

Where measurable, report:

* total charges evaluated,
* total claims recommended,
* correctly supported claims,
* incorrectly recommended claims,
* missed recoverable claims,
* uncertain/review cases,
* claim precision,
* important failure modes,
* latency,
* cost.

Do not report a number without explaining what it represents.

---

## 4. Test Difficult Cases

Your evaluation should include more than easy cases.

Test cases should include situations such as:

* clearly supportable claims,
* clearly unsupported claims,
* contradictory evidence,
* missing upstream evidence,
* ambiguous cases,
* different charge types,
* different operational paths,
* incomplete records,
* identifier mismatches.

---

## 5. Hold Out Evaluation Data

Where possible, keep your final evaluation cases separate from the examples used while developing or tuning your system.

Do not repeatedly tune the system against the same final evaluation set and then present those results as an unbiased evaluation.

---

## 6. Human Review

Where human evaluation is used:

1. Have reviewers independently assess the relevant evidence.
2. Record their decisions before comparing them to the agent.
3. Compare the agent's decision against the human-reviewed result.
4. Document disagreement and ambiguity.

If multiple reviewers are used, report their agreement where practical.

---

# 7. Honesty Rules

## 1. Say What You Built

Describe the actual implementation.

Do not claim:

* immutable records,
* tamper-proof records,
* production-grade security,
* guaranteed accuracy,
* autonomous recovery,
* or other properties

unless you actually implemented and demonstrated them.

A `content_hash` alone does not automatically make a record tamper-evident or immutable.

---

## 2. Report Measured Results

Do not write:

> "The agent works very well."

Instead, show the actual result.

For example:

```text
48 charges evaluated
44 claims recommended
38 correctly supported

Claim Precision = 86.36%
```

Then explain the remaining errors.

An honest result with clear failure analysis is more useful than an unsupported accuracy claim.

---

## 3. Report Failure Modes

Document where your system fails and why.

Examples may include:

* missing upstream evidence,
* ambiguous charge relationships,
* incorrect unit matching,
* incomplete report parsing,
* contradictory evidence,
* model errors,
* dependency failures.

Do not hide these cases from the evaluation.

---

## 4. Contradictions Are Findings

If source documents, datasets or evidence records appear to contradict one another:

**identify the contradiction.**

Do not silently choose whichever interpretation produces the desired result.

---

# 8. Documentation Requirements

Your repository should make the solution understandable to another engineer.

At minimum, document:

* problem understanding,
* solution overview,
* architecture,
* data flow,
* setup,
* usage,
* important assumptions,
* limitations,
* evaluation methodology,
* evaluation results,
* failure modes.

The repository should include an `ARCHITECTURE.md` describing the major components and important engineering decisions.

---

# 9. Submission Requirements

Your Round 2 submission should contain:

## GitHub Repository

Your own fork containing the complete Recovery Manager implementation.

## README.md

The README should explain:

* problem understanding,
* solution overview,
* setup,
* usage,
* assumptions,
* limitations,
* evaluation approach.

## ARCHITECTURE.md

Document:

* system architecture,
* components,
* data flow,
* evidence flow,
* model/agent usage,
* decision logic,
* important engineering decisions.

## Working Agent

The Recovery Manager must be demonstrable.

## Evaluation Results

Include:

* evaluation methodology,
* test cases/dataset,
* claim precision,
* relevant measurements,
* uncertainty/review handling,
* failure modes.

## Demo Video

Demonstrate the actual working solution.

The evaluator should be able to understand the flow from:

```text
Input
 ↓
Charge
 ↓
Evidence Matching
 ↓
Reasoning
 ↓
Decision
 ↓
Evidence Trail
```

## Deployment URL

Provide a working deployment URL where applicable.

## LinkedIn Post

A LinkedIn post is **mandatory** for Round 2.

The post must:

* mention the selected CUBE track,
* explain what you built,
* explain the problem addressed,
* share a meaningful engineering detail, result or learning,
* tag **CodeQuesters**,
* tag **Sydon.AI**.

Include the LinkedIn post URL in the official submission form.

---

# 10. Recommended API Interface

The repository does not require a specific framework.

You may choose your own implementation architecture.

For HTTP-based implementations, the following endpoints are recommended:

```text
POST /agent
```

for the main Recovery Manager operation.

And:

```text
GET /health
```

for basic service/deployment health checking.

These are recommended engineering interfaces unless the organisers explicitly make them mandatory.

Document your actual interface in the README.

---

# 11. GitHub Workflow

The official Round 2 workflow is:

### Step 1 — Fork

Fork the official Recovery Manager repository into your GitHub account.

### Step 2 — Clone Your Fork

```bash
git clone https://github.com/<your-github-username>/cube-05-recovery-manager.git
cd cube-05-recovery-manager
```

### Step 3 — Build

Develop the complete Recovery Manager inside your fork.

### Step 4 — Commit and Push

Use meaningful commit messages.

```bash
git add .
git commit -m "Build Recovery Manager"
git push origin main
```

You may use branches inside your own fork if that helps your development process.

### Step 5 — Submit

Submit your fork through the official Cube Buildathon submission form.

You do not need to create a pull request into the organiser repository.

---

# 12. Final Submission Cut-off

The final submission deadline is:

## 1 October 2026 · 6:00 PM IST

Before this deadline:

* finish your implementation,
* finish your documentation,
* complete evaluation,
* complete your demo,
* publish your LinkedIn post,
* verify your links,
* and submit the final form.

After the deadline, the submission form will close permanently.

---

# 13. No Resubmission

Once the form has been submitted:

**your submission is final.**

There is no resubmission or replacement submission facility.

Double-check:

* GitHub repository,
* deployment URL,
* demo video,
* evaluation information,
* LinkedIn URL,
* documentation,
* and all other form fields

before submitting.

---

# 14. Round 2 Scoring

Recovery Manager is evaluated out of **100 points**.

| Criterion                                    |  Points |
| -------------------------------------------- | ------: |
| Problem Understanding & Solution Relevance   |  **15** |
| Agent Functionality & Decision Quality       |  **25** |
| Evaluation, Accuracy & Uncertainty Handling  |  **25** |
| Evidence, Traceability & Engineering Quality |  **20** |
| UX, Demo & Documentation                     |  **15** |
| **TOTAL**                                    | **100** |

---

## 1. Problem Understanding & Solution Relevance — 15

Evaluators assess:

* understanding of the Recovery problem,
* operational relevance,
* appropriate scope,
* assumptions,
* alignment with the stated challenge.

---

## 2. Agent Functionality & Decision Quality — 25

Evaluators assess:

* report parsing,
* charge identification,
* evidence matching,
* claim reasoning,
* structured output,
* edge cases,
* handling of missing or contradictory evidence,
* decision quality.

---

## 3. Evaluation, Accuracy & Uncertainty Handling — 25

Evaluators assess:

* evaluation methodology,
* measured performance,
* claim precision,
* evidence correctness,
* false claim analysis,
* missed-claim analysis where measurable,
* appropriate use of `UNCERTAIN`,
* failure-mode analysis,
* reproducibility.

---

## 4. Evidence, Traceability & Engineering Quality — 20

Evaluators assess:

* evidence supporting decisions,
* charge-to-unit traceability,
* connection to upstream evidence,
* confidence and useful metadata,
* overrides,
* architecture,
* reliability,
* security,
* maintainability,
* appropriate model usage.

---

## 5. UX, Demo & Documentation — 15

Evaluators assess:

* workflow clarity,
* decision visibility,
* evidence visibility,
* usability,
* demo quality,
* README quality,
* architecture documentation,
* deployment,
* required links.

---

# 15. Round 2 → Round 3

Round 2 evaluates your **individual performance**.

Participants selected for Round 3 will work in **five-person Pods**.

A Pod consists of:

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

The objective of Round 3 is to integrate the five specialised agents into one connected end-to-end commerce system.

Round 3 therefore focuses on:

* collaboration,
* integration,
* system-level thinking,
* interfaces,
* evidence flow,
* dependencies,
* and combined execution.

---

# 16. Final Scoring

For participants who reach Round 3:

```text
Round 2 Score
      +
Round 3 Score
      =
Final Combined Score
```

Round 2 is scored out of **100**.

Round 3 is scored separately out of **100**.

The combined final score is therefore based on **200 points**.

Your Round 2 score carries forward and contributes to the final result.

---

# 17. Final Principle

The goal is not to build the largest codebase.

The goal is to build a Recovery Manager that can answer:

> **Should this charge be recovered, and can you prove why?**

Build carefully.

Trace your decisions.

Measure your performance.

Document your limitations.

Be honest about failure.

And submit the final version before the deadline.

---

**Cube Buildathon · 05 · Recovery Manager**

**Round 2 — Individual Build**

**Build → Test → Measure → Document → Publish → Submit**
