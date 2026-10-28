# Cavinti E-Gov portal: scrapeability and integration report

**Purpose:** record what the public [Cavinti E-Gov portal](https://www.cavinti-egov.com/) exposes, what BetterCavinti already covers, and a safe path for importing missing material. This is an inventory and method report; it does not itself add new canonical records or assets.

## Findings

| Source | Publicly available data | BetterCavinti status | Recommended handling |
| --- | --- | --- | --- |
| [Ordinances](https://www.cavinti-egov.com/ordinances) and [resolutions](https://www.cavinti-egov.com/resolutions) | 406 ordinance rows and 4,492 resolution rows in the captured public feeds; titles, displayed numbers, dates, authors, committees, and document links. | Already captured in `data/cavinti/legislation/`. Attachments are not parsed or validated. | Refresh with the existing legislation adapter; preserve duplicates and source order. Link to official PDFs instead of mirroring them. |
| [Other documents](https://www.cavinti-egov.com/otherDocuments) | 33 public rows, dated June 10–October 16, 2026; title, publication date, public access label, and PDF URL. | Not imported. `data/cavinti/source-register.json` still says the full catalog is unaudited. | Add a metadata-only dataset and link each item to its official file. Do not treat a title or link as verified attachment contents. |
| [News archive](https://www.cavinti-egov.com/) | 21 stories across seven paginated home pages, dated February 21–October 12, 2026. Detail pages expose title, date, category, Filipino text, and image URLs. | No news dataset or route. | Optional: add source-linked titles, dates, excerpts, and article URLs. Keep source text in Filipino unless reviewed translations are provided. Avoid copying article images without checking their individual rights. |
| [Executive directory](https://www.cavinti-egov.com/executive) and [legislative directory](https://www.cavinti-egov.com/legislative) | One executive photo; 12 named legislative profile photos; one Sangguniang Bayan institutional image. Profile cards include source-published names, committee/expertise labels, and authored/co-authored counts. | Names, some profile evidence, committees, and counts are in `data/cavinti/officials.json`; portrait URLs are absent and the officials UI does not render portraits. One profile’s membership status is unresolved. | Add a portrait only to its matching source profile. Preserve uncertainty; do not label the unresolved profile as an elected/current official based on the image or card alone. |
| [About Cavinti](https://www.cavinti-egov.com/aboutUs) | Geographic narrative, coordinate ranges, neighboring municipalities, 17 barangay headings, vision and mission. | Coordinates, neighbors, slogan, and the 17-name source claim are represented. Vision/mission and some narrative detail are missing. | Add source-original text with its source link; the page has no visible publication/update date. Retain the 17-versus-19 barangay conflict; do not infer a canonical roster. |
| [Contact page](https://www.cavinti-egov.com/contact) | Municipal Hall address, public emails and phone number, and six emergency-service listings. | Public contacts are already captured in `data/cavinti/emergency-contacts.json` and surfaced with freshness caveats. | Keep the existing caveat: published numbers are not live-validated. Do not submit the contact form or collect submissions. |
| [Citizens’ Charter](https://www.cavinti-egov.com/citizenscharter) | Table headings for service/document, office/contact, and document; no service rows rendered during inspection. | `data/cavinti/services.json` records zero observed rows, correctly treating this as a coverage gap. | No service requirements, fees, or steps can be scraped from the inspected page. Do not infer that the municipality offers no services. |
| Homepage official channels | Eight links: Cavinti PIO, Tourism, Water, MENRO, MPS, BFP, RHU, and MDRRMO Facebook pages. | The official links are not surfaced as a visitor-facing channel directory. Some are present as research leads; no post bodies were captured. | Link to the official pages only. Do not scrape personal profiles, comments, reactions, or posts blocked by the platform. |

The canonical snapshot manifest was generated on 2026-10-21. The source register and dataset manifests are the authority for capture scope and freshness; a live page check does not silently refresh those records.

## Official portraits

The directory HTML yielded **14 image URLs**: 12 named legislative profile portraits, one Sangguniang Bayan institutional image, and one executive portrait. The site footer on the [contact page](https://www.cavinti-egov.com/contact) states: “All content is in the public domain unless otherwise stated.” No photographer credit or per-image license was visible in the directory markup. This is a source-published site-wide reuse statement, not a photographer attribution; inspect each asset for an individual rights notice or embedded rights metadata before copying it.

| Profile label from source | Image URL |
| --- | --- |
| HON. ARRANTLEE ROMULO ARROYO | [Portrait](https://www.cavinti-egov.com/author_images/686dd7f8ed04cDPZ_3975-Edit-Edit-Edit-Edit%20(1).jpg) |
| HON. MILBERT L. OLIVEROS | [Portrait](https://www.cavinti-egov.com/author_images/686dd7ce69fddAMC_3469-Edit-Edit-Edit-Edit-Edit-Edit%20(2).jpg) |
| HON. DR. JOVENIL T. GORDULA | [Portrait](https://www.cavinti-egov.com/author_images/68635716a3459AMC_3496-Edit-Edit-Edit%20(1).jpg) |
| HON. GLENN HAZE M. CABUHAT | [Portrait](https://www.cavinti-egov.com/author_images/686dd7a4613bdAMC_3424-Edit-Edit-Edit-Edit%20(1).jpg) |
| HON. GODFREY L. LUBUGUIN | [Portrait](https://www.cavinti-egov.com/author_images/686dd790eedb8AMC_3449-Edit-Edit-Edit-Edit%20(1).jpg) |
| HON. HABSHAN DAVE D. BLASTIQUE | [Portrait](https://www.cavinti-egov.com/author_images/686dd778dec35Layer%2021%20copy2.jpg) |
| HON. DR. JOSE MARI L. ESGUERRA | [Portrait](https://www.cavinti-egov.com/author_images/6863578f30391AMC_3503-Edit-Edit-Edit%20(2).jpg) |
| HON. JOSEPH P. BALTAZAR | [Portrait](https://www.cavinti-egov.com/author_images/6863574b29e7aAMC_3489-Edit-Edit-Edit%20(2).jpg) |
| HON. ROMEL J. VILLANUEVA | [Portrait](https://www.cavinti-egov.com/author_images/686479ee075f5AMC_3413-Edit-Edit-Edit-Edit%20(1).jpg) |
| HON. LIMRON O. SANCHEZ | [Portrait](https://www.cavinti-egov.com/author_images/686479501d9a0AMC_3438-Edit-Edit-Edit-Edit%20(1).jpg) |
| HON. MANUEL A. DELA ROCA | [Portrait](https://www.cavinti-egov.com/author_images/686ddbc21aa4dAMC_3397-Edit-Edit-Edit-Edit%20(1).jpg) |
| HON. MELANDRO B. LOPE | [Portrait](https://www.cavinti-egov.com/author_images/6863562f2b4f0AMC_3515-Edit-Edit-Edit%20(1).jpg) |
| NITZSHELL TORRES-DELA TORRE J.D. | [Portrait](https://www.cavinti-egov.com/author_images/686dd80a51e03AMC_3460-Edit-Edit-Edit-Edit%20(1).jpg) |
| SANGGUNIANG BAYAN (institutional card) | [Image](https://www.cavinti-egov.com/author_images/647e9a8f8503cAMC_1897.jpg) |

### Recommended portrait integration

- Store the original image URL, matching official source ID, source page, capture date, and the exact rights note with each portrait record. Do not invent a photographer credit; record it as uncredited when none is published.
- Prefer reviewed local assets over runtime hotlinks. This avoids broken upstream URLs and prevents each visitor’s browser from requesting portraits from the municipal host. Keep asset filenames associated with stable official record IDs, not names inferred from image filenames.
- Render portraits only on the matching official profile. Provide meaningful accessible text where the image conveys information; use empty alt text when the adjacent visible name already identifies the person and the image is decorative.
- Preserve the unresolved status of Nitzshell Torres-Dela Torre’s membership in the official directory. Do not use the Sangguniang Bayan institutional image as a person’s portrait.

## Scraping methods

All methods below are limited to public pages and public records. The site’s `robots.txt` has no `Disallow` paths. The inspected `/home` and `/sitemap.xml` URLs resolved to an admin-login page; do not bypass that access boundary.

1. **Legislation feeds — structured GET requests.** The public catalog pages request `/ordinancesData` and `/resolutionsData` using server-side DataTables parameters. The project already has the `municipal-legislation-datatables/v1` adapter. Use `npm run data:refresh -- --lgu cavinti`; retain exact source responses/hashes, pagination completeness, duplicates, source ordering, and the capture timestamp. Do not scrape or redistribute attachment bodies as part of this metadata refresh.
2. **Other documents — observed DataTables XHR.** Open `/otherDocuments`, inspect its public table request to `/otherDocumentsData`, and preserve the request’s DataTables query parameters (`draw`, `start`, `length`, title filter, and access/type filter). Request `length=100`, `start=0`, and `type=Public`; the inspected response reported 33 total/filtered rows. Extract publication date, title, access label, and the official download URL from each row. This source family has no refresh adapter in the project yet.
3. **News — paginated HTML and detail pages.** Read the public home page (page 1) and `?page=2` through `?page=7`; each inspected page listed three stories. Follow `/news/{id}` links and extract title, published date, category, body text, and source image URL. The sampled detail routes were IDs 1, 10, and 21. No public structured news feed was identified in this inspection.
4. **Officials and portraits — public HTML.** Read `/executive` and `/legislative`; extract each profile’s visible name and source fields. Portraits were found with `img[src*="/author_images/"]`; pair each `alt` value with its `src` and the profile card, not by guessing from filenames. Review image metadata and rights exceptions before local copying.
5. **About, contact, and charter — public HTML.** Extract source-original fields from `/aboutUs` and `/contact`. The Citizens’ Charter currently renders headers without data rows; preserve that observation rather than interpreting it as an empty service catalogue.

### Publication safeguards

- Follow `docs/sources-and-data-policy.md`: manual refresh, source-specific evidence and rights notes, no fabricated translations or inferred facts, and no full-document mirroring by default.
- Record availability, verification, and freshness separately. A successful page fetch does not establish a current office term, a live phone number, or the validity of an attachment’s contents.
- Do not scrape admin pages, contact-form submissions, private records, household/patient data, social posts/comments, or blocked material. Do not defeat authentication, rate limits, or platform controls.
- Preserve source wording and conflicts. In particular, retain the 17-name About-page claim alongside the separate 19-barangay evidence until authoritative roster evidence resolves it.

## Existing official-site links

BetterCavinti already links to the official homepage from the shared “Official website” utility link and the About page (`src/app/App.tsx`). Source records for the official pages are available through the Sources & coverage surface. No duplicate homepage link is needed; add future links beside the matching data section (for example, Other Documents beside document downloads and the news archive beside local updates).
