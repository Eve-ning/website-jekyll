---
layout: post
title:  "The Stepmania Format"
date:   2020-05-18 08:00:00 +0800
categories: vsrg
---

StepMania's file format is the most roundabout way to represent a note's timestamp

# Snapping

Unbeknownst to some players, stepmania cannot have millisecond snapping is because of the file format.

It uses snapping to determine where notes are. Take for example this:

```
// This is a measure
0001
0000
0000
1000
```

Simple enough, the beat would go like `X__X` in a measure.

---

```
// This is a measure
0001
0000
0010
0100
0000
1000
```

This goes `X_XX_X`

---

```
0000
0001
... [188 More 0000s]
1000
0000
```

It can go up to 192 divisions per measure, so each beat has 48 divisions.

Simple enough right? Here comes the sledgehammer

---

## Measure Resetting

If you map osu!, you'd notice that a new BPM resets the measure. 

However, in Stepmania, it **continues that measure**!

That's the largest difference, and it's a pain if you want to instantly reset the measure to snap the following notes.

---

Let's consider a 4/4 metronome where you want to transition
from 150 BPM to 300 BPM here.

The catch is that you want to transition on the 2nd beat of the 4/4 150 BPM metronome.

```
<osu!'s System>
Rhythms X   X   X   X   X   X X X X X X
150 BPM 1   2   3   4   1   2       |
300 BPM |               |   1 2 3 4 1 2
Measure |___.___.___.___|___|_._._._|_.
        1               2   3       4
```

*side note: if you're thinking to adjust the metronome, good luck finding out how to even do it*

---

Well, what happens is that the measure actually continues so you get

```
<StepMania's System>
Rhythms X   X   X   X   X   X X X X X X
150 BPM 1   2   3   4   1   2     |
300 BPM |               |   2 3 4 1 2 3
Measure |___.___.___.___|___._._._|_._.
        1               2   ^     3  
                          [2.25]
```

---

To fix this, you add another BPM point so that the measure is reset correctly

```
<StepMania's System>
Rhythms X   X   X   X   X   X X X X X X
150 BPM 1   2   3   4   1   |       |
600 BPM |               12341       |
300 BPM |               |   1 2 3 4 1 2
Measure |___.___.___.___|...|_._._._|_.
        1               2   3       4
```

Why

---

## Stops

This is another mystery, how do stops even work?

Consider this first, it's a pretty regular map

```
<StepMania's System>
Rhythms X   X   X   X   X       X       X
150 BPM 1   2   3   4   |       |       |
300 BPM |               1 2 3 4 1 2 3 4 1
Measure |___.___.___.___|_._._._|_._._._|
```

---

Let's say for some reason notes after 300 BPM is slightly delayed by let's say 1/4 of a beat.

```
<StepMania's System>
Rhythms X   X   X   X   |X      |X      |X
150 BPM 1   2   3   4   |       |       |
300 BPM |               1 2 3 4 1 2 3 4 1
Measure |___.___.___.___|_._._._|_._._._|
```

---

Well, now everything is misaligned.

Add another BPM on the 4th Beat of 150BPM? You could do that, or add a stop that is the seconds length of 1/4 of 150BPM.

```
<StepMania's System>
Rhythms X   X   X   X    X       X       X
150 BPM 1   2   3   4    |       |       |
300 BPM |                1 2 3 4 1 2 3 4 1
Measure |___.___.___.___X|_._._._|_._._._|
Stops                   ^ 
```

I hope you noticed I said **seconds length**. The format is a jumble of seconds, beats and measures.

Why

---

## Format Jumbling

Here's a sample:
```
#OFFSET:-0.046;
#BPMS:0.0=125.5,
4.0=150.0;
#STOPS:4.25=0.120;
//--------------- dance-single -  ----------------
#NOTES:
     dance-single:
     aaaa:
     Challenge:
     16:
     1,1,1,1,1:
1002
0000
0000
0000
1003
0000
0000
0000
,
1211
...
```


`#OFFSET:-0.046;` means the map starts 46ms earlier than the song, that's not uncommon.

`#BPMS:0.0=125.5,` means 125.5 BPM on `#OFFSET`

`4.0=150.0;` means 4 beats after 125.5 BPM, we have 150.0 BPM.

`#STOPS:263.354=0.120;` means 0.25 beats after 150.0 BPM, we stop the measure for 120ms

`// ...` this is a comment

`dance-single:` is the chart type, it basically says 4 key

`aaaa:` is the description

`Challenge:` is the chart difficulty name

`16:` is the chart difficulty value

`1,1,1,1,1:` is the groove radar values, don't ask me

`1002\n...\n0000\n,` denotes the first measure

`1311\n...` and so on is the rest of the chart

There's a lot more details, [found here](https://github.com/stepmania/stepmania/wiki/sm) on the official SM wiki.

Why

---

## Converting osu! to Stepmania

This is an article for next time, there's so many nuanced problems that you wouldn't believe how roundabout it is to convert from a unit to another.

Here's a small snippet

1. Read the osu file into ms
   1. Remember to split it into OsuHitObjects and OsuTimingPoints
   2. We'll call it Notes and BPMs for simplification
2. Firstly settle your BPMs by converting it into beats
   1. You'll create your own algorithm here
3. Now you'd have to align your BPMs because if you have an BPM that is not 4/4 synced to the previous, you'd run into snapping problems
   1. You cannot use Stops to align it, that requires the assumption that the stops do not overlap any notes
   2. Adding a **CorrectionBPM** 1 beat before the affected BPM works, calculate the value required to push the BPM to an integer measure
   3. If there's a BPM where the **CorrectionBPM** is supposed to be, amend that instead of creating it
      1. This should be adjustable by a factor
   
   Now that you have a working BPM List, we work on the Notes
4. Split the Notes into beats also, take note that we're using the amended BPM List with the CorrectionBPMs.
5. File the notes into the BPMs
   1. If the notes only require 4 slots to represent it, file it into a measure with 4 slots only. This calculation can be eased using a **Greatest Common Divisor** function.
   2. If the notes require less than 4, use 4 slots
   3. If the notes require 3, use 6 slots
6. Move over all the metadata
7. Great, you're done
8. Why
