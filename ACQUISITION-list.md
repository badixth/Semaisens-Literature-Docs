# Documents to acquire — with URLs, costs and what each protects against

**Status: working checklist.** Compiled 11 August 2026 from the two research passes behind `MODEL-nutrient-band.md`. Ordered by **what it protects against**, not by how interesting it is.

**Verification key.** ✅ retrieved and read in full · ⚠️ reachable but unusable (no text layer, preview only) · 🔒 paywalled or login-walled · ☠️ dead · ❓ URL reconstructed, not confirmed

---

## Priority 1 — free, and it blocks encoding

### 1.1 · FFTC Extension Bulletin 398 — the rubber band and its rate lookup

**Pushparajah, E. (1994), *Leaf Analysis and Soil Testing for Plantation Tree Crops*, FFTC Extension Bulletin No. 398, IBSRAM, Bangkok, 12 pp.**

| | |
| --- | --- |
| Original | ☠️ `http://www.fftc.agnet.org/htmlarea_file/library/20110804160807/eb398.pdf` |
| Wayback | ❓ `https://web.archive.org/web/20170918073128/http://www.fftc.agnet.org/htmlarea_file/library/20110804160807/eb398.pdf` |
| Mirror | 🔒 `https://km.fftc.org.tw/article/4049` — login required |

**Why this is first.** Every rubber number in `MODEL-nutrient-band.md` comes from this document, and **all seven of its tables are scanned bitmaps read visually, once, by a single agent that could not do a second pass.** Two values break otherwise monotonic columns and are almost certainly scan errors:

- Potassium table, leaf K **1.2–1.4%**, clone group II → prints **`100`** where the column trend implies ~800–900
- Potassium table, leaf K **1.6–1.8%**, clone group II → prints **`9200`** where the column trend implies ~100–200

**Do not encode a single rubber row until someone has re-read Table 1 and Table 3 against the page images.** Encoding `9200 g K₂O/tree/yr` would be a two-order-of-magnitude over-application.

**Also worth checking on the same re-read:** Table 1 has boundary overlaps in the original — K group I shows 1.25 in both Low and Medium, 1.50 ending Medium and starting High, and a gap between High 1.65 and Very high > 1.70. Decide how the schema resolves them rather than inheriting the ambiguity.

**And a caution about that bulletin's Table 4:** it is captioned as oil palm frond 17 but its columns read *Local tall / Dwarf / Hybrid* with N 1.80–2.20% — **those are coconut figures.** The table images appear transposed. Do not use Table 4.

---

## Priority 2 — RM 120 and two emails, and it protects against a wrong clone mapping

Both sit in the **Malaysian Rubber Board library portal**, `https://rios.lgm.gov.my`, purchasable at **RM 60.00** each.

### 2.1 · The manual the industry standardised on

**RRIM (1990), *Manual for diagnosing nutritional requirements for Hevea*, Rubber Research Institute of Malaysia, Kuala Lumpur.** Record **`vital1:25068`**.

**Why it matters more than its price suggests.** Pushparajah's clone groups date from **1972** and name RRIM 600, GT1 and PB clones. **Our own docs already care about RRIM 2000-series clones** — `CLAUDE.md` records their Corynespora susceptibility as a defect-causing fact. If the 2000-series are not in those groups, we would be reading modern clones against a band that predates them, **which is the young-mature error wearing a clone name.**

Its abstract carries an attribution absent from every citation seen elsewhere: *"The efforts of the RRIM/MRPC Joint Working Group on Soil and Foliar Data Interpretation, under the chairmanship of Dr Chan Heun Yin, in compiling this Manual should be complimented."*

### 2.2 · How the industry actually used it

**Chang, A.K. & Teoh, C.H. (1981), *Commercial experience in the use of leaf analysis for diagnosing nutritional requirement of Hevea*, Proc. RRIM Planters' Conference 1981, pp. 220–231.** Record **`vital1:27813`**.

Context rather than correctness — useful for the derivation layer, not blocking.

---

## Priority 3 — free, and it upgrades provenance from industry to authority

