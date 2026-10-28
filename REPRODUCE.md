# How this repository was made (Qatar)

This file is written by the build on 2026-10-28, the day the source data was retrieved. Everything below is enough to rebuild this repository hash for hash.

## Sources

| Source | Address | Retrieved | Requests | Licence |
| --- | --- | --- | ---: | --- |
| Al Meezan, Qatar Legal Portal (Ministry of Justice) | <https://almeezan.qa> | 2026-10-28 | 133 | Official texts published by the Ministry of Justice of Qatar, credited to Al Meezan. The portal states no licence; see the README on copyright in official texts. |

Every response is cached by the sha256 of its URL, and the table above lists every source with its retrieval date. Lexifina keeps its fetch scripts privately; `tools/parse.py` reads only the cache and writes `corpus.json`, so the same cache always gives the same corpus.

## What is in it

| Act | Title | Versions | Span |
| --- | --- | ---: | --- |
| `dustur` | إصدار الدستور الدائم لدولة قطر | 2 | 2005-06-09 to 2024-11-06 |
| `qanun-al-uqubat` | قانون رقم (11) لسنة 2004 بإصدار قانون العقوبات | 8 | 2004-06-14 to 2024-11-17 |
| `himayat-al-bayanat-al-shakhsiya` | قانون رقم (13) لسنة 2016 بشأن حماية خصوصية البيانات الشخصية | 1 | 2017-01-29 to 2017-01-29 |

The build made 11 commits:

| Commits | Kind |
| ---: | --- |
| 3 | first version of an act |
| 8 | one law on its in-force day |
| 0 | changes no amendment note explains, credited to the publisher |

Other counts:
- 0 commits on the branch `future`: laws not yet in force on 2026-10-28.
- 0 commits dated 1 January 1970: in force before 1970, with the real day in their `In-force` trailer.
- 11 laws in the record.
- 22 tags.

## Dates

Each commit is dated at noon (Asia/Qatar) on the day the change took effect. The other dates, enacted and published, are trailers.

When a law gives no date of its own, the default rule applies: Constitution art. 142: "تنشر القوانين بعد المصادقة عليها وإصدارها في الجريدة الرسمية، وذلك خلال أسبوعين من تاريخ صدورها. ويعمل بها بعد شهر من تاريخ نشرها، ما لم ينص على تاريخ آخر في القانون ذاته." (Laws take effect one month after publication in the Official Gazette unless the law itself fixes another date.) https://almeezan.qa/legislations/constitution/2004/0/general

## Commands, in order

```sh
# fill the cache from the sources listed above (network; Lexifina's fetch scripts are kept privately)
python -X utf8 tools/parse.py                  # cache -> corpus.json (no network)
python -X utf8 tools/build.py <folder> --repo <empty folder> --docs-date 2026-10-28
python -X utf8 tools/check_repo.py <repo> --corpus <folder>/corpus.json --write
python -X utf8 tools/build.py <folder> --repo <another empty folder> --docs-date 2026-10-28 --compare <repo>
```

In the main Lexifina repository these files live as follows:
- `fetch.py`, `parse.py` and `repo/`: in `scripts/qatar-law/`.
- `build.py`, `check_repo.py` (`check.py` there) and `on.py`: in `scripts/gitlaw_common/`, with the corpus format in `SCHEMA.md`.

## Exact inputs

- `corpus.json` sha256 `965c7ee016f966de26e941bbdc0238ddb64ba3050e5a28a8c613e322d55065c4` (406884 bytes)
- Script blob hashes (`git hash-object`):

| Script | Blob |
| --- | --- |
| build.py | `2bfc4ac9df193a7be844556d7f69cdb9d93ec3f2` |
| check.py | `cd346c7ff9996d450240e3bab52bcae03b2bc865` |
| on.py | `644a6c0857fbcc72562032b58d57aa113fee0641` |
| fetch.py | `2156e5dfe302662235754daff0e842e0ea42f142` |
| parse.py | `b54e719ed611c30fe73be986bf8ca61e30f7da84` |

- Tools:
  - Python 3.14.0
  - git version 2.52.0.windows.1
  - curl 8.21.0 (Windows) libcurl/8.21.0 Schannel zlib/1.3.2 WinIDN WinLDAP

## Checks

11 versions of 3 acts read back from git on the day each took effect and on the day before: 0 mismatches. Trailers: 0 commits whose trailers git cannot parse. Health: 0 merge commits on main, 0 commit dates that go backwards, 0 commits dated after the build.

Not yet run for this build.

These results are from the check run on the build before this one, from the same corpus.

## Decisions made by hand

