# Topical Authority — codev.id

## Role and boundary

`codev.id` should become an Indonesian company-level reference for planning, buying, designing, building, releasing, operating, and maintaining software. “Codev” means code development, as confirmed by the owner. The current repository specifically evidences commercial work in websites and web applications, WordPress and custom websites, UI/UX for websites and mobile applications, front-end/back-end/full-stack development, content production, redesign, repair, maintenance, hosting on Cloudflare's edge network, and a named portfolio. It also mentions mobile development and DevOps in educational/service copy. The plan may therefore explain the full software-delivery lifecycle, but a page must not convert an educational topic into a capability claim until team, process, technology, and project evidence support that claim.

The primary readers are Indonesian founders, SMEs, organizations, product owners, procurement staff, and technical teams deciding how to commission or improve a digital product. Indonesia-specific privacy, contracting, payment, connectivity, language, and operating conditions belong where they materially change the decision. No city, province, or region-swapped article is planned.

The root domain owns cross-cutting software-development knowledge and the Codev company/service evidence layer. Related subdomains receive independent plans and retain their own product intent: `domain.codev.id` for the evidenced domain-management system; `automate.codev.id`, `gadget.codev.id`, `invest.codev.id`, `iot.codev.id`, `pools.codev.id`, `tools.codev.id`, and `whois.codev.id` for their respective products after each repository is audited. Root articles may explain how to discover, architect, secure, integrate, test, deploy, or operate such products, but must not become their feature catalog, product documentation, or sales landing pages.

Existing `/web-development`, `/ui-ux`, `/website-wordpress`, `/website/*`, `/konten/*`, `/edit-website`, `/redesign-website`, and `/web-google-adsense` routes own their commercial intent until a later evidence-led consolidation. Editorial pages answer one informational or decision job and link to a commercial route only when the actual offer is substantiated.

## Evidence audited

Audit date: 2026-07-23. Repository observations are from clean `main` at commit `3954141`, tracking `origin/main` (`cfpages-sandbox-elin/Codev.id`). The live homepage responded and matched the exported site's website-development offer; child-route fetching through the research tool was inconclusive, so route findings below come from the canonical checkout and must be rechecked at deployment before implementation.

| Evidence | Observed count/finding | Planning implication |
|---|---:|---|
| Sitemap manifests | 5 | `page-sitemap.xml`, three post sitemaps, and `sitemap-complete.xml` were parsed completely |
| Raw sitemap entries | 1,108 | 45 page rows, 490 post rows, and 573 complete-sitemap rows |
| Exact sitemap URL strings | 1,062 | `.html` and extensionless variants create duplicate-route risk |
| Normalized sitemap routes | 573 | All 534 normalized child-sitemap routes appear in `sitemap-complete.xml`; the latter adds 39 archive/pagination routes |
| Local HTML files | 573 | Static WordPress export with no source content collection or application build metadata |
| City-swapped article files | 489 | `pembuatan-website-<place>.html`; omitted from the new map and not counted as authority |
| Commercial/general pages in page sitemap | 45 | Home, news, and contact; 9 other service/general routes including portfolio, demo, and theme; 28 industry website routes; and 5 content routes |
| Industry website routes | 28 | `/website/*` pages share “jasa pembuatan website” commercial intent and need proof/differentiation |
| Content-service routes | 5 | `/konten` plus website, SEO, blog-post, and product-description children |
| Province/category archives | 34 | Geography taxonomies, not editorial coverage |
| News/listing pages | 4 | `/berita` plus three pagination pages; no substantive non-location article collection was found |
| Root city pages | 489 | Large location-template footprint; no corresponding unique case, local regulation, team, address, or measured local evidence was found in the repository audit |
| Portfolio page | 1 | Names projects and describes project types, but supplies no consent records, baselines, methods, test reports, or measured outcomes |
| Existing topical-authority files | 0 | These two documents establish the first evidence-based map |

The audit inspected repository instructions, Markdown, Git status/branch/remote, every sitemap entry, local route inventory, titles and H1s, navigation, homepage and service-page copy, portfolio names/descriptions, and representative city templates. The strongest supported offer is website delivery; mobile, API, data, Cloudflare, security, AI, and automation articles require capability-neutral wording unless further evidence is added.

