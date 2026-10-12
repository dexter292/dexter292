<div align="center">

# Dexter

**Tech Lead — Team S10 @ [Seatincorp](https://github.com/Seatincorp)**

Architecture · Technology direction

</div>

---

### Role

Tech Lead for **S10** at Seatincorp: set technical direction, own architecture decisions, and align delivery across the core platform, internal ops tools, and brand products.

**Stack snapshot:** Java 21 / Spring · Rust (risk) · Next.js 15 SSR/ISR · Portal admin · Actions → ECR → Helmfile → EKS

### How I work

Guided by the S10 **[TD — Technical Document](https://sgame.atlassian.net/wiki/spaces/S10/folder/2890465464/TD+Technical+Document)** program (Proposal → TDD → Spec → Onboarding → Incident).

- **Docs before code** — Proposal & TDD first; contracts and system overview as shared source of truth
- **Architecture ownership** — clear service boundaries (gateway, lobby, data, risk, loyalty, portal ↔ runtime); explicit fail modes over ad‑hoc coupling
- **Delivery with DevOps** — consume/operate [devops-actions-s10](https://github.com/Seatincorp/devops-actions-s10) (Actions → ECR → Helmfile → EKS); release promote, domain/CDN cutovers, rollback via helm history
- **Multi-brand scale** — shared FE core + brand hosts under [outsourcecentral](https://github.com/outsourcecentral)
- **Operate what we ship** — weekly sync, runbooks, incident write‑ups that feed design; smoke/monitor before calling it done

### Main project — SGames platform

**[Seatincorp/s10-kbgroup](https://github.com/Seatincorp/s10-kbgroup)** — player / runtime microservices (`sgames-10`)

Java 21 · Spring Boot 3 · Spring Cloud Gateway · Redis · MySQL · MongoDB · RabbitMQ

| Service | Role |
|---|---|
| `gateway-service` | Player API edge |
| `lobby-service` | Lobby / catalog / public LDP |
| `gift-code-service` · `affiliate-service` | Promotions & affiliate runtime |
| `chat-service` · `notification-service` | Realtime chat & notifications |
| `agent-orchestrator` · `task-scheduler` | AI agent API & background jobs |

### Platform pillars

| Pillar | Repo | Focus |
|---|---|---|
| **Data** | [accumulation-service](https://github.com/Seatincorp/s10-kbgroup/tree/main/sgames-10/accumulation-service) | RabbitMQ ingest → unified events (Mongo / Redis / Arango); feeds analytics & risk signals · Java 21 · JobRunr |
| **Risk** | [risk-decision-service](https://github.com/Seatincorp/risk-decision-service) | `POST /decide` → ACCEPT / REJECT / CHALLENGE; profile rules + Redis signals; dry-run / DFP · Rust · Axum |
| **Loyalty** | [loyalty-service](https://github.com/Seatincorp/s10-kbgroup/tree/main/sgames-10/loyalty-service) | Player missions / VIP / raffles runtime; Portal owns admin CRUD · Java 21 · Mongo / Redis |
| **Delivery** | [devops-actions-s10](https://github.com/Seatincorp/devops-actions-s10) | Shared CI/CD & Helmfile (**DevOps-owned**); S10 Tech Lead operates releases & env changes with this stack |

### DevOps / CI/CD (familiar surface)

Collaborate with DevOps on **[devops-actions-s10](https://github.com/Seatincorp/devops-actions-s10)** — reusable Actions + Helmfile for stg → preprod → prod on **AWS EKS**.

| Layer | Stack |
|---|---|
| Pipeline | GitHub Actions (`workflow_call`) · self-hosted runners |
| Ship path | Docker → **ECR** → **Helmfile** / omnibus chart → **EKS** (IAM role assume) |
| Gates | TruffleHog · SonarQube · Gitleaks |
| Day-2 ops | Brand/CDN onboard, domain switch (add/replace), deploy-only for ingress/CSP, FE readiness drain & warm-up, rollback via helm history |

TD: [DevOps Actions S10](https://sgame.atlassian.net/wiki/spaces/S10/pages/2979135510/Technical+Documentation+-+DevOps+Actions+S10)

### Internal tools — SGames Portal

**Admin plane** — configure / operate / observe runtime platforms. Couples via **shared stores** + HTTP ops bridges (loyalty, notification, accumulation, agent) — not the player gateway.

| Layer | Repo | Stack |
|---|---|---|
| Backend | [portal-be](https://github.com/Seatincorp/portal-be) | Spring Boot 3 · Java 21 · JWT RBAC · MySQL×5 · Mongo×6 · Redis · Arango · S3 |
| Frontend | [portal-fe](https://github.com/Seatincorp/s10-fe/tree/main/portal-fe) | React 18 · TS · Vite · Ant Design 5 · React Query · i18n en/vi |

**Owns:** identity/RBAC · promo & loyalty ops · accumulation analytics / pre-agg · risk ops · site/domain config · chat & notification · CMS LDP (publish → Next ISR) · audit / AIMS / storage

Docs: [portal-be](https://github.com/Seatincorp/portal-be/blob/main/docs/README.md) · [portal-fe](https://github.com/Seatincorp/s10-fe/blob/main/portal-fe/docs/README.md) · [portal ↔ sgames](https://github.com/Seatincorp/portal-be/blob/main/docs/explanation/portal-sgames-relationship.md)

### Brand products — Outsource Central

Multi-brand player sites under **[outsourcecentral](https://github.com/outsourcecentral)** on a shared Next core.

| Layer | Role |
|---|---|
| [s10-fe-core](https://github.com/outsourcecentral/s10-fe-core) | Git submodule — APIs, hooks, headless UI, axios web/RN split, Lingui, feature flags |
| [s10-app-core](https://github.com/outsourcecentral/s10-app-core) · mini-apps | Native/Expo shells · embeddable chat / static |
| Brand hosts | Private Next apps — skin + `core.config.ts`; same rails per brand |

**Web stack:** Next.js 15 App Router · React 19 · TanStack Query (SSR prefetch + hydrate) · `fetchWithCache` LRU · nuqs · Zustand · RHF + Zod · Lingui RSC · OTel / APM

**SSR · ISR:** RSC prefetch for first paint · multi-layer cache (server LRU → Query → CDN `stale-while-revalidate`) · CMS LDP on-demand `revalidate` from Portal without full redeploy

### Links

[S10 TD](https://sgame.atlassian.net/wiki/spaces/S10/folder/2890465464/TD+Technical+Document) · [Seatincorp](https://github.com/Seatincorp) · [outsourcecentral](https://github.com/outsourcecentral)

---

<div align="center">

<sub>Seatincorp · S10 Tech Lead</sub>

</div>
