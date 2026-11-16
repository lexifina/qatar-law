# Tools

The scripts behind this Qatar-only run of global-law's vulnerability detectors. They run inside a copy of
[global-law/vulnerabilities/scripts](https://github.com/lexifina/global-law/tree/main/vulnerabilities/scripts) (Python 3.11+,
`pip install duckdb pyarrow`), with a bare clone of this repository at `sources/corpus/qatar-law.git`, a blobless bare
clone of global-law at `sources/global-law.git`, and a `pins.json` that names only qatar-law and its commit (language
`ar`). `build_vulns.py`, `flags_chart.py`, `late_ext_select.py` and `late_ext_rebut.py` sit two folders above the scripts,
next to `config.json`.

| Script | What it does |
| --- | --- |
| `run.sh` | Indexes the history and texts, maps moved paths (`map_moved.py`), adds this repository's history rows, and runs the detectors (`bash run.sh <shards>`) |
| `map_moved.py` | Rewrites the paths in the history table to each act's folder for a repository that moved its folders (`data/paths.json`); this one has none, so it does nothing here |
| `add_root.py` | Builds the history rows from this repository's own commits (global-law replays it only up to an earlier commit), so the history detectors read every change |
| `run_triage.sh` | The model review: the 40 strongest flags per category, the 40 strongest lapsed time limits in law texts, and the empty periods (here 52 flags: the 12 lapsed time limits, reviewed in both passes, and 40 of the 793 retroactive dates; no empty periods) |
| `late_ext_select.py` | Chooses the empty periods for global-law's review of that category (there were none here) |
| `late_ext_rebut.py` | Adds the second, arguing-against pass to the empty-period review, as `triage.py` does for the other categories |
| `build_vulns.py` | Builds `data/flags.csv`, `data/reviewed.csv` and `data/by_category.csv` from the findings and the model review |
| `flags_chart.py` | Draws the chart at the top of the READMEs, in Arabic and English, light and dark; the Arabic chart is drawn right to left |
| `config.json` | The repository, the Arabic category names, the files that are not law, and the chart's labels |
| `patch_detectors.py` | Fixes blind spots found in runs on several country repositories in the copy of global-law's scripts (`python -X utf8 patch_detectors.py <scripts dir>`) |
| `patch_detectors_vi.py`, `patch_es.py`, `patch_ca.py`, `patch_en.py`, `patch_de.py`, `patch_en_et.py`, `patch_co.py`, `patch_sv.py`, `patch_pt.py`, `patch_ru.py`, `patch_r18.py`, `patch_xk.py`, `patch_mo.py`, `patch_lu.py`, `patch_li.py`, `patch_mx.py`, `patch_mn.py`, `patch_mt.py`, `patch_np.py`, `patch_en_nr.py`, `patch_dv.py`, `patch_az.py`, `patch_en_nz.py`, `patch_nb.py`, `patch_mk.py`, `patch_pa.py`, `patch_py.py`, `patch_pe.py`, `patch_en_ph.py` | The same for the blind spots other runs found in other languages and repositories; this run's copy carries them all. None of them reads Arabic, beyond the reference scan's "المادة (N)", which was already there |
| `patch_ar.py` | The fixes for Arabic made for Oman's run: "until" (حتى, لغاية ...) and "before the end of the year" before a date, the Gregorian month names, and amounts in rials and baisas (`python -X utf8 patch_ar.py <scripts dir>`) |
| `patch_qa.py` | The fixes made for this run, below (`python -X utf8 patch_qa.py <scripts dir>`; each change carries a `patch_qa` marker, and one already in place is skipped) |
| `apply_patches.py` | Applies every patch above once, in order, recording each in `.kit_patches.json` in the scripts folder (`python -X utf8 apply_patches.py <scripts dir>`); `patch_qa.py` is last in its list |
| `patch_build_r7.py` | Brings an older country's `build_vulns.py` up to date (`redo.py` runs it; the `build_vulns.py` here already has its changes) |
| `redo.py` | Runs `apply_patches.py`, then again only the scans and detectors the fixes change, the model review of lapsed time limits, references to deleted provisions and empty periods, and the build (`python -X utf8 redo.py <run folder> [--keep-reviews] [--full]`; `--full` runs every step again through `run.sh`; `EMBED_FONTS` names the script that embeds the chart's fonts); not needed here, as every fix was in place before the run whose results are published |

The model review needs `LLM_BASE_URL`, `LLM_API_KEY`, `LLM_MODEL` and `LLM_AUTH` set for an OpenAI-compatible Responses API.
It was run as `bash run_triage.sh '^(data/|tools/|vulnerabilities/|README|REPRODUCE|LICENSE|jurisdiction)'`; every law is
in an act folder, so the review of lapsed time limits in law texts drew the same 12 flags as the review of all of them, and
`build_vulns.py` keeps the later review of each.

Changes made for this repository:

- `pins.json` names the corpus language as `ar`: every text is in Arabic.
- `add_root.py` is unchanged: the commits that hold the law carry `Kind: law` (606: laws and decree-laws as issued, the
  Constitution and the changes to the Penal Code), `Kind: decree` (927, decisions), `Kind: publisher` (188, acts held as today's text) and
  `Kind: referendum` (the 2024 constitutional amendments); the kit counts all four as law. The commits with no `In-force`
  trailer (READMEs, tables, tools, `REPRODUCE.md`) are left out.
- `patch_qa.py`, time limits (`ingest/textscan.py`): a period "ending on" a date was not read. Qatari texts write a term
  or an exemption as running "لمدة سنتين تنتهي في 30/10/2000" (for two years ending on 30/10/2000) or "تبدأ من أول مايو
  2008 وتنتهي في 31 ديسمبر 2009"; "تنتهي"/"ينتهي" (it ends), with "و" (and) before and "في" (on) after, now count as
  "until" before a date, as "expires on" does in English. Lapsed time limits went from 1 to 12 (the one found before is
  "حتى 31/12/1999"). Left out, as not a limit or not past: "تنتهي بعد 30 يونيو 2028" (ends after), "انتهاء المدة الأصلية
  في 8/ 10/ 1990" (the start of an extension), "حتى 31 ديسمبر من السنة التالية" (no year).
- `patch_qa.py`, currencies (`detectors/currency_scan.py`): the rupee was not read. The dead-currency search wants a
  number next to the currency; Qatari texts write it after the amount in words or in brackets ("خمسة آلاف روبية", "50 ألف
  روبية", "(20) روبية"), so in a corpus whose code is `QA` the word (روبية, روبيات) is read on its own. Dead currencies went
  from 0 to 4. The riyal ("ريال") is current, and `patch_ar.py` already reads amounts in it for the search for stale amounts,
  which found none: no amendment in the history replaces an amount.
- Not fixed: the reference scan (`ingest/refscan.py`) reads "المادة (N)", but not ordinal words ("المادة الخامسة", which
  mostly name the articles of an issuing law). Its detector also needs a deleted article in the history, which only the
  Constitution and the Penal Code can show; their references to articles of the same act were checked by hand against the
  articles held.
- Not a detector problem: the in-force days of 22 acts (see EDGE-CASES.en.md) come from this repository's build, which
  did not read their clauses; the retroactive-date detector reads the days as the commits give them.
- `config.json` counts as not law `data/`, `tools/`, `vulnerabilities/`, the root files and every `README.md` (each
  folder's list of its articles). Decisions of the Council of Ministers and Amiri decisions are law: they bind as
  regulations and are published in the Official Gazette.
