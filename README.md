# Print_Memory
Intro Game one print data type and casting
https://rhoded-uwp.github.io/Print_Memory/


# Pioneer Pete's Memory Mine

A single-file browser game where you match Python code prompts to their output. Built as a study aid for intro programming students.

## What the game does

There are three timed memory boards plus a results screen.

| Level | Name        | Grid | Cards | New material            |
|-------|-------------|------|-------|-------------------------|
| 1     | Greenhorn   | 2x4  | 8     | Easy Python expressions |
| 2     | Prospector  | 4x4  | 16    | Adds intermediate ones  |
| 3     | Trailblazer | 4x6  | 24    | Adds harder ones        |
| 4     | Summit      | -    | -     | Total time + access code |

Each level carries every pair from the previous level forward and adds new ones, so the questions you've already seen come back as you climb. You have to finish a level before the next one unlocks. Page refresh resets everything.

When you click a card it flips over. Match a code line to its output and the cards stay revealed. Miss, and the cards flip back after two seconds. If you click a third card before they flip back, the missed pair flips back immediately and your third click counts as the start of a new turn.

## How to run it

It's one file. You have two options:

1. **Open `index.html` in any browser.** Double-click it. That's it. No install, no server, nothing to download.
2. **Publish to GitHub Pages.** Drop `index.html` into a repo, enable Pages on the main branch root, and the URL `https://yourname.github.io/your-repo/` will serve it.

There are no dependencies, no build step, no external libraries, no fonts loaded over the internet. Everything (HTML, CSS, JavaScript, and the Pioneer Pete artwork) lives inside the one file.

## How the code is laid out

If you want to read the source as a learning exercise, here's what to look for:

- **Top of `<style>`**: a `:root` block defines the color palette as CSS variables (`--blue`, `--orange`, `--cream`, `--navy`). Notice how the rest of the stylesheet references those names instead of repeating hex codes.
- **The card flip**: search for `.card-inner` and `rotateY(180deg)`. The flip animation is pure CSS, no JavaScript. The trick is `backface-visibility: hidden` on each face plus a 3D rotate on the parent.
- **Question data**: at the top of `<script>` you'll see three arrays — `EASY`, `MEDIUM`, `HARD`. Each is a list of `{ prompt, answer }` objects. This is your textbook example of an array of objects.
- **`pickRandom(arr, n)`**: implements the Fisher-Yates shuffle. Worth reading slowly if you've never seen one.
- **`handleCardClick(level, idx)`**: the core game logic. It's a small state machine — `first` and `second` track which cards are currently face-up, and the code branches on which state we're in.
- **`recordScore(totalMs)`**: empty on purpose. There's a high-score table on the Summit screen with a `TODO` comment marking where the sort-and-insert logic would go. **This is a deliberate hook for a future assignment.**

## Full disclosure: how this project was built

This entire project was written by **Claude**, an AI assistant from Anthropic, in a single conversation with the instructor. Nothing here was hand-coded line by line — the instructor described what they wanted, answered Claude's clarifying questions, and Claude produced the HTML/CSS/JavaScript file.

That matters for two reasons:

1. **You should know who wrote the code you're reading.** A teacher used AI to produce a teaching tool, and you're entitled to know that so you can think critically about the result. Real engineers increasingly do the same thing; understanding the workflow is part of being a programmer in 2026.
2. **The code is not magic and not perfect.** It was reviewed and tested but never compiled, type-checked, or peer-reviewed by another human. If you find a bug, you've found a real bug. Tell your instructor.

### The collaboration looked like this

1. **The instructor wrote an initial prompt** (reproduced below) describing the game they wanted, along with an Excel spreadsheet of prompt/answer pairs.
2. **Claude pushed back on problems in the prompt** — most notably that a 3x3 grid (9 cards) and a 5x5 grid (25 cards) can't be filled with pairs because those numbers are odd. Claude proposed three alternatives.
3. **The instructor picked an option** (use 2x4, 4x4, 4x6 grids instead) and answered follow-up questions about persistence, level unlocking, the click-during-2-second-wait edge case, and the visual theme (Pioneer Pete, a UTEP-style miner mascot).
4. **Claude wrote a refined prompt** capturing every decision in one place.
5. **Claude wrote `index.html`** following that refined prompt.

The instructor reviewed the output and the game was tested in a browser before being given to you.

### The instructor's original prompt, exactly as written

Below is the **first** message the instructor sent to Claude. It is included verbatim — typos, grammar, and all — so you can see what a real-world starting point looks like. Notice how much was vague, contradictory, or unspecified. The conversation that followed was mostly about pinning those parts down.

> @C:\Claude\Memory\problems for memory v1.xlsx Please help me improve this prompt to Claude Code to : Create a single file htlm, css and Javascript program that plays a classic memory game, where 9, 16, then 25 cards are placed face down and the user must match the card to score. It will be a single player game with three tabs. The first tab will be a 3 by 3 array, then the second tab 4 by 4 and the last tab 5 by 5. Players much match prompt to answer to score and remove the combination. Player click on tow cards at a time to reveal a prompt and the answer, after a 2 seconds both cards are flipped back over. Once all cards are collected, the times is recorded on the final victory screen. There will be a final tab that contains the sum of all the times. Room for A high score board(top 5 scores, don't implement yet) should be saved. On the bottom should be text labeled "ACCESS CODE: _CS1430_ROCKS = True" The questions are for the activity are included in the attached excel file. Notice old items are continues from board to board.  Feel free to ask me questions!

### Things that changed between the initial prompt and the final game

- **Grid sizes.** 3x3, 4x4, 5x5 became 2x4, 4x4, 4x6 because odd-card grids can't be filled with pairs.
- **Carry-over rule.** "Old items continues from board to board" was clarified to mean "every previous level's pairs come back, plus a few new ones from the next difficulty tier."
- **Persistence.** Originally implied to save high scores; the instructor decided the v1 should reset on every page reload, with the high-score table as a styled placeholder for later.
- **Level unlocking.** Added — the Summit tab is locked until you've cleared all three boards.
- **Edge case for clicking a third card.** Defined explicitly: the two mismatched cards flip back immediately and your click starts a new turn.
- **Visual theme.** Pioneer Pete (blue and burnt orange) was chosen after the rest of the spec was nailed down. The mascot is drawn as inline SVG so the whole game stays in one file.

## If you want to extend this

Some ideas, easiest first:

1. **Add your own questions.** Edit the `EASY`, `MEDIUM`, or `HARD` arrays at the top of `<script>`. The game will pick up your additions automatically.
2. **Change the colors.** Edit the four CSS variables in `:root` at the top of `<style>`. Pick your own school colors.
3. **Implement the high-score table.** Replace the empty `recordScore(totalMs)` function. Store the top 5 totals in `localStorage`. Prompt the player for a name on the Summit screen. Render the top 5 sorted ascending by time.
4. **Add a "moves" counter** alongside the timer.
5. **Add a hint button** that briefly reveals one unmatched card.

## Access code

If you make it to the Summit screen you'll see the access code printed at the bottom:

```
ACCESS CODE: _CS1430_ROCKS = True
```

That's there for the instructor's grading workflow. You'll know what to do with it.
