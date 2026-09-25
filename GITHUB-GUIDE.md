# How to Use This Repository

This repository is the official starting point for:

**Cube Buildathon · 05 · Recovery Manager**

Round 2 is an **individual build**.

Each participant should create their own fork of this repository and complete their entire solution inside that fork.

---

# 1. Round 2 GitHub Workflow

The official workflow is:

```text
Official Repository
        ↓
      Fork
        ↓
 Your GitHub Fork
        ↓
 Build + Test
        ↓
 Commit + Push
        ↓
 Final Submission
```

You are **not** required to work directly inside the organiser's repository.

You are **not** required to create a participant branch in the organiser repository.

You are **not** required to create `submissions/<username>/`.

You are **not** required to open a pull request into the organiser repository.

---

# 2. Prerequisites

Before you begin, make sure you have:

* A GitHub account.
* Git installed on your computer.
* A development environment appropriate for your chosen technology stack.
* Access to any APIs/services you decide to use.
* Basic knowledge of cloning, committing and pushing a Git repository.

You should also read:

* [`README.md`](README.md)
* [`RULES.md`](RULES.md)

before starting development.

---

# 3. Fork the Repository

Open the official Recovery Manager repository:

`https://github.com/Cube-Build-A-Thon/cube-05-recovery-manager`

Click:

**Fork → Create fork**

Create the fork under your own GitHub account.

Your fork will become your primary Round 2 development repository.

---

# 4. Clone Your Fork

After creating the fork, clone **your fork**, not the organiser repository.

### HTTPS

```bash
git clone https://github.com/<your-github-username>/cube-05-recovery-manager.git
cd cube-05-recovery-manager
```

### SSH

```bash
git clone git@github.com:<your-github-username>/cube-05-recovery-manager.git
cd cube-05-recovery-manager
```

Replace `<your-github-username>` with your actual GitHub username.

---

# 5. Configure Git

If you have not configured Git before:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Use the email associated with your GitHub account.

You can verify your configuration with:

```bash
git config --global --list
```

---

# 6. Start Development

Once your fork is cloned:

1. Read the problem statement.
2. Review the sample data in `data/`.
3. Understand the upstream evidence structure.
4. Understand the expected Recovery Manager workflow.
5. Decide your application architecture.
6. Build your implementation.
7. Test locally.
8. Evaluate your results.
9. Document your architecture and limitations.
10. Deploy where applicable.

Your complete solution should live in your fork.

---

# 7. Repository Structure

The exact project structure is up to you.

A possible structure is:

```text
cube-05-recovery-manager/
│
├── data/
│   ├── fee_report_sample.csv
│   └── upstream/
│
├── src/
│   └── ...
│
├── tests/
│   └── ...
│
├── README.md
├── RULES.md
├── GITHUB-GUIDE.md
├── ARCHITECTURE.md
├── requirements.txt / package.json
└── ...
```

This is only an example.

You may organise your application differently as long as your solution is understandable and runnable.

---

# 8. Build Your Agent

Your Recovery Manager should implement the workflow described in the problem statement.

At a high level:

```text
Fee / Recovery Report
        ↓
Parse Charges
        ↓
Identify Unit
        ↓
Match Upstream Evidence
        ↓
Evaluate Evidence
        ↓
Recovery Reasoning
        ↓
CLAIM / DO NOT CLAIM / REVIEW
        ↓
Evidence-backed Output
```

Keep the architecture understandable.

Avoid putting the entire application into one large script or one opaque model call when a clearer structure is possible.

---

# 9. Recommended API Interface

You may choose any framework or architecture.

For an HTTP-based implementation, the following interface is recommended:

```text
POST /agent
```

This can serve as the primary Recovery Manager operation.

A health endpoint is also recommended:

```text
GET /health
```

These are engineering recommendations.

They are not a requirement to use a specific programming language or framework.

Document your actual API structure in your README.

---

# 10. Working With Sample Data

The repository contains synthetic development data.

Use it to:

* understand the relationships between charges and units,
* test evidence matching,
* test decision logic,
* build representative examples,
* develop your evaluation workflow.

The values in the sample files are **synthetic**.

Do not treat them as authoritative external fee schedules or channel policies.

---

# 11. Commit and Push Your Work

Commit your development work regularly.

Example:

```bash
git status
git add .
git commit -m "Build Recovery Manager"
git push origin main
```

You may also use your own development branches if you prefer.

For example:

```bash
git checkout -b feature/recovery-reasoning
```

The branch structure inside your own fork is your choice.

The important requirement is that your final submitted repository contains the complete Round 2 solution.

---

# 12. Meaningful Commit Messages

Use commit messages that describe what changed.

Good examples:

```text
Add charge parsing pipeline
Implement evidence matching
Add uncertainty handling
Add recovery decision schema
Add evaluation metrics
Document architecture
Add deployment configuration
```

Avoid meaningless messages such as:

```text
update
changes
fix
test
final
final2
final-final
```

Clear commits make your development history easier to understand.

---

# 13. Build-Phase Commit Rule

All code commits that form your Round 2 submission must be made during the **authorised build phase**.

Once the build phase ends:

* do not continue making Round 2 code changes,
* do not add new implementation features,
* do not silently replace the submitted implementation,
* do not modify the code after the build cut-off and then submit an earlier state.

Keep the final repository consistent with the authorised build period.

---

# 14. Submission Is Through the Official Form

Your GitHub fork is **not automatically submitted** merely because you pushed code to GitHub.

You must complete the official Cube Buildathon submission form.

The submission form opens from:

**27 September 2026**

Final deadline:

**1 October 2026 · 6:00 PM IST**

Once the deadline is reached, the submission form closes permanently.

---

# 15. No Resubmission

Once you submit the official form:

**your submission is final.**

