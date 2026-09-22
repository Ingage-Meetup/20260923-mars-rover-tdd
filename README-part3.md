# Part 3 - Zoom Out

*(Parts 1 and 2 are in [README.md](README.md) and [README-part2.md](README-part2.md).)*

## The story

Mission Control has been studying the sensor map and one of the analysts squints at the screen. *"Hold on. If you zoom out a little..."*

Pizzazz has been busy. Somewhere on Mars, there is now a very large, very deliberate message that is visible from orbit. It seems to be a message for the Code, Craft, Community meetup!

## Your mission

Feed the instructions below to your rover on a **21x22** grid, starting at **(0, 0) facing East**, and render the sensors it drops as ASCII art: `#` for a sensor and `.` (or a space) for an empty cell. Print the top row first, so `y = 21` is the first line of output and `y = 0` is the last.

The rover should finish at **(0, 21) facing East** with **89 sensors** dropped. Those are two extra tests you can write before you even look at the picture.

## The instructions

```
MMMMMMMMMMMMMMMMMMMMMLMRMMMMMMMMMDMMMMMMMMMMMLMRMMMMMMMMDMDMMMMMMMMMMLMRMMMMMMMDMMMDMMMMMMMMMLMRMMMMMMDMDMMMDMMMMMMMMLMRMMMMMDMMMDMMMDMMMMMMMLMRMMMMDMMDMMMDMMDMMMMMMLMRMMMDMMMMDMMMMMMDMMMMMLMRMMDMMDMMMMMDMMMMDMMMMLMRMDMMMMMMMMMMMMMMMDMMMLMRDDDDDDDDDDDDDDDDDDDMMLMRDDDDDDDDDDDDDDDDDDDMMLMRMMMMMMMMMMMMMMMMMMMMMLMRMMMMMMMMMMMMMMMMMMMMMLMRMMMMDDDMMMMMDDDMMMMMMLMRMMMDMMMDMMMDMMMDMMMMMLMRMMMDMMMMMMMMMMMDMMMMMLMRMMMDMMMMMMMMMDDMMMMMMLMRMMMDMMMMMMMMMMMDMMMMMLMRMMMDMMMDMMMDMMMDMMMMMLMRMMMMDDDMMMMMDDDMMMMMMLMRMMMMMMMMMMMMMMMMMMMMM
```

You don't need to type that in! Copy it into a constant in your test, or a text file that your code reads.

## Ideas

1.  **Make it a test.** Write a test that runs the instructions above and checks the final position, the sensor count, and (once you have a renderer) the rendered picture. You should do the expected picture last - and try not to peek at it until you have something on the screen.
2.  **Write a renderer.** Given a grid size and a set of sensors, produce the text picture. This is a great place for a small, well-tested function.
3.  **Read the rover's mind.** Notice how the instructions are built: they sweep the grid row by row from the bottom, use `D` where a sensor should go and `M` where it shouldn't, and take advantage of wrapping to get back to the start of the row. Can you spot the pattern in the string?
4.  **Draw your own.** Write your own instruction string that draws your own picture. A logo? Your initials? Your pair's favourite pizza topping? Show the room!

## AI policy

We're asking everyone to write the code themselves. The one exception: if you want to build a **cool visualization** for the picture (colours in the terminal, an animation of the rover placing sensors one at a time, an HTML canvas, whatever), you may use AI to help with *that part only*. Your rover logic and your tests should still be yours.

## Spoiler: what it should look like

<details>
<summary>Click only when you have something on screen!</summary>

```
.....................
.....###.....###.....
....#...#...#...#....
....#...........#....
....#.........##.....
....#...........#....
....#...#...#...#....
.....###.....###.....
.....................
.....................
.###################.
.###################.
..#...............#..
...#..#.....#....#...
....#....#......#....
.....#..#...#..#.....
......#...#...#......
.......#.#...#.......
........#...#........
.........#.#.........
..........#..........
.....................
```

(`.` is an empty cell, `#` is a sensor. It is the whole 21x22 grid: 22 lines of 21 characters, so some lines are completely empty. The top line is `y = 21` and the bottom line is `y = 0`.)

</details>
