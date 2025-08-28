
# Study Plan – DVA-C02 (4 weeks) — **Theory + Practice Exams**

> **Official distribution:** Development 32%, Security 26%, Deployment 24%, Troubleshooting 18%.
> **Method:** guided reading → trade-off tables → flashcards → timed blocks → error-log with spaced review.

---


## Every week

- [ ] **Timed blocks:** 30–35 questions/block (≈45–55 min).
- [ ] **Weekly goal:** ≥4 blocks (120–140 questions).
- [ ] **Error-log** after each block (template below) and **D+1 review**.
- [ ] **Flashcards** (20–40/week) with limits, field names, and “where to configure X”.
- [ ] **Trade-off tables** by topic (copy/complete as below).

---


## Week 1 — Development with AWS Services (32%)

### Theory (checklist)

- [ ] Idempotency (natural/synthetic key; idea of `ConditionExpression`).
- [ ] Event-driven vs monolithic.
- [ ] Choreography (EventBridge) vs orchestration (Step Functions).
- [ ] Retries with backoff + jitter; **where are retries/DLQs configured** per service.
- [ ] **SQS Standard vs FIFO** (ordering/duplicates/throughput; **High-Throughput FIFO**).
- [ ] API Gateway **HTTP vs REST** (cache/stage vars only in REST; HTTP timeout ≈ **30s**).
- [ ] S3: versioning, lifecycle tiers, **SSE-KMS** (header indicators).
- [ ] Lambda: **max timeout 900s**, concurrency, **partial batch response** with SQS.

### Practice Exams (focus week 1)

- [ ] 4 blocks (≥120 questions) prioritizing **SQS/Lambda**, **DynamoDB**, **API Gateway**, **S3**.
- [ ] Review mistakes using the tables below.

### Key Tables

**SQS — Standard vs FIFO**

| Topic        | Standard                | FIFO (includes High-Throughput FIFO)          |
|-------------|-------------------------|-----------------------------------------------|
| Ordering    | Best-effort             | **Guaranteed by `MessageGroupId`**            |
| Duplicates  | Possible                | Dedup (by content or explicit dedup ID)       |
| Throughput  | Very high               | Lower (improves with High-Throughput FIFO)    |
| Typical use | Massive events, tolerates reordering | Flows with strict order per group |

**API Gateway — HTTP vs REST**

| Feature         | HTTP API     | REST API              |
|-----------------|--------------|-----------------------|
| Timeout         | **≈30s**     | >29s (quota/region)   |
| Cache           | ❌           | ✅ (per stage/method) |
| Stage Variables | ❌           | ✅                    |
| Latency/$$      | Lower/Simpler| More features         |

**Retries & DLQ — where to configure?**

| Flow/Service            | Retry is set…                | DLQ is set…                    | Note                                               |
|------------------------|------------------------------|--------------------------------|----------------------------------------------------|
| **Lambda ← SQS**       | ESM (Lambda) + SQS           | **SQS Redrive Policy**         | `VisibilityTimeout ≥ Lambda timeout`               |
| **EventBridge → target** | **RetryPolicy on target**   | **DeadLetterConfig on target** | Archive + Replay in EventBridge                    |
| **Step Functions**     | `Retry`/`Catch` in state     | Send to SQS or Destinations    | Pay attention to payloads and timeouts per task    |

**DynamoDB — Query vs Scan**

| Operation | Key required | Cost/Perf | Typical use |
|---|---|---|---|
| **Query** | Partition key (and optional sort key/prefix) | **Better** | Key/index-driven access |
| **Scan** | No | Worse (reads whole table) | Auditing/rare edge, avoid in hot path |

---


## Week 2 — Security (26%)

### Theory

- [ ] Least privilege; **IAM**: identity- vs resource-based policies.
- [ ] RBAC (roles), **assume role/STS**, **conditions** (`aws:PrincipalOrgID`, prefix/key conditions).
- [ ] KMS: **SSE-S3 vs SSE-KMS vs client-side**, CMK vs managed, **grants** (cross-account).
- [ ] AuthN/Z in APIs: **JWT**, Cognito (**User Pool** vs **Identity Pool**), Authorizers.

### Practice Exams

- [ ] 4 blocks focused on IAM/KMS/Cognito/Policies.
- [ ] Flashcards on **policy evaluation**, **KMS policy vs grants**, **Cognito flows**.

**Cheat Sheet**