### 3.1 · MPOB Oil Palm Bulletin 72

**Afandi (2016), *Oil Palm Fertiliser Recommendation for Sabah Soils*, MPOB Oil Palm Bulletin 72, May 2016, pp. 1–24.**

⚠️ `https://palmoilis.mpob.gov.my/publications/OPB/opb72-afandi.pdf` — **live, no text layer. Needs OCR.**

**What it would buy.** The oil palm tissue band we would ship is **PPI/IPNI's** — a fertiliser-industry institute — because MPOB cites *"the critical level"* as a known standard without printing it. Index snippets show Bulletin 72 contains a table classifying nutrients **very low to very high** and a nutrient-balance table. **If MPOB's own bands are in there, the provenance problem disappears.**

**Proof the site works and the problem is the file:** ✅ `https://palmoilis.mpob.gov.my/publications/OPB/opb71-khalid.pdf` (Bulletin 71, Nov 2015) extracts as clean text from the same directory.

**Same OCR problem, same directory** — worth doing in one batch:
- ⚠️ `https://palmoilis.mpob.gov.my/publications/TT/TT-307.pdf`
- ⚠️ `https://palmoilis.mpob.gov.my/publications/TT/TT-416.pdf`
- ⚠️ `https://palmoilis.mpob.gov.my/publications/TT/tt135.pdf`

---

## Priority 4 — would close remaining gaps, none of them blocking

### 4.1 · The last place a Malaysian pineapple standard could be

**Malip Mujib & Ahmad Tarmizi Sapii, *Manual Teknologi Penanaman Nanas*, Penerbit MARDI, ISBN 9789679365641.**

🔒 `https://mardi.elib.com.my/book/details/123033` — login-walled.

**Only changes anything if it finds something.** We refuse pineapple either way; this would turn a refusal-by-absence into a band. Worth one login attempt.

### 4.2 · The rice calibration equations

**"Location specific fertilizer management for rice using soil test target yield approach in Malaysia", FFTC 2018.**

🔒 `https://doi.org/10.56669/fvyg4353` — paywalled. **The most likely home of MARDI's actual soil-test-to-rate equations**, which RiceFERT keeps inside Visual Basic software.

### 4.3 · Whether any Malaysian limit is regulatory rather than agronomic

**MS 2530:2022** (MSPO), parts 3 (estates) and 4 (smallholders).

🔒 Sold through MySOL / Malaysia Standards Online, linked from `https://mspo.org.my/standards/`.

Would settle whether buffer-zone setbacks or maximum application rates are **law** rather than agronomy. Relevant to PR #5's restricted-product carve-out.

### 4.4 · MARDI's bulletin estate — host is down, Wayback only

☠️ `ebuletin.mardi.gov.my` returns NXDOMAIN. Everything below is archive-only.

| Document | URL |
| --- | --- |
| ✅ Bil. 8 (2015) — soil fertility and nutrient management for rice | `https://web.archive.org/web/20180403161630id_/http://ebuletin.mardi.gov.my/buletin/08/Pengurusan%20kesuburan%20tanah.pdf` |
| ✅ Bil. 19 (2020) — RiceFERT, site-specific fertiliser | `https://web.archive.org/web/20220121215728id_/http://ebuletin.mardi.gov.my/buletin/19/Theeba.pdf` |
| ☠️ **Bil. 48 — site-specific rice rates, off-season** | `http://ebuletin.mardi.gov.my/buletin/48/Theeba.pdf` — **indexed, on-point, and no Wayback capture exists. Effectively lost.** |

⚠️ `https://jtafs.mardi.gov.my/jtafs/53-2/Nitrogen%20application.pdf` — NurulNahar Esa et al. (2025), *A review for nitrogen application in Malaysian rice production*, JTAFS 53(2): 27–47. **Returns empty.** Only other copy is login-walled on ResearchGate.

### 4.5 · One paper on tropical atmospheric correction

**Chraibi, E., et al. (2022), *Int. J. Appl. Earth Obs. Geoinf.* 112, 102884.** DOI `10.1016/j.jag.2022.102884` — gold open access, but ScienceDirect and the HAL deposit were both blocked. **Worth one manual download** if anyone revisits the satellite question.

