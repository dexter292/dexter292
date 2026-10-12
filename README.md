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
| `accumulation-service` | Event ingest · risk foundation · hot Redis |
| `loyalty-service` · `gift-code-service` · `affiliate-service` | Loyalty / VIP / promotions runtime |
| `chat-service` · `notification-service` | Realtime chat & notifications |
| `agent-orchestrator` · `task-scheduler` | AI agent API & background jobs |

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
- **Architecture ownership** — decide service boundaries (gateway, lobby, accumulation, risk, portal ↔ sgames); prefer explicit fail modes and shared contracts over ad‑hoc coupling
- **Delivery discipline** — Gitflow (feature → develop → release → main / hotfix); squash PRs with clear titles; release checklist (CI green, SemVer, QA, security scan, rollback plan, smoke on prod)
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
