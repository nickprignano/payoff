# Payoff spec (v1)

Status: red-teamed with Fable (changes listed at the end). The owner named it and created the
repo (2026-11-09); the other defaults stand. Merging this spec approves it. Nothing gets built
until then.

## What it is

A bite-size game theory course for phones, in the spirit of language-learning apps: short
lessons, instant feedback, a daily streak, and spaced review of what you got wrong. It's for
**13 and up**. Each lesson takes 2 to 4 minutes and mixes quick questions with tiny games you
play against a bot, so the ideas come from playing, not from reading.

Working name **Payoff**, served at `payoff.prignano.ai`. Plain JavaScript and DOM with no build
step, static files on Cloudflare, installable as an app (PWA) from day one like pinball.

## Decisions so far

| # | Question | Answer |
| --- | --- | --- |
| 1 | Feel | Like a language-learning app, for game theory (owner, 2026-11-08) |
| 2 | Audience | 13 and up (owner, 2026-11-08) |
| 3 | Name | **Payoff** (owner, 2026-11-09) |
| 4 | v1 content | Default 5 units of about 5 lessons each. Approved when this spec merges |
| 5 | Mechanics | Default XP, daily goal, streak with rest days, spaced review. No hearts, leaderboards, push or guilt. Approved when this spec merges |
| 6 | Accounts | Default none: progress on the device, export code, no server, no AI at runtime. Approved when this spec merges |
| 7 | Home | `nickprignano/payoff`, created by the owner (2026-11-09), served at `payoff.prignano.ai`, deployed on merge by Workers Builds |
| - | License | MIT. All content, art and sound are our own |

## Prior art

Checked 2026-11-08. Nothing to fork; the ai-brain prior-art check found nothing on this topic.

| Source | What it is | What we take |
| --- | --- | --- |
| *The Evolution of Trust* (Nicky Case, 2017) | Interactive explainer of the repeated prisoner's dilemma | Ideas only. The author dedicates it to the public domain (CC0, per ncase.me/faq), but the repo has no license file and bundles a CC BY-NC sound and a third-party font, and its strategy names, characters and chapter order are its own. We use the academic strategy names, write unit 4 from Axelrod's published work, and credit it on the About screen as inspiration |
| Language-learning apps | The short-lesson, streak and review pattern | The general mechanics only. None of any app's name, mascot, colors, sounds or coined terms (so "rest day", not "streak freeze"), and a plain lesson list, not a winding node path |
| gametheory.net and university applets | Payoff-table applets for courses | Nothing. Confirms a 2x2 table is the core teaching object |
| Game theory apps in the app stores (for example "Rival Edge", "GameTactic") | Strategy and puzzle apps | Nothing. None found is a short-lesson course with play |
| Axelrod's tournaments (1980s), Nash (1950), Flood and Dresher (1950) | The science | Facts, cited in each exercise's `source` field and the lesson's "where this comes from" line. No textbook text is quoted |
| pinball PR #7 | Our own PWA recipe | Manifest, icons, iOS tags, service worker with a stamped file list, stale-file test, update button |

## The player's path

1. **First open.** One screen: what this is, **Start**, or **I know some game theory** (opens the
   unit 1 checkpoint). No sign-up. The daily goal starts at 10 XP and changes in Settings.
2. **Home.** A list of units, each with its lessons, the next one highlighted. A header shows
   the streak, today's XP against the goal, and **Review** when review items are due.
3. **A lesson.** 8 to 12 exercises, one per screen, then a results screen with XP.
4. **Checkpoint.** Ends each unit: 10 exercises drawn from the unit, no repeats, pass at 8 of 10,
   retry any time. Passing unlocks the next unit. Any unlocked unit's checkpoint can be taken
   early to skip ahead, which is how a player who knows some game theory moves fast.

## Lessons

### Exercise types

Every answer is a tap (or a key on desktop). The only text input is pasting an export code.

