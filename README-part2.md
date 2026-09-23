# Part 2 - Deploy the Sensors

*(Part 1 is in [README.md](README.md). Finish that first - this builds directly on your rover.)*

## The story

Pizzazz has been roving happily, and Mission Control is thrilled. But the science team has a new request: driving around is nice, but they want **data**. Pizzazz carries a hopper of small ground sensors that can measure moisture, temperature, and chemistry. The plan is to lay them down in a pattern across the landing zone and map the whole area.

## The new instruction

| Char | Meaning                                                                                  |
|------|------------------------------------------------------------------------------------------|
| `D`  | **Drop** a sensor. Moves forward exactly like `M`, then leaves a sensor on the cell it lands on |

Everything from Part 1 still applies: `M`, `L`, `R`, the grid, and wrapping (a `D` that crosses an edge wraps just like an `M`, and the sensor goes on the cell where the rover lands).

A few details to pin down:

-   The cell the rover **starts** on does *not* get a sensor, unless a `D` later lands there.
-   The rover has an unlimited supply of sensors, but dropping a second sensor on a cell that already has one changes nothing. Each cell holds **at most one** sensor.
-   `M` never drops a sensor, even over a cell that already has one.

**The goal.** Given the same inputs as before (grid size, start, instructions), work out the rover's final position and heading **and** the set of cells that ended up with a sensor. The order of the sensors doesn't matter, so make your tests compare them as a set (or sort them first).

## Test data

Same notation as Part 1. `Sensors` lists the `(x, y)` cells that hold a sensor at the end. All the Part 1 tests should still pass - if you see red, stop and fix that first.

### 1. Baby steps

| Grid | Start | Instructions | End   | Sensors    |
|------|-------|--------------|-------|------------|
| 5x5  | 2 2 N | `MMM`        | 2 0 N | *(none)*   |
| 5x5  | 0 0 N | `D`          | 0 1 N | (0,1)      |
| 5x5  | 2 2 E | `DD`         | 4 2 E | (3,2) (4,2) |
| 5x5  | 0 0 N | `MDMD`       | 0 4 N | (0,2) (0,4) |

### 2. Turning corners

| Grid | Start | Instructions | End   | Sensors                   |
|------|-------|--------------|-------|---------------------------|
| 5x5  | 0 0 N | `LD`         | 4 0 W | (4,0)                     |
| 5x5  | 0 0 N | `DRDLD`      | 1 2 N | (0,1) (1,1) (1,2)         |
| 5x5  | 0 0 N | `DRMD`       | 2 1 E | (0,1) (2,1)               |
| 5x5  | 0 0 E | `DMDMD`      | 0 0 E | (0,0) (1,0) (3,0)         |

### 3. Wrapping while dropping

| Grid | Start | Instructions | End   | Sensors            |
|------|-------|--------------|-------|--------------------|
| 5x5  | 4 4 N | `DRD`        | 0 0 E | (4,0) (0,0)        |
| 5x5  | 0 0 N | `DDDDD`      | 0 0 N | (0,0) (0,1) (0,2) (0,3) (0,4) |
| 5x5  | 0 0 S | `D`          | 0 4 S | (0,4)              |
| 3x5  | 2 4 N | `DRD`        | 0 0 E | (2,0) (0,0)        |

The `5x5` `DDDDD` row is a full lap: the very last `D` lands back on the starting cell, so it *does* get a sensor. The `3x5` row wraps on both axes of a grid that isn't square.

### 4. Duplicates

Dropping on a cell that already has a sensor must not create a second one, and an `M` over a sensor must leave it in place (the `DDRRM` row drives back over its own sensor).

| Grid | Start | Instructions   | End   | Sensors                       |
|------|-------|----------------|-------|-------------------------------|
| 5x5  | 0 0 N | `DDDDDDD`      | 0 2 N | (0,0) (0,1) (0,2) (0,3) (0,4) |
| 5x5  | 2 2 N | `DRRDLDRRD`    | 2 2 W | (2,2) (2,3) (3,2)             |
| 5x5  | 0 0 N | `DDRRM`        | 0 1 S | (0,1) (0,2)                   |
| 3x3  | 0 0 N | `DDDDRDDDD`    | 1 1 E | (0,0) (0,1) (0,2) (1,1) (2,1) |

### 5. Shapes

Draw these on graph paper if it helps.

| Grid | Start | Instructions   | End   | Sensors                                                        |
|------|-------|----------------|-------|----------------------------------------------------------------|
| 5x5  | 2 2 N | `DRDRDRD`      | 2 2 W | (2,3) (3,3) (3,2) (2,2)  - a 2x2 square                        |
| 5x5  | 1 1 N | `DRDRDRDRD`    | 1 2 N | (1,2) (2,2) (2,1) (1,1)  - the same square, plus a fifth `D` onto an existing sensor |
| 5x5  | 0 0 N | `DDRDDRDDRDDR` | 0 0 N | (0,1) (0,2) (1,2) (2,2) (2,1) (2,0) (1,0) (0,0)  - a ring      |
| 5x5  | 0 0 N | `DDRMDLDD`     | 2 4 N | (0,1) (0,2) (2,2) (2,3) (2,4)  - gaps where it used `M`         |

## Done early?

-   How would you print the grid, with `#` for a sensor and `.` for empty? Print the top row first, so `y = height - 1` is the first line and `y = 0` is the last. (You'll want that next.)
-   Is a sensor a position? A value object? Does your rover *own* the sensors, or does the grid?

**We'll add a `README-part3.md` to the repo after about 30 minutes into part 2, or whenever most people seem to have gotten part 2.**

You will be able to do a `git pull` to get that update.
