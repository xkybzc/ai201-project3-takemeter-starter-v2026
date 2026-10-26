# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->
TakeMeter is a classifier for text posts from r/nba, a public subreddit
about the NBA. It reads a post and sorts it into one of two labels: `fact`,
for a post that contains at least one fact anyone can look up, or
`hot_take`, for a post that makes a claim without such a fact.


---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `facts`

**Definition:** a post that contains at least one fact that everyone can look up.

**Example 1:**
>The source for all this data is basketball reference. Data is for the past 4 seasons + current one (2019-2020 through today) Insights:- Sixers win 62% of home games Embiid sits (16-10), but win 44% of road games Embiid sits (22-28)- This is extremely exaggerated when Embiid sits vs. top 10 teams (Sixers are 4-15 on road vs. Top 10 teams when Embiid sits)- Sixers play .500 ball vs. top 10 teams at Home with Embiid sitting (5-5 record) If Embiid decides to sit less road games (his personal stats would suffer a bit), his TEAM would perform better record wise. His team has shown they can win vs. bad teams AND great teams at home, but cannot replicate the success on the road (this is not unusual - role players play worse on the road in most cases).

**Example 2:**
>So there's been lots of talk lately about how high offensive stats have gotten now, especially for the top tier, and how it's hard to directly compare them to the past. But how would stats look if you adjust them for that purpose? I chose 2015/16 as the year because it's a more neutral environment where efficiency had clearly risen from 2000s levels but hadn't exploded yet, plus it's a good year to use to compare to the LeBron/Steph/KD generation. Without further ado..... In 2025/26, the NBA scores 115.7 points per 100 possessions on exactly 100.0 possessions a game. The league average TS% is 58.3%. Meanwhile, in 2015/16, the league scored 106.4 points per 100 possessions on 95.7 possessions a game. The league average TS% was 54.1%. With this in mind, the following adjustments will be made: For PPG and APG: Multiply by 0.88 (the product of the points per 100 gap and the possessions per game gap) For RPG: Just multiply by 0.957 (the pace ratio only, more missed shots can mean more rebounds so increased scoring doesn't necessarily inflate this) For TS%: Multiply by 0.928 (the ratio of 54.1% compared to 58.3%) So what are the results?Adj. PPG	Adj. RPG	Adj. APG	Adj. TS% Jokic	26.0	11.8	9.6	66.4% SGA	28.5	4.5	5.6	63.4% Luka	31.0	8.4	8.0	56.7% Overall, this is a pretty crude method, but I do think these adjustments are significantly useful. For example, it illustrates that Jokic's scoring efficiency and Luka's scoring volume are legitimately all-time levels, but Jokic's scoring volume and Luka's scoring efficiency are good but not quite up there in full context (SGA being in the middle of the two.)

### `hot_take`

**Definition:** a post that makes a claim but does not contain fact that everyone can look up.

**Example 1:**
>My hot take: Wilt is undebatable top-3. Did you see his stats? He was the best in the offense and in the defense. People say he wouldn't be so good in our time. But he was actually so fast that he would easily guard the perimeter in our time.

**Example 2:**
>Shaq is top 5 all time

### The hardest boundary

**Which two labels:** 'hot_take' and 'analysis'

**The decision rule I used every time:**
<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->
If the posts contains at least one fact that everyone can look up, it's `fact`.
If the number is decoration on a conclusion the writer had already reached, it is a
`hot_take` with a statistic in it.



---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** r/nba (subreddit about NBA)

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected? Say so plainly — the disclosure is required, not penalised. -->
I lable the first 15 by myself, than I use Copilot to do the rest.
**Counts per label:**

| Label | Count | Share |
|---|---|---|
| fact | 113 | 56.8% |
| hot_take | 87 | 43.2% |
| **Total** | 200 | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* "Jayson Tatum is your 2023-24 NBA MVP."
>
> *Could have been:* fact or hot_take
>
> *I chose, because:* hot_take, because Jayson Tatum is not 2023-2024 NBA MVP

**2.**
> *The post:* Kyrie Irvin hit the game-winning 3-pointer over Stephen Curry in Game 7 of the 2016 NBA Finals.
>
> *Could have been:* fact or hot_take
>
> *I chose, because:* fact, because it did happen in that game

**3.**
> *The post:* Dirk Nowitzk is the highest-scoring European-born player in NBA history with 31560 regular-season points.
>
> *Could have been:* fact or hot_take
>
> *I chose, because:* fact, because it contain a fact.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** distilbert-base-uncased

**Settings:** <!-- epochs, learning rate, batch size, seed --> 3 epochs, learning rate 2e-5, batch size 16, max length 128
tokens, seed 42.

