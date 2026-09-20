# Which A24 movie is this?

**Neeharika Hemrajani · nh599 · CPSC 1710, Homework 2 (September 2026)**

You read a plot summary — Letterboxd, Wikipedia, the back of the box — and check off
what it mentions: cult imagery, long silences, a teen protagonist, a frantic pace.
The page guesses which corner of the A24 catalog the movie belongs to, then shows you
the receipts. You don't need to have seen the film.

## About this project

This was built for **CPSC 1710: Intro to AI Apps**, Homework 2 — *"From One Pixel to
Your Classifier."* The brief was to invent a classification problem slightly more
complicated than the course's [One Pixel ML lab](https://xiuyechen.github.io/cpsc1710-labs/lab-02/),
then direct an AI coding agent to build a working page for it, test it like an
experimenter, and improve it based on what the testing revealed.

The assignment's two rules shaped everything here:

1. **The page has to show its reasoning, not just an answer.** A visitor should see
   evidence, not a verdict. That's why every prediction comes with a table of
   per-signal push scores, including the ones arguing *against* the answer.
2. **The point is directing the work, not writing the code.** The development log
   below records the moments I changed direction. The most useful one was #6, where
   the fix turned out to be in the data rather than the model.

The starter lab it builds on teaches that *labels create the task* — the same twelve
grey pixels become a different learning problem depending on how you label them. This
page is an attempt to show the same idea one layer up, where the **inputs** create the
task too: which boxes a reader decides to tick changes the answer more than the film does.

### How it was built

Written with Claude Code (Claude Opus 5) across a single session. I chose the subject,
the labels, the signals and the training films, decided what to test and what was
wrong with it, and directed each round of changes; the agent wrote the HTML, CSS and
JavaScript and made the commits. The git history shows the sequence.

## How to open it

Download or clone this repo, then double-click `a24-genre-classifier.html`.
It opens in any browser. No installation, no accounts, no internet needed.

`one-pixel.html` is the starter lab from class, included for reference.

## How it makes a prediction (in plain language)

It learned from **19 real A24 films** listed at the bottom of the page. I tagged
each one with the signals it has and labeled it with a genre.

For every signal, it counted how often that signal shows up in each genre compared
to how often it shows up on average. That comparison becomes the signal's **weight**:

- "Cult or ritual imagery" appears in 3 of 4 horror films and almost nowhere else,
  so it votes hard for Elevated Horror (+1.13).
- "Darkly funny" appears in all five genres, so it barely votes at all (+0.03).

When you check boxes, it adds up the votes for each genre and the highest total wins.
The evidence table shows you every individual vote, including the negative ones, plus
the training movie whose tags most closely match what you picked.

**Inputs:** 16 checkboxes · **Output:** 1 of 5 genres · **Evidence:** per-signal push
scores, confidence bars, and the nearest training movie.

Six plot summaries are built into the page to test with. None of them are in the
training data, so every one is a genuine new case.

## A limitation I found

Testing three films showed that the prediction often rests on a **single** checkbox.
"Teen protagonist" appears in four training films and all four are Coming-of-Age, so
ticking it alone decides the answer — it predicted Coming-of-Age for *Pearl*, a horror
film, at 75% confidence. "Mostly one location" was learned from four horror films and
*Ex Machina* and nothing else, so it dragged *The Drama*, a romantic comedy, to 48%
Elevated Horror.

The deeper problem was **not** in the model. My first set of plot summaries was written
in a vague, literary style that never stated how old the characters were, whether the
story stayed in one place, or what the tone was. With nothing to go on, a reader fills
the gaps from what they already know about the film — which is exactly what happened in
all three of my tests. I rewrote all six summaries to cover the same ground every time.
The classifier did not change; the inputs did.

## Development log

1. Asked for a small self-contained classifier page, built around romance novel
   tropes predicting a reading "vibe."
2. Changed direction after seeing the first version — swapped the whole subject to
   A24 movies and film genres, which gave the categories real overlap instead of
   invented overlap.
3. Reframed the inputs around **plot summaries** instead of "things you'd notice
   while watching," so someone can test the page without having seen any A24 films.
4. Added *Marty Supreme* and found it didn't fit any existing label — so I added a
   fifth genre, Frantic Hustler, with *Good Time* and *Uncut Gems* alongside it, plus
   two new signals to support it.
5. Realized a visitor had to leave the page to find a plot summary and then come back,
   so I asked for six summaries to be built into the page itself. I specifically asked
   that picking a movie **not** auto-check the boxes — reading a summary and deciding
   what it mentions is the whole exercise, and filling it in would have removed it.
6. Tested three films and kept getting confident wrong answers. Rather than change the
   classifier, I worked out that the plot summaries were the problem — they were written
   too vaguely for anyone unfamiliar with the film to judge, so testers were answering
   from memory instead of from the text. Asked for all six to be rewritten to a
   consistent template: who the character is and roughly how old, where it happens and
   whether it stays there, the tone, and how it ends.

## Note on the labels

The films are real. The genre labels and signal tags are my own judgment calls —
A24 doesn't officially sort its catalog this way. That's the lesson from the One
Pixel lab: the same data becomes a different learning problem when you change the
labels.

The six plot summaries in the "Try a real movie" panel were written for this project.
Five are condensed from the films themselves; *The Drama* (2026) is condensed from its
Wikipedia plot section. They are deliberately written to a fixed template so that two
different readers have a fair chance of ticking the same boxes — see the limitation
above for why that turned out to matter.

## Credits

Starter lab and assignment by Xiuye Chen, CPSC 1710.
Assignment text drafted by Codex (OpenAI) from the instructor's teaching goals.
This page built with Claude Code.
