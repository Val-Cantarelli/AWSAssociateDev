
# Study Plan – DVA-C02 (4 weeks)

> Official distribution: Dev 32%, Security 26%, Deployment 24%, Troubleshooting 18%.

## Week 1 — Development with AWS Services (32%)

**Theory**
- [ ] Idempotency
- [ ] Event-driven vs monolithic
- [ ] Choreography vs orchestration
- [ ] Retries with backoff + jitter
- [ ] DLQ (dead-letter queue)
- [ ] SQS **Standard vs FIFO** (ordering/duplicates, throughput; High-Throughput FIFO)

**Labs**
- [ ] **Lab 1 — Lambda + SQS/SNS**: DLQ + retry policy; A/B Standard vs FIFO (message groups, deduplication)
- [ ] **Lab 2 — DynamoDB**: ConditionExpression for idempotency; Query vs Scan; GSI; TTL
- [ ] **Lab 3 — EventBridge**: rule with filter → Lambda (include replay)
- [ ] **Lab 4 — Step Functions (Standard)**: orchestrate 2 Lambdas; success→next; failure→DLQ via Destinations
- [ ] **Lab 5 — S3**: versioning; lifecycle (IA/Glacier); SSE-KMS; pre-signed URL; event→Lambda
- [ ] **Lab 6 — API Gateway deep dive**: stages; custom domains (Route 53/ACM, optional CloudFront); mapping; cache; limits/errors (HTTP API max **30s**; REST Regional/Private **>29s** via quota; 413/429/502/504)
- [ ] **Lab 7 — CloudFront + WAF**: distribution in front of EB/BFF or API; basic cache policy; WAF rule (rate-limit/IP block)

---

## Week 2 — Security (26%)

**Theory**
- [ ] RBAC (role-based access control)
- [ ] Resource-based vs identity-based policies
- [ ] JWT / OAuth / STS
- [ ] Managed vs customer-managed policies
- [ ] Principle of least privilege

**Labs**
- [ ] **Lab 8 — IAM**: Conditions (aws:PrincipalOrgID, s3:prefix, dynamodb:LeadingKeys)
- [ ] **Lab 9 — KMS**: SSE-S3 vs SSE-KMS vs client-side; rotation; **cross-account grants**
- [ ] **Lab 10 — Cognito User Pool**: JWT Authorizer in API Gateway (protected routes)
- [ ] **Lab 11 — Cognito Identity Pool**: federated access to S3; compare User Pool vs Identity Pool
- [ ] **Lab 12 — Secrets Manager & Parameter Store**: DB creds/keys; encrypted env vars; access via role

---

## Week 3 — Deployment (24%)

**Theory**
- [ ] Strategies: blue/green, canary, rolling
- [ ] Lambda versions & aliases
- [ ] IaC: CDK vs SAM (DX trade-offs, diffs, pipelines)

**Labs**
- [ ] **Lab 13 — CodePipeline + CodeBuild**: Django tests triggered by push
- [ ] **Lab 14 — CodeDeploy (Lambda)**: canary + automatic rollback via alarm (PreTraffic/PostTraffic hooks)
- [ ] **Lab 15 — AppConfig**: feature flags with controlled rollout; runtime reading
- [ ] **Lab 16 — SAM vs CDK**: same stack (API Gateway + Lambda + DynamoDB); compare templates/flows
- [ ] **Lab 17 — Containers**: build image; push to **ECR**; **ECS Fargate** “hello-world” service
- [ ] **Lab 18 — Costs**: **AWS Budgets** with alerts (optional: Cost Anomaly Detection)

---

## Week 4 — Troubleshooting & Optimization (18%)

**Theory**
- [ ] Logging vs monitoring vs **observability**
- [ ] CloudWatch Logs Insights (queries)
- [ ] X-Ray (service maps, traces, subsegments)
- [ ] Common HTTP errors (429/502/504) and SDK exceptions
- [ ] Cost vs performance (conscious tuning)

**Labs**
- [ ] **Lab 19 — Logs Insights**: queries for throttling (429), timeout, cold starts, 502/504; dashboards
- [ ] **Lab 20 — X-Ray end-to-end**: EB → API Gateway → Lambda → **RDS Proxy**; subsegments in DB calls
- [ ] **Lab 21 — RDS Proxy + IAM Auth**: token (TTL ~15 min) + TLS; alarm for exhausted connection pool
- [ ] **Lab 22 — Lambda tuning**: memory vs latency/cost; reserved concurrency; max timeout **900s**; align **SQS visibility timeout** with **Lambda timeout**
- [ ] **Lab 23 — Caching**: **ElastiCache (Redis)** or **DynamoDB DAX**; compare **lazy-loading** vs **write-through** and impact on p50/p95

---

## Quick notes

- [ ] HTTP API max timeout **30s**; REST Regional/Private can be **>29s** via quota (trade-off with throttle)
- [ ] SQS **FIFO**: guaranteed ordering; lower throughput (or High-Throughput FIFO). **Standard**: best-effort ordering + duplicates; very high throughput
- [ ] Lambda: timeout **max 900s** (15 min)

---

## Final rehearsals (worth gold)

- [ ] **Fire drill**: induce DB latency/error and solve with Logs Insights + X-Ray + canary rollback (CodeDeploy)
- [ ] **Official practice (Skill Builder)**: goal ≥ **80%** consistent before the exam
- [ ] **Environment checklist**: delete stacks/labs that generate cost, review Budgets and WAF rules
- [ ] **Domain and custom domain**: validate mappings in API Gateway/CloudFront/Route 53

---

## Definition of Done 

- [ ] Versioned code (branch `labs/<n>` or dedicated folder)
- [ ] Infra by IaC (CDK/SAM) and possible to teardown
- [ ] Automated tests running in the pipeline
- [ ] Observability: logs, metrics, and visible traces
- [ ] Costs monitored (active Budget + alerts)
