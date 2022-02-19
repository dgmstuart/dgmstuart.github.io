---
layout: post
title: "\"Somebody is upside down!\": building a bingo app in React"
date: 2022-02-18 12:00:00 +0100
comments: true
categories: [reactjs]
tags: [reactjs, react]
---

TL;DR: I built a small React app which is a bingo card with
[Wordle](https://www.nytimes.com/games/wordle)-style shareable emoji grids.
You can find it [here](https://dgmstuart.github.io/bingo-frontend/), or view
the [source code](https://github.com/dgmstuart/bingo-frontend).

## "Everyone shouts at the same time": Starting a new social game

Just before the Pandemic started I came up with a concept for a fun activity
at the friday night social at [WCJ](https://wcj.se/), the swing dance
association here in Gothenburg. We would watch a bunch of
[Lindy Hop](https://en.wikipedia.org/wiki/Lindy_Hop)
routines (specifically team performances) and play bingo with the things we
saw.

I used the
[bingo card generator at osric.com](https://osric.com/bingo-card-generator/?)
to print out way too many cards with commonly seen moves and formations, and
also some silly things like brightly coloured trousers.

It was fun: we had some beers, I gave out sweets as prizes. We quickly
realised that yelling stuff out was the most fun part.

## "Syncopated clapping": Taking it online

Then we weren't able to meet up again for some time, but it had been fun
running the game so I started working to make
[Team Lindy Bingo Online](https://www.facebook.com/teamlindybingo)
a reality.

I built up a
[huge YouTube playlist](https://www.youtube.com/playlist?list=PLgsIo5h4KQoYzMCvcuBFTy7-qW8VgsFpT)
of notable competitions and performances, refined after each game.

I found a couple of services with voice chat where you could all watch the
same video at the same time (all now closed down - presumably due to
unsustainable pandemic usage levels).

These services were awkward: they didn't work on everyone's computer, and they
didn't filter out the video audio from people's voice channels. But it kind of
worked: it was fine to have the video volume way down low. Yelling stuff out
was the most fun part.

I shared out links like [bit.ly/teamlindybingocard3](https://bit.ly/teamlindybingocard3).
The card generator was mostly designed to generate cards to print out, so the
card is at the bottom of a lot of UI , but it kind of worked (note the anchor
tag at the end of the URL which mostly scrolls you to where the card is).

While waiting for people to arrive and sort their sound out, I played a
[10-hour loop of the Price Is Right theme tune](https://www.youtube.com/watch?v=XlUjryOP3LE)
to set that gameshow mood, then (as a matter of tradition) I played this Wild
short showcase by a Lithuanian group to kick things off:

<iframe width="560" height="315"
src="https://www.youtube.com/embed/JiEF4nkjeP8" title="YouTube video player"
frameborder="0" allow="accelerometer; autoplay; clipboard-write;
encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

We played pretty much every week for a few months during that first winter and
into the spring. There were never a lot of people in the chat, but dancers
dropped in from all over the world and I had a small group of regulars.

We came to dread getting a card with "Brightly coloured trousers".

We shouted at our screens a lot.

We debated what "Wedge formation" actually means _(it's a filled-in
triangle, not a V formation. Don't @ me)_.

We watched a lot of great dancing.

## "Itches": The old card

The osric.com bingo cards looked like this:

!["An osric.com bingo card"](/images/content/osric_bingo_card.png)

Eventually I tracked down the developer and got a
[Pull Request](https://github.com/cherdt/BingoCardGenerator/pull/27)
accepted to add a flag which would hide the rest of the UI so that you can
only see the card. This made the experience pretty reasonable on mobile, so
you could have the bingo card on your phone while watching the videos on a
bigger screen.

The word list was stored in URL query parameters, which came with a couple of
challenges:

Firstly, In order to be able to share sane URLs I used the
[Bit.ly](https://bit.ly) URL shortener, and I needed to create a new Bit.ly
link every time I updated the word list, since on a free account, Bit.ly links
are not editable. I ended up editing the word list after most games as I
learned what wasn't working and what was missing.

Secondly I fairly quickly hit the 2000 character limit of the length of URL
that Bit.ly will let you shorten. I'm guessing this limit is partly
historical: most browsers have extremely large URL limits (tens of thousands
of characters), but Microsoft browsers used to support a max length of 2048.
In the end adding a new word meant removing an old one.

Finally, the word list needed to be formatted as a continuous comma-separated
string in order to be used with the bingo card generator, which was a bit
awkward to manage.

## "Slow motion": Winding down

Running the game every week was a much needed injection of undemanding social
interaction in an otherwise pretty lonely time. But after a few months,
interest started to tail off: people got 'zoom fatigue' or just struggled to
enjoy watching an activity they weren't currently able to do themselves.

So after 12 games, Team Lindy Bingo Online went on indefinite Hiatus, only
coming back for a one-off game at the online christmas party in 2021 (during
which I realised far too late that the new video-watching system I was trying
out supported a maximum of 8 people with voice chat 🤦).

But now dancing is starting up again, and I'll be able to run a game
in-person soon, armed with the learnings of running it online so many times.

## "Square Formation": A new card

I worked on a
[Next.js](https://nextjs.org/)
project at work before the summer and really enjoyed working with
[Typescript](https://www.typescriptlang.org/) and
modern [React](https://reactjs.org/), with functional components and hooks.
I appreciated the clarity of working with immutable state, and the way that
the compiler points out mistakes before I notice them.

So I thought I'd have a crack at building a new bingo card in plain React with
Typescript. After a false start, I initialised an application with:

{% highlight shell %}
npx create-react-app my-app --template typescript
{% endhighlight %}

...and remarkably quickly I had an grid of randomised words from a
[JSON word list](https://github.com/dgmstuart/bingo-frontend/blob/main/src/wordList.json),
where clicking a cell changed its colour:

{% highlight tsx %}
import React, { useState } from 'react';
import classNames from 'classnames';

const Cell: React.FC<{ word: string }> = ({ word }) => {
  const [isStamped, setStamped] = useState(false);

  const toggleStamped = () => {
    setStamped(!isStamped)
  };

  var classes = classNames(
    "Cell",
    { "stamped": isStamped }
  )

  return(
    <td
      className={classes}
      onClick={toggleStamped}
    >
      {word}
    </td>
  )
}

export default Cell;
{% endhighlight %}

### "The Snatch (aerial)": The problem of state

Then I ran into the problem that I understand all React developers run into
sooner or later: managing state.

In this implementation, each cell is managing its own state with `useState`.
But now I wanted to add a button to reset the card by clearing all the stamps.

My Object Oriented brain wanted to approach this by sending a message to all
the cells to tell them to reset their state. But that's not how things work in
React: parent components send _props_ down to child components, and child
components fire _events_ which can be intercepted by their parents.

`reset!` is an event-shaped concept: if I send a prop down to a component like
`reset: true`, then when I try to reset the component again, it won't
re-render because the value of `reset` has not changed.

### "Mid-routine song change": An elegant solution?

Thankfully I have some helpful and experienced friends and colleagues:
[@Burgestrand](https://github.com/Burgestrand) and
[@PooSham](https://github.com/PooSham) pointed me in the right direction.

The solution I landed on was to have the `App` component manage the state with
`useState`, and instead of passing down an array of words to the `Grid`
component, I send down an array of objects which contains both the word, its
`stamped` state, and a toggle function which flips the `stamped` state and
updates the whole system state to reflect that change.

I won't show
[all the code which makes that work](https://github.com/dgmstuart/bingo-frontend/tree/main/src)
here, but here's a simplified version of the type definition of the elements
of that array:

{% highlight ts %}
type CellProps = { word: string, stamped: boolean, toggleStamped: () => void }
{% endhighlight %}

I'm pretty pleased with the result - I was worried that the code was going to
be either hacky or hard to wrap my head around, but it's turned out to be
quite elegant. Here's what the new card looks like:

!["A Team Lindy Bingo card"](/images/content/team_lindy_bingo_card.png)

You can have a play for yourself at
[dgmstuart.github.io/bingo-frontend](https://dgmstuart.github.io/bingo-frontend/)
(best viewed on a phone).

## "A hat": polishing and bikeshedding

I've since done a bunch of unnecessary polishing and
[bikeshedding](https://en.wiktionary.org/wiki/bikeshedding).

- Passed all the hurdles to make it a [Progressive Web App (PWA)](https://web.dev/progressive-web-apps/),
  So it's installable on phones and in some browsers.
- Added icons of different sizes (plus a favicon), so it looks nice if eg. you
  save it to your home screen on your phone.
- Test-drove all the code with unit tests written in
  [Jest](https://jestjs.io/).
- Encapsulated access to `localStorage` behind
  [a class to handle JSON data](https://github.com/dgmstuart/bingo-frontend/blob/main/src/lib/JsonSession.ts)
  and [a class to handle missing data](https://github.com/dgmstuart/bingo-frontend/blob/main/src/lib/GuaranteedJsonSession.ts).
- Allowed [Prettier](https://prettier.io/) to apply its strong opinons on how
  code should be formatted.
- Configured a bunch of [ESlint](https://eslint.org/) rules.

All unnecessary for sure, but apart from being a learning opportunity it makes me
happy, and that's part of what side projects are for.

But by far the most unnecessary feature I've implemented (and one of the most
fun) is shareable emoji grids.

## "Shimmy": Shareable emoji grids
At the time of writing, [Wordle](https://www.nytimes.com/games/wordle) is a
big deal: it's a simple word game that releases one puzzle every day, and it
just got acquired by the New York Times.

Its killer feature is that you can share an abstract image of the result of
your game, but rather than messing about with images, the image is made up of
emoji, which are just text so can be pasted pretty much anywhere. It's kind
of genius. I think they look especially great in Discord:

!["A Wordle emoji grid"](/images/content/wordle_emoji_grid.png)

So of course I had to implement a similar share button to output an emojified
bingo card:

!["A Team Lindy Bingo emoji grid"](/images/content/team_lindy_bingo_emoji_grid.png)

Building the actual grid is pretty simple: it's just `map`ping over the array
of `CellData` state.
[Testing it](https://github.com/dgmstuart/bingo-frontend/blob/6293523c7c446c56ba89025974c43fe6d23678e1/src/App.test.tsx#L55-L88)
was a bit more challenging, partly because that involves interacting with the
clipboard, but maybe that's a post for another day.

In the meantime I'm looking forward to being in a room with other dancers and
shouting "Somebody is on the floor!" some evening soon.
