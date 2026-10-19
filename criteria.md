# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1.

<!-- Your criterion. It must name a number. -->
Overall accuracy is at least 0.70


**Why this target:** Most post will fit the label, but for mixed posts,
which contain a fact and 'what if' opinions, will cause some mistake.



---

## 2.

<!-- Your criterion. -->
`analysis` has an F1 of at least 0.80 and `hot_take` has an F1 of at least
0.60 on the held-out set.




**Why this target:** A `hot_take` is defined by what it lacks (a fact
everyone can look up), which is harder for the model to spot.
Posts that mention stats vaguely, like "did you see his stats?",


---

## 3.

<!-- Your criterion. -->
When the model's confidence is at least 0.8, its predictions are correct at
least 85% of the time on the held-out set.




**Why this target:** I put the model's confidence at 0.80 so it only covers cases where the model is clearly sure.
I expect the unsure predictions to be the mixed posts so confident predictions should be
noticeably more accurate than my 0.70 overall target.

---

## 4.

<!-- Your criterion. -->
Neither `analysis` nor `hot_take` makes up more than 60% of `labels.csv`.




**Why this target:** Capping at 60% keeps that guessing baseline clearly below my accuracy target.



---

## 5.

<!-- Your criterion. -->
When I relabel a random sample of 30 posts, the first labels match at least 26 of 30


**Why this target:** I can't agree with myself on 26/30 of posts, the labels are
too noisy for the model to learn them, and a model accuracy of 0.70 would mean little.


---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