| Type | What the player does |
| --- | --- |
| **Read the table** | Tap the payoff a named player gets for a named pair of moves |
| **Best response** | Given the other player's move, tap your best row |
| **Cross it out** | Tap a strictly dominated move to strike it, repeat, and tap **Nothing left** when none remains |
| **Find the equilibrium** | Tap every cell where neither player wants to switch alone, then **Done** (any number of cells, including none) |
| **Name the game** | Read a two-sentence story and pick prisoner's dilemma, stag hunt, chicken, or zero-sum |
| **Predict** | Pick what a named strategy plays next given the history so far |
| **Play** | A short game against a bot, then one question about it |
| **Tournament** | Watch a seeded round-robin of a fixed set of strategies, then one question about it |

Feedback is instant: right or wrong, one sentence saying why, and the deciding cells
highlighted. A missed exercise comes back once at the end of the lesson; missed again, the app
shows the answer and moves on. Lesson XP doesn't depend on misses.

### Where the content comes from

Most exercises are generated, not hand-written, to keep the volume honest:

- About **60 payoff tables** and **40 short stories** are written by hand in `data/`.
- Read the table, Best response, Cross it out, Find the equilibrium and Predict are built at
  runtime from a table (or a strategy and history) plus a seed, and graded by the solver.
- Name the game, Play and Tournament questions, and each lesson's one-idea intro, are written
  by hand.

Every hand-written item carries `idea`, `skill`, `explain` and `source` fields, required by the
schema. Claude writes the content; the owner reviews unit 1 in full and a random 10 items from
every other unit in their PRs.

### v1 units

1. **Games and payoffs.** Players, moves, payoffs; reading a 2x2 table; payoffs show what a
   player prefers, not money; simultaneous vs one-after-the-other; zero-sum vs not.
2. **Dominant moves.** Best responses; strictly dominant moves; the prisoner's dilemma; why two
   self-interested players end up worse off; crossing out strictly dominated moves step by step
   (including one 3x3).
3. **Equilibrium.** Nash equilibrium as "no one wants to switch alone"; coordination and the
   stag hunt (two equilibria); chicken (two lopsided equilibria); matching pennies (none in
   plain moves, the setup for unit 5).
4. **Playing again.** The repeated prisoner's dilemma; a fixed field of strategies by their
   academic names (Always Cooperate, Always Defect, Tit for Tat, Generous Tit for Tat, Grim
   Trigger, Win-Stay Lose-Shift, where "win" means a payoff of at least mutual cooperation);
   a tournament in that field; mistakes and noise; **brinkmanship**: chicken with rising stakes,
   ending with a link to Zero Sum ("Now try it on a whole crisis"). Lesson text always says "in
   this field" and never claims a strategy is best in general.
5. **Mixing it up.** First line: "Now the size of the numbers matters, not just their order."
   Rock-paper-scissors and why any pattern can be exploited; mixed strategies; making the other
   side indifferent; a penalty-kick game; the mixed equilibrium of a 2x2 that has no plain one.

About 25 lessons.

### The solver and the rules it grades by

`src/solve.js` handles 2x2 games, plus strict iterated elimination for 3x3:

- best responses, strict dominance, iterated elimination of strictly dominated moves (order
  doesn't matter for strict dominance, so any valid order is graded right);
- pure Nash equilibria;
- the interior mixed equilibrium of a 2x2, returned only when both probabilities are strictly
  between 0 and 1, otherwise "none";
- the game's type, on ordinal payoffs of symmetric 2x2s with labels that can't overlap:
  prisoner's dilemma T > R > P > S, stag hunt R > T > P > S, chicken T > R > S > P, zero-sum
  when every cell's payoffs sum to the same number.

The content schema forbids ties among payoffs a question compares. Tests check every table's
stated type against the solver; whether a story matches its table is checked by the owner's
review sample, because no test can read a story.

### Bots, Play and Tournament

- Each strategy in `src/strategies.js` is a pure function of the history and a seeded random
  source. Same seed, same game. Random-move bots are never used in Predict.
- **Play** questions are templates whose answers are computed from that game's history (for
  example "How many rounds did the bot defect first?"), not stored answers.