| Topic                 | Key point |
|-----------------------|-----------|
| Resource vs Identity  | Who “carries” permission and who needs a **trust policy** |
| KMS                   | Key policy is sovereign; **grants** in integrations        |
| Cognito               | **User Pool** = JWT/identity; **Identity Pool** = temporary AWS credentials |

---


## Week 3 — Deployment (24%)

### Theory

- [ ] Strategies: **blue/green**, canary, rolling; rollback by alarm.
- [ ] Lambda **versions/aliases**; **weighted alias** (canary).
- [ ] IaC: **CDK vs SAM** (DX, packaging, diffs, pipelines).

### Practice Exams

- [ ] 4 blocks focusing on CodePipeline/CodeBuild/CodeDeploy, AppConfig, CDK vs SAM.

**Cheat Sheet**

| Topic                | Key point                                         |
|----------------------|---------------------------------------------------|
| CodeDeploy (Lambda)  | PreTraffic/PostTraffic hooks; rollback by alarm   |
| AppConfig            | Feature flags and rollout without redeploy        |
| CDK vs SAM           | SAM shines in pure serverless; CDK in multi-service composition |

---


## Week 4 — Troubleshooting & Optimization (18%)

### Theory

- [ ] Logging × monitoring × **observability** (metrics, logs, traces).
- [ ] **CloudWatch Logs Insights** (common queries), **X-Ray** (service map, subsegments).
- [ ] Common HTTP errors: **429/502/504**, timeouts, cold start.
- [ ] Cost vs latency: Lambda memory tuning, **reserved concurrency**, connections (RDS Proxy).

### Practice Exams

- [ ] 4 blocks targeting diagnostics: find “where it broke” and **which service configures what**.

**Cheat Sheet (symptom → quick check)**

| Symptom | Check |
|---|---|
| 502/504 via API GW | Timeout/error in Lambda; integration/binary mapping |
| 429 | Throttling (API GW or Lambda); adjust limits/RC |
| Variable latency | Lambda memory/CPU, network, VPC (environment), cold starts |

---


## Essential Flashcards (start now)

- [ ] “**Where is the DLQ** in the EventBridge target?” → On the **target** (`DeadLetterConfig`).
- [ ] “**Which header proves SSE-KMS** on S3 GET?” → `x-amz-server-side-encryption: aws:kms`.
- [ ] “**When is `MessageGroupId` required?**” → In **FIFO**.
- [ ] “**HTTP vs REST API**: which has cache and stage vars?” → **REST**.
- [ ] “**Lambda max timeout?**” → **900s**.
- [ ] “**Partial batch response** belongs to?” → **Lambda+SQS** (`batchItemFailures` in ESM).

---


## Error-log template

```text
Q (summary):
My answer:
Correct:
Root cause of error: (concept/limit/field name)
Correct rule / Mnemonic tip:
Anki card created? [ ] YES [ ] NO
```

---


## Weekly goals (check off)

- **Week 1**
  - [ ] Theory (Development)
  - [ ] 4 practice blocks
  - [ ] 20–40 flashcards
  - [ ] D+1 review
- **Week 2**
  - [ ] Theory (Security)
  - [ ] 4 practice blocks
  - [ ] 20–40 flashcards
  - [ ] D+1 review
- **Week 3**
  - [ ] Theory (Deployment)
  - [ ] 4 practice blocks
  - [ ] 20–40 flashcards
  - [ ] D+1 review
- **Week 4**
  - [ ] Theory (Troubleshooting)
  - [ ] 4 practice blocks
  - [ ] 20–40 flashcards
  - [ ] D+1 review
  - [ ] General review (tables & notes)

---


## Quick notes (to remember)

- [ ] **HTTP API timeout ≈30s**; **REST** can go **>29s** (quota/region).
- [ ] **SQS FIFO**: ordering guaranteed via `MessageGroupId`; dedup by content/ID; lower throughput (use **High-Throughput FIFO** if needed).
- [ ] **Lambda**: max **900s**; align **VisibilityTimeout ≥ timeout** (SQS→Lambda).

---


## Definition of Done (pre-exam)

- [ ] **≥80% average** in the last week’s blocks.
- [ ] Zero doubts in the trade-off tables (know them by heart).
- [ ] Flashcards reviewed on the eve (spaced repetition).
- [ ] Error-log read and “golden rule” memorized for each repeated error.

---


## Next step

- Read **Week 1 (Development)** → **1 timed block** today → fill in the **error-log** → create **10–15 flashcards** from mistakes.
