# Global Research — codev.id

Project/domain: **Codev / codev.id**
Stage status: **Verified pre-writing evidence map; not article drafts, legal advice, certification, or a production design**
Last verified: **2026-07-25 (Asia/Jakarta)**
Frozen coverage: **20 parent topics / 120 catalog articles**
Authority map: [TOPICAL_AUTHORITY.md](TOPICAL_AUTHORITY.md)
Article catalog: [ARTICLE_CATALOG.md](ARTICLE_CATALOG.md)

> Article writers must open and cite the original sources, not this research file. Standards, laws, provider capabilities, security guidance, and product documentation must be rechecked when an outline is created.

## Evidence grades

| Grade | Meaning | Permitted use |
|---|---|---|
| A-ID | Current Indonesian legislation or official Indonesian record | Visible legal status and scope; exact legal application remains subject to current consolidated text and qualified review. |
| A-STD | Official standards/specification body | Definitions, normative requirements, and test/conformance scope within the cited edition; not proof that a project conforms. |
| B-GOV | Government technical framework | Lifecycle/control guidance; not Indonesian law or a product certification. |
| B-IND | Recognized nonprofit industry project | Practical control and risk guidance with version/scope stated. |
| C-VEN | Vendor/product documentation | Current behavior for that vendor/product only; recheck plan, region, runtime, and version. |
| P | Frozen project artifact | Editorial ownership and boundaries only. |
| G | Unresolved evidence gate | Definitive claim remains conditional pending project facts, tests, documents, or professional review. |

## Research records

### KR-01 — Frozen scope and lifecycle model
- **Sources:** [topical authority](TOPICAL_AUTHORITY.md) and [article catalog](ARTICLE_CATALOG.md).
- **Grade:** P.
- **Purpose:** Preserve the canonical software-development knowledge architecture.
- **Summary:** The catalog follows the full lifecycle from discovery through proof and vendor exit, with security, privacy, accessibility, performance, operations, and AI as cross-cutting controls.
- **Grounded facts:** There are 20 topic families and six distinct articles per family. Commercial routes retain substantiated service intent.
- **Incorporation:** Route every outline to one `CDV-*` owner and link dependencies at their decision point.
- **Limits/recheck:** Local planning is not technical evidence; never reopen topic boundaries during research.

