# Qatar: law history in git

The law of Qatar as a git history: one file per provision, one commit per law on the day it took effect, and the law that made each change as the commit's author. `git blame` names the law behind every paragraph; `git log` lists every change; picking a date gives the text in force that day.

It follows the shared design of [lexifina/global-law](https://github.com/lexifina/global-law).

**What is in it now (a first build, not complete)**

| Folder | Act | Versions | History |
| --- | --- | ---: | --- |
| `dustur/` | الدستور الدائم لدولة قطر (Permanent Constitution of Qatar) | 2 | As in force on 9 June 2005; amended by the constitutional amendments of 2024 (in force 6 November 2024) |
| `qanun-al-uqubat/` | قانون العقوبات (Penal Code, Law 11/2004) | 8 | As issued (in force 14 June 2004); amended by Laws 28/2006, 23/2009, 8/2010, 22/2015, 4/2017, 2/2020 and 14/2024 |
| `himayat-al-bayanat-al-shakhsiya/` | قانون حماية خصوصية البيانات الشخصية (Personal Data Privacy Protection Law, Law 13/2016) | 1 | As issued (in force 29 January 2017); Al Meezan shows no amendment |

**Language.** Arabic, the authoritative text. Al Meezan's English translations are not used. This README is in English.

**Source.** The texts are from [Al Meezan](https://almeezan.qa), the legal portal of the Ministry of Justice of Qatar, through the public JSON API behind it. Each article's dated history chain on Al Meezan names the amending law; changes are credited only to the law it names.

## Dates

Each commit is dated by the day the change took effect: the law's own in-force clause where it has one, otherwise one month after publication in the Official Gazette (Constitution art. 142). The six Penal Code amendments that only say "shall be published in the Official Gazette" take effect on that default. The 2024 constitutional amendments took effect on the announcement of the referendum result (6 November 2024).

## How it is checked

Every version is read back from git on the day it took effect and on the day before (`tools/check_repo.py`) and must match `corpus.json`; results are in `REPRODUCE.md`.

## What "author" means

The author of a commit is the law that made the change, with an e-mail address that cannot exist (`<law key>@almeezan.qa.invalid`). The 2024 constitutional amendments are recorded as approved by referendum. The committer is always Lexifina.

## Limits

Only three acts are here so far. Part and chapter headings are not files. Al Meezan's record of the draft 2024 constitutional amendments is credited to the amendments as issued. Al Meezan's English texts are partial and unofficial and were not used.
