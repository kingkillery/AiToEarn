---
name: sync-service-docs
description: Maintain sync between product docs and code for a specified service. If docs do not exist, generate from OpenAPI spec from scratch; if docs exist, update affected sections based on git diff and refresh last update time and code baseline hash.
origin: custom
---

# sync-service-docs

Generate or sync product docs for a specified service.

## Invocation

```
/sync-service-docs [service]
```

If `service` is not provided, list available services and stop.

## Service configuration table

| Service | OpenAPI | Source code path | Doc directory |
|---------|-------------|---------|---------|
| `ai` | `openapi/aitoearn-ai.yaml` | `apps/aitoearn-ai/src/core/` | `docs/ai-service/` |
| `server` | `openapi/aitoearn-server.yaml` | `apps/aitoearn-server/src/` | `docs/server/` |
| `payment` | `openapi/aitoearn-payment.yaml` | `apps/aitoearn-payment/src/` | `docs/payment/` |
| `task` | `openapi/aitoearn-task.yaml` | `apps/aitoearn-task/src/` | `docs/task/` |
| `admin` | `openapi/aitoearn-admin-server.yaml` | `apps/aitoearn-admin-server/src/` | `docs/admin/` |

---

## Execution flow

### Step 1 — Initialization

1. Validate `service` parameters,error and stop
2. `git rev-parse --short HEAD` -> `<hash>` Date -> `<today>`
3. CheckDoc directory `.md` :
 - **not exists or empty** -> Step 2(Initial doc creation)
 - **already exists** -> Step 3(Diff update)

---

### Step 2 — Initial doc creation

#### 2a. Read OpenAPI spec

 OpenAPI ,extract:
- `paths`:path + HTTP method + operationId + summary
- group by `tags`(each unique tag -> one `.md` file)
- each endpoint `parameters`,`requestBody` schema,`responses` schema

**Principle:Request/response structures must come fully from OpenAPI definitions; do not guess fields.**

#### 2b. Read source code(fully trace business flow)

 tag module,:

```
Controller -> Service -> Helper/Util -> Consumer/Processor -> Repository
 ↓
 libs/ shared library methods (if called, follow and read)
```

**each endpointMethod**,record each step in actual execution order,:
- parametersValidate,Check,Check
- data query, data construction, data transformation
- credits/billing operations(/,,)
- async queue produce/consume logic
- logging(AiLog /)
- external API calls
- file upload/storage operations
- all possible error branches and codes

**Principle:Business flow must come from line-by-line source tracing; do not omit or guess any step.**

#### 2c. Large codebase segmented processing

module ≥ 4 moduleinterface ≥ 8 ,use(Agent tool):

- **Each sub-agent**is responsible for 1-2 tag/module: OpenAPI + + module `.md`
- ** prompt **:module OpenAPI paths ,Source code path,Doc structure specification( skill ),Constraints
- **Main flow**is responsible for:Initialization,Tasks,,, README.md

Example split method:
```
 1: chat module(5 endpoints)
 2: image module(14 endpoints)
 3: video + agent module( 5-6 endpoints)
```

#### 2d. Generate doc files

 tag `.md` ("Doc structure specification"), `README.md`(module + Common conventions).

After all file titles, immediately include:
```
> Last updated:<today> &nbsp;|&nbsp; Code baseline:`<hash>`
```

---

### Step 3 — Diff update

#### 3a. Code baseline

already hasextract `Code baseline:\`<hash>\`` hash Value(`<doc_hash>`).

#### 3b. Detect change scope

,Check OpenAPI :

```bash
git log <doc_hash>..HEAD --oneline -- <openapi_file> <source_path>
```

If no output, skip this doc. If output exists, continue analysis.

#### 3c. Analyze diffs and update

```bash
git diff <doc_hash>..HEAD -- <openapi_file> <source_path>
```

Precisely update corresponding sections based on diff content:

| Change type | Doc action |
|---------|---------|
| New path/method | ,specificationNewinterface |
| Modify requestBody/parameters/responses | update request/response sections of the endpoint |
| source business logic changes | interface,Business flow |
| Delete endpoint | remove corresponding doc section and update endpoint list |

