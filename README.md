# Qatar: law history in git

Part of [lexifina/global-law](https://github.com/lexifina/global-law), one git history for every country's law: this is Qatar's national law, which global-law reads at `qatar/national/acts/`.

The law of Qatar as a git history: one file per provision, one commit per law on the day it took effect, and the law that made each change as the commit's author. `git blame` names the law behind every paragraph; `git log` lists every change; picking a date gives the text in force that day.

It follows the shared design of [lexifina/global-law](https://github.com/lexifina/global-law).

**What is in it now**

Three acts with their full dated history, and (from November 2026) every other constitution, law and decree-law that Al Meezan marks in force (قيد التطبيق), each in its own folder named `<type>-<year>-<number>/` (for example `law-2016-13/`, `decree-law-1995-24/`). `data/versions.json` lists them all.

From 5 November 2026 the decrees, Amiri decisions and orders, Council of Ministers and Prime Minister decisions, ministerial decisions and decisions of other authorities that Al Meezan lists and marks in force are added in batches, each in a folder named `<type>-<year>-<number>/` (for example `cabinet-decision-2022-4/`), or `<type>-<issuer>-<year>-<number>/` where Al Meezan files the instrument under its issuer (for example `ministerial-decision-finance-minister-2026-25/`). Left out: individual instruments (appointments, promotions, retirements, transfers, pardons, awards, nationality), instruments whose text carries personal identifiers, and instruments whose Akoma Ntoso XML on Al Meezan does not carry the same Arabic text, since each added instrument is checked against it.

The three acts with full history:

| Folder | Act | Versions | History |
| --- | --- | ---: | --- |
| `dustur/` | الدستور الدائم لدولة قطر (Permanent Constitution of Qatar) | 2 | As in force on 9 June 2005; amended by the constitutional amendments of 2024 (in force 6 November 2024) |
| `qanun-al-uqubat/` | قانون العقوبات (Penal Code, Law 11/2004) | 8 | As issued (in force 14 June 2004); amended by Laws 28/2006, 23/2009, 8/2010, 22/2015, 4/2017, 2/2020 and 14/2024 |
| `himayat-al-bayanat-al-shakhsiya/` | قانون حماية خصوصية البيانات الشخصية (Personal Data Privacy Protection Law, Law 13/2016) | 1 | As issued (in force 29 January 2017); Al Meezan shows no amendment |

**Language.** Arabic, the authoritative text. Al Meezan's English translations are not used. This README is in English.

**Source.** The texts are from [Al Meezan](https://almeezan.qa), the legal portal of the Ministry of Justice of Qatar, through the public JSON API behind it. Each article's dated history chain on Al Meezan names the amending law; changes are credited only to the law it names.

**The other acts.** An act whose articles Al Meezan shows no amendment, addition or repeal for is recorded as enacted, credited to itself. An act Al Meezan shows as amended is recorded as Al Meezan's current text: one version, the oldest version available, credited to the publisher, dated by the act's own in-force day, with repealed articles left out. (For the decrees and decisions added from November 2026, an amended instrument's current text is dated instead by the latest in-force day among the instruments its changed articles' history names.) Its dated history (the original wording and each amending law) is not yet recorded; it will be added on top, never by rewriting.

## Dates

Each commit is dated by the day the change took effect: the law's own in-force clause where it has one, otherwise one month after publication in the Official Gazette (Constitution art. 142). For many older laws Al Meezan records only the year of the Official Gazette issue; where the law takes effect on publication, or the record has no in-force rule, those laws are dated by the day they were issued. Each act's rule is listed in REPRODUCE.md. The six Penal Code amendments that only say "shall be published in the Official Gazette" take effect on that default. The 2024 constitutional amendments took effect on the announcement of the referendum result (6 November 2024).

## How it is checked

Every version is read back from git on the day it took effect and on the day before (`tools/check_repo.py`) and must match `corpus.json`; results are in `REPRODUCE.md`.

## What "author" means

The author of a commit is the law that made the change, with an e-mail address that cannot exist (`<law key>@almeezan.qa.invalid`). The 2024 constitutional amendments are recorded as approved by referendum. The committer is always Lexifina.

## Limits

Decrees and decisions are being added in batches (see above); until a batch is published, those instruments are not here. Amended acts other than the three above have no dated history yet, so their one version reads as in force from the act's own start although its text is today's. Part and chapter headings are not files. Al Meezan's record of the draft 2024 constitutional amendments is credited to the amendments as issued. Al Meezan's English texts are partial and unofficial and were not used.
