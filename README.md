# Global Device Trials

**A searchable database of every medical-device, diagnostic-test and combination-product trial on ClinicalTrials.gov — worldwide.**

### → **[Open the site](https://chenru1z.github.io/global-device-trials/)**

---

| | |
|---|---|
| **98,400** | device trials, 16% of the registry |
| **76,177** | tagged `DEVICE` |
| **19,578** | diagnostic tests |
| **3,444** | combination products |
| **23,001** | industry-led |
| **12,311** | currently recruiting |
| **13,160** | with posted results |

Derived from the ClinicalTrials.gov bulk download, snapshot **2026-09-25** (604,566 studies).

---

## What it does

- **Dashboard** — registration volume since 2010, country league table, therapeutic area, device type, study purpose, recruitment status, the largest device companies and academic sponsors.
- **Search** — full-text across titles, conditions, interventions and sponsors, with filters for country, therapeutic area, device type, phase, status, sponsor, sponsor type and start year.
- **Trial pages** — design, eligibility, outcome measures, every site, collaborators, MeSH terms and linked publications, with a link back to the ClinicalTrials.gov record.

No account, no tracking, no backend. The whole thing is static files.

---

## Two findings worth the visit

**Global device-trial volume is flat.** Between 7,200 and 8,500 trials have been registered each year since 2020. There is no worldwide boom.

**China's share more than quintupled** — from about 2% of device trials in 2010 to 11% in 2026. Nearly all of the sector's growth in registered trials is coming from one country while the rest of the world holds steady. The site has a chart for exactly this, and it is the reason the global dataset exists alongside the [China-focused companion site](https://chenru1z.github.io/china-clinical-trials/).

Top countries by trial count: United States 28,773 · China 7,101 · France 6,426 · Germany 4,659 · Italy 4,465 · Canada 4,320.

Largest device companies: Boston Scientific 477 · Abbott 454 · Alcon 295 · Johnson & Johnson Vision Care 212 · Zimmer Biomet 204.

---

## How a "device trial" is decided

A trial qualifies when **any** of its interventions is typed `DEVICE`, `DIAGNOSTIC_TEST` or `COMBINATION_PRODUCT` on ClinicalTrials.gov.

That is the registry's own tag, applied by whoever submitted the record — and it is applied loosely. Interventions such as *"mask ventilation"*, *"low-dose acupuncture"* and *"adjunctive bright white light therapy"* carry it too. **Treat 98,400 as an upper bound** on genuine medical devices, not a precise count.

Unlike sponsor nationality, which ClinicalTrials.gov does not record at all, trial *location* is a structured field. The country breakdown here is read straight from the registry rather than inferred.

### Two things that do not transfer from drug trials

**Phase does not apply.** Devices do not run Phase I–IV, and about 87% of these trials carry no usable phase. The dashboard shows **study purpose** — treatment, diagnostic, screening, prevention — instead.

**Device category does not exist in the data**, so both category axes are derived:

- **Therapeutic area** from condition MeSH terms — covers about 77% of trials.
- **Device type** from keywords in intervention names — stent, catheter, stimulation, imaging, ablation and so on — covers about 41%.

Neither is complete, so each keeps an explicit **Uncategorised** bucket, and the dashboard states the unmatched share under the chart rather than hiding it.

---

## Limitations

- **The device tag is submitter-applied and loose** — see above.
- **Device trials are registered far less consistently than drug trials.** Registration is voluntary in much of the world, so this undercounts real activity, and more so outside the United States. Country counts reflect *registration* behaviour as much as research activity.
- **Registration is self-reported.** "Recruiting" can be years stale.
- **2026 is a partial year** in every chart and count.
- The search index covers titles, conditions, interventions and sponsors, **not** trial summaries. Summaries are shown in full on each trial page.

---

## How it is built

The same four-pass Python pipeline that builds the China site, with one step swapped:

1. **Index all 604,566 studies**, recording each one's intervention types.
2. **Select** every study carrying a device-type intervention. (The China site substitutes its own rules here; everything else is shared.)
3. **Build a SQLite database** — 9 normalised tables plus an FTS5 index.
4. **Export to static JSON.** The column-oriented, dictionary-encoded search index fits 98,400 trials into **7.4 MB gzipped**; it downloads once, after which search and filtering run entirely in the browser. Trial pages fetch one shard out of 1,000.

Python standard library only. No frameworks, no build step, no dependencies.

---

## Attribution

Data courtesy of the **U.S. National Library of Medicine**, from [ClinicalTrials.gov](https://clinicaltrials.gov). ClinicalTrials.gov data is in the public domain.

**NLM does not endorse or verify this site.** This is an independent project and is not affiliated with the NLM, the NIH, or any sponsor or manufacturer listed in the data.

Nothing here is medical advice. To find a trial to participate in, consult ClinicalTrials.gov and a qualified clinician.

---

## Accuracy

If you find a trial that is wrongly included or wrongly missing — particularly one where the device tag is doing something odd — please [open an issue](../../issues) with the NCT ID. The classification rules can be tightened.