**Principle:Only update sections with actual changes; unchanged sections must remain untouched (including wording and format).**

#### 3d. Update doc metadata

Update date and hash for all changed docs to current values.

---

### Step 4 — Output summary

After completion, output change summary:

| | Action | Description |
|------|------|------|
| `image.md` | | 14 endpoints |
| `video.md` | | New Grok Video |
| `chat.md` | | |

---

## Doc structure specification

### module( tag `.md` )

```markdown
# <module>

> Last updated:2026-03-12 &nbsp;|&nbsp; Code baseline:`a7d3afb3`

## Overview

<1-2 FunctionDescription>

**Related source code:** `apps/<service>/src/core/<module>/`

---

## Endpoint list

| Method | Path | Function |
|------|------|------|
| POST | `/api/...` | ... |
| GET | `/api/...` | ... |

---

## 1. <interfaceFunction>

### `<METHOD> <path>`

<1-2 Description>

**Request body:**
```json
{ "field": "value" }
```

**Response body:**
```json
{
 "code": 0,
 "data": { ... }
}
```

**Business flow:**

```
1. Validateparameters/
 └── Fail -> ErrorCode

2. /
 └── Not found -> ResourceNotFound

3. Pre-deduct credits(deductCredits, points=<>)
 └── Insufficient balance -> UserCreditsInsufficient

4. AiLog(status=generating)

5. enqueue async queue(QueueName.Xxx)

6. return { logId } for frontend polling

── Async stage(Consumer) ──────────────────

7. consume task -> API

8a. Success:
 - upload result to S3
 - update AiLog(status=success, response=...)

8b. Fail:
 - refund credits(addCredits, expiredAt=null)
 - update AiLog(status=failed, errorMessage=...)
```
```

**Business flowDescription:**
- The template above is only**+**,actual flow should be described truthfully based on source
- Synchronous post-deduction mode: API -> extract -> Credits -> deductCredits -> AiLog
- Pure synchronous no-billing:Credits,Description
- Each step in the flow should come from source code, ordered by actual execution sequence
- Write error branches in the corresponding step `└──` sub-lines; do not create a separate "error code" section

### README.md structure

```markdown
# <Service> service product docs

> Last updated:<date> &nbsp;|&nbsp; Code baseline:`<hash>`

## module

| | FunctionDescription |
|------|---------|
| [chat.md](./chat.md) | AI Dialogue, |
| ... | ... |

## Common conventions

### Authentication
- All endpoints require JWT token(`Authorization: Bearer <token>`)
- `userId` / `userType` parsed and injected by gateway

### Response format
```json
{ "code": 0, "message": "success", "data": { ... } }
```
`code=0` Success, 0 Wrong( `libs/common/src/enums/response-code.enum.ts`).

### Credits(Credits)
<Describe according to this service's actual billing mode, such as pre-deduct/post-deduct/VIP free, etc.>
```

---

## Constraints

### Must do

1. **Request/response structures come from OpenAPI** — Field,Type, OpenAPI schema
2. **Business flow** — Controller -> Service -> Helper -> Consumer ,
3. **libs/ Method** — Service `libs/` Method( creditsHelper,queueService),Implementation
4. **Common conventions README.md** — Authentication,Response format,CreditsDescriptionmodule README
5. **Check git diff first in update mode** — ,
6. **use HEAD hash** — hash
7. **Chinese descriptions, keep code identifiers in English** — `deductCredits`,`AiLog`,`status=generating`
8. **use** — module,

### Forbidden

1. **Do not generate testing points** — This is pure product documentation and does not include test cases, testing points, or boundary condition testing.
2. **Do not create separate "billing flow" or "error code" sections** — Business flow,WrongFail
3. **moduleCommon conventions** — Authentication,Response formatmodule
4. **Do not guess fields or flows** — OpenAPI Fielddo not include,
5. **Modify** — Diff update,()
6. **Do not omit business steps** — (Validate,,,,,,)
