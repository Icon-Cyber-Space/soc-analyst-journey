# OverTheWire Bandit - Levels 0-10

**Platform:** OverTheWire (SSH-based wargame)
**Environment:** WSL Ubuntu
**Date:** [today's date]

## What Bandit is
A set of Linux challenges where each level hides a password somewhere,
sometimes in a file, sometimes disguised, sometimes buried in binary
data. You find it, SSH into the next level, repeat.

## Level-by-level notes

### Level 0 to 1
First time actually using SSH for real, not just reading about it.
`ssh bandit0@bandit.labs.overthewire.org -p 2220`. Didn't think much of
the `-p 2220` at first, just copied it, then realised afterward that's
because the server intentionally doesn't use the default port 22.

### Level 1 to 2
This one actually stumped me for a bit. The file was just named `-`,
and every time I typed `cat -` nothing sensible happened, because the
terminal read it as a flag, not a filename. Had to look up why, turns
out you need `./` in front so the terminal knows it's a path. Small
thing, but it actually clicked once I understood *why*, not just that
it worked.

### Level 2 to 3
Filename had spaces in it: `--spaces in this filename--`. Figured out
pretty quickly I needed quotes around it, combined with the same `./`
trick from the last level.

### Level 3 to 4
Used `cd inhere`, then `ls -a` to find a hidden file (starts with a dot).
This one felt more familiar already, hidden files clicked fast.

### Level 4 to 5
Ten files, only one was actually readable text. Instead of opening each
one manually, used `file ./*` to check them all at once. First time a
wildcard (`*`) actually felt useful instead of just something I'd seen
in examples.

### Level 5 to 6
`find ./inhere -size 1033c` to search by file size instead of name.
Realised find isn't about names at all really, it's about properties,
size, owner, group. Different way of thinking about searching.

### Level 6 to 7
Searched the whole filesystem this time: `find / -user bandit7 -group
bandit6 -size 33c`. Got a wall of "Permission denied" messages and my
first instinct was that I'd done something wrong. Turns out that's
expected, you just don't have access to most of the system, and the
one file you need still shows up anyway.

### Level 7 to 8
`grep millionth data.txt` to find one specific line in a big file
instead of scrolling through it manually. Straightforward once I saw
it, but I wouldn't have thought to use grep for this without the hint.

### Level 8 to 9
`sort data.txt | uniq -u` to find the one line that only appears once.
This was the first time a pipe (`|`) actually made sense to me as
"take the output of one thing, feed it into the next," rather than
just syntax I was copying. Also learned uniq only catches duplicates
that are next to each other, so sorting first matters, wouldn't have
guessed that on my own.

### Level 9 to 10
`strings data.txt | grep =` to pull readable text out of a mostly
binary file. Kind of surprised binary files have any readable text
in them at all, assumed it'd be totally unreadable.

## What actually felt hard vs easy
- Filenames with dashes/spaces (levels 1-3) took the most trial and
  error
- Once I got past those, find/grep/pipes felt like less of a stretch,
  more like building on the same idea each time
- Honestly still not 100% confident I could explain grep flags from
  memory without checking notes

## What I'd do differently
Spent too long stuck on level 1 before actually searching for why `./`
was needed, next time I'll try `man` or `--help` first instead of
guessing longer than I should.
