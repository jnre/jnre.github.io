---
layout: post
title: to enum? or not
date: 2022-11-22 09:17 +0800
categories: [enums]
tags: [typescript]
---
recently i got into a strange issue when using enum, using an example of a card deck where i would want a deck to have certain suits as params

```typescript
export enum cardSuits {
  diamond,
  club,
  hearts,
  spades,
}

console.log(Object.keys(cardSuits))
```

this return us:

```typescript
["0", "1", "2", "3", "diamond", "club", "hearts", "spades"]
```

hmm thats strange, but i guess if we use string enums we can:

```typescript
export enum cardSuits {
  diamond = "diamond",
  club = "club",
  hearts = "hearts",
  spades = "spades",
}

console.log(Object.keys(cardSuits))
```

returned us as

```typescript
["diamond", "club", "hearts", "spades"]
```

great! but this isnt really recommended since if we want to use it in javascript we have to import the typings

```typescript

getSuits("diamond") //wont work

import {cardSuits} from "suits"
getSuits(cardSuits.Diamond)
```

the alternative is usually is to use type

```typescript
type cardSuits = "diamond" | "club" | "hearts" | "spades"
let suits: cardSuits = "diamond" //OK
```

but this doest allow us to iterate through the suits

```typescript
const suits = ["diamond", "club", "hearts", "spades"] as const
type cardSuits = typeof suits[number]
const suit4 = suits.map((v)=>"4"+v) //["4diamond", "4club", "4hearts", "4spades"]
```

but can we do better? what if there are point system involve for the suits?
(e.g us wanting to rank them in bridge order - clubs diamond hearts spade)

```typescript

const cardSuits = {
  club: 0,
  diamond: 1,
  hearts: 2,
  spade: 3 
} as const // prevent override!

type possibleSuits = keyof typeof cardSuits

function compareSuits(suit1: possibleSuits, suit2:possibleSuits){
  if(cardSuits[suit1] < cardSuits[suit2]){
    console.log(`${suit1} is smaller than ${suit2}`)
  }
  else{
    console.log(`${suit1} is bigger than ${suit2}`)
  }
}

compareSuits("club","hearts")

//convert to array to use iteration
const cardSuitsArray = Object.keys(cardSuits)
const suit3 = cardSuitsArray.map((v)=> "3"+v) //["3club", "3diamond", "3hearts", "3spade"]
```