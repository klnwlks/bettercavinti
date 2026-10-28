# Cavinti civic portal: template and implementation plan

**Status:** implementation plan plus first cited Cavinti data snapshot  
**Research snapshot:** 2026-10-21  
**Target:** a reusable BetterLGU-contribution template, populated first for Cavinti, Laguna  
**Not in scope:** visual design, styling, animations, or a redesign specification

## 1. Goal and non-negotiable behavior

Build a clean, static-first civic information portal that covers the public-facing goals documented by BetterLGU and the BetterLB template, without copying BetterLB's implementation or reproducing empty/broken widgets as if they were data. Cavinti is the first configured municipality; a later municipality should be addable through configuration and a validated data pack, not a second site fork or a multi-tenant service.

Public information must be traceable to its publisher and source record. Preserve source wording, publication dates, retrieval dates, original links, units, geography, and coverage limits. A search result is a lead, not proof. A blocked page is a coverage gap, not a zero. Duplicate legislation remains separate if the source publishes separate rows. Conflicts remain visible until a primary record resolves them.

The first release is a static website and downloadable JSON/CSV. It has no private admin console, runtime API, authentication system, community comment feed, or database. Schemas should be API-ready, but serving an API is out of scope. Source refresh is a deliberate manual command; a successful refresh of a known structured source may publish its cited parsed records automatically after completeness and schema checks. Extraction status and conflicts remain in the public data. Do not convert an unverified extraction into a verified fact.

## 2. Current repository baseline

The repository is a React 19, Vite 8, TypeScript 6 starter. `src/App.tsx` is placeholder content and an entrance animation; `package.json` has React, Vite, TypeScript, Tailwind's Vite integration, GSAP, Oxlint, and Playwright. There is no portal data model or i18n system yet. Replace the placeholder as implementation begins; remove GSAP if no required product behavior uses it. Keep the present stack unless implementation proves a dependency necessary; avoid adding a CMS or server for static, cited content.

## 3. Functional parity with the public template goals

Use BetterLGU's directory and contribution guide as the contribution contract, and BetterLB's README as the documented capability inventory. Parity is behavioral, not a copy of layout, stack, branding, APIs, or empty states.

### Context references

