---
layout: post
title:  "Users First, You Second"
date:   2020-03-04 08:00:00 +0800
categories: design
---

In the past few years I've written posts/notices/programs for my interest groups.

It's always consistent, you care about who is using first.

<br>

## Rules of Thumb

- Assume Your Audience
- Inverted "News" Pyramid
- Do they care about $thing$? If not then don't include $thing$

<br>

# Summary

Everyone is lazy, put important stuff first.

Who do you expect to use it?

Is it self-explanatory?

**e.g.**

A how-to article? Step 1 within the first 2 paragraphs.

An extension for a game? It should consider all points of good UX

A new product? It better be easy to use

---
<br>
<center>Say you're making a save editor.</center>
<br>

## Assumptions

- plays the game
- knows game lingo
- knows how to use a simple program
- knows what data is manipulated
- knows how to save

You'd work with these assumptions **because you cannot please everyone.**

<br>

### Valid Assumptions
<center>
Downloaders $\leftrightarrow$ <b>Plays the game</b>
</center>
These assumptions, you are almost sure that they hold true. However, it's hard to avoid outliers.

<br>

### Invalid Assumptions
<center>
I have sharp eyes $\leftrightarrow$ <b><small>Font size 11</small> for</b> $aesthetics$
<br>
I have good English $\leftrightarrow$ <b>Simple words not needed</b>
</center>

These are assumptions that you shouldn't avoid, and instead, design to counter it.

<br>

### Unsure Assumptions
<center>
Downloaders $\leftrightarrow$ <b>Knows Game Lingo</b>
</center>

These are assumptions that you're not sure if is true. Depending on your choice, it can make the program less/more friendly.

<br>

## Interface / Inverted "News" Pyramid

Interface doesn't only apply to programs, even product design uses these ideas.

Use the "Inverted News Pyramid", it's a term in **journalism** to say.

<br>

<center><i>important first, less important later</i></center>

<br>

### The More Important Stuff

Referring to making a save editor.

You'd do a mock test of what would someone who use it go through.

1. Input
2. Change statistics
3. Output

Make these steps happen, then work on improvements.

<br>

### The Less Important Stuff

After a some time in production/testing, you'd find ways of improving. This is where the other less important stuff comes in.

1. Scalability
2. Aesthetics
3. Ease of Usage
4. Size of Program
5. etc.

<br>

## Do they care about $thing$

If not, don't add the $thing$

After some time, you'd get a lot of feature requests on $thing\,A\,B\,C\,D...$

<br>

### Adding too much $things$

Adding $things$ is good, but too much $things$ is bad. 

Sounds contradictory but sometimes it's better to not have all the $things$s

Here are some reasons:

- Scaling is harder if you have to consider more $things$
- Debug tracing is harder
- Fixing is harder as well
- Your main $thing$ might get lost in the midst of other $things$
- etc.

<br>

Remember to always balance features and usability.

No one carries a 100-tool swiss army knife, though it works on everything, guess why.