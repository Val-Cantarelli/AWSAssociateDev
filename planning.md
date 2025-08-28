# Study Plan – DVA-C02 (4 weeks) — **Teoria + Simulados (sem labs)**

> **Distribuição oficial:** Development 32%, Security 26%, Deployment 24%, Troubleshooting 18%.  
> **Método:** leitura dirigida → tabelas de trade-offs → flashcards → blocos cronometrados → error-log com revisão espaçada.

---

## Regras do jogo (todas as semanas)

- [ ] **Blocos cronometrados:** 30–35 questões/bloco (≈45–55 min).
- [ ] **Meta semanal:** ≥4 blocos (120–140 questões).
- [ ] **Error-log** após cada bloco (modelo abaixo) e **revisão D+1**.
- [ ] **Flashcards** (20–40/semana) com limites, nomes de campos e “onde configura X”.
- [ ] **Tabelas de trade-offs** por tema (copie/complete as abaixo).

---

## Week 1 — Development with AWS Services (32%)

### Teoria (checklist)

- [ ] Idempotência (chave natural/sintética; ideia do `ConditionExpression`).
- [ ] Event-driven vs monolítico.
- [ ] Coreografia (EventBridge) vs orquestração (Step Functions).
- [ ] Retries com backoff + jitter; **onde ficam retries/DLQs** por serviço.
- [ ] **SQS Standard vs FIFO** (ordenação/duplicatas/throughput; **High-Throughput FIFO**).
- [ ] API Gateway **HTTP vs REST** (cache/stage vars só no REST; timeout do HTTP ≈ **30s**).
- [ ] S3: versioning, lifecycle tiers, **SSE-KMS** (headers indicativos).
- [ ] Lambda: **timeout máx 900s**, concurrency, **partial batch response** com SQS.

### Simulados (foco semana 1)

- [ ] 4 blocos (≥120 questões) priorizando **SQS/Lambda**, **DynamoDB**, **API Gateway**, **S3**.
- [ ] Revisão das erradas usando as tabelas abaixo.

### Tabelas-chave

**SQS — Standard vs FIFO**

| Tema         | Standard                | FIFO (inclui High-Throughput FIFO)            |
|--------------|-------------------------|-----------------------------------------------|
| Ordenação    | Best-effort             | **Garantida por `MessageGroupId`**            |
| Duplicatas   | Possíveis               | Dedup (por conteúdo ou dedup ID explícito)    |
| Throughput   | Altíssimo               | Menor (melhora com High-Throughput FIFO)      |
| Uso típico   | Eventos massivos, tolera reordem | Fluxos com ordem rígida por grupo |

**API Gateway — HTTP vs REST**

| Recurso         | HTTP API     | REST API              |
|-----------------|--------------|-----------------------|
| Timeout         | **≈30s**     | >29s (quota/região)   |
| Cache           | ❌           | ✅ (por stage/method) |
| Stage Variables | ❌           | ✅                    |
| Latência/$$     | Menor/Simpler| Mais recursos         |

**Retries & DLQ — onde configuro?**

| Fluxo/Serviço            | Retry fica…                 | DLQ fica…                      | Observação                                         |
|--------------------------|-----------------------------|--------------------------------|----------------------------------------------------|
| **Lambda ← SQS**         | ESM (Lambda) + SQS          | **Redrive Policy da SQS**      | `VisibilityTimeout ≥ timeout da Lambda`            |
| **EventBridge → alvo**   | **RetryPolicy no target**   | **DeadLetterConfig do target** | Archive + Replay no EventBridge                    |
| **Step Functions**       | `Retry`/`Catch` no estado   | Envio p/ SQS ou Destinations   | Atenção a payloads e timeouts por task             |

**DynamoDB — Query vs Scan**

| Operação | Chave necessária | Custo/Perf | Uso típico |
|---|---|---|---|
| **Query** | Partition key (e opcional sort key/prefix) | **Melhor** | Acesso dirigido por chave/índice |
| **Scan** | Não | Pior (lê tabela toda) | Auditoria/rare edge, evitar em hot path |

---

## Week 2 — Security (26%)

### Teoria

- [ ] Menor privilégio; **IAM**: identity- vs resource-based policies.
- [ ] RBAC (roles), **assume role/STS**, **conditions** (`aws:PrincipalOrgID`, prefix/key conditions).
- [ ] KMS: **SSE-S3 vs SSE-KMS vs client-side**, CMK vs gerenciada, **grants** (cross-account).
- [ ] AutN/Z em APIs: **JWT**, Cognito (**User Pool** vs **Identity Pool**), Authorizers.

### Simulados

- [ ] 4 blocos focados em IAM/KMS/Cognito/Policies.
- [ ] Flashcards de **policy evaluation**, **KMS policy vs grants**, **Cognito flows**.

**Mini-cola**

