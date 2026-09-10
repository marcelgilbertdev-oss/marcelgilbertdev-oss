# Marcel Gilbert

**Full-stack engineer — TypeScript · Python · PostgreSQL · AI systems**

I build software where being wrong has consequences, and I test it like it matters.
Twenty-one years leading technical teams in safety-critical aviation across four countries
taught me that; I now apply the same discipline to type systems, test suites, and reviewable code.

📍 Kuwait → relocating to **Japan** · 🇯🇵 residency comes through my wife, a permanent resident — **no employer sponsorship required** (spouse status held previously)
🌏 Open to **remote worldwide**, Japan-based, or hybrid
💬 English (native) · Japanese (studying)

**Start here:** [**zerofayyz-fintech.vercel.app**](https://zerofayyz-fintech.vercel.app) — a
deployed payments platform you can click through in three minutes, with
[the source](https://github.com/marcelgilbertdev-oss/zerofayyz-fintech) beside it.

---

### 🔭 What I'm building

**Gabriel** — a desktop-class AI workstation platform, built solo. *(private repo)*

A single React interface over conversational AI, local image generation, image-to-3D mesh
conversion, voice synthesis, an animation and storyboard studio, Blender and Unity integration,
and a system-health lane that can inspect and repair the machine it runs on.

| | |
|---|---|
| Frontend | **~25,000 lines** of **strict TypeScript** + React 19, 23 feature panels across 19 tabs, 84 explicit types |
| Backend | **Python** + FastAPI, 175 HTTP routes, job orchestration with cancellation |
| Tests | **9,685** automated tests across 347 modules |
| History | **1,200** commits |
| Safety | Allowlisted + sandboxed operations, reversible actions, audit receipt on every state change |

Every operation that changes state writes an auditable record. A system that acts on a real
machine should be **verifiable, not trusted** — that principle shaped the whole architecture.

---

### 📌 Public work

#### [`zerofayyz-fintech`](https://github.com/marcelgilbertdev-oss/zerofayyz-fintech) · TypeScript · Go · PostgreSQL

**A cloud payments and operations platform — deployed, monitored, and reviewable.**
→ **[zerofayyz-fintech.vercel.app](https://zerofayyz-fintech.vercel.app)** · Stripe sandbox
only; no real funds move, so everything is safe to click.

One Fastify/TypeScript API on PostgreSQL, consumed **unmodified by three independent
frontends** — Next.js, Vue 3 and Svelte 5 — each validating every response against one shared
Zod contract at the network boundary. Yen-denominated and bilingual, with translations
enforced by the type system: a missing string is a compile error.

- **Idempotency lives in a database constraint**, not application branching, so a duplicate
  webhook is refused under concurrency and across restarts.
- **An append-only audit log the application itself cannot rewrite** — enforced by a database
  trigger, not by convention.
- **Row-level security in a request lane:** user-serving reads adopt a low-privilege database
  role per transaction, so PostgreSQL's own policies — not WHERE clauses — decide which rows
  exist. Proven by tests that SELECT with no per-user filter.
- **Passwordless sign-in that stores no credential:** the token in a magic link exists only
  in the email; the database keeps a SHA-256 hash of it. Single use is one atomic UPDATE, so
  two clicks racing on the same link produce exactly one session, and expiry is decided in SQL
  rather than by the application's clock.
- **Four-eyes refunds:** the requester cannot approve their own request, enforced by the API
  *and* by a CHECK constraint, and the ledger moves only on Stripe's signed event.
- **An independent reconciler in Go**, deliberately a separate process — a checker that shares
  code with the thing it checks agrees with its bugs.
- **386 automated tests across ten suites**, all gated in ten CI jobs, including thirteen
  Cucumber scenarios that state the payment rules in plain language and execute.
- **A durable job queue in the same database that holds the ledger** — atomic claims over
  `FOR UPDATE SKIP LOCKED`, lease-based crash recovery, capped backoff and dead-lettering,
  surfaced to operators at `/admin/jobs`. The guarantee is at-least-once and is written down
  as such: exactly-once does not survive a channel that can die between doing the work and
  acknowledging it.
- **Monitored hourly in production** by a 30-check suite whose webhook probe signs a real
  event: a health endpoint reports that a signing secret is *present*, never that it is
  *correct*. I broke the secret on purpose to watch the alert fire, and restored it.
- **A QA surface an AI agent can drive** over the Model Context Protocol — six tools, with a
  protocol handshake check in CI. Its first human-driven run found a live defect.

*The defect worth citing:* the webhook handler had nineteen passing unit tests and had never
once worked. Every test stubbed the database, so none executed the real SQL — and the SQL was
invalid. The first integration test against real PostgreSQL found it immediately. **A test
suite has a shape, and defects collect where that shape does not reach.**

Thirteen decision records explain the trade-offs, including the ones still being carried.

---

#### [`endpoint-pulse`](https://github.com/marcelgilbertdev-oss/endpoint-pulse) · TypeScript · MIT

A Manifest V3 browser extension that watches health endpoints from the toolbar — badge shows
failures, one notification when an endpoint goes down and one when it recovers, never a repeat.

It ships watching the payments platform above, making it the **fourth independent consumer of
the same API** — and the first from outside that repository, which is the strongest test of the
contract claim. MV3 done properly: the service worker owns no state (Chrome kills it when
idle), and host access is requested **per origin at runtime**, never as a blanket grab at
install. The test most extension repos skip: Playwright loads the built extension into
Chromium and proves the worker registers, its alarm exists, and both pages render.

#### [`receipt-portal`](https://github.com/marcelgilbertdev-oss/receipt-portal) · TypeScript · Supabase · Deno

→ **[receipt-portal-one.vercel.app](https://receipt-portal-one.vercel.app)**

A customer's own receipts, built entirely on Supabase — Auth, row-level security policies,
Storage, and one Edge Function on Deno that mirrors payments from the platform above. It is the
**fifth independent consumer of the same API**, and it exists to enforce the same rule the
platform enforces by hand — *a customer sees only their own rows* — the other way round, so the
two row-level-security models can be compared honestly. That comparison is ADR 17 in the platform
repo.

- **Ten isolation tests against the live project:** two signed-in customers, queries with no
  per-customer filter, rows absent because a policy refused them. Writes refused, other people's
  files refused, other people's signed URLs refused.
- **The Edge Function holds the only privileged key at runtime.** It authenticates its caller with
  a shared secret compared in constant time, answers identically for a mailbox it has never seen
  so it cannot enumerate accounts, and is idempotent — ninety-three rows on the first run,
  ninety-three on the second.

*The trap worth knowing:* with "automatically expose new tables" off — which Supabase recommends —
a new table has no permissions for anyone, including the service role. Default-deny reaching the
privileged lane; correct, and it has to be granted back on purpose.

#### [`fair-scan`](https://github.com/marcelgilbertdev-oss/fair-scan) · Python · MIT

A two-phase fair scan over a grouped document corpus, where **group order never decides results**.

Extracted from Gabriel's retrieval layer after I found a defect that made search silently
answer from the wrong source — no error, no empty result, just a confident wrong answer, with a
whole category of documents quietly unreachable.

The repo keeps the **original broken implementation next to the fix** and runs both on the same
corpus, so you can watch it fail and then pass:

```
BEFORE  ordered scan with an early break
  found the answer : NO
  -> answered confidently from the wrong engine's documentation.
     No error. No empty result. Just wrong.

AFTER   two-phase fair scan
  found the answer : yes

Group order swapped -> identical results: yes
```

10 hermetic tests · zero dependencies · CI on Python 3.9 / 3.11 / 3.13

---

### 🧰 Tech

**Languages** TypeScript · Python · Go · SQL · C# · JavaScript · Bash

**Frontend** React 19 / Next.js · Vue 3 (Composition API + Pinia) · Svelte 5 (runes) · Vite ·
strict-mode TypeScript · runtime-validated shared contracts (Zod) · i18n enforced by the type system

**Backend** Node + Fastify · Python + FastAPI · PostgreSQL · REST API design · schema validation ·
webhook signature verification & idempotency · sessions, roles and rate limiting · background workers

**Testing & QA** Playwright (end-to-end, visual, accessibility) · Vitest · pytest · `node --test` ·
Cucumber / Gherkin · integration tests against a real database · axe-core in CI ·
ESLint 9 + typescript-eslint · `tsc -b` as a build gate

**Infrastructure** GitHub Actions CI/CD · Docker · Kubernetes manifests · Render / Vercel / Neon ·
database migrations · structured logging · error tracking · hourly production smoke monitoring

**AI/ML** Local LLM inference & model routing · RAG · embeddings · evaluation gates · MCP tool servers

**Generative media & 3D** SDXL / FLUX · image-to-3D mesh conversion · TTS voice cloning ·
Blender + `bpy` automation · Unity (C#) · Unreal

**Platforms** macOS · Linux · Unix-like · Windows

---

### 🎨 Also: generative media & 3D pipelines

Alongside the engineering I build content tooling — SDXL and FLUX image generation running
locally, image-to-3D mesh conversion, TTS voice synthesis, and two complete character rigs
authored in Blender with Python (`bpy`) automation I wrote: weight transfer, a repeatable
rig-transplant procedure that retargets a validated armature onto new meshes, 16 animation
clips, and LOD-tiered FBX export. Unity and Unreal scene work, plus a 2.5D mobile platformer
in progress.

The tooling exists because doing it by hand didn't scale — which is the same reason I write
any tool.

---

### 🎓 Background

**B.S. Information Technology** · University of Phoenix
**Advanced Cyber Security Certificate** (Undergraduate, awarded with honor)

Before software: 21 years in aircraft maintenance and program leadership — U.S. Air Force,
Lockheed Martin, DynCorp, Zenetex, AAR.

**Poland** — program systems owner for an 80-person USAFE F-16 depot. Authored the program's
Performance Work Statement from the governing Air Force Instruction, then built the systems
that satisfied it: qualification and training records from nothing, a leave-forecasting tool
that surfaced manning shortfalls weeks early, and a digitised onboarding pipeline covering
security-clearance investigation, fingerprint submission and expense reporting. The records
those systems produced held up under a Defense Contract Management Agency review.

**Japan** — alternate site lead at MCAS Iwakuni, leading a team of eight on a C-130J
programme, and built that site's training and qualification tracking from the ground up.

**Oman** — built the Microsoft Project system the Kuwait Air Force F-16 phase-inspection
programme was planned and run from: work breakdown, dependencies, resource loading, schedule
baselines. Also taught F-16 systems to non-native English speakers.

Work governed entirely by technical data, quality gates, and traceable records.

U.S. Air Force veteran · U.S. Secret clearance (renewed 2023)

---

### 📫 Reach me

**marcel.gilbert.dev@gmail.com**

*Open to software engineering roles — remote worldwide or Japan-based.*

<!-- profile -->