- Texts are Al Meezan's Arabic texts, which are authoritative; Al Meezan's English translations were not used.
- Versions are rebuilt from Al Meezan's per-article history chains (relatedAlmawad): the original wording, each replaced wording, and the amending law and article named for it. Unamended articles carry their current wording from the start.
- Every change is credited to the amending law Al Meezan names on the article's history entry (toLegislation / parentLegislation); nothing is credited by guess.
- In-force days come from the Official Gazette date Al Meezan records for each law, by Constitution art. 142 (one month after publication, the same day of the next month) unless the law fixes its own date. Al Meezan's per-article dates are the laws' issue dates and were not used.
- Repealed articles: Al Meezan keeps the repealed wording as the article's text and the repealing provision in its history; the article is removed on the repealing law's in-force day.
- Added articles (مكرر) take the wording Al Meezan shows for the article, from the adding law's in-force day.
- Text normalisation: HTML line breaks become lines; NBSP, RLM/LRM and tatweel removed; no space before Arabic punctuation; curly quotes and dashes unified; an amending law's quotation marks around a new wording dropped. Words and diacritics are untouched.
- Issuance articles (مواد الإصدار) of the Penal Code are units isdar-NN; each act's promulgation preamble is the unit 'preamble'.
- The constitutional amendments of 2024 are kind 'referendum': approved by referendum on 5 November 2024, in force on the announcement of the result, 6 November 2024 (their art. 2), and published in the Official Gazette on 31 December 2024.
- Al Meezan's history chains for constitution arts 1, 7, 13, 75 bis, 77-79, 82, 83, 125 and 150 name its record of the draft constitutional amendments of 2024 (69c36fc9, issued 2024-10-29, gazette 16 of 2024); they are credited to the issued amendments approved by referendum (d8a3673d) and dated 2024-11-06. To verify against both gazette texts.
- dustur-2004: in force 2005-06-09 by the effective date Al Meezan records (the day after its publication in the Official Gazette); the Constitution was issued on 8 June 2004 and, by its promulgation, published a year later; Al Meezan records issue 2004-06-08, gazette 2005-06-08 (الجريدة الرسمية - العدد 6 لسنة 2005), effective 2005-06-09.
- dustur مادة 1: dropped 1 Al Meezan history entry with no relation, dated 2005-06-09 but carrying the current wording.
- tadilat-dusturiya-2024: in force 2024-11-06 by the law's own rule, مادة 2: "يُعمل بالتعديلات الدستورية لسنة 2024 ... اعتباراً من تاريخ إعلان نتيجة الاستفتاء في 1446/05/04 هجرية، الموافق 2024/11/06 ميلادية"; Al Meezan records issue 2024-11-06, gazette 2024-12-31 (الجريدة الرسمية - العدد 26 لسنة 2024), effective 2024-11-06.
- law-2004-11: in force 2004-06-14 by the law's own rule, مادة 3 (إصدار): "ويعمل به بعد خمسة عشر يوماً من تاريخ نشره في الجريدة الرسمية"; Al Meezan records issue 2004-05-10, gazette 2004-05-30 (الجريدة الرسمية - العدد 7 لسنة 2004), effective 2004-06-14.
- law-2020-2: in force 2020-01-20 by the law's own rule, مادة 4: "ويُعمل به من اليوم التالي لتاريخ نشره في الجريدة الرسمية"; Al Meezan records issue 2020-01-08, gazette 2020-01-19 (الجريدة الرسمية - العدد 2 لسنة 2020), effective 2020-01-20.
- law-2009-23: in force 2010-02-21 by Constitution art. 142 (one month after publication in the Official Gazette); Al Meezan records issue 2009-12-15, gazette 2010-01-21 (الجريدة الرسمية - العدد 1 لسنة 2010), effective 2010-01-21.
- law-2024-14: in force 2024-11-17 by Constitution art. 142 (one month after publication in the Official Gazette); Al Meezan records issue 2024-09-01, gazette 2024-10-17 (الجريدة الرسمية - العدد 14 لسنة 2024), effective 2024-09-01.
- law-2015-22: in force 2016-01-13 by Constitution art. 142 (one month after publication in the Official Gazette); Al Meezan records issue 2015-11-15, gazette 2015-12-13 (الجريدة الرسمية - العدد 19 لسنة 2015), effective 2015-12-13.
- law-2010-8: in force 2010-07-30 by Constitution art. 142 (one month after publication in the Official Gazette); Al Meezan records issue 2010-06-08, gazette 2010-06-30 (الجريدة الرسمية - العدد 6 لسنة 2010), effective 2010-06-30.
- law-2006-28: in force 2006-11-10 by Constitution art. 142 (one month after publication in the Official Gazette); Al Meezan records issue 2006-07-27, gazette 2006-10-10 (الجريدة الرسمية - العدد 9 لسنة 2006), effective 2006-10-10.
- law-2017-4: in force 2017-04-30 by Constitution art. 142 (one month after publication in the Official Gazette); Al Meezan records issue 2017-03-08, gazette 2017-03-30 (الجريدة الرسمية - العدد 4 لسنة 2017), effective 2017-03-30.
- law-2016-13: in force 2017-01-29 by Constitution art. 142 (one month after publication in the Official Gazette); Al Meezan records issue 2016-11-03, gazette 2016-12-29 (الجريدة الرسمية - العدد 15 لسنة 2016), effective None.

## What AI did and did not do

The fetch, parse and check scripts were written by Claude Code running Claude Opus 5.5 (`claude-opus-5-5`) under Lexifina's direction.

No model wrote, translated or corrected legal text. No model runs at build time. Every word of law in this repository comes from the official sources above, through code that anyone can rerun.

The commit messages are written by `build.py` from the source data.

Model use at data-build time:
- None at data-build time: fetch.py and parse.py are deterministic code written with Claude Code (claude-opus-5-5); no model reads, writes, translates or summarises legal text.