- [BetterLGU directory](https://lgu.bettergov.ph/) — community directory and contribution destination.
- [BetterLGU guide](https://lgu.bettergov.ph/GUIDE) — portal goals and contribution guidance.
- [BetterLB repository](https://github.com/BetterLosBanos/betterlb) — code and project documentation.
- [BetterLB README](https://github.com/BetterLosBanos/betterlb/blob/main/README.md) — documented capability inventory used for parity.
- [BetterLB live site](https://betterlb.org/) — public portal reference; live behavior is not treated as proof of documented or complete functionality.


| Public capability | Cavinti implementation | Current data state / honest first-release behavior |
|---|---|---|
| Municipal information and contact routes | Current municipality facts, offices, contacts, barangays, official links, and a source trail | Municipal page and official emergency contacts are captured; hotlines have not been live-validated and the barangay roster conflicts across sources. |
| Citizens' Charter and service help | Searchable service directory and service detail records: eligibility, requirements, steps, fees, timeframe, office/contact, forms, source, and effective date | The official Charter page rendered headings but no service rows. Show the coverage gap and official link; do not invent services or generic steps. |
| Government, officials, committees, and barangays | Executive and legislative directories, individual sourced profiles, committees, terms, office directory, barangay pages | 12 council profile cards and one executive profile are recorded. Terms and some roles are not established. The current barangay roster is unresolved. |
| OpenLGU: ordinances, resolutions, executive orders | Search/filter by type, number, year, author, topic, date, and source; direct attachment links; document detail with original title and metadata | Full captured metadata feeds: 406 ordinances and 4,492 resolutions, with public PDF links. Attachment contents are not fully reviewed. Executive-order and other-document coverage remains unaudited. |
| Municipal finance and income statistics | Budgets, revenues, expenditures, audits, fiscal-year comparisons, and downloads where primary records exist | FDP/BLGF/COA access or local extraction was incomplete. Show source status and available documents, never zero-value cards that imply no spending or records. |
| Procurement transparency and analytics | PhilGEPS notices/awards, contracts, supplier/sector/date filters, source-backed totals, data export, and analytics only for comparable complete rows | No complete Cavinti procurement query or totals were captured. One disputed date lead is quarantined from completed-award claims. |
| Infrastructure/project tracking | Project, agency, budget, location, contract, schedule, progress, source documents, and a structured citizen verification report (project ID, observation date/location, evidence URL or permitted attachment); reports enter a reviewed GitHub issue queue and never overwrite published progress directly | No complete local project register was verified. Do not populate a project count or progress rate from an empty feed. |
| Statistics: demographics, competitiveness, municipal income | Pages for population and published demographic series; DTI CMCI ranking/indicators; municipal income/fiscal data; source and period attached to each value | PSA 2024 population is a search-indexed primary lead with direct access blocked; 2015/2020 values and barangay table are secondary. DTI's Cavinti page returned 403; no rank is published from the partial search snippet. |
| Open data | Downloadable validated JSON and CSV with schemas, source manifest, coverage, capture date, and data hash | Cavinti municipality, barangay evidence, officials, contacts, service coverage, transparency evidence, sector coverage, source register, and complete captured legislative metadata are included. |
| Search | Local search over portal content, public records, and source metadata; results link to their evidence | Generate a compact local search index during data publication; no hosted search dependency is required. |
| Map and geography | Municipality/barangay mapping, source-specific boundary status, locations only when sourced | The official About page provides coordinate bounds and neighbors, but no official polygon was verified. Do not draw or label secondary geometry as an official boundary. |
| Weather and advisories | Current PAGASA/LGU advisories and dated historical context, with retrieval time and safety caveat | Official source routes are identified, but no Cavinti station series or current advisory feed was verified. Never show a fabricated forecast or warning. |
| Accessibility and localization | Keyboard and assistive-technology operability; English and Filipino content catalogs | Build accessibility requirements into implementation and tests; localized interface strings must not change source records. |
| Public corrections / civic contribution | Structured GitHub issue forms linked from records; data corrections include record ID, field, proposed value, and evidence, while project observations include project ID, observation time/location, and supporting evidence | No anonymous free-form comments or unmoderated edits. Submissions are review leads; source update and merge remain repository-reviewed. |

Features with no source data still exist as discoverable routes with an explicit source/coverage state. They must not impersonate a complete dashboard. Hide derived metrics unless later project scope authorizes them; the current decision is to publish source-published figures only.

## 4. Target project structure

Keep municipality-specific content out of UI components. A single site build consumes one selected LGU configuration and its versioned data pack.

```text
src/
  app/
    App.tsx
    router.tsx
    route-manifest.ts
  config/
    lgu.ts                    # Cavinti identity, locale, data pack, external links
    navigation.ts             # route labels/availability; no data facts
  domain/
    schemas/                  # runtime validators and TypeScript types
    evidence.ts
    status.ts
  data/
    loaders.ts                # generated static JSON loading and cache boundaries
    search-index.ts           # local index reader/filtering
  i18n/
    locales.ts
    catalogs/en.json
    catalogs/fil.json
    localize-content.ts
    format.ts
  components/
    SourceCitation.tsx
    DataStatus.tsx
    DataFreshness.tsx
    ExternalDocumentLink.tsx
    CorrectionLink.tsx
  features/
    home/
    services/
    government/               # officials, offices, terms, committees, barangays
    legislation/              # ordinances, resolutions, executive orders
    transparency/              # finance, procurement, infrastructure
    statistics/                # population, competitiveness, municipal income
    open-data/
    maps/
    places-and-tourism/
    advisories/
    search/
    sources/
    corrections/
    accessibility/
    privacy/
    not-found/
data/
  cavinti/
    source-register.json
    dataset-manifest.json
    municipality.json
    barangays.json
    officials.json
    emergency-contacts.json
    services.json
    sector-inventory.json
    transparency.json
    legislation/
      ordinances.json
      ordinances.csv
      resolutions.json
      resolutions.csv
    schemas/                    # published JSON Schema documents
scripts/
  data/
    refresh.ts                  # explicit, on-demand source refresh
    sources/                    # one small adapter per source family
    normalize.ts
    validate.ts
    build-public.ts             # validate, create public downloads and search index
    report-coverage.ts
public/
  data/                         # generated output; do not hand-edit
.github/
  ISSUE_TEMPLATE/
    data-correction.yml
    missing-record.yml
    project-observation.yml
docs/
  CAVINTI_TEMPLATE_PLAN.md
  sources-and-data-policy.md  # split out only when that policy becomes operationally detailed
```

`data/cavinti/` is canonical and reviewed. `public/data/` is generated before `dev` and `build`, ignored by version control, and must never become a second editable copy. Each generated download has a manifest referencing its schema, source IDs, capture time, row count, and source-response hash. The build fails if required provenance is missing or a structured feed is incomplete; it does not silently publish half a page of results.

For the reusable template, `src/config/lgu.ts` selects one `MunicipalityConfig`; data adapters and generated data use a municipality code/slug, never UI hard-coding of “Cavinti.” A new LGU is a new validated data pack and configuration, with the same schemas. Do not create a multitenant runtime, separate per-LGU application fork, or a generic CMS.

## 5. Data architecture and schemas

All canonical records use versioned schemas. TypeScript types are inferred from or checked against runtime JSON Schema validators, so exported downloads and site code share the same contract. Store immutable source evidence separately from display-ready normalized values.

### 5.1 Source and evidence

```ts
type SourceFormat = 'html' | 'json' | 'csv' | 'pdf' | 'map' | 'social' | 'markdown' | 'image' | 'other';
interface Source {
  sourceId: string;
  publisher: string;
  sourceTier: 'primary-lgu' | 'primary-lgu-social' | 'primary-government' | 'primary-government-platform'
    | 'secondary-reference' | 'secondary-aggregator' | 'regional-directory-lead'
    | 'community-directory' | 'community-guidance' | 'project-documentation';
  canonicalUrl: string;
  formats: SourceFormat[];
  retrievalStatus: string;
  termsUrl?: string;
  reuseLicense?: string;       // null/unknown is not permission
  lastCheckedAt: string;
  sourcePublishedAt?: string;
  sourceContentHash?: string;
  coverageNote: string;
}
type VerificationStatus = 'primary-direct' | 'primary-index-only' | 'secondary-only'
  | 'machine-extracted-unreviewed' | 'conflicted' | 'not-reviewed';
type AvailabilityStatus = 'available' | 'access-blocked' | 'not-published'
  | 'empty-in-capture' | 'not-found-in-checked-source' | 'unknown';
type FreshnessStatus = 'current-at-capture' | 'stale' | 'not-checked' | 'not-applicable';
interface EvidenceRef {
  sourceId: string;
  sourceUrl: string;           // direct evidence URL; clickable in every public citation
  sourceRecordKey?: string;
  quotedText?: string;         // bounded excerpt where reuse permits
  sourcePublishedAt?: string;
  retrievedAt: string;
  contentHash?: string;
  verificationStatus: VerificationStatus;
  availabilityStatus: AvailabilityStatus;
  freshnessStatus: FreshnessStatus;
  verificationNote: string;     // preserve source-specific evidence and limits
}
```

Every publishable value references one or more `EvidenceRef`s. Keep source tier and status per claim, not just per page. Preserve original title/name strings and source dates; do not replace a missing value with an empty string that looks authoritative.

### 5.2 Domain records

- **Municipality / geography:** stable PSGC when confirmed, official names, region/province, source-specific locality names, coordinate text or coordinates with datum, boundary file and license, neighbors as published. `canonicalBarangayRoster` remains unset until authoritative reconciliation.
- **Statistic:** indicator ID and source label, numeric/text value, unit, geography code/name, reference period, source, extraction status, and published/derived flag. In the current scope, `derived` values are rejected from publication. `null` plus a status explains unavailable, blocked, not published, or unverified. Numeric zero is allowed only when the cited source explicitly publishes zero.
- **Official:** stable person record ID, exact published name/title, office/committee, role and term only with evidence, source-published biography/profile fields, optional public office contact, last checked date, confidence/status. Do not infer an office from an honorific or a card's position in a page.
- **Office/contact:** office name, function, public contact, channel, location, source, active/freshness check. Phone numbers are time-sensitive and carry last validation; unverified contacts are not labelled current.
- **Service:** exact service name, office, eligibility, requirements, steps, fee/currency, turnaround, schedule, forms, legal basis, source URL, effective/review dates, language and verification status. Do not derive missing steps from another LGU or an enabling law alone.
- **Legislation:** source-scoped record ID/fingerprint, original type, displayed number/status/title/author/committee/date, direct PDF URL, index URL, source row ordinal if no upstream ID, capture date/hash, translation status, content-parsing status. Keep duplicate title/number/PDF rows; flag exact duplicates rather than silently deduplicating.
- **Procurement/project:** procuring entity/implementing office, notice/contract/project IDs, title, amount/unit/currency, supplier/contractor, dates/status, location, progress, and supporting documents. Fields may be asserted only if the source publishes them at the same jurisdiction and time scope. No percentage completion calculated by the site.
- **Advisory/weather:** issuer, exact message, issue/valid times, target area, source, retrieval time, expiry/staleness, and explicit safety disclaimer. A stale cached notice is not a current warning.
- **Dataset manifest:** schema version, LGU code, source IDs, generated-at/capture date, row count, source hash, coverage period, known gaps/conflicts, rights/attribution, and validation result.

Verification, availability, and freshness are separate dimensions. Production validators map the current research files' descriptive status strings into these stable codes while preserving each status note. `empty-in-capture` means no rows were returned by one checked source, not that no records exist. Numeric zero is valid only when the cited source explicitly publishes zero.

## 6. Cavinti data collection and refresh plan

### 6.1 Capture already completed

The data pack in `data/cavinti/` is part of this deliverable. At snapshot 2026-10-21:

- The municipal public JSON index returned **406 ordinance rows** and **4,492 resolution rows**. Requests covered all returned rows (`recordsTotal = recordsFiltered = rows returned`). Every captured row had a title, displayed date, and PDF URL. Ordinance rows contain 401 distinct displayed number strings and 399 distinct PDF URLs; resolutions contain 4,436 distinct displayed number strings and 4,456 distinct PDF URLs. Preserve all rows and duplicates. The index date ranges are 1992-07-24–2026-09-28 for ordinances and 1992-07-17–2026-10-12 for resolutions. Source-response SHA-256 hashes and row-level source links are in the JSON. The accompanying CSVs are convenience exports; JSON is canonical.
- Ordinance/resolution metadata is extracted from official index cells. The PDFs are linked but not all their text has been read or validated; title text is not a validated amount, enacted clause, budget line, or complete legal summary.
- The municipality dataset includes the 2024 population value 24,740 as a **PSA search-index lead with direct retrieval blocked**, and 2020/2015/1903 values plus a published 2.12% annualized 2015–2020 rate from secondary PhilAtlas. Do not promote the PSA lead to directly verified or calculate a new trend.
- The secondary barangay population table contains 19 source rows for 2015 and 2020. Totals displayed in that source profile are preserved separately. No new growth, share, or density metrics were calculated.
- The municipal About page shows 17 barangay headings; an official ordinance title says 19; PhilAtlas lists 19 including Labayo and Layasin and calls Mahipon what the municipal page calls Mahipon-Lumot. No list is canonical until checked against current PSA PSGC.
- The official directories yield 12 legislative cards and one executive-office entry. The indexed 2026 resolution title identifies the mayor by office. The cards' committee labels and authored/co-authored counts are recorded as displayed; the full roles/terms are not complete.
- The official About page publishes six emergency-service entries/numbers; publication date and current operation were not established. `emergency-contacts.json` explicitly says not live-validated.
- The official Citizens' Charter page showed column headings but no service rows. `services.json` records that as an empty observed table plus a gap—not an empty service catalogue.
- Education, environment, weather, tourism, and finance/procurement findings are in `sector-inventory.json` and `transparency.json`. Only the two high-school entries are retained as secondary-directory leads. Elementary school IDs discovered from a search result are marked research-only. CBMS transfer is confirmed by an official PSA event page, but no public aggregate data table was retrieved.

### 6.2 Source order and collection rules

1. **Primary LGU sources first:** official Cavinti portal, Citizens' Charter, executive/legislative directories, municipal ordinances/resolutions/other documents, dated official public social advisories. Social collection is limited to public official posts; no comments, reactions, or private/personal profiles.
2. **Primary national/regional sources:** PSA/PSGC and OpenSTAT for population/administrative units; DTI CMCI for competitiveness; BLGF, DILG FDP, COA and DBM for public finance; PhilGEPS for procurement; DPWH for agency projects; DepEd for school lists; DOH for facility records; PAGASA/PHIVOLCS/HazardHunterPH/NAMRIA for weather, hazard, and map data; PSA/DA/ATI, DOT/TIEZA, LLDA/NAPOCOR for agriculture, tourism, and environment.
3. **Secondary sources only as leads or explicit secondary evidence:** PhilAtlas and regional directories may aid discovery but never silently replace official municipal/PSA records. Keep source strings and primary cross-check status.
4. For finite public archives, retrieve all accessible historical rows up to the capture date; record earliest/latest dates, pagination completeness, and known archive truncation. For official social sources, collect dated public posts across the accessible archive only; never ingest comments, reactions, or user posts.
5. Respect source terms, privacy, rate limits, and robots/website access controls. Do not bypass blocked pages or access controls. No blanket mirror of source PDFs, social media, or private establishment/CBMS records. Archive a permitted snapshot only where useful and licensed; otherwise keep URL, retrieved date, response hash, parsed rows, and coverage metadata.

### 6.3 Refresh pipeline

Run an explicit command such as `npm run data:refresh -- --lgu cavinti`:

1. Fetch each registered source through its source-specific adapter. Record status, final URL, content type, HTTP status, source date, retrieved time, and hash. Do not use one broad scraper that assumes every page has the same HTML shape.
2. For paginated tables, follow the source's pagination contract until all reported rows have been fetched. Fail the refresh if `recordsTotal`, `recordsFiltered`, and downloaded rows disagree; log changed totals, empty pages, malformed cells, missing links, and duplicate keys. Preserve the source row and direct attachment URL.
3. Parse structured JSON/HTML/CSV into source-shaped staging rows. For PDFs/OCR, preserve the source text/page reference and mark extracted claims unreviewed. A valid source link does not validate all text within its attachment.
4. Normalize only documented fields; retain original source string next to any normalized date, amount, place name, or translated text. Never normalize away number suffixes, spelling differences, currencies, units, period labels, or source duplicates.
5. Validate schema, required evidence, date/unit/geography, source URL, accepted licenses, duplicate reports, roster consistency, and feed completeness. A malformed or incomplete refresh must not replace the last complete capture.
6. Automatically publish cited parsed records from known structured sources when these checks pass; keep their extraction/verification status visible. Conflicts are published as competing claims, not resolved by code. PDF-derived values with no reliable extraction remain leads until evidence quality is adequate.
7. Generate the public JSON/CSV files, per-dataset manifest, download index, and search index. Include `capturedAt`, row count, source URL(s), source-response SHA-256, schema version, coverage dates, and known gaps. Site pages read only generated validated files.
8. Manual, on-demand refresh only. There is no scheduled job in the initial scope. If a source is blocked or its parser fails, keep the last complete data snapshot with a visible stale/last-checked state; do not substitute a fabricated fallback or imply that it is current.

### 6.4 Priority research to close the current data gaps

1. Obtain an official, dated PSA PSGC extract for Cavinti and its barangays; reconcile 17/19 and Mahipon-Lumot/Mahipon/Labayo/Layasin. Retrieve the official 2020 and 2024 census tables and replace secondary values only after row-level cross-check. Do not bypass PSA access requirements.
2. Re-run the official Cavinti service catalogue/Charter download path with the municipal office's publicly released documents; collect every service record and source PDF. No service detail ships from inference.
3. Parse the legislative PDF collection by document type, with page-level citations and document checksums. Separate index metadata from extracted full text; verify document number/date/status and preserve a `textExtractionStatus`. Start with current-year budgets, audit-related documents, and records directly linked from the portal.
4. Obtain local fiscal files from DILG FDP, BLGF raw downloads, and COA reports. Verify jurisdiction and fiscal year for each row; keep municipal and barangay finance separate. Search-engine snippets and secondary figures remain leads only.
5. Capture a complete, date-filtered PhilGEPS query using Cavinti's procuring entity identifiers; validate award/contract dates and amounts against the notice and award documents. Obtain official project records from the LGU/DPWH before adding project counts or progress.
6. Retrieve the complete DTI CMCI Cavinti profile and downloadable annual series. The current official profile URL is registered, but direct reader access returned 403 and the search snippet was incomplete; do not publish its rank/score yet.
7. Obtain a current DepEd masterlist, DOH facility listing, publishable CBMS aggregates, official tourism/cultural inventory and public visitor series, and official hazard/map data. Keep sensitive household/health information out of the portal.
8. Confirm emergency numbers directly with the municipal offices and store the confirmation date. Verify map geometry and its license before plotting official boundaries.

## 7. Site information architecture and page inventory

Use locale-prefixed routes so a shared URL preserves language. Every data page has a stable source/coverage route.

- `/{locale}` — municipal landing, current official notices/links, source-backed featured facts, coverage status.
- `/{locale}/services` — searchable Citizens' Charter service index.
- `/{locale}/services/{serviceId}` — one service's eligibility, steps, requirements, fees, office, source documents, review/effective dates.
- `/{locale}/government` — executive and legislative branches, offices, public contacts, terms.
- `/{locale}/government/officials` and `/{locale}/government/officials/{officialId}` — officials index and sourced profiles.
- `/{locale}/government/offices` and `/{locale}/government/offices/{officeId}` — municipal offices and sourced department/contact records.
- `/{locale}/government/committees` — committee names, membership only where explicitly sourced.
- `/{locale}/government/barangays` and `/{locale}/government/barangays/{barangayId}` — source-specific roster evidence, published demographics, geography, offices; keep unresolved names unmerged.
- `/{locale}/open-lgu/legislation` — ordinances, resolutions, executive orders and other documents with filters/search.
- `/{locale}/open-lgu/legislation/{recordId}` — source metadata, original title, official PDF, extracted-text status, source links, correction path.
- `/{locale}/open-lgu/terms` and `/{locale}/open-lgu/terms/{termId}` — legislative terms, officials and committee rosters where documented.
- `/{locale}/transparency/finance` — budget, income/expenditure, audit, FDP/BLGF/COA source records; clearly separate unavailable periods.
- `/{locale}/transparency/procurement` — PhilGEPS notices/awards/contracts and downloadable rows when complete.
- `/{locale}/transparency/projects` — public project inventory with location, agency, funding and progress evidence; include a structured citizen report path that creates a reviewable issue, never a live status mutation.
- `/{locale}/statistics/demographics` — source-published census and population tables.
- `/{locale}/statistics/competitiveness` — municipal context including PSA search-indexed population and income class and PhilAtlas historical income attributed to BLGF, with verification limits; DTI's five pillars and source link. Publish CMCI rankings and annual indicators only after direct retrieval.
- `/{locale}/statistics/municipal-income` — BLGF/COA/municipal published finance series.
- `/{locale}/open-data` — datasets, schemas, CSV/JSON downloads, capture dates, licenses, coverage and hashes.
- `/{locale}/map` — official boundaries/data where licensed; separate and label any nonofficial context layer.
- `/{locale}/places` — tourism, culture, environment, agriculture and public facilities; categories only appear as sourced records.
- `/{locale}/advisories` — dated official weather, disaster and municipal advisories with expiry/staleness.
- `/{locale}/sources` and `/{locale}/sources/{sourceId}` — publisher, source URL, tier, checked date, terms, status, affected records, known gaps.
- `/{locale}/report-data` — correction/missing-record instructions and GitHub issue forms for data corrections and project-verification observations.
- `/{locale}/accessibility`, `/{locale}/privacy`, and `/{locale}/about` — nonvisual implementation policies, data-use limits, project scope, and contact.
- Not-found route — preserve the requested URL and offer search/navigation; do not route an unverified record to a blank document view.

The content hierarchy is municipality → topic/service/office/barangay/document → individual sourced record → source evidence. A route can be linked even if its record set is unavailable, but its page state must say exactly whether the data is uncollected, inaccessible, unpublished, stale, or empty in the inspected source.

## 8. English and Filipino localization

- Ship UI catalogs for `en` and `fil` (`en-PH` and `fil-PH` number/date formatting). Locale is explicit in URLs, and switching locale preserves the current route when a translation exists.
- Keep source-original names, laws, office titles, document titles, and quotations immutable. Add translations as separate fields: `{ original, originalLanguage, translations: { en, fil }, translationStatus, translator/reviewer, reviewedAt }`.
- Human-review all Filipino/English translations of eligibility, fees, legal language, emergency guidance, and service steps. Never silently publish machine translation as official content.
- If a translation is missing, display the original text with a clear language label. Do not make names or legal citations disappear because one locale is incomplete.
- Keep localized UI labels in catalogs, not duplicated in data JSON. Format numeric/currency/date display with locale-aware formatters, but preserve raw source value, units, and period in the record.
- Use real pluralization and date/number locale rules; test direct visits, deep links, source citations, and missing-translation behavior in both locales.

## 9. Licensing, privacy, safety, and corrections

- License project code and project-authored schema/normalization content as **CC0** as selected. For every upstream source, record its own terms and attribution. A public URL is not a license to republish the source's full text, photos, PDFs, or map tiles. Link to official documents instead of mirroring them unless rights explicitly permit mirroring.
- Treat publicly posted official contact details as time-sensitive. Do not publish staff personal numbers unless the source clearly designates them as public office contacts and there is a need. Never ingest patient records, household-level CBMS data, private posts, or user comments.
- Emergency/advisory records include issuer, publication time, last check, validity/expiry, and safety limits. Stale contacts/advisories cannot be described as current or relied upon as emergency instructions.
- Correction forms must request only the record/field, proposed correction, and evidence URL. Do not collect sensitive details. Public issue content is reviewed before data changes; every correction preserves prior source evidence and change history.
- Retain a source-specific attribution and licence field for every generated dataset. Public downloads must not accidentally inherit CC0 if upstream data terms are unknown or restrictive.

## 10. Delivery phases and acceptance criteria

### Phase A — contracts and source catalog

- Implement runtime schemas, status enums, source-register UI/data contracts, locale configuration, and single-LGU config.
- Acceptance: validator rejects a statistic without unit/period/geography/source; source links resolve to a registered URL; `null` and explicit numeric zero are not conflated; translation absence is visible.

### Phase B — trusted content foundation

- Build municipality, source, official, service, barangay, and document routes from generated static data. Add bilingual UI strings and citations.
- Acceptance: current Cavinti evidence appears with exact source status; the Citizens' Charter gap cannot render as “no services”; role/term gaps are not guessed; all official-document links are clickable.

### Phase C — OpenLGU and downloadable data

- Add full legislative index search/filter/detail, attachment links, download files, and source record pages. Add generated JSON/CSV manifests.
- Acceptance: every captured row is represented exactly once per source row, duplicates remain inspectable, pagination totals reconcile, and every row has its source/feed and attachment URL. Search results open the correct source record.

### Phase D — statistics and transparency modules

- Add demographics, CMCI, finance, procurement and project pages, backed by official source adapters and comparable periods only.
- Acceptance: every displayed point links to a primary record or is explicitly labeled secondary/unverified; no aggregate is generated in this scope; incomplete feeds show their coverage state, not zero totals.

### Phase E — map, places, advisories, and public correction flow

- Add licensed geographic layers, tourism/culture, weather/advisory integrations, and GitHub issue forms as source coverage allows.
- Acceptance: map layer provenance/license is attached; advisories show issuer and validity; missing geometry and data are explicit; issue forms point back to a stable record/source.

### Phase F — template readiness

- Confirm that an implementer can configure another LGU by providing a config and schema-valid data pack without editing feature components.
- Acceptance: no Cavinti names/URLs are embedded in generic feature logic; project docs explain source capture, validation, translation review, licensing and contribution; generated data is reproducible from committed source records.

## 11. Verification requirements for implementation

- Validate all checked-in JSON and CSV against versioned schemas; verify source-ID references, direct URLs, dates, row counts, source hashes, and known conflict records.
- Regression scenarios must exercise: duplicate legislation; missing source link; stale contact; inaccessible/empty source; conflicting barangay names; source-published zero versus unknown; missing translation; incomplete paginated response; future-dated procurement status; and unknown official role.
- Smoke-test the built static site and its actual routes, downloads, external source links, locale switch, and correction-form URLs. A passing unit test does not establish that a user can open or verify a source.
- Keep manual refresh output deterministic. A changed source response must produce an understandable diff/manifest update, not a silent rewrite of disputed facts.

## 12. Current deliverables and unresolved evidence

Data files delivered alongside this plan:

- `data/cavinti/dataset-manifest.json` — dataset inventory, capture dates, row counts, source IDs, hashes, licenses, and limitations.
- `data/cavinti/source-register.json` — primary/secondary source catalog and known gaps.
- `data/cavinti/municipality.json` — sourced municipal identity, population claims, coordinates/neighbors, and boundary limitation.
- `data/cavinti/barangays.json` — official and secondary roster claims, complete secondary 2015/2020 table, and unresolved naming conflicts.
- `data/cavinti/officials.json` — executive and legislative directory evidence, exact displayed committees/counts, and explicit role/term gaps.
- `data/cavinti/emergency-contacts.json` — six official-source contact entries marked freshness-unverified.
- `data/cavinti/services.json` — Citizens' Charter table observation and the missing-service-catalogue status.
- `data/cavinti/sector-inventory.json` and `data/cavinti/transparency.json` — sector-level source coverage, leads, access failures, and non-zero/unknown safeguards.
- `data/cavinti/legislation/ordinances.{json,csv}` and `resolutions.{json,csv}` — all rows returned by the captured public indexes, with row-level source data and hashes.

This snapshot materially improves coverage, especially for the full official legislation index, but it does **not** claim that every fact about Cavinti is available. The direct official PSA population/PSGC tables, full Citizens' Charter services, DTI CMCI data, complete finance/procurement/project rows, current school/health rosters, official GIS boundaries, and several local sector series remain unverified or inaccessible. They are recorded as concrete follow-up work rather than filled with estimates or invented content.

## 13. Implementation and evidence corrections

The implementation uses the supplied snapshot, with 29 additional collection observations in `data/cavinti/research-updates.json`. It includes runtime contracts, transactional publication, manual structured-source refresh, bilingual routes, local search, source/coverage views, downloads, portable route entry pages and reviewed GitHub issue forms. Operational commands and deployment requirements are in `README.md`; evidence, translation and licensing rules are in `docs/sources-and-data-policy.md`.

Inspection during implementation found source metadata details that qualify the original capture summary: committee text is null in 39 ordinance and 52 resolution rows; publication status is null in one ordinance and six resolution rows. Source-published contact/copy-request URLs also occur, so “every row has a PDF URL” must not be read as a guarantee of a direct PDF attachment. The implementation preserves nulls and original URLs, labels non-PDF links, and does not replace them with guessed PDF locations.

The supplemental collection confirmed the Charter endpoint's zero-row response, retrieved selected legislative PDFs with checksums and page references, and recorded blocked or incomplete primary sources. Native PDF text was blank for inspected scanned documents; bounded transcription remains explicitly unreviewed. It did not establish a current authoritative roster, live-validated hotlines, a complete service/fiscal/procurement/project dataset, official licensed geometry or a current advisory feed. The relevant pages retain explicit coverage states.

User-selected deployment: portable static entry pages, with Tailwind and restrained Kapwa styling. Contribution repository: <https://github.com/klnwlks/bettercavinti>. No database, runtime API, scheduled refresh, unmoderated edits or derived civic metrics were added.