### KR-02 — Requirements, user evidence, and acceptance
- **Sources:** [UK Government Service Manual—agile delivery](https://www.gov.uk/service-manual/agile-delivery), [W3C WCAG 2.2](https://www.w3.org/TR/WCAG22/).
- **Grade:** B-GOV / A-STD.
- **Purpose:** Ground discovery, user/task research, incremental delivery, and testable acceptance.
- **Summary:** Requirements should connect user problems and service outcomes to observable behavior and quality constraints; accessibility belongs in requirements rather than a final scan.
- **Grounded facts:** Assumptions, stakeholders, journeys, functional behavior, quality attributes, constraints, acceptance evidence, and traceability answer different questions.
- **Incorporation:** `CDV-01`, `CDV-02`, `CDV-10`, `CDV-17`; turn uncertain claims into hypotheses and acceptance examples.
- **Limits/recheck:** A template does not validate demand or represent all users; require project research and decision ownership under GATE-01.

### KR-03 — Architecture decisions and web delivery
- **Sources:** [AWS Architecture Decision Records guidance](https://docs.aws.amazon.com/prescriptive-guidance/latest/architectural-decision-records/adr-process.html), [WHATWG HTML Living Standard](https://html.spec.whatwg.org/), [HTTP Semantics RFC 9110](https://www.rfc-editor.org/rfc/rfc9110).
- **Grade:** C-VEN / A-STD.
- **Purpose:** Support explicit architecture trade-offs and standards-based web behavior.
- **Summary:** An ADR records context, decision, alternatives, and consequences; web architecture must distinguish browser standards, HTTP semantics, rendering, state, content, identity, and operational constraints.
- **Grounded facts:** Static, server-rendered, client-rendered, CMS, custom, monolithic, modular, and serverless are options—not maturity ranks.
- **Incorporation:** `CDV-03`, `CDV-04`, `CDV-05`; show constraints, alternatives, reversibility, failure modes, and operating cost.
- **Limits/recheck:** AWS examples are vendor guidance, not a required method. No stack recommendation without GATE-01 and GATE-02.

### KR-04 — APIs, contracts, and authentication
- **Sources:** [OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html), [OAuth 2.0 Security BCP—RFC 9700](https://www.rfc-editor.org/info/rfc9700/), [WebAuthn Level 3](https://www.w3.org/TR/webauthn-3/), [OWASP API Security Top 10 2023](https://owasp.org/API-Security/editions/2023/en/0x11-t10/).
- **Grade:** A-STD / B-IND.
- **Purpose:** Ground contract-first APIs, authorization flows, passkeys, and API abuse controls.
- **Summary:** API syntax, identity proof, authorization, token handling, object/function access, rate/resource control, versioning, and deprecation are separate design concerns.
- **Grounded facts:** RFC 9700 is a 2025 best-current-practice update for OAuth 2.0. OpenAPI describes an interface; it does not prove implementation behavior or security.
- **Incorporation:** `CDV-06`, `CDV-08`, `CDV-09`, `CDV-10`; pair contract examples with threat, error, idempotency, consumer-test, and retirement evidence.
- **Limits/recheck:** Never publish secrets/private schemas or prescribe a flow without client/threat context. Apply GATE-03 and GATE-04.

### KR-05 — Data lifecycle, Indonesian privacy, and recovery
- **Sources:** [UU No. 27 Tahun 2022—BPK](https://peraturan.bpk.go.id/Details/229798/uu-no-27-tahun-2022), [PP No. 71 Tahun 2019—BPK](https://peraturan.bpk.go.id/Details/122030/pp-no-71-tahun-2019), [NIST Privacy Framework](https://www.nist.gov/privacy-framework).
- **Grade:** A-ID / B-GOV.
- **Purpose:** Ground personal-data mapping, electronic-system context, retention, rights, security, deletion, backup, and recovery.
- **Summary:** Data engineering must identify meaning, owner, purpose, access, lineage, quality, retention, backup/restore, deletion, processors/vendors, and incident paths.
- **Grounded facts:** Indonesia's PDP Law is the primary national personal-data statute; PP 71/2019 governs electronic systems and transactions at a broader level. A backup exists only as evidence when restoration is tested.
- **Incorporation:** `CDV-07`, `CDV-09`, `CDV-12`, `CDV-15`; use data maps and restore/reconciliation evidence.
- **Limits/recheck:** Do not infer lawful basis, role, transfer permission, retention period, notification duty, or sector rule without GATE-05.

### KR-06 — Secure software development and application assurance
- **Sources:** [NIST SP 800-218 SSDF 1.1](https://csrc.nist.gov/pubs/sp/800/218/final), [NIST SSDF publications and revision status](https://csrc.nist.gov/Projects/ssdf/publications), [OWASP ASVS project](https://owasp.org/www-project-application-security-verification-standard/).
- **Grade:** B-GOV / B-IND.
- **Purpose:** Establish lifecycle security, verification requirements, vulnerability handling, and evidence limits.
- **Summary:** Security is integrated into people/process, software protection, secure production, and vulnerability response; ASVS can structure application requirements and verification.
- **Grounded facts:** SSDF 1.1 remains final while revision 1.2 is draft as verified. A scan or ASVS checklist is not a penetration certificate or proof of complete security.
- **Incorporation:** `CDV-09`, `CDV-10`, `CDV-15`, `CDV-17`; map threats to controls, owners, verification, exceptions, and response.
- **Limits/recheck:** Version-pin ASVS/SSDF and obtain qualified testing for consequential systems under GATE-03.

### KR-07 — Dependencies, integrations, and software supply chain
- **Sources:** [CISA SBOM resources](https://www.cisa.gov/sbom), [NIST SP 800-161 Rev.1](https://csrc.nist.gov/pubs/sp/800/161/r1/final), [OpenSSF Scorecard](https://securityscorecards.dev/).
- **Grade:** B-GOV / B-IND.
- **Purpose:** Ground dependency inventory, vendor evaluation, provenance, and integration failure planning.
- **Summary:** Third-party code and services create lifecycle risk across identity, availability, data, licensing, change, compromise, and exit.
- **Grounded facts:** An SBOM improves component transparency but does not establish safety. A repository score is a signal, not due diligence.
- **Incorporation:** `CDV-08`, `CDV-09`, `CDV-15`, `CDV-17`; require dependency registers, data paths, retries/reconciliation, status/support evidence, and exit plans.
- **Limits/recheck:** Current vendor terms, APIs, quotas, subprocessors, and vulnerabilities require GATE-04 and GATE-09.

### KR-08 — Testing strategy and release evidence
- **Sources:** [NIST SP 800-218 SSDF 1.1](https://csrc.nist.gov/pubs/sp/800/218/final), [W3C WCAG-EM 1.0](https://www.w3.org/TR/WCAG-EM/), [OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html).
- **Grade:** B-GOV / A-STD.
- **Purpose:** Separate test levels, specialist checks, acceptance, and release decisions.
- **Summary:** Unit, integration, contract, end-to-end, exploratory, accessibility, security, performance, resilience, and user acceptance tests cover different risks.
- **Grounded facts:** Passing automated tests proves only the sampled assertions, environment, build, and data. Traceability connects risks and requirements to results and unresolved defects.
- **Incorporation:** `CDV-10`, `CDV-13`, `CDV-14`, `CDV-18`; state environment, version, dataset, result, reviewer, exceptions, and release authority.
- **Limits/recheck:** No universal test pyramid or coverage threshold; use GATE-06.

### KR-09 — Cloudflare deployment and delivery controls
- **Sources:** [Cloudflare Pages documentation](https://developers.cloudflare.com/pages/), [Cloudflare Workers documentation](https://developers.cloudflare.com/workers/), [Cloudflare deployments documentation](https://developers.cloudflare.com/workers/configuration/versions-and-deployments/).
- **Grade:** C-VEN.
- **Purpose:** Ground Pages/Workers selection, environments, configuration, deployments, and rollback.
- **Summary:** Pages and Workers overlap but have different workflows and capabilities; production safety depends on versioned code/configuration, secret handling, migrations, preview/staging separation, cache/DNS behavior, and rollback evidence.
- **Grounded facts:** Provider docs establish current product behavior only for the cited platform/date. A successful upload is not an end-to-end release.
- **Incorporation:** `CDV-11`, `CDV-12`, `CDV-18`; use pipeline diagrams and tested deployment/rollback checklists.
- **Limits/recheck:** Recheck limits, pricing, APIs, runtime compatibility, regional/data implications, and actual account configuration under GATE-07.

### KR-10 — Observability, SLOs, incidents, capacity, and cost
- **Sources:** [Google SRE Workbook—SLOs](https://sre.google/workbook/implementing-slos/), [OpenTelemetry documentation](https://opentelemetry.io/docs/), [NIST incident response SP 800-61 Rev.3](https://csrc.nist.gov/pubs/sp/800/61/r3/final).
- **Grade:** B-IND / B-GOV.
- **Purpose:** Ground service health definitions, telemetry, alerting, response, learning, and capacity/cost controls.
- **Summary:** Useful telemetry follows user/service risks; SLOs, logs, metrics, traces, alerts, runbooks, incident roles, post-incident actions, quotas, and cost attribution are distinct artifacts.
- **Grounded facts:** Instrumentation creates signals, not reliability. An SLO is a service objective and decision mechanism, not a contractual uptime promise.
- **Incorporation:** `CDV-12`, `CDV-14`, `CDV-18`; define owner, window, data source, privacy, alert action, escalation, and learning loop.
- **Limits/recheck:** No 24/7 or uptime claim without actual operating evidence/contract. Apply GATE-07 and GATE-08.

### KR-11 — Accessibility conformance and inclusive testing
- **Sources:** [WCAG 2.2 Recommendation](https://www.w3.org/TR/WCAG22/), [WCAG-EM 1.0](https://www.w3.org/TR/WCAG-EM/), [WAI Easy Checks](https://www.w3.org/WAI/test-evaluate/preliminary/).
- **Grade:** A-STD.
- **Purpose:** Ground accessible design, implementation, evaluation, procurement, and maintenance.
- **Summary:** WCAG 2.2 addresses perceivable, operable, understandable, and robust content with conformance requirements; representative evaluation combines automated and manual inspection.
- **Grounded facts:** Full-page and process scope matter. Keyboard/focus, semantics, forms/errors, reflow/zoom, authentication, media, and assistive-technology behavior cannot be certified by one scanner.
- **Incorporation:** `CDV-02`, `CDV-05`, `CDV-10`, `CDV-13`, `CDV-17`; map criteria, techniques, manual tests, user review, defects, and retest.
- **Limits/recheck:** WCAG conformance is not automatically Indonesian legal compliance. Apply GATE-05 and GATE-06.

### KR-12 — Web performance and reliability measurement
- **Sources:** [web.dev Core Web Vitals](https://web.dev/articles/vitals), [Chrome UX Report documentation](https://developer.chrome.com/docs/crux), [HTTP caching RFC 9111](https://www.rfc-editor.org/rfc/rfc9111).
- **Grade:** C-VEN / A-STD.
- **Purpose:** Ground lab/field measurement, budgets, caching, regression, and causal claims.
- **Summary:** User performance varies by device, network, geography, content, cache, workload, percentile, and measurement method; field and lab data answer different questions.
- **Grounded facts:** Core Web Vitals are provider-defined evolving metrics. A before/after claim needs stable scope, sample, conditions, version, and caveats.
- **Incorporation:** `CDV-04`, `CDV-10`, `CDV-12`, `CDV-14`; publish budgets and measurement protocols, not universal scores.
- **Limits/recheck:** No ranking, load-time, energy, or conversion guarantee. Recheck thresholds/tools and apply GATE-08.

### KR-13 — Maintenance, modernization, migration, and retirement
- **Sources:** [NIST SSDF publications](https://csrc.nist.gov/Projects/ssdf/publications), [CISA Known Exploited Vulnerabilities Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog), [Google Search site-move guidance](https://developers.google.com/search/docs/crawling-indexing/site-move-with-url-changes).
- **Grade:** B-GOV / C-VEN.
- **Purpose:** Ground dependency/runtime maintenance, vulnerability prioritization, migration, recovery, and decommissioning.
- **Summary:** Refactor, upgrade, replatform, strangler migration, rewrite, archive, and deletion have different risk/exit profiles.
- **Grounded facts:** Vulnerability severity is not the sole prioritization input; exposure, exploitation, business impact, fix safety, rollback, and ownership matter. URL/data migrations need inventories and reconciliation.
- **Incorporation:** `CDV-15`, `CDV-18`, `CDV-19`; use health audits, compatibility matrices, staged migration, recovery tests, and retirement evidence.
- **Limits/recheck:** Never prescribe replacement from age alone or delete history/data without GATE-02, GATE-05, and GATE-08.

### KR-14 — AI and automation risk boundaries
- **Sources:** [NIST AI RMF 1.0](https://www.nist.gov/itl/ai-risk-management-framework), [NIST AI 600-1 Generative AI Profile](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf), [NIST SP 800-218A](https://csrc.nist.gov/pubs/sp/800/218/a/final).
- **Grade:** B-GOV.
- **Purpose:** Ground AI use-case selection, data rights, evaluation, human control, monitoring, fallback, and retirement.
- **Summary:** AI risk management is sociotechnical and lifecycle-wide; rules, predictive ML, generative models, retrieval, agents, and human workflows require different evaluation and control.
- **Grounded facts:** Fluent output is not correctness. Evaluation sets must represent the intended task and failure/abuse cases; human review needs real authority, information, and fallback.
- **Incorporation:** `CDV-16`, plus `CDV-07`, `CDV-09`, `CDV-10`, `CDV-12`, `CDV-18`; record baseline, model/provider/version, data path, metrics, overrides, monitoring, and disable path.
- **Limits/recheck:** Never invent accuracy, autonomy, privacy, copyright permission, or provider retention behavior. Apply GATE-05 and GATE-10.

### KR-15 — Procurement, governance, handover, and vendor exit
- **Sources:** [NIST SP 800-161 Rev.1](https://csrc.nist.gov/pubs/sp/800/161/r1/final), [CISA Secure by Design](https://www.cisa.gov/securebydesign), [UK Technology Code of Practice](https://www.gov.uk/guidance/the-technology-code-of-practice).
- **Grade:** B-GOV.
- **Purpose:** Ground comparable procurement, risk allocation, delivery roles, evidence handover, and operational independence.
- **Summary:** Buyers need requirements, assumptions, exclusions, milestones, change control, ownership, accounts, source, data, security, accessibility, tests, acceptance, warranty/support, cost, lock-in, transition, and exit.
- **Grounded facts:** Lowest build price is not total lifecycle cost. A portfolio or certification logo does not prove the proposed team's scope or outcomes.
- **Incorporation:** `CDV-17`, `CDV-18`, `CDV-20`; use RFP/bid matrices, RACI, decision logs, release gates, handover indexes, and exit plans.
- **Limits/recheck:** No legal interpretation, market price, vendor endorsement, or capability claim without GATE-09 and qualified contract review.

### KR-16 — Content systems, crawl/indexing, and claim quality
- **Sources:** [Google Search Essentials](https://developers.google.com/search/docs/essentials), [Google people-first content guidance](https://developers.google.com/search/docs/fundamentals/creating-helpful-content), [Schema.org documentation](https://schema.org/docs/documents.html).
- **Grade:** C-VEN / B-IND.
- **Purpose:** Ground content models, crawl/index controls, structured data, migrations, and evidence-led SEO.
- **Summary:** Content quality, crawlability, indexability, canonicalization, structured data eligibility, ranking, qualified leads, and revenue are separate outcomes.
- **Grounded facts:** Sitemaps and structured data aid discovery/interpretation but do not guarantee indexing, rich results, ranking, traffic, or revenue.
- **Incorporation:** `CDV-19`, `CDV-15`, `CDV-20`; require content schemas, redirect ledgers, validation, Search Console evidence, and human editorial controls.
- **Limits/recheck:** Search systems and policies change. No city swaps, link schemes, approval, ranking, or income promises; apply GATE-08.

### KR-17 — Capability proof and case-study evidence
- **Sources:** [UK Government Service Standard](https://www.gov.uk/service-manual/service-standard), [NIST SSDF 1.1](https://csrc.nist.gov/pubs/sp/800/218/final), [W3C WCAG-EM](https://www.w3.org/TR/WCAG-EM/).
- **Grade:** B-GOV / A-STD.
- **Purpose:** Establish an evidence hierarchy for provider selection, portfolios, cases, and measured outcomes.
- **Summary:** Credible proof identifies legal/team identity, role, dates, scope, methods, artifacts, consent, baselines, verification, outcomes, caveats, failures, and current status.
- **Grounded facts:** A screenshot, domain, logo, testimonial, tool badge, or live page alone does not prove authorship, scope, conformance, security, or business impact.
- **Incorporation:** `CDV-20`, `CDV-17`, `CDV-18`; separate observation, owner assertion, third-party verification, and measured result.
- **Limits/recheck:** Do not invent clients, results, certifications, team competence, or consent. Apply GATE-09.

## Recurring intent-shape map

| Intent shape | Representative catalog families | Main records | Control |
|---|---|---|---|
| Foundation/definitions | CDV-01, CDV-03, CDV-06, CDV-13 | KR-02, KR-03, KR-04, KR-11 | Define term, scope, version, and non-equivalent concepts. |
| Selection/decision | CDV-03–CDV-05, CDV-11, CDV-16 | KR-03, KR-09, KR-14 | Compare constraints, alternatives, operating burden, exit, and evidence. |
| Specification | CDV-01, CDV-06, CDV-07, CDV-17 | KR-02, KR-04, KR-05, KR-15 | Translate goals into testable contracts and acceptance. |
| Design/interfaces | CDV-02, CDV-04, CDV-08 | KR-03, KR-04, KR-07, KR-11 | Model users, states, trust boundaries, data, and failure paths. |
| Execution/QC | CDV-09–CDV-14 | KR-06, KR-08–KR-12 | Version evidence and connect controls to tests and release authority. |
| Diagnosis | CDV-10, CDV-12, CDV-14, CDV-15 | KR-08, KR-10, KR-12, KR-13 | Preserve environment/timeline; distinguish symptom, cause, and decision. |
| Maintenance | CDV-07, CDV-12, CDV-15, CDV-19 | KR-05, KR-10, KR-13, KR-16 | Track ownership, change, restore/rollback, deletion, and refresh. |
| Procurement/handover | CDV-17, CDV-18, CDV-20 | KR-07, KR-15, KR-17 | Normalize scope and preserve source, accounts, evidence, and exit. |

## Evidence gates

| Gate | Release condition | Held claims |
|---|---|---|
| `GATE-01` | Named users/stakeholders, current process, constraints, baseline, and acceptance owner are evidenced. | Need, priority, MVP, usability, or fitness for purpose. |
| `GATE-02` | Current system/data/dependency inventory, quality attributes, capacity/cost assumptions, and ADR review exist. | Architecture/stack suitability, rewrite/migration, scalability, or cost. |
| `GATE-03` | Versioned threat model, control set, secure-development evidence, and qualified verification cover the exact release. | Secure, ASVS-aligned, vulnerability-free, or penetration-tested. |
| `GATE-04` | Current provider/API/auth contract, scopes, quotas, data flow, failure behavior, tests, and exit are verified. | Integration compatibility, OAuth safety, API reliability, or vendor behavior. |
| `GATE-05` | Current consolidated Indonesian law, sector/local rules, roles, data map, purposes/bases, rights, vendors/transfers, retention, incident and legal review exist. | PDP/legal compliance, consent need, transfer, retention, deletion, or notification duty. |
| `GATE-06` | Defined release scope, environments, representative data/users, criteria, test results, defects/exceptions, and decision owner exist. | Quality, accessibility conformance, acceptance, or release readiness. |
| `GATE-07` | Actual Cloudflare/account/runtime configuration, current provider docs/limits, deployment, migration, secret, rollback, DNS/cache and smoke tests are recorded. | Production design, deployability, rollback, regional behavior, price, or quota. |
| `GATE-08` | Dated lab/field/telemetry evidence states build, environment, sample, percentile, cache, tool/version, baseline, incident/cost window, and owner. | Speed, reliability, uptime, capacity, SEO, conversion, or operating-cost result. |
| `GATE-09` | Contract/current offer and verified provider evidence cover entity/team, scope, ownership, consent, source/accounts, subcontractors, warranty/support, results, handover, and exit. | Price, competence, client relationship, certification, warranty, or case outcome. |
| `GATE-10` | AI use-case baseline, data rights/provider handling, versioned evaluation set, failure/abuse results, human authority, monitoring, fallback, and retirement plan exist. | AI accuracy, autonomy, safety, privacy, copyright, savings, or business outcome. |

## Parent-topic coverage matrix

| Topic family | Main research IDs | Safe ground for the six articles | Remaining gate |
|---|---|---|---|
| `CDV-01` | KR-01, KR-02 | Software discovery and requirements: problem, users, assumptions, functional/quality requirements, MVP, traceability, acceptance. | GATE-01 |
| `CDV-02` | KR-02, KR-11 | Product/UX/interface design: research, flows, prototypes, content, usability, responsive states, accessible handoff. | GATE-01, GATE-06 |
| `CDV-03` | KR-03, KR-07 | Architecture and technology decisions: constraints, ADRs, trade-offs, resilience, cost, operability, exit. | GATE-02 |
| `CDV-04` | KR-03, KR-11, KR-12 | Website/web-app engineering: standards, rendering/state, CMS/custom, forms, identity, progressive enhancement. | GATE-02, GATE-06 |
| `CDV-05` | KR-03, KR-11 | Mobile/installable apps: web/PWA/native choices, offline/sync, permissions, release, telemetry, accessibility. | GATE-02, GATE-06 |
| `CDV-06` | KR-04, KR-08 | API lifecycle: contracts, HTTP, auth, errors, pagination, idempotency, versions, deprecation, consumer tests. | GATE-03, GATE-04 |
| `CDV-07` | KR-05, KR-13 | Data architecture/quality/privacy/recovery: meaning, integrity, lifecycle, migration, backup/restore, deletion. | GATE-02, GATE-05 |
| `CDV-08` | KR-04, KR-07 | Integrations: identity, payments/messaging, webhooks/queues, retry, reconciliation, fallback, vendor exit. | GATE-04, GATE-09 |
| `CDV-09` | KR-04–KR-07 | Security and privacy engineering: threat/control lifecycle, identity/access, supply chain, PDP mapping, incidents. | GATE-03, GATE-05 |
| `CDV-10` | KR-08, KR-11, KR-12 | Testing and release evidence: risk-based levels, specialist tests, traceability, defects, acceptance, release report. | GATE-06 |
| `CDV-11` | KR-09, KR-10 | Cloudflare deployment: Pages/Workers choice, environments, CI/CD, secrets, migrations, cache, rollback. | GATE-07 |
| `CDV-12` | KR-10, KR-12 | Operations: SLOs, telemetry, alerts, runbooks, incidents, capacity, quotas, cost and vendor status. | GATE-07, GATE-08 |
| `CDV-13` | KR-02, KR-08, KR-11 | Accessibility: WCAG 2.2 scope, design/dev criteria, manual/automated/AT evaluation, procurement and maintenance. | GATE-05, GATE-06 |
| `CDV-14` | KR-10, KR-12 | Performance/reliability: budgets, lab/field evidence, bottlenecks, caching, load/resilience and regression. | GATE-08 |
| `CDV-15` | KR-07, KR-13 | Maintenance/modernization/decommissioning: dependencies, debt, upgrades, migration/recovery, archive/deletion. | GATE-02, GATE-05, GATE-08 |
| `CDV-16` | KR-05, KR-14 | AI/automation delivery: use-case baseline, data/model/provider, evaluation, human control, monitoring, fallback. | GATE-05, GATE-10 |
| `CDV-17` | KR-07, KR-15 | Estimation/procurement/contracts/TCO: comparable scope, uncertainty, ownership, risk, change, lifecycle cost, lock-in. | GATE-09 |
| `CDV-18` | KR-08–KR-10, KR-15 | Governance/release/handover/vendor exit: roles, decisions, change, gates, source/accounts, runbooks, transition. | GATE-06, GATE-07, GATE-09 |
| `CDV-19` | KR-13, KR-16 | Content systems/discoverability/migration: models, workflow, crawl/index/canonical, structured data, redirects, analytics. | GATE-05, GATE-08 |
| `CDV-20` | KR-15, KR-17 | Capability proof/cases/provider selection: identity, role, consent, scope, methods, baselines, results, caveats, references. | GATE-09 |

## Mechanical acceptance

- [x] Seventeen stable records contain every required field and direct source links.
- [x] All 20 frozen topic IDs appear exactly once in the coverage matrix.
- [x] Actual recurring intent shapes map to reusable records.
- [x] Ten stable gates hold legal, security, project, vendor, release, production, performance, procurement, and AI claims.
- [x] No article prose, production change, portfolio-ledger edit, invented metric, vendor endorsement, or compliance declaration is included.