- In unit 5 the player plays 10 rounds of rock-paper-scissors against a "pattern" bot (a fixed
  cycle) and a "mixer" bot (uniform random). Each result screen shows the player's 10 rounds
  beside 1,000 simulated rounds of an always-rock player against the same bot, and asks about the
  long-run average, so a lucky streak doesn't contradict the lesson.
- **Tournament** runs a fixed field per lesson, 200 rounds a match, computed live. Questions are
  computable facts ("Which strategies never defected first?", "Which finished above Always
  Defect?"), not "why".

## Mechanics

- **XP:** 10 per lesson, 5 per review session, 20 per checkpoint.
- **Daily goal:** 10, 20 or 30 XP, set in Settings (10 to start).
- **Streak:** a day counts when the goal is met. Worked out on open from `lastGoalDate` using
  local calendar dates (year, month, day, never milliseconds divided by a day, which breaks on
  daylight saving). A gap of N missed days spends N rest days if the player has them, otherwise
  the streak starts over.
- **Rest days:** the player holds up to 2 and earns one every 7 streak days. When one is used
  the home screen says so in one neutral line. A streak ending is also one neutral line, with no
  animation. No push notifications or warnings.
- **Spaced review:** each exercise has a skill tag (for example `dominance`, `nash-pure`). A miss
  puts that skill in box 1 of five Leitner boxes (due after 1, 2, 4, 8, 16 days). Right in
  review moves it up a box; right in box 5 retires it; wrong sends it back to box 1. **Review**
  is 6 exercises, taken round-robin across due skills, generated or drawn only from lessons the
  player has finished.
- **No hearts or lives**, and no leaderboards (there's no server).

## Look, sound and access

- Original look on the shared prignano theme (pinned files, like pinball): light and dark, big
  tap targets, payoff tables as real HTML tables. No mascot in v1.
- Two colors identify the two players everywhere, always with a label or shape too.
- Screen readers read tables cell by cell with row and column names; every exercise works by
  keyboard on desktop. Reduce motion turns off animations.
- Sound synthesized with the Web Audio API (a right chime, a soft wrong tone, a lesson-done
  flourish), muted until the first tap, mute saved on the device.

## Data and privacy

- Progress (lessons done, XP, streak, rest days, Leitner boxes, settings) lives in
  `localStorage`. If storage is blocked, the app works for the session and says progress won't
  be kept.
- **Export code:** Settings shows progress as a copyable code with a `version` field, and takes
  one pasted back. It's checked against the schema before it's applied; an old or bad code is
  refused with a plain message.
- Nothing leaves the device. No accounts, analytics, ads, server or AI calls. The only network
  requests are the pinned theme files, and the only outside link is Zero Sum. The About screen
  says so.

## Installable app and hosting

Pinball PR #7's PWA recipe (manifest, original icons, iOS tags, offline service worker stamped
by `tools/sw-version.mjs`, stale-list test, update button, safe-area padding).

Cloudflare Workers static assets with no Worker script (`wrangler.jsonc` named `payoff`,
`.assetsignore` leaving out tests, tools and docs), connected with Workers Builds so each merge to
`main` deploys. The owner creates the repo and adds the custom domain. Claude writes
`docs/DEPLOY.md` like pinball's and changes nothing on the account without his yes.

## Tests

`node --test` covers:

1. **Content:** every data file matches the schema (required `idea`, `skill`, `explain`,
   `source`); lessons have 8 to 12 exercises; every skill tag exists; no compared payoffs tie;
   every table's stated type matches the solver.
2. **Solver:** prisoner's dilemma, stag hunt, chicken, matching pennies, a coordination game and
   a 3x3 elimination case give their textbook answers; the prisoner's dilemma has no interior
   mixed equilibrium; matching pennies' is 50/50.
3. **Generated exercises:** for 1,000 seeds per type, every generated exercise has exactly one
   right answer under the solver.
4. **Bots and tournament:** each strategy plays as defined; same seed, same game; over 1,000
   seeds each claim a unit 4 lesson makes holds in at least 95% of runs (for example, with 5%
   noise in the lesson's field, Grim Trigger against itself averages below Generous Tit for Tat
   against itself). Lesson text and test share one list of claims in `data/claims.json`.
5. **Mechanics** with a fake clock: XP; streak across midnight, a daylight-saving change, one
   rest day spent, a gap longer than the rest days held; Leitner moves, retirement and
   round-robin; review never draws from unfinished lessons; export then import gives the same
   progress; a bad or old code is refused.
6. **PWA:** the service worker file list is current.

**Done means:** all tests pass in CI, and before each PR the build thread runs a scripted browser
check (Playwright, not in CI, like pinball) at phone and desktop sizes that completes one lesson
of each exercise type and a review session. Installing and offline use are a manual check in
`docs/DEPLOY.md`.

## Pull requests

1. The app shell: home, lesson player, the table exercise types (Read the table, Best response,
   Cross it out, Find the equilibrium, Name the game), solver, mechanics, PWA recipe,
   `wrangler.jsonc`, `docs/DEPLOY.md`, CI, and **unit 1** for the owner to review in full.
2. Units 2 and 3 with their checkpoints.
3. Units 4 and 5: strategies, Predict, Play, Tournament, the claims file, the Zero Sum link.
4. About screen (credits, sources, what the app sends where), export code, polish.

## Not in v1

Accounts and sync, leaderboards or friends, an AI tutor (could reuse the chess Worker pattern
later), more units (sequential games and backward induction, auctions, bargaining, signaling,
evolutionary games, voting), other languages, a mascot, drag-and-drop exercises.

## Open questions

1. **Name clash.** A quick search (2026-11-08) found no game theory app called Payoff, but the
   word is common, including in finance. Default: a fuller app-store search before the first
   build PR; the owner decides if anything close turns up.

## Fable red team (2026-11-08)

What it found, and what changed:

1. Tournament and noise lessons would have taught one cherry-picked run as a law. **Now a fixed
   field per lesson, results computed live, questions on computable facts, "in this field"
   wording, and every claim tested over 1,000 seeds from a shared claims file.** Added Generous
   Tit for Tat and cut Random from the field.
2. "Name the game" labels overlapped (a stag hunt is a coordination game). **Labels are now
   defined on ordinal payoff orders that can't overlap, and "coordination" was dropped as a
   label.** Story-to-table fit is checked by human review.
3. "You can't beat the mix" is false over 10 rounds, and a 2x2 doesn't always have an interior
   mixed equilibrium. **Result screens show a 1,000-round simulation beside the player's 10;
   the solver returns a mixed equilibrium only when interior; unit 5 opens by saying the size of
   the numbers now matters.**
4. Lesson rules contradicted each other (repeat until right vs repeat once, no-cost mistakes vs
   a no-miss bonus) and checkpoints were undefined. **Second miss shows the answer; no bonus;
   checkpoint is 10 exercises, pass at 8.**
5. Scope contradictions. **Cut drag-and-drop "Build the table" and the separate placement test
   (an early checkpoint does that job); goal picker moved to Settings; PR 1 now carries only the
   table exercise types; solver limited to 2x2 plus 3x3 strict elimination.**
6. Leitner edges were unspecified. **Box 5 retires, wrong goes to box 1, review draws only from
   finished lessons, round-robin across skills.**
7. The streak was tested but not defined. **Defined on calendar dates on open; a streak ending
   is one neutral line.**
8. 300 hand-written exercises was unrealistic to review. **About 60 tables and 40 stories by
   hand, the rest generated and solver-graded; the owner samples every unit.**
9. Play and Predict had no answer rules. **Play questions are computed from the game; Random is
   never used in Predict; "win" for Win-Stay Lose-Shift is defined.**
10. Weak-dominance elimination depends on order. **Strict dominance only, a Nothing left button,
    any number of equilibrium cells.**
11. The vocabulary bundle read like a clone. **"Rest day" instead of "streak freeze", "Review"
    instead of "Practice", a plain lesson list instead of a node path; unit 4 written from
    Axelrod, not from The Evolution of Trust's chapter order.**
12. Privacy was clean. **Added a version field to the export code and an About-screen line on
    what the app sends where.**
