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

Ops / admin stack for S10 (users, promotion, loyalty, casino, risk, accumulation, CMS):

| Layer | Repo |
|---|---|
| Backend | [Seatincorp/portal-be](https://github.com/Seatincorp/portal-be) — Spring Boot 3 · Java 21 |
| Frontend | [Seatincorp/s10-fe → portal-fe](https://github.com/Seatincorp/s10-fe/tree/main/portal-fe) — React 18 · TypeScript · Ant Design |

### Brand products — Outsource Central

Also own / oversee product delivery under **[outsourcecentral](https://github.com/outsourcecentral)** — multi-brand FE apps & shared cores (`s10-fe-core`, `s10-app-core`, `s10-mini-apps`, brand repos such as `s10-6698`, `s10-6716`, `s10-9199`, …).

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