Primary-source gates checked on 2026-07-23:

- [NIST SP 800-218, Secure Software Development Framework 1.1](https://csrc.nist.gov/pubs/sp/800/218/final) provides a current high-level secure-development framework usable by producers and purchasers.
- [OWASP ASVS 5.0.0](https://owasp.org/www-project-application-security-verification-standard/) provides testable web-application security requirements; use versioned requirement IDs when citing individual controls.
- [WCAG 2.2](https://www.w3.org/TR/WCAG22/) is the current W3C Recommendation for testable web-content accessibility criteria.
- [OpenAPI Specification 3.2.0](https://spec.openapis.org/oas/latest.html) is the current authoritative OpenAPI rendering; tooling compatibility must be verified before recommending a particular minor version.
- [Cloudflare Pages documentation](https://developers.cloudflare.com/pages/) documents Git/direct deployments, Pages Functions, rollbacks, and redirects. [Workers observability documentation](https://developers.cloudflare.com/workers/observability/) documents current logs, traces, metrics, analytics, and OpenTelemetry export. Recheck limits, pricing, and feature status at publication.
- [UU No. 27 Tahun 2022 tentang Pelindungan Data Pribadi](https://peraturan.bpk.go.id/Details/229798/uu-no-27-tahun-2022.12UUD) governs personal-data processing, controller/processor duties, transfers, rights, and sanctions in Indonesia. Articles need current qualified legal review and must account for later decisions and implementing rules.

## Existing coverage and risks

The site presents a broad, useful service taxonomy but has five material authority risks:

1. The 489 place-swapped pages and 34 geography archives dominate the crawl footprint without proving different local substance. They are omitted from this catalog and require performance/equity review before any redirect, noindex, or removal.
2. Post sitemaps use `.html` URLs while `sitemap-complete.xml` uses extensionless forms for the same normalized routes. Canonicals, live responses, internal links, Search Console selection, and backlink equity must be checked before choosing one form.
3. Existing copy mixes education and sales, contains unsupported absolutes or promises (including security, ranking, speed, delivery time, availability, and nationwide service), and sometimes states current technologies as timeless facts.
4. The portfolio proves that project names are displayed, not that Codev delivered a particular scope or result. No case metric, client consent, technical decision record, acceptance evidence, or before/after baseline may be invented.
5. The static export has no maintainable article source model, build/test configuration, content schema, or modern deployment record. The first publication wave therefore requires an implementation decision, not merely more HTML files.

## Coverage matrix

| Completeness lens | Topic owner(s) | Coverage decision |
|---|---|---|
| Definition, vocabulary, taxonomy, and evolution | CDV-01, CDV-03–CDV-06 | Explain product, project, platform, website, app, API, architecture, and lifecycle terms only where they improve a decision |
| Need recognition, discovery, and diagnosis | CDV-01, CDV-02, CDV-15 | Establish problem, user, evidence, constraints, and whether software/change is warranted |
| Requirements and design | CDV-01–CDV-03, CDV-13 | Functional, quality, privacy, accessibility, data, operational, and acceptance requirements |
| Web, app, API, data, and integration systems | CDV-04–CDV-08 | Separate system surfaces and interfaces so one broad “development” page does not own every query |
| Security, privacy, and governance | CDV-09, CDV-16–CDV-18 | Secure development and Indonesian privacy gates across procurement and delivery |
| Testing and acceptance | CDV-10 | Layered verification, traceability, environments, defects, and release evidence |
| Deployment and Cloudflare | CDV-11 | Provider-supported patterns; no invented production architecture |
| Operation, incidents, and economics | CDV-12 | SLOs, logs, traces, alerts, runbooks, capacity, and cost controls |
| Accessibility | CDV-13 | WCAG 2.2 planning, implementation, testing, content, and procurement |
| Performance and reliability | CDV-14 | Budgets, measurement, front/back-end diagnosis, caching, and resilience |
| Maintenance, upgrade, and replacement | CDV-15 | Dependencies, debt, modernization, migrations, and decommissioning |
| AI and automation | CDV-16 | Evidence-led use-case selection, data/risk controls, evaluation, human oversight, and monitoring |
| Budget, procurement, and vendor decisions | CDV-17 | Scope, estimation, RFP, contracts, IP, TCO, and lock-in |
| Delivery, handover, and exit | CDV-18 | Roles, change control, release readiness, documentation, ownership, and transition |
| Content systems and discoverability | CDV-19 | Existing content/SEO offer represented without ranking guarantees or thin keyword pages |
| Commercial support and case evidence | CDV-20 | Capability proof, portfolio provenance, case protocol, testimonials, results, and selection |
| Stakeholders and scale | CDV-01–CDV-03, CDV-12, CDV-17–CDV-20 | Founder, product owner, user, engineer, operator, procurement, legal/privacy, and vendor paths |
| New build versus retrofit | CDV-03–CDV-05, CDV-08, CDV-15 | Greenfield choices separated from legacy constraints and migration risk |
| DIY versus professional | CDV-01, CDV-09, CDV-11, CDV-17 | Safe checklists and transparent stop conditions; no false assurance from tools or templates |
| Indonesia and geography | CDV-07, CDV-09, CDV-12, CDV-17, CDV-19 | National legal/market/connectivity context where material; no city or province swaps |
| Safety, failure modes, and myths | CDV-03, CDV-07–CDV-16 | Failure-mode and recovery thinking; correct absolute “100% secure,” “instant ranking,” and one-tool myths |
| Environmental impact | CDV-03, CDV-11, CDV-12, CDV-15 | Measure workload, storage, transfer, hardware/service life, and waste before claims; no greenwashing |
| Calculators, checklists, diagrams, and cases | CDV-01–CDV-20 | Assumptions, version, evidence source, uncertainty, and review triggers are mandatory |
| News and trends | CDV-03, CDV-09, CDV-11, CDV-16 | No recurring news feed; publish only durable changes with a review date |

Every lifecycle stage and major stakeholder has an owner. Device-specific IoT, gadgets, investment products, domain inventory/WHOIS operations, pool products, general tools, and automation-product documentation are intentionally handled by the related subdomain audits rather than expanded here.

## Topical map

| Topic ID | Parent topic | Reader outcome | Required subtopics/questions | Evidence/formats | Boundary | Article target |
|---|---|---|---|---|---|---:|
| CDV-01 | Software discovery and requirements | Decide whether to build/change software and create testable requirements | Problem framing; stakeholders; user research; current process; user journeys; functional and quality requirements; constraints; MVP; backlog; acceptance; assumptions; traceability | Interview guide, journey map, requirements template, decision table | Does not choose architecture or estimate price; CDV-03 owns architecture and CDV-17 owns estimation/procurement | 6 |
| CDV-02 | Product, UX, and interface design | Turn evidence about users and tasks into a testable product experience | Research plan; information architecture; flows; wireframes; prototypes; usability tests; design systems; content; responsive states; design handoff | Research protocol, flow diagram, annotated prototype, usability report | Accessibility conformance is owned by CDV-13; `/ui-ux` owns substantiated commercial enquiries | 6 |
| CDV-03 | Software architecture and technology decisions | Select a proportionate structure and document trade-offs before implementation | Quality attributes; constraints; ADRs; monolith/modular/serverless; framework/runtime; build/buy; resilience; capacity; cost; operability; review | ADR template, context/container diagram, trade-off matrix, architecture review | Does not prescribe one stack or represent subdomain product architecture; system implementation is owned by CDV-04–CDV-08 | 6 |
| CDV-04 | Website and web-application engineering | Understand web delivery options and specify front-end, back-end, CMS, forms, identity, and transactional behavior | Static/dynamic/rendering choices; HTML/CSS/JS; server logic; CMS/custom; forms; sessions; workflows; ecommerce; browser/device support; progressive enhancement | Request-flow diagram, route/content model, comparison matrix, test checklist | `/web-development`, `/website-wordpress`, and `/website/*` own commercial intent; apps and APIs are CDV-05 and CDV-06 | 6 |
| CDV-05 | Mobile and installable application engineering | Choose an app approach and plan offline, device, release, and lifecycle constraints | Responsive web/PWA/native; native/cross-platform; local/offline data; synchronization; permissions; app stores; telemetry; updates; accessibility; maintenance | Decision matrix, state/sync diagram, release checklist, platform primary docs | Current repository supports UI/UX and general mobile-development claims, not a proven app portfolio; `/ui-ux` owns design enquiries and capability claims require proof | 6 |
| CDV-06 | API design and lifecycle | Define an interface that consumers can understand, secure, test, version, and operate | Contract-first design; HTTP/resource semantics; OpenAPI; authentication/authorization; errors; pagination; idempotency; versioning; deprecation; documentation; testing | OpenAPI document, sequence diagram, error catalog, consumer test | Does not document a related subdomain API or promise Codev API delivery without evidence; CDV-08 owns cross-system integration | 6 |
| CDV-07 | Data architecture, quality, privacy lifecycle, and recovery | Design data around meaning, integrity, lifecycle, access, migration, backup, and deletion | Entities/relationships; SQL/NoSQL/object storage; constraints; data quality; ownership; migration; lineage; retention; backup/restore; deletion; analytics boundaries | Data model, dictionary, migration/reconciliation plan, restore drill | Legal privacy obligations are owned by CDV-09; domain inventory/product data belongs to `domain.codev.id` | 6 |
| CDV-08 | Third-party integration engineering | Evaluate and operate dependencies across identity, payments, messaging, webhooks, and external services | Integration discovery; contracts; OAuth; payments; email/messaging; webhooks/queues; retries; rate limits; timeouts; idempotency; reconciliation; fallback; acceptance | Sequence diagram, dependency register, failure matrix, sandbox test | Does not become vendor/product documentation; product-specific integrations stay with the relevant service and related subdomain | 6 |
| CDV-09 | Application security and privacy engineering | Put verifiable security and Indonesian privacy controls into the lifecycle | Threat modeling; SSDF; ASVS; identity/session/access; secrets; dependencies/supply chain; encryption decisions; logging privacy; PDP roles/bases/rights/transfers; vulnerability and incident response | Threat model, control matrix, privacy data map, expert/legal review | No “100% secure” promise, penetration certificate, or legal opinion; route copy owns only substantiated commercial terms | 6 |
| CDV-10 | Testing, quality, and release evidence | Build a risk-based verification strategy from unit tests to user acceptance | Quality risks; unit/integration/contract/E2E; exploratory, accessibility, security, performance; test data; environments; traceability; regression; defects; acceptance; release report | Test strategy, test pyramid/portfolio, traceability matrix, defect workflow | Does not duplicate specialist criteria in CDV-09, CDV-13, or CDV-14; CDV-18 owns release governance | 6 |
| CDV-11 | Cloudflare deployment and delivery pipeline | Choose and control a Pages/Workers delivery path with environments, secrets, migrations, rollback, and cache behavior | Pages versus Workers; Functions; repository/direct upload; environment config; CI/CD; preview/staging/production; database migrations; redirects; caching; TLS/DNS interfaces; rollback | Cloudflare primary docs, pipeline diagram, deployment/rollback checklist | `domain.codev.id` owns domain inventory/operations; this topic owns application deployment patterns, not an unsupported production design | 6 |
| CDV-12 | Operations, observability, incidents, capacity, and cost | Define healthy service behavior, detect failure, respond, learn, and control operating cost | SLI/SLO; logs; metrics; traces; privacy; alerts; dashboards; runbooks; on-call; incident roles; post-incident review; capacity; quotas; cost; vendor status | SLO sheet, telemetry schema, runbook, incident timeline, cost model | Does not promise 24/7 support or uptime without contract/evidence; provider-specific current facts require primary docs | 6 |
| CDV-13 | Accessible digital products | Plan, implement, test, procure, and maintain accessibility against WCAG 2.2 | Conformance scope; keyboard/focus; semantics; forms/errors; authentication; color/contrast; media; responsive/reflow; assistive technology; content; design/dev handoff; monitoring | WCAG 2.2 mapping, annotated examples, manual/automated test protocol, disabled-user review | Does not claim certification or legal compliance from automated scans; `/ui-ux` owns substantiated service enquiries | 6 |
| CDV-14 | Web performance and reliability | Set budgets, measure real behavior, isolate bottlenecks, and prevent regressions | User/task metrics; lab/field data; performance budgets; images/fonts; CSS/JS; rendering; caching/CDN; API/backend/database; third parties; load and resilience tests | Measurement plan, waterfall/trace, budget table, before/after evidence | No universal score, “under three seconds,” or ranking guarantee; CDV-12 owns production health and CDV-20 owns real result claims | 6 |
| CDV-15 | Maintenance, modernization, and decommissioning | Keep software supportable, plan upgrades, reduce debt, migrate safely, and retire systems responsibly | Maintenance scope; dependency/runtime upgrades; vulnerabilities; technical debt; refactoring; rewrite/strangler decisions; schema/content migration; compatibility; ownership; archival/deletion; exit | Health audit, dependency inventory, modernization matrix, migration/recovery plan | `/edit-website` and `/redesign-website` own current commercial intent; CDV-18 owns handover/exit governance | 6 |
| CDV-16 | AI-assisted systems and automation delivery | Select useful automations and control data, model, human, evaluation, and operational risk | Use-case and baseline; rules versus ML/generative AI; data rights/privacy; model/provider choice; prompting/RAG; evaluation; hallucination/abuse; human approval; monitoring; fallback; retirement | Use-case scorecard, risk register, evaluation set, human-control workflow | `automate.codev.id` owns its product/workflow catalog; root owns general engineering guidance and must not imply an unsupported AI service | 6 |
| CDV-17 | Estimation, procurement, contracts, and total cost | Commission software with comparable scope, transparent assumptions, ownership, and lifecycle cost | Discovery brief/RFP; work breakdown; uncertainty; fixed/time-material/milestone; vendor evidence; source/IP/data; hosting/accounts; security/accessibility; changes; warranty/support; TCO; lock-in | RFP template, estimate range model, bid matrix, contract checklist, legal review | No invented market price or legal advice; `/kontak` owns actual quotation and terms | 6 |
| CDV-18 | Delivery governance, release, handover, and vendor exit | Clarify roles, control change, accept releases, receive evidence, and retain operational independence | Product/vendor/client roles; RACI; cadence; decisions; risk/issues; change control; demos; release gate; acceptance; credentials/accounts; source; runbooks; training; warranty; exit/transition | RACI, decision log, release checklist, handover index, exit plan | CDV-10 owns test design and CDV-17 owns commercial terms; actual warranty/support claims stay on verified commercial routes | 6 |
| CDV-19 | Content systems, technical discoverability, and migration | Build maintainable content workflows and technically discoverable websites without thin pages or ranking promises | Content model; taxonomy; CMS workflow; metadata; crawl/index/canonical; structured data; sitemaps; redirects; migration; analytics; editorial quality; AI-content governance | Content schema, crawl map, redirect ledger, validation checklist, Search Console evidence | `/konten/*` and `/web-google-adsense` own commercial intent; no city swaps, link schemes, instant ranking, or approval guarantees | 6 |
| CDV-20 | Capability proof, portfolio, case studies, and provider selection | Evaluate Codev or another provider using auditable scope, methods, evidence, and outcomes | Legal/team identity; supported services; skills/process; subcontractors; portfolio provenance; consent; challenge/scope; decisions; verification; baselines/results; testimonials; failures/lessons; references | Evidence hierarchy, case template, consent record, metric sheet, buyer audit | `/portfolio` and service routes own substantiated conversion; no invented client relationship, result, testimonial, or certification | 6 |

Total planned articles: **120**.

## Related-domain opportunities

| Related property | Useful root-domain viewpoint | Boundary for the later subdomain plan |
|---|---|---|
| `domain.codev.id` | Requirements, architecture, data, security, integration, deployment, operations, procurement, and case methods for a domain-management system | Owns its product features, domain inventory, registrar/DNS workflow, user help, operations, and product conversion |
| `automate.codev.id` | General automation discovery, human controls, integration reliability, evaluation, and operating risk | Owns its actual workflows, connectors, templates, pricing, documentation, and cases after audit |
| `gadget.codev.id` | General software/app/API/accessibility lifecycle for device-facing products | Owns gadget/device catalog, comparisons, specifications, and product workflows after audit |
| `invest.codev.id` | Secure and explainable software delivery for financially sensitive products | Owns investment product intent; financial claims require its own legal/evidence gate |
| `iot.codev.id` | General IoT software architecture, device/cloud boundaries, security, telemetry, and operations | Owns device/protocol/product-specific IoT content and commercial claims after audit |
| `pools.codev.id` | Generic service architecture, data, APIs, observability, and reliability | Owns the meaning, product model, documentation, and commercial scope of “pools” after audit |
| `tools.codev.id` | How to evaluate, secure, test, and maintain developer/user tools | Owns individual tools, calculators, utilities, help, and product intent |
| `whois.codev.id` | API, caching, rate-limit, privacy, and reliability concepts | Owns WHOIS/RDAP lookup features, data interpretation, limitations, and product help |

Cross-domain overlap is allowed. These notes prevent the root site from swallowing subproducts; they do not reserve generic software knowledge away from `codev.id`.

## Consolidation plan

| Existing URL/pattern | Observed role/problem | Decision | Destination/owner | Verification needed |
|---|---|---|---|---|
| `/` | Broad website-company landing page with unsupported speed, ranking, security, delivery-time, and nationwide claims | expand | Company/service hub plus CDV-01 and CDV-20 | Legal/team identity, supported services, delivery process, real response/service area, and evidence for every absolute |
| `/web-development` | Main development service/education page; mixes front-end, back-end, full stack, mobile, DevOps, and maintenance | keep | Commercial hub; CDV-03–CDV-06 and CDV-15 own neutral education | Actual technologies, staff, project evidence, scope limits, support, and lead attribution |
| `/ui-ux` | UI/UX service page for web/mobile with research, prototype, accessibility, and testing claims | keep | Commercial route; CDV-02 and CDV-13 own education | Research artifacts, accessibility process, real prototypes/tests, reviewer competence, client consent |
| `/website-wordpress` | WordPress commercial route with absolute security, speed, SEO, and feature claims | expand | Existing route plus CDV-04 and CDV-15 | Supported versions/plugins, update/security/backup process, performance evidence, warranty limits |
| `/website/{28-industry-types}` | Industry-swapped service pages overlap broad “website” intent and often lack industry-specific workflows/evidence | manual review | Consolidate under validated solution archetypes in CDV-04; retain a route only when requirements and proof differ | Search Console/backlinks/leads, unique workflow/compliance needs, portfolio evidence, one-query/outline comparison |
| `/website/custom` | Custom website route but copy still says WordPress and template/custom in the same intent | expand | Custom commercial route; CDV-03/CDV-04 own education | Define “custom,” architecture choices, exclusions, change process, source ownership, acceptance evidence |
| `/edit-website` | Repair/maintenance commercial intent | keep | Existing route plus CDV-15 | Supported stacks, diagnostic process, access/security, backups, acceptance, warranty |
| `/redesign-website` | Design refresh, CMS, and custom development mixed together | expand | Existing route plus CDV-02/CDV-15 | Separate cosmetic redesign, UX change, replatform, and modernization; verify current price/package claims |
| `/web-google-adsense` | Website build plus monetization advice and implied approval/traffic/income outcomes | manual review | CDV-19 education; commercial route only for substantiated build scope | Current Google primary policy, no approval/income promise, portfolio/results, risky backlink/listing advice |
| `/konten` and `/konten/*` | Content-writing service cluster with originality, SEO/ranking, availability, and price claims | keep | Commercial cluster; CDV-19 owns content systems and quality education | Author/editor process, originality method, AI disclosure, plagiarism limits, expertise, current prices, result evidence |
| `/tema` and `/demo` | Theme/demo selection routes | keep | CDV-02/CDV-04 support; commercial route owns actual themes | Live demos, licenses, update/support terms, accessibility/performance/security checks |
| `/portfolio` | Named project gallery without scope, consent, methods, acceptance, or measured outcomes | expand | CDV-20 case-evidence hub | Client/project relationship, contribution, dates, permission, screenshots, technical decisions, result baselines |
| `/kontak` | Transactional enquiry route | keep | Existing commercial route | Form delivery, consent/privacy notice, retention, response ownership, spam protection, verified contacts |
| `/berita` | Listing page with only location posts | expand | Editorial directory for CDV-01–CDV-20 | New maintainable content source, navigation, pagination/canonicals, author/reviewer metadata |
| `/berita/page/{2,3,4}` | Pagination archives included only by complete sitemap | noindex | `/berita` directory | Live canonical/pagination behavior and whether discovery still needs crawlable links |
| `/category/{34-provinces}` | Geography archives exist only to group location-template pages | manual review | No replacement topical hub; CDV-19 governs taxonomy | Search equity, internal links, live response; if no user value, remove from sitemap and noindex before later removal |
| `/pembuatan-website-<place>` and `/pembuatan-website-<place>.html` (489 normalized locations) | Doorway-like city swaps dominate the site; two URL forms occur across manifests | manual review | One validated national service route, not 489 replacement articles | Per-URL Search Console, backlinks, leads, unique evidence, canonical/live status; batch decisions only after sampling and rollback plan |
| `post-sitemap*.xml` versus `sitemap-complete.xml` | `.html` versus extensionless variants for the same normalized post routes | canonicalize | One canonical URL form and one public sitemap index | Fetch both forms, redirects/canonicals, internal links, backlinks, GSC-selected canonical, deployment constraints |
| `/cdn-cgi/l/email-protection/` | Cloudflare utility appears in README-derived crawl inventory, not editorial content | noindex | none | Live response and sitemap inclusion; do not expose as an article |
| `wp-content/uploads/wp-file-manager-pro/fm_backup/index.html` | Empty plugin-backup artifact outside sitemaps and a potential trust/security concern | remove | none | Confirm it is not required, contains no sensitive backup, and remove from deployed output safely |

No redirect, deletion, or noindex action is authorized by this planning commit. Preserve URL history until deployed responses, Search Console, backlinks, qualified leads, and rollback options are checked.

## Internal-link architecture

- `/berita` becomes the editorial directory and links to the 20 hubs; every article links upward to its topic hub and each hub links to all six children.
- Decision path: CDV-01 discovery → CDV-02 experience → CDV-03 architecture → CDV-04/CDV-05/CDV-06 implementation surface → CDV-07/CDV-08 dependencies → CDV-09/CDV-10 assurance → CDV-11 release → CDV-12 operation → CDV-15 maintenance.
- Procurement path: CDV-17 brief/estimate/vendor checks → CDV-18 governance/handover → CDV-20 evidence/case verification. Commercial routes receive links only where the page's actual capability is proved.
- Accessibility, performance, security, privacy, and operations are cross-cutting gates, not end-of-project add-ons. Relevant implementation articles link to CDV-09, CDV-12, CDV-13, and CDV-14 at the exact decision point.
- API and integration articles link laterally by failure mode: CDV-06 contract/idempotency/versioning → CDV-08 retry/reconciliation/fallback → CDV-10 contract/integration tests → CDV-12 telemetry/incident response.
- Maintenance and migration pages link to CDV-07 for data reconciliation, CDV-11 for safe deployment/rollback, CDV-19 for URL/content migration, and CDV-18 for handover/exit.
- AI/automation pages link to CDV-07 data, CDV-09 security/privacy, CDV-10 evaluation/testing, CDV-12 monitoring, and CDV-18 human responsibility. They link to `automate.codev.id` only after that product's role is audited.
- Related IDs in `ARTICLE_CATALOG.md` define useful per-page lateral paths; no generic related-link block is copied to every page and no planned article is orphaned.

## Evidence and editorial standards

- Label content as repository observation, owner statement, stable explanation, architecture option, provider feature, legal requirement, standard, test result, measured project result, or editorial recommendation.
- Recheck primary sources on publication day. Pin standard/provider/spec versions in the article and record the review date; do not describe current Cloudflare, OpenAPI, browser, framework, model, pricing, quota, or app-store behavior from memory.
- Security pages use NIST SSDF, versioned OWASP ASVS/control sources, applicable provider documentation, and qualified review. Never promise “100% secure,” confuse a scan with assurance, expose secrets, or publish exploit-ready client detail.
- Privacy pages map purposes, personal-data categories, roles, lawful grounds, notices, retention, rights, vendors, transfers, security, incidents, and deletion. Indonesian legal interpretation requires current qualified review; do not infer consent as the only possible basis.
- Accessibility pages map claims to WCAG 2.2 success criteria and include manual keyboard, focus, semantics, form/error, zoom/reflow, media, and assistive-technology checks. Automated tools are supporting evidence, not certification.
- Performance pages state environment, device/network profile, dataset, date, sample size, percentile, tool/version, cache state, and before/after method. No fixed score or load time is guaranteed across users.
- Architecture and estimation pages show assumptions, alternatives, excluded scope, failure modes, operating burden, migration/exit path, and uncertainty. A calculator gives a range and inputs, never a fake precise quote.
- API pages prefer an explicit, versioned contract and test examples. Do not publish credentials, private schemas, or product APIs without authorization.
- AI/automation pages establish a baseline and bounded evaluation, document data rights and provider handling, test failure/abuse cases, assign human approval and appeal, monitor drift/cost, and provide disable/rollback paths. Never fabricate accuracy.
- Cases require written permission, verified role/scope, dated baseline, decision record, implementation evidence, acceptance method, measured outcome, caveats, and current status. A portfolio logo or live domain alone does not prove delivery or results.
- Commercial copy states only current, contractually supportable facts about team, technology, time, price, geography, availability, warranty, hosting, security, ranking, traffic, revenue, and support. Remove superlatives unless a named comparison method proves them.
- Content and SEO pages follow people-first editorial quality, disclose material AI use, cite sources, avoid location swaps and link schemes, and distinguish indexation from ranking and ranking from qualified leads/revenue.

## First bounded publication cluster

Publish these 12 assets only after a maintainable content implementation and named reviewers are assigned:

1. CDV-01-A01 — problem-framing brief.
2. CDV-01-A04 — functional versus quality requirements.
3. CDV-03-A01 — architecture decision records.
4. CDV-04-A01 — static, dynamic, and rendered web options.
5. CDV-09-A02 — secure-development lifecycle using SSDF and ASVS.
6. CDV-10-A01 — risk-based test strategy.
7. CDV-11-A01 — Cloudflare Pages versus Workers decision.
8. CDV-13-A01 — WCAG 2.2 conformance plan.
9. CDV-14-A01 — performance budget.
10. CDV-17-A01 — software project brief/RFP.
11. CDV-18-A05 — handover evidence index.
12. CDV-20-A01 — capability-evidence audit.

This cluster is coherent because it improves the current buyer path from problem to requirements, architecture, implementation choice, assurance, deployment, inclusive/performance quality, procurement, handover, and provider proof. It establishes standards and evidence before expanding into specialized system pages.

Monitor discovery and indexation, selected canonicals, impressions grouped by distinct intent, useful hub-to-spoke navigation, template/checklist completion, accessibility/performance task outcomes, qualified enquiries containing usable scope, enquiry-to-response and response-to-conversion rates where available, and query/page pairs showing cannibalization. Ranking alone is not success.

## Definition of done

- All 20 hubs and 120 briefs retain unique titles, slugs, primary intent, coverage promise, explicit owner/exclusion, valid related IDs, and no same-domain collision.
- The homepage and commercial routes prove or remove claims about capability, clients, speed, security, SEO, geography, availability, price, warranty, hosting, and results.
- One maintainable source and deployment process generates articles, metadata, canonicals, structured data, sitemaps, navigation, previews, and rollback.
- The 489 normalized location pages, 34 geography archives, 3 pagination archives, and dual URL forms receive evidence-led decisions without destroying useful history.
- One canonical sitemap contains only intended canonical URLs; utility/plugin artifacts are excluded.
- Every article links to its hub and useful next steps; implementation pages connect security, privacy, accessibility, performance, testing, deployment, operations, and maintenance where relevant.
- Current technical/legal claims cite primary sources and pass the defined expert/legal review gate.
- Related subdomains keep product-specific features, help, pricing, documentation, and cases; root `codev.id` remains the company/software-development knowledge layer.
- The first 12-asset cluster is implemented, reviewed, published, and measured before another wave is authorized.
- The skill validator reports zero errors, coverage counts match, proposed slugs do not collide with existing routes, and Git contains only the two intended documentation files.