There is no resubmission facility.

Before clicking Submit, verify:

* GitHub repository URL
* deployment URL
* demo video
* LinkedIn post
* evaluation information
* documentation
* all other required fields

---

# 16. Mandatory LinkedIn Post

Round 2 requires a LinkedIn post.

Your post should:

* mention that you built for Cube Buildathon,
* mention **Recovery Manager**,
* explain the problem you solved,
* share a meaningful engineering detail, result or learning,
* tag **CodeQuesters**,
* tag **Sydon.AI**.

Include the live LinkedIn post URL in the official submission form.

The organisers will share an official LinkedIn post template separately.

---

# 17. No Secrets

Never commit sensitive credentials.

Do not commit:

* API keys
* passwords
* access tokens
* private keys
* secret credentials
* `.env` files containing real secrets

Use environment variables instead.

For example:

```text
OPENAI_API_KEY=...
DATABASE_URL=...
```

Keep actual secret values outside Git.

If you accidentally expose a credential, revoke it immediately.

---

# 18. Testing Before Submission

Before submitting, test the complete flow from beginning to end.

At minimum, verify:

```text
Input
 ↓
Charge Parsing
 ↓
Unit Matching
 ↓
Evidence Retrieval
 ↓
Reasoning
 ↓
Decision
 ↓
Evidence Trace
```

Test normal and difficult cases.

Include cases involving:

* missing evidence,
* contradictory evidence,
* ambiguous evidence,
* invalid identifiers,
* different charge types,
* dependency/model failure.

---

# 19. Evaluation

Your evaluation should focus on **claim correctness and precision**.

Useful measurements include:

* total charges evaluated,
* claims recommended,
* correctly supported claims,
* incorrectly recommended claims,
* missed recoverable claims,
* claim precision,
* uncertain/review rate,
* failure modes,
* latency,
* cost where relevant.

Document how each metric was calculated.

---

# 20. Evidence Traceability

A reviewer should be able to understand:

```text
Charge
  ↓
Unit
  ↓
Upstream Evidence
  ↓
Evidence Interpretation
  ↓
Recovery Decision
```

Your output should preserve or reference the evidence that supports the decision.

Do not return an unexplained claim recommendation.

---

# 21. What Happens in Round 3

Round 2 is individual.

Participants selected for Round 3 will work in five-person Pods.

A Pod combines:

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

The objective is to integrate the five specialised agents into one connected commerce system.

Design your Round 2 solution so another engineer can understand and integrate it later.

Clear interfaces, structured outputs and evidence traceability will make that integration easier.

---

# 22. Round 2 Scoring

Round 2 is scored out of **100 points**.

| Criterion                                    |  Points |
| -------------------------------------------- | ------: |
| Problem Understanding & Solution Relevance   |      15 |
| Agent Functionality & Decision Quality       |      25 |
| Evaluation, Accuracy & Uncertainty Handling  |      25 |
| Evidence, Traceability & Engineering Quality |      20 |
| UX, Demo & Documentation                     |      15 |
| **Total**                                    | **100** |

For participants who reach Round 3, the Round 2 score contributes to the final combined result.

---

# 23. Common Git Commands

Check repository status:

```bash
git status
```

See recent commits:

```bash
git log --oneline -10
```

Add changes:

```bash
git add .
```

Commit:

```bash
git commit -m "Add evidence matching"
```

Push:

```bash
git push origin main
```

Pull updates to your own branch:

```bash
git pull
```

Create your own development branch:

```bash
git checkout -b feature/my-change
```

Switch branches:

```bash
git checkout main
```

---

# 24. Common Problems

| Problem                         | What to check                                                                          |
| ------------------------------- | -------------------------------------------------------------------------------------- |
| Repository not found            | Confirm that you cloned your own fork and the URL is correct.                          |
| Permission denied               | Make sure you are authenticated to the GitHub account that owns your fork.             |
| Push rejected                   | Run `git pull` and resolve any local conflicts before pushing again.                   |
| Environment not working         | Check the README setup steps, dependencies and environment variables.                  |
| API/model failure               | Preserve the available input and use an appropriate review/pending state.              |
| Missing evidence                | Do not invent evidence; return an appropriate uncertain/review outcome.                |
| Accidentally committed a secret | Revoke the secret immediately and remove it from the repository history appropriately. |

---

# 25. Before You Submit

Run through this checklist:

```text
[ ] Correct Recovery Manager repository
[ ] Working implementation
[ ] Own GitHub fork
[ ] Required code commits completed during the authorised build phase
[ ] README.md complete
[ ] RULES.md reviewed
[ ] ARCHITECTURE.md complete
[ ] Evaluation completed
[ ] Claim precision reported
[ ] Failure modes documented
[ ] Uncertainty/review handling tested
[ ] Demo video ready
[ ] Deployment URL verified, if applicable
[ ] LinkedIn post published
[ ] CodeQuesters tagged
[ ] Sydon.AI tagged
[ ] LinkedIn URL copied correctly
[ ] All submission-form fields completed
[ ] Final repository checked
[ ] No secrets committed
[ ] Submission ready before 1 October 2026 · 6:00 PM IST
```

---

# Final Reminder

You are building independently in Round 2.

The correct workflow is:

```text
FORK
  ↓
CLONE
  ↓
UNDERSTAND
  ↓
BUILD
  ↓
TEST
  ↓
EVALUATE
  ↓
DOCUMENT
  ↓
DEPLOY
  ↓
PUBLISH
  ↓
SUBMIT
```

Build a Recovery Manager that does more than produce an answer.

Make the decision traceable.

Make the evidence visible.

Measure the result.

Document the limitations.

And submit before the final deadline.

---

**Cube Buildathon · 05 · Recovery Manager**

**Round 2 — Individual Build**
