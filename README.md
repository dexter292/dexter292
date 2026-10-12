<div align="center">

# Dexter

**Tech Lead — Team S10 @ [Seatincorp](https://github.com/Seatincorp)**

Architecture · Technology direction · Multi-brand SGames platform

</div>

---

### Role

Tech Lead for **S10**: set technical direction, own architecture decisions, and align delivery across the core platform, internal ops tools, and brand products.

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

Platform pillars below (data · risk · loyalty · delivery) are first-class ownership areas.

### Data platform

**[accumulation-service](https://github.com/Seatincorp/s10-kbgroup/tree/main/sgames-10/accumulation-service)** — event-driven data foundation for analytics & risk

- Ingest from RabbitMQ (bet, invoice/deposit, user, login, register, open-game, risk decisions)
- Normalize into a unified event model → MongoDB (+ Redis / ArangoDB for hot & graph paths)
- Feeds portal accumulation stats and Redis signal contracts consumed by the risk platform
- Stack: Java 21 · Spring Boot 3.2 · MongoDB · Redis · ArangoDB · RabbitMQ · JobRunr

### Risk platform

**[Seatincorp/risk-decision-service](https://github.com/Seatincorp/risk-decision-service)** — real-time risk decision engine

- `POST /decide` → ACCEPT / REJECT / CHALLENGE from profile rules (Mongo) + accumulated signals (Redis)
- Parallel profile eval, most-restrictive-wins merge, dry-run / shadow mode, DFP & blacklist loops
- Shared Redis key contracts with accumulation-service; CI/CD across EKS envs via devops-actions-s10
- Stack: Rust · Axum · Tokio · MongoDB · Redis

### Loyalty platform

**[loyalty-service](https://github.com/Seatincorp/s10-kbgroup/tree/main/sgames-10/loyalty-service)** — player loyalty / VIP / missions runtime

- Runtime evaluation for missions, VIP, raffles, leaderboards, auto-event rewards (e.g. global jackpot)
- Admin CRUD & config owned by Portal; player APIs & evaluation owned by loyalty-service
- Brand-routed Mongo + Redis caches; gateway path `/api/loyalty/`
- Stack: Java 21 · Spring Boot 3.2 · MongoDB · Redis

### DevOps / CI/CD

**[Seatincorp/devops-actions-s10](https://github.com/Seatincorp/devops-actions-s10)** — shared delivery rails for S10

- Reusable GitHub Actions workflows & Helmfile patterns for multi-env EKS (stg / pre / prod)
- Standardize build → deploy → promote across kbgroup services, portal, risk, and brand apps
- Domain / release ops aligned with S10 workflow & release checklist in TD docs

### Internal tools — SGames Portal

**Admin plane** for S10 — configure, operate, and observe the runtime platforms above. Portal couples to sgames via **shared datastores** + a few **HTTP ops bridges** (loyalty / notification / accumulation / agent), not via the player gateway for normal admin work.

| Layer | Repo | Stack |
|---|---|---|
| Backend | [Seatincorp/portal-be](https://github.com/Seatincorp/portal-be) | Spring Boot 3 · Java 21 · JWT RBAC · MyBatis/JPA |
| Frontend | [Seatincorp/s10-fe → portal-fe](https://github.com/Seatincorp/s10-fe/tree/main/portal-fe) | React 18 · TS · Vite · Ant Design 5 · React Query |

**What it owns**

| Area | Capabilities |
|---|---|
| **Identity & access** | JWT auth, users/roles/permissions/routes (`userms`), menu driven by BE |
| **Growth & promo** | Campaigns, gift codes, packages; brand-routed promotion Mongo (`BC12*` / `BC22*`) |
| **Loyalty ops** | Missions, VIP, raffles, leaderboards, jackpot config — CRUD in Portal, runtime in loyalty-service |
| **Data & analytics** | Unified events browse, Financial / User overviews, pre-aggregation & backfill ops |
| **Risk ops** | Fraud cases, graph (Arango), detection, user journey, data quality — alongside risk-decision-service |
| **Site & config** | Domain/mobile config, site items, launch templates, app/resource links |
| **Engagement** | Chat rooms/announcements, notification rules & send, CMS landing pages (LDP publish / ISR) |
| **Platform** | Audit trail, proxy tools, AI agent quota (AIMS), storage (S3) |

**Data plane (portal-be)** — MySQL ×5 · Mongo ×6 · Redis ×3 · ArangoDB · S3; 120+ controllers documented under Diátaxis hubs (Promotion, Loyalty, Accumulation/stats, Risk, Chat, Notification, CMS, …).

**FE surface** — modular SPA (`accumulation`, `loyalty`, `risk`, `promotion`, `cms`, `overview` FO/UO, `chat-management`, `user-management`, …); lazy routes + permission matrix; i18n en/vi; docs verified in CI.

Docs: [portal-be/docs](https://github.com/Seatincorp/portal-be/blob/main/docs/README.md) · [portal-fe/docs](https://github.com/Seatincorp/s10-fe/blob/main/portal-fe/docs/README.md) · [portal ↔ sgames](https://github.com/Seatincorp/portal-be/blob/main/docs/explanation/portal-sgames-relationship.md)

### Brand products — Outsource Central

Product delivery under **[outsourcecentral](https://github.com/outsourcecentral)** — multi-brand player sites on a shared **Next.js core**, plus mini-apps and mobile shells.

| Layer | Repos | Role |
|---|---|---|
| **Shared web/native core** | [s10-fe-core](https://github.com/outsourcecentral/s10-fe-core) (git submodule `./core`) | APIs, hooks, headless UI, axios (web/RN split), i18n, feature flags |
| **App / mobile core** | [s10-app-core](https://github.com/outsourcecentral/s10-app-core) · brand `*-app` (e.g. Flutter) | Native / Expo shells sharing core patterns |
| **Brand hosts** | Per-brand Next.js apps (private) | Skin + `core.config.ts` overrides; same rails, different brand |
| **Mini-apps** | [s10-mini-apps](https://github.com/outsourcecentral/s10-mini-apps) | Embeddable chat widgets, static sites |

#### Tech stack (brand web)

| Concern | Choice |
|---|---|
| Framework | **Next.js 15** App Router · **React 19** · TypeScript |
| Data | **TanStack Query v5** — `queryOptions` / hooks / **SSR prefetch** + `HydrationBoundary` |
| SSR cache | **`fetchWithCache`** — server LRU memory cache (TTL, size, retry) before hydrate to client |
| URL state | **nuqs** — typed query params shared client/server |
| Client state | **Zustand** for forms / UI; Query for server state |
| Forms | **react-hook-form** + **Zod** |
| i18n | **Lingui** — RSC + client providers (`vi` / `en`), extract/compile CLI |
| UI | Headless core components + brand styling (Radix primitives, Tailwind-class composition) |
| HTTP | Axios with **SSR URL rewrite** (`apiRewrites` / origin) vs CSR `location.origin` |
| Config | `withNextBaseConfig()` + build-time `core.config.ts` overrides per brand |
| Observability | OpenTelemetry web SDK · Elastic APM RUM · FingerprintJS |
| Edge HTML | Next **instrumentation** HTML minify (prod); gzip at nginx/Cloudflare (`compress: false` in Next) |

#### SSR · ISR · caching model

- **SSR first paint** — RSC layouts/pages prefetch critical queries (features, lobby lists, …) via core `prefetch*`; dehydrate into the client so first paint matches server
- **Multi-layer cache** — server LRU (`fetchWithCache`) → TanStack Query (client) → CDN/nginx (`Cache-Control` / `stale-while-revalidate`); feature probes aware that HTML may be edge-cached
- **ISR / on-demand revalidate** — CMS landing pages (LDP): Portal publish → brand Next `POST /api/revalidate` (`revalidateTag` / secret); content updates without full redeploy
- **Cross-platform core** — one import path; Metro picks `.native.ts`, Next picks `.ts` for web vs Expo/RN

Docs: [s10-fe-core](https://github.com/outsourcecentral/s10-fe-core) · Core TAD (S10 Confluence) · CMS LDP ↔ Next ISR via [portal-be CMS handbook](https://github.com/Seatincorp/portal-be/blob/main/docs/solutions/cms-system-handbook.md)

### How I work

Guided by the S10 **[TD — Technical Document](https://sgame.atlassian.net/wiki/spaces/S10/folder/2890465464/TD+Technical+Document)** program (Proposal → TDD → Spec → Onboarding → Incident).

- **Docs before code** — write / review Technical Proposal & TDD first; keep TS / API contracts and system overview as the shared source of truth
- **Architecture ownership** — decide service boundaries (gateway, lobby, accumulation, risk, loyalty, portal ↔ sgames); prefer explicit fail modes and shared contracts over ad‑hoc coupling
- **Delivery discipline** — Gitflow (feature → develop → release → main / hotfix); squash PRs with clear titles; release checklist (CI green, SemVer, QA, security scan, rollback plan, smoke on prod) via devops-actions-s10
- **Multi-brand scale** — branch onboarding templates, FE mono / core module patterns, and Outsource Central product ownership so brands ship on the same platform rails
- **Operate what we ship** — weekly tech sync, runbooks for critical flows, incident write‑ups that feed back into design; monitoring after go‑live before calling it done
- **Raise the bar continuously** — competency expectations for the team; evolve delivery (e.g. trunk-based + release train + feature flags) when Gitflow cost outweighs benefit

### Documentation

→ [S10 TD Technical Document](https://sgame.atlassian.net/wiki/spaces/S10/folder/2890465464/TD+Technical+Document)  
→ [Seatincorp](https://github.com/Seatincorp) · [outsourcecentral](https://github.com/outsourcecentral)

---

<div align="center">

<sub>Seatincorp · S10 Tech Lead</sub>

</div>
