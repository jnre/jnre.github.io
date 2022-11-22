---
layout: post
title: Types and Interfaces in typescript
date: 2022-11-15 10:59 +0800
categories: [typing]
tags: [typescript]
---

types and interfaces are ways to enforce javascript to adher to a certain specification for your code to allow the developer to know what params can be passed

both types and interfaces can almost do the same thing, but there are some minute differences

## narrowing the scope of variable

```typescript

const colors = ["black","brown"] as const

type Color = typeof colors[number]

let color : Color = "black"

interface ColorInterface {
    color: typeof colors[number]
}

let color2: ColorInterface = {
    color:"black"
}
```
in this case when we specify types of IColor, the values we can accept is only of the array const. this is helpful beause we can also use the array colors. Interfaces also cant be implemented on a value itself.

for object
```typescript
export const programModeEnumMap = {
  GROUP: "group",
  ANNOUNCEMENT: "announcement",
  ONE_ON_ONE: "1on1",
  SELF_DIRECTED: "selfDirected",
  PLANNED_ONE_ON_ONE: "planned1on1",
  PLANNED_SELF_DIRECTED: "plannedSelfDirected",
} as const

export type ProgramMap = typeof programModeEnumMap;

export type ProgramKeys = keyof ProgramMap; 
export type SubSetProgram = Exclude<ProgramKeys,"ANNOUNCEMENT">
//"GROUP" | "ONE_ON_ONE" | "SELF_DIRECTED" | "PLANNED_ONE_ON_ONE" | "PLANNED_SELF_DIRECTED"

export type Program = ProgramMap[keyof ProgramMap];
export type IndividualProgram = ProgramMap[keyof Omit<ProgramMap,"GROUP">];
//"announcement" | "1on1" | "selfDirected" | "planned1on1" | "plannedSelfDirected"
```

## extending

interfaces allow you to redeclare the same interface to extend its typing, this is not the case for types

```typescript
interface Kitten {
  purrs: boolean;
}

interface Kitten {
  colour: string;
}

//types cant do type = Kitten {} twice
```

## working with classes

types cannot implement union types on classes

# Additional useful operations

## using typeof

typeof allows you to infer what is the type of mentioned variable

## keyof

often used together with Property, retrieve the key properties of a type, a really good example is as shown

```typescript
type Getters<Type> = {
    [Property in keyof Type as `get${Capitalize<string & Property>}`]: () => Type[Property]
};
 
interface Person {
    name: string;
    age: number;
    location: string;
}
 
type LazyPerson = Getters<Person>;
```

it extracts the types of Person and does a templating on it, type casting it with `as` 