---

## Already retrieved — the sources the model currently rests on

Recorded so the citations can be checked without repeating the search.

**Oil palm**

| | |
| --- | --- |
| ✅ MPOB peat rates — Hasnol Othman, MPOB–SOPPOA 2016 | `http://soppoa.org.my/wp-content/uploads/2016/12/1.2_Fertilizer-Recommendation.pdf` |
| ✅ Goh (AAR) — derivation systems, the four rate functions | `https://aarsb.com.my/wp-content/AgroMgmt/OilPalm/FertMgmt/Computation/Fertilizer%20recommendation%20systems%20for%20oil%20palm%20-%20estimating%20the%20fertilizer%20rates.pdf` |
| ✅ Goh, Teo, Chew & Chiu (AAR) — agronomic principles | `https://www.aarsb.com.my/wp-content/AgroMgmt/OilPalm/FertMgmt/Principle/Fertiliser%20management%20in%20oil%20palm-Agronomic%20Principles.pdf` |
| ✅ Rankine & Fairhurst 1999 leaf tables, via Akvopedia mirror | `https://akvopedia.org/wiki/Sustainable_Oil_Palm_Farming_/_Leaf_sampling` |
| ✅ MPOB Bulletin 71 | `https://palmoilis.mpob.gov.my/publications/OPB/opb71-khalid.pdf` |

**Rice**

| | |
| --- | --- |
| ✅ DOA *Pakej Teknologi Padi* 2008 — LCC threshold, dose, and Jadual 10 | `https://www.doa.gov.my/doa/resources/aktiviti_sumber/sumber_awam/penerbitan/pakej_teknologi/padi/pt_padi_2008.pdf` |
| ✅ DOA *Rice Check: Padi* 2022 | `https://www.doa.gov.my/doa/resources/aktiviti_sumber/sumber_awam/penerbitan/pakej_teknologi/padi/rice_check_padi_2022.pdf` |
| ⚠️ Dobermann & Fairhurst 2000 — **33-page preview only; Table 6 not in it** | `https://books.irri.org/9810427425_content.pdf` |
| ✅ Linquist (2020), UC Davis Fact Sheet #9 — the fullest attributed transcription of Table 6 | search *"Optimal and Critical Nutrient Concentrations in Rice Tissue" UC ANR* |

**Rubber and pineapple**

| | |
| --- | --- |
| ✅ Vrignon-Brenas et al. 2019 — immature rubber review, open access | DOI `10.1007/s13593-019-0554-6` |
| ✅ RISDA smallholder GAP — visual signs, no leaf band | `https://kms.risda.gov.my/wp-content/uploads/2019/09/1534394574.pdf` |
| ✅ MPIB *Manual Pengurusan Penanaman Nanas* — no tissue concept at all | `https://mpib.gov.my/wp-content/uploads/2017/11/buku-manual-pengurusan1.pdf` |

---

## What is genuinely lost

- **MARDI ebuletin Bil. 48** — no capture anywhere.
- **Thiagalingam (2000)**, credited as the source of "Malaysian rubber norms" in the Cameroon literature, is **a Papua New Guinea NARI/AusAID training manual** (ISBN 9980932430), not online and not Malaysian. **Do not cite it as a Malaysian standard.** Its N range matches RRIM clone group 2, but its P, K and Mg do not match Table 1 and it carries a Ca range Table 1 does not have.
- **A measured NDVI saturation threshold for mature *Hevea*.** Our rule is physically sound and consistent with the age–NDVI curve, but **it is an inference, not a citable Hevea number.** Worth knowing before anyone asks for the source.

---

## The one gap no document closes

**Whether Malaysian estates will pay for leaf analysis at the frequency this model needs.** MPOB's protocol is per Leaf Sampling Unit, composited from about 30 palms. Smallholders almost certainly will not.

**No amount of reading answers that, and it decides whether layer 2 is a feature or a wall.** One conversation with an estate agronomist is worth more than everything in Priority 4.
