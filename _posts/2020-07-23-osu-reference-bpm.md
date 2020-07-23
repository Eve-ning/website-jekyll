---
layout: post
title:  "osu! reference BPM"
date:   2020-07-23 08:00:00 +0800
categories: vsrg
---

In this article, we'll discuss how osu! calculates the reference BPM for any chart.

# Experimentations

osu! seems to be looking for the reference bpm using a naive approach, where it assumes timing points are sorted

Consider the following

```
OFFSET BPM
0      200 <Start of Map>
100    300
200    150
300    600
5000    -  <End of Map>
```

osu! will display 150 - 600 (600) where 600 is the reference BPM.

Let's try with this instead

```
OFFSET BPM
0      200 <Start of Map>
100    300
300    600
200    150
5000    -  <End of Map>
```

Here, it is 150 - 300 (150).

And another example

```
OFFSET BPM
0      200 <Start of Map>
300    600
200    150
100    300
5000    -  <End of Map>
```

Here, it is 200 - 300 (300).

# Hypothesis

It is obviously suffering from a naive assumption that all timing points are sorted.

I'm guessing that this is the algorithm used.

## Example 1

Consider this example

```
OFFSET BPM
0      200 <Start of Map>
300    600
200    150
100    300
5000    -  <End of Map>
```

osu! stores these information without sorting, in a stack.

```
bpms: [0ms, 200bpm]
      [300ms, 600bpm]
      [200ms, 150bpm]
      [100ms, 300bpm]
lastOffset: 5000ms
```

It then calculates how long a bpm lasts, the differences

```
diffs = differences(bpms)
assert(diffs == [300ms, 200bpm]
                [-100ms, 600bpm]
                [-100ms, 150bpm]
                [4900ms, 300bpm])
```

The largest difference will be the reference bpm

```
refBpm = max(diffs.offsets()).bpm()
assert(refBpm == 300bpm)
```

I'm not too sure how the range is determined but I'm not too concerened about it.

## Example 2

```
OFFSET BPM
0      200 <Start of Map>
100    300
300    600
200    150
5000    -  <End of Map>

bpms = [0ms, 200bpm]
       [100ms, 300bpm]
       [300ms, 600bpm]
       [200ms, 150bpm]

lastOffset = 5000ms

diffs = differences(bpms)
assert(diffs == [100ms, 200bpm]
                [200ms, 300bpm]
                [-100ms, 600bpm]
                [4800ms, 150bpm])

refBpm = max(diffs.offsets()).bpm()
assert(refBpm == 150bpm)
```

Result: 150 - 300 (150)

## Example 3

```
OFFSET BPM
0      200 <Start of Map>
300    600
200    150
100    300
400    100
5000    -  <End of Map>

bpms: [0ms, 200bpm]
      [300ms, 600bpm]
      [200ms, 150bpm]
      [100ms, 300bpm]
      [400ms, 100bpm]

lastOffset: 5000ms

diffs = differences(bpms)
assert(diffs == [300ms, 200bpm]
                [-100ms, 600bpm]
                [-100ms, 150bpm]
                [300ms, 300bpm]
                [4600ms, 100bpm])

refBpm = max(diffs.offsets()).bpm()
assert(refBpm == 600bpm)
```

# Usages

Let's say you have a map that you've planned on reference bpm **200**, but after some time, it changes to **300** or **100**. It's a pain to adjust all SVs to fix it.

What you can do is manipulate osu into using a specific bpm by adding a **0 offset BPM Line**.

If you already have BPMs that are negative, it shouldn't matter, however, they will affect the range displayed.

Take for example this map
```
...
//Storyboard Layer 4 (Overlay)
//Storyboard Sound Samples

[TimingPoints]
-23,240,4,2,0,50,1,0
6697,240,4,2,0,50,1,0
15817,240,4,2,0,50,1,0
17737,-166.666666666667,4,2,0,50,0,0
18457,240,4,2,0,25,1,0

...

174702,240,4,2,0,50,1,0
202782,240,4,2,0,50,1,0
240222,428.571428571429,4,2,0,50,1,0
281364,480,4,2,0,50,1,0
329364,240,4,2,0,50,1,0

[HitObjects]
448,192,17737,5,0,0:0:0:0:
```

Its reference is **250**, let's say you want to change it to **600**.

You have to add a **0 offset BPM line** at the end in the .osu **MANUALLY**.

```
...
...

174702,240,4,2,0,50,1,0
202782,240,4,2,0,50,1,0
240222,428.571428571429,4,2,0,50,1,0
281364,480,4,2,0,50,1,0
329364,240,4,2,0,50,1,0
0,100,4,2,0,50,1,0 < ------- HERE

[HitObjects]
448,192,17737,5,0,0:0:0:0:
```

This adds a **0 Offset**, **600 Bpm** line at the very start of the map.

Reminder: **60,000 / 600 = 100**. That's how you get the second value in the line. **60,000** is a fixed value. 100 just represents the ms between beats.

This will change the reference bpm to **600** as expected.

# Caveats

There are 2 problems:

1. If you save the map, it'll sort itself and it won't work anymore.
2. If you upload the map, it'll save itself (back to 1.)

The solution to (1.) is to just not save or keep moving it back.

The solution to (2.) is to save the map before clicking the final upload button.