| Tema                   | Ponto de prova |
|------------------------|----------------|
| Resource vs Identity   | Quem “carrega” permissão e quem precisa de **trust policy** |
| KMS                    | Política da chave é soberana; **grants** em integrações     |
| Cognito                | **User Pool** = JWT/identidade; **Identity Pool** = credenciais AWS temporárias |

---

## Week 3 — Deployment (24%)

### Teoria

- [ ] Estratégias: **blue/green**, canary, rolling; rollback por alarme.
- [ ] Lambda **versions/aliases**; **weighted alias** (canary).
- [ ] IaC: **CDK vs SAM** (DX, empacotamento, diffs, pipelines).

### Simulados

- [ ] 4 blocos focando CodePipeline/CodeBuild/CodeDeploy, AppConfig, CDK vs SAM.

**Mini-cola**

| Tema                  | Ponto de prova                                   |
|----------------------|---------------------------------------------------|
| CodeDeploy (Lambda)  | Hooks PreTraffic/PostTraffic; rollback por alarme |
| AppConfig            | Feature flags e rollout sem redeploy              |
| CDK vs SAM           | SAM brilha em serverless puro; CDK em composição multi-serviço |

---

## Week 4 — Troubleshooting & Optimization (18%)

### Teoria

- [ ] Logging × monitoring × **observability** (métricas, logs, traces).
- [ ] **CloudWatch Logs Insights** (queries comuns), **X-Ray** (service map, subsegments).
- [ ] Erros HTTP frequentes: **429/502/504**, timeouts, cold start.
- [ ] Custo vs latência: tuning de memória da Lambda, **reserved concurrency**, conexões (RDS Proxy).

### Simulados

- [ ] 4 blocos mirando diagnóstico: achar “onde quebrou” e **qual serviço configura o quê**.

**Mini-cola (sintoma → check rápido)**

| Sintoma | Check |
|---|---|
| 502/504 via API GW | Timeout/erro na Lambda; integração/binary mapping |
| 429 | Throttling (API GW ou Lambda); ajustar limites/RC |
| Latência variável | Memória/CPU da Lambda, rede, VPC (ambiente), cold starts |

---

## Flashcards essenciais (comece já)

- [ ] “**Onde fica o DLQ** no EventBridge target?” → No **alvo** (`DeadLetterConfig`).  
- [ ] “**Qual cabeçalho prova SSE-KMS** no S3 GET?” → `x-amz-server-side-encryption: aws:kms`.  
- [ ] “**Quando `MessageGroupId` é obrigatório?**” → Em **FIFO**.  
- [ ] “**HTTP vs REST API**: quem tem cache e stage vars?” → **REST**.  
- [ ] “**Lambda timeout máx?**” → **900s**.  
- [ ] “**Partial batch response** é de quem?” → **Lambda+SQS** (`batchItemFailures` no ESM).

---

## Error-log template

```text
Q (resumo):
Minha resposta:
Certa:
Causa-raiz do erro: (conceito/limite/nome de campo)
Regra correta / Dica mnemônica:
Anki card criado? [ ] SIM [ ] NÃO
```

---

## Metas semanais (marque)

- **Semana 1**
  - [ ] Teoria (Development)
  - [ ] 4 blocos simulados
  - [ ] 20–40 flashcards
  - [ ] Revisão D+1
- **Semana 2**
  - [ ] Teoria (Security)
  - [ ] 4 blocos simulados
  - [ ] 20–40 flashcards
  - [ ] Revisão D+1
- **Semana 3**
  - [ ] Teoria (Deployment)
  - [ ] 4 blocos simulados
  - [ ] 20–40 flashcards
  - [ ] Revisão D+1
- **Semana 4**
  - [ ] Teoria (Troubleshooting)
  - [ ] 4 blocos simulados
  - [ ] 20–40 flashcards
  - [ ] Revisão D+1
  - [ ] Revisão geral (tabelas & anotações)

---

## Quick notes (para fixar)

- [ ] **HTTP API timeout ≈30s**; **REST** pode ir **>29s** (quota/região).  
- [ ] **SQS FIFO**: ordenação garantida via `MessageGroupId`; dedup por conteúdo/ID; throughput menor (use **High-Throughput FIFO** se precisar).  
- [ ] **Lambda**: máx **900s**; alinhar **VisibilityTimeout ≥ timeout** (SQS→Lambda).

---

## Definition of Done (pré-prova)

- [ ] Média **≥80%** nos blocos da última semana.  
- [ ] Zero dúvidas nas tabelas de trade-offs (conhece de cabeça).  
- [ ] Flashcards revistos na véspera (spaced repetition).  
- [ ] Error-log lido e “regra-de-ouro” memorizada para cada erro repetido.

---

## Próximo passo

- Leitura da **Semana 1 (Development)** → **1 bloco cronometrado** hoje → preencher o **error-log** → criar **10–15 flashcards** das erradas.