**Anything I changed from the defaults, and why:** Nothing

**Split sizes:** <!-- train / val / test, and per-label counts in the test split. If a label had fewer than about 8 in test, say so — it explains a lot of next unit's variance. -->

70/15/15 stratified split.
Train 139 / validation 31 / test 30.

| label | train | val | test |
|---|---|---|---|
| fact | 79 | 18 | 17 |
| hot_take | 60 | 13 | 13 |



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* I asked Claude to help me writing criterion 3, criteria 4 and 5.
- *What came back:* Claude give some advice on adjusting the number.
- *What I changed:* I change the number in those 3 criteria.

**Moment 2**

- *What I asked for:* Training crashed in section 4 with a ValueError
- *What came back:* Claude return that labels in my CSV that don't match my
  label map turn into NaN
- *What I changed:* I change my labels cause my in labels.csv I put `fact` while in the notebook I put `facts`

**Pre-labelling disclosure:**I labeled 15 posts myself with no AI help
(marked `cold` in the `note` column). I then had Microsoft Copilot pre-label
the remaining 185 posts, using my label definitions and decision rule.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.567 | 0.833 | +0.267 |
| Macro F1 | 0.362 | 0.824 | +0.462 |
| F1 — `fact` | 0.723 | 0.865 | +0.141 |
| F1 — `hot_take` | 0.000 | 0.783 | +0.783 |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->
     I think the model will beat the baseline on


**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->
     The trained model beat the baseline on every measure. Most of the gain was hot_take, because the baseline never chose it at all.
     This tells me my written definitions were too broad. While My labeled examples showed a narrower meaning of “fact”.



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.70 | 0.833 | 0.800 | 0.767 | MET |
| 2. Every label F1 | `fact` ≥ 0.80 and `hot_take` ≥ 0.60 | 0.865 / 0.783 | 0.850 / 0.700 | 0.821 / 0.667 | MET |
| 3. Confidence | ≥ 85% correct when confidence ≥ 0.80 | not reported | not reported | not reported | COULD NOT BE MEASURED |
| 4. Balance | No label above 60% of `labels.csv` | 0.57 | 0.57 | 0.57 | MET |
| 5. Consistency | ≥ 0.867 | 1.00 | 1.00 | 1.00 | MET |

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.833 | 0.800 | 0.767 | 0.067 |
| Macro F1 | 0.824 | 0.775 | 0.744 | 0.080 |
| F1 — `fact` | 0.865 | 0.850 | 0.821 | 0.044 |
| F1 — `hot_take` | 0.783 | 0.700 | 0.667 | 0.116 |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted | fact | hot_take |
|---|---|---|---|
| **fact** | 16 | 1 |
| **hot_take** | 4 | 9 |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->
     4 real 'hot_take' posts were called 'fact', only 1 went the other way, so the model leans toward fact on the boundary.
     I think it because fact has more posts, so when the model is unsure, it guess fact


---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall accuracy | ≥ 0.70 | MET | above 0.70 on all three seeds. |
| 2 | Every label F1 | `fact` ≥ 0.80 and `hot_take` ≥ 0.60 | MET | Lowest `fact` F1 was 0.821 and lowest `hot_take` F1 was 0.667, both on seed 2024. |
| 3 | Confidence | ≥ 85% correct when confidence ≥ 0.80 | COULD NOT BE MEASURED | The notebook reports accuracy by confidence thirds, not above a fixed threshold.|
| 4 | Balance | No label above 60% of `labels.csv` | MET | 114 `fact` / 86 `hot_take` |
| 5 | Consistency | ≥ 0.867 agreement with staff | MET | 30 / 30 = 1.00 in `agreement_results.json`. |
**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->

**Criterion 3 (could not be measured):** the 0.80 threshold sat above the model's usual
confidence.


---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** 30 / 30 = 100%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:** Added 30 'hot_take' posts.

**Which diagnosis pointed at it:** the lean toward 'fact'

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.70 | 0.857 | 1.000 | 0.800 | MET |
| 2. Every label F1 | `fact` ≥ 0.80 and `hot_take` ≥ 0.60 | 0.865 / 0.848 | 1.000 / 1.000 | 0.811 / 0.788 | MET |
| 3. Confidence | ≥ 85% correct when confidence ≥ 0.80 | not reported | not reported | not reported | COULD NOT BE MEASURED |
| 4. Balance | No label above 60% of `labels.csv` | 0.50 | 0.50 | 0.50 | MET |
| 5. Consistency (agreement with staff, 30 posts) | ≥ 0.867 | 1.00 | 1.00 | 1.00 | MET |


**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->
     



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->




**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->


<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
