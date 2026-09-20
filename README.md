# Which A24 movie is this?

A tiny classifier for CPSC 1710, Homework 2.

You read a plot summary — Letterboxd, Wikipedia, the back of the box — and check off
what it mentions: cult imagery, long silences, a teen protagonist, a frantic pace.
The page guesses which corner of the A24 catalog the movie belongs to, then shows you
the receipts. You don't need to have seen the film.

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
  so it votes hard for Elevated Horror.
- "Darkly funny" appears in all four genres, so it barely votes at all.

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
