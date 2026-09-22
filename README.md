# 2026-09-23 - Mars Rover TDD

**C3: Code, Craft, Community**

Pick a language from the [Templates](Templates) folder, open it in your IDE, and make sure the sample test runs. Then delete the sample and start the kata.

The recommended IDEs are as follows, but feel free to use whatever IDE you are comfortable with.

-   [C#](Templates/C%23) - [Microsoft Visual Studio](https://visualstudio.microsoft.com/vs/community/)
-   [Java](Templates/Java) - [IntelliJ Idea](https://www.jetbrains.com/idea/download) (Community Edition is fine)
-   [JavaScript](Templates/JavaScript) - [Microsoft Visual Studio Code](https://code.visualstudio.com/)
-   [Kotlin](Templates/Kotlin) - [IntelliJ Idea](https://www.jetbrains.com/idea/download) (Community Edition is fine)
-   [Python](Templates/Python) - [Pycharm](https://www.jetbrains.com/pycharm/download/?section=windows) (Community Edition is fine)
-   [TypeScript](Templates/TypeScript) - [Microsoft Visual Studio Code](https://code.visualstudio.com/)

## Meetup Agenda

| Part | Time   | Where                          |
|------|--------|--------------------------------|
| 1    | ~30 min | This README                   |
| 2    | ~30 min | `README-part2.md` (revealed later) |
| 3    | ~30 min | `README-part3.md` (revealed later) |
| 4    | ~15 min | Wrap up, discuss, and giveaway |

**Ground rules**

-   **Use TDD.** Red, green, refactor. Write a failing test, make it pass with the simplest thing that works, clean up, repeat.
-   **Swap the keyboard** every 10 minutes or so (or after every green test).
-   **No AI assistants**, please - this one is about you and your pair. (Part 3 has one small exception.)
-   There is **no prescribed design**. No required classes, methods, or signatures. How you model the rover, the grid, and the instructions is up to you and your pair. The examples below are just data: inputs and the expected result.

---

# Part 1 - Mission Briefing

## The story

It is the year 2026 and, deep inside NASA's Mission Control, the room is silent. On the big screen, a small six-wheeled rover named **Pizzazz** sits on the rust-coloured plains of Mars, 225 million kilometres away. The science team is hunting for signs of water, ancient microbial life, anything at all that says *we are not alone*.

The catch: every command takes minutes to reach the rover, so the team can't joystick it around. Instead, they write a whole string of instructions, beam it up, and hope Pizzazz ends up where they wanted it. Your job is to build the software that follows those instructions, so Mission Control can predict exactly where the rover will land **before** they hit "send".

## The rules

**The grid.** The landing zone is a rectangular grid with a `width` and a `height`. Coordinates are `(x, y)`:

-   `(0, 0)` is the **bottom-left** corner.
-   `x` increases to the **east** (right), `y` increases to the **north** (up).
-   Valid `x` is `0` to `width - 1`, valid `y` is `0` to `height - 1`.

**The rover.** It has a position `(x, y)` and a heading: `N`, `E`, `S`, or `W`.

**The instructions.** A string made of these characters, executed left to right:

| Char | Meaning                                                    |
|------|------------------------------------------------------------|
| `M`  | **Move** one cell forward in the direction it is facing    |
| `L`  | Turn **left** 90 degrees (stay in the same cell)           |
| `R`  | Turn **right** 90 degrees (stay in the same cell)          |

**Wrapping.** Mars is round (that's our story and we're sticking to it). Drive off one edge of the grid and you appear on the opposite edge, same row or column, still facing the same way. On a 5x5 grid, moving north from `(2, 4)` puts you at `(2, 0)`.

**Valid input.** The examples below only use valid input: the starting position is always inside the grid, and instructions contain only `M`, `L`, and `R`. (What to do with anything else is up to you - see "Done early".)

**The goal.** Given a grid size, a starting position and heading, and an instruction string, work out where the rover ends up (position and heading).

## Test data

In every table: `Grid` is `width x height`, `Start` is `x y heading`, and `End` is the expected final `x y heading`.

Work top to bottom. Each group builds on the last, so **don't read ahead** - write the first failing test, make it pass, then move to the next row.

### 1. It just sits there

| Grid | Start   | Instructions | End     |
|------|---------|--------------|---------|
| 5x5  | 0 0 N   | *(empty)*    | 0 0 N   |

### 2. Turning

| Grid | Start | Instructions | End   |
|------|-------|--------------|-------|
| 5x5  | 2 2 N | `L`          | 2 2 W |
| 5x5  | 2 2 N | `R`          | 2 2 E |
| 5x5  | 2 2 E | `R`          | 2 2 S |
| 5x5  | 2 2 S | `R`          | 2 2 W |
| 5x5  | 2 2 W | `R`          | 2 2 N |
| 5x5  | 2 2 N | `LL`         | 2 2 S |
| 5x5  | 2 2 N | `LR`         | 2 2 N |
| 5x5  | 2 2 N | `RRRR`       | 2 2 N |

### 3. Moving in the open (no edges involved)

| Grid | Start | Instructions | End   |
|------|-------|--------------|-------|
| 5x5  | 2 2 N | `M`          | 2 3 N |
| 5x5  | 2 2 E | `M`          | 3 2 E |
| 5x5  | 2 2 S | `M`          | 2 1 S |
| 5x5  | 2 2 W | `M`          | 1 2 W |
| 5x5  | 0 0 N | `MM`         | 0 2 N |

### 4. Turning and moving together

| Grid | Start | Instructions | End   |
|------|-------|--------------|-------|
| 5x5  | 1 1 N | `MRMLM`      | 2 3 N |
| 5x5  | 0 0 N | `MMRMMLM`    | 2 3 N |
| 5x5  | 1 1 N | `RMM`        | 3 1 E |
| 5x5  | 3 3 N | `LMM`        | 1 3 W |
| 5x5  | 1 1 E | `MRMRMRM`    | 1 1 N |
| 5x5  | 2 2 N | `MMLMMLMMLMMLMM` | 2 4 N |

### 5. Wrapping - one edge at a time

Each of the four edges, facing straight at it:

| Grid | Start | Instructions | End   |
|------|-------|--------------|-------|
| 5x5  | 2 4 N | `M`          | 2 0 N |
| 5x5  | 4 2 E | `M`          | 0 2 E |
| 5x5  | 2 0 S | `M`          | 2 4 S |
| 5x5  | 0 2 W | `M`          | 4 2 W |

### 6. Wrapping - corners and repeated laps

| Grid | Start | Instructions      | End   |
|------|-------|-------------------|-------|
| 5x5  | 0 0 S | `M`               | 0 4 S |
| 5x5  | 0 0 W | `M`               | 4 0 W |
| 5x5  | 0 0 N | `LM`              | 4 0 W |
| 5x5  | 3 3 N | `RMM`             | 0 3 E |
| 5x5  | 4 4 N | `MRM`             | 0 0 E |
| 5x5  | 4 4 E | `MLM`             | 0 0 N |
| 5x5  | 0 0 N | `MMMMM`           | 0 0 N |
| 5x5  | 0 0 N | `MMMMMMM`         | 0 2 N |
| 5x5  | 1 1 N | `MMMMMMMMMMMMMMM` | 1 1 N |

### 7. Grids that aren't 5x5

Don't assume the grid is square - if your tests only ever use `5x5`, a mix-up between width and height can hide implementation issues. Every row below wraps, and would give a different answer if width and height were swapped.

| Grid  | Start | Instructions | End   |
|-------|-------|--------------|-------|
| 8x6   | 0 0 N | `MMMMMM`     | 0 0 N |
| 8x6   | 0 0 E | `MMMMMMMM`   | 0 0 E |
| 8x6   | 7 5 N | `MRM`        | 0 0 E |
| 3x5   | 2 4 N | `MRM`        | 0 0 E |
| 1x3   | 0 0 N | `MMMM`       | 0 1 N |
| 1x1   | 0 0 N | `MMRMLL`     | 0 0 W |

### 8. Full mission plans

Put it all together.

| Grid  | Start | Instructions         | End   |
|-------|-------|----------------------|-------|
| 5x5   | 2 2 N | `RMMLMMLMMRMM`       | 2 1 N |
| 10x10 | 5 5 N | `MMRMMLMMMRMLLMRMMM` | 7 3 N |

## Done early?

-   Refactor! Does your code read like the rules above? Could a NASA engineer follow it?
-   Which of your tests are redundant? Which are missing?
-   What should happen with an invalid instruction character (say `X`)? Decide, write a test, and be ready to defend your choice.

**We'll add a `README-part2.md` to the repo after about 30 minutes into part 1, or whenever most people seem to have gotten part 1.**

You will be able to do a `git pull` to get that update.
