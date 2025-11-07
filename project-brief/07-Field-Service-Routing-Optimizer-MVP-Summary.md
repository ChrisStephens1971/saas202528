# Handyman/Field‑Service: Zone Routing Optimizer

**Project Type:** Wedge MVP (4–6 weeks)  
**Primary Buyer:** Dispatcher / owner  
**Target Users:** Dispatch, technicians  
**Problem Statement:** Manual map‑staring routes waste miles and hours; wrong tech to wrong job.

---

## 1) Objective
Build a narrow, high-ROI MVP that removes the most expensive step in the workflow, without rip-and-replace. Ship in 4–6 weeks with production quality.

## 2) Pain Signals (from the field)
- Double‑booking; long drive times
- Missed SLAs; overtime
- Angry customers

## 3) Core Jobs-To-Be-Done
- Build efficient day plans
- Respect skills and time windows
- React to cancellations quickly

## 4) Wedge MVP Scope (what ships in 4–6 weeks)
- Zone and skill tagging per tech
- Auto‑cluster day plan with travel time caps
- Time window constraints and conflict warnings
- One‑click replan when a job cancels
- Basic KPI board (miles, jobs, on‑time %)

**Out of scope for MVP (build later):**
- Full inventory/parts
- Quoting/estimates
- Customer portal

## 5) 60‑Day ROI Metrics (must improve or kill it)
- Miles per job down 20%
- Jobs/day up 10%
- On‑time % up 15%

## 6) Pricing Hypothesis (launch)
- Pilot: $149/mo org, up to 5 techs
- Pro: $399/mo org, up to 50 techs
- Ops: $799/mo org, unlimited users + priority support

## 7) Key Workflows (E2E)
1. Dispatcher defines zones and skills
2. System clusters jobs and flags conflicts
3. Replan button handles cancellations
4. KPI board tracks targets

## 8) Core Entities & ERD Notes
- Entities: Tech, Job, Zone, Route, Constraint
- Relationships: Tech 1‑N Routes; Route 1‑N Jobs; Job N‑N Constraints
- Multi‑tenant isolation by `org_id`; all queries scoped.

## 9) Integrations (MVP)
- Maps/distance matrix, SMS for updates

## 10) Architecture Blueprint (AI can scaffold)
- Web: Next.js 14+ (App Router), React Server Components, Tailwind
- API: Node/TS (tRPC or REST) or Go (Fiber/Chi) behind a gateway
- DB: PostgreSQL (Prisma), row‑level security by `org_id`
- Auth: Passwordless/email + OAuth; organization membership & roles
- Realtime: Pusher/Ably (pub/sub) where needed
- Background jobs: durable queue (BullMQ or Cloud tasks)
- Files: S3‑compatible blob storage (signed URLs)
- Notifications: Transactional email + SMS provider
- Payments: Stripe (standard or Connect as noted)
- Hosting: Cloud provider with per‑env stacks; infra as code
- Observability: structured logs, traces, error tracker
- Feature flags: simple boolean flags in DB
- Admin: internal admin panel with audit log

## 11) Data Model Highlights
- `organizations(id, name, plan, …)`
- `users(id, email, …)`
- `memberships(user_id, org_id, role)`
- Domain entities: techs, jobs, zones, routes, constraints
- All tables include `org_id`, `created_at`, `updated_at`, `actor_id`

## 12) Security & Compliance
- RBAC: owner, manager, staff (expand per domain)
- Audit trail on create/update/delete of domain entities
- PII minimization; encryption at rest & TLS in transit
- Rate limits on public endpoints; signed URLs for files
- Least‑privilege DB roles; weekly offsite backups

## 13) Analytics & Telemetry
- Product analytics funnels on the top 3 workflows
- Business KPIs: the ROI metrics above, per org and cohort
- Export CSVs and webhook for BI tools

## 14) Launch Plan (Weeks 1–6)
- Week 1: confirm scope with 3 pilot customers; finalize schema; scaffold repo
- Week 2: implement domain entities and primary workflow
- Week 3: notifications + payments + basic admin
- Week 4: polish UX; seed data; write acceptance tests
- Week 5: onboard pilots; instrument metrics; fix field bugs
- Week 6: prove ROI; decision to expand or stop

## 15) Risks & Go/No‑Go Gates
- Edge cases in routing; ETA accuracy; SMS costs
- No‑Go if pilots don’t hit the ROI metrics by day 60

## 16) Acceptance Tests (MVP)
- Create a workable plan for 30 jobs and 10 techs in <60 seconds.

## 17) Sample User Stories
- As a dispatcher, I can lock critical jobs before auto‑planning.

## 18) API Surface (outline)
- POST /auth/sign‑in
- POST /orgs
- CRUD /jobs
- CRUD /routes
- POST /webhooks/job-status
- GET  /reports/routing-kpis

## 19) Backlog After MVP
- Live GPS, traffic awareness, customer ETA links

---

**Notes for the AI:** keep tenancy airtight, ship the smallest workflow that proves money saved or revenue recovered, collect evidence by default (timestamps, deltas, before/after). Do not gold‑plate. 
