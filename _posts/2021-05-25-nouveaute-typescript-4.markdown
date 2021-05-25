---
layout: post
title:  "Nouveauté de typescript 4"
date:   2020-12-20 11:39:03 +0100
author: Lucas ZIENTEK
categories: dev
tags: [dev, typescript]
---

## Tuples

### Le typage
Le spread de types avec des tableaux ou des tuples fonctionnent maintenant. On peut donc fusionner des tuples et avoir l'inference du type ou spread deux tableaux de types différents et la aussi avoir l'inference.

__Exemple:__ 
```ts
type ArrNum = number[];
type ArrStr = string[];
// (string | number)[]
type Spread = [...ArrNum, ...ArrStr];

type Strings = [string, string];
type Numbers = [number, number];
// [string, string, number, number]
type StrStrNumNum = [...Strings, ...Numbers];

// [string, string, ...Array<number | boolean>]
type Unbounded = [...Strings, ...ArrNum, boolean];
  
// [string, string, number, number, boolean]
type Unbounded2 = [...Strings, ...Numbers, boolean];
````

### Les tuples nommés

En Typescript 4 on peut mettre des labels sur les elements d'un tuple. Ca permet de facilité l'utilisation d'une fonction et améliorer la lecture du code.

```ts
function foo(x: [first: string, second: number]) {
  // a is string type
  // b is number type
  const [a, b] = x;
}
```

## Short-Circuiting Assignment Operators
En TS 3 on ne peut pas utiliser tout les operateurs "short", on peut utiliser `a *= 2` mais certains ne sont supporté que depuis TS4.
```ts
// Now supported in TS 4
a &&= b;
a ||= b;
a ??= c;
```

Cela peut sembler anecdotic mais maintenant on peut ecrire ça:

```ts
let values: string[];

(values ??= []).push("hello"); 
```

## @deprecated
On peut avec typescript 4 indiquer une classe une methode ou autre en depracated avec un petit retour sympa dans vscode.
```ts
/** @deprecated */
class DeprecatedClass {
  /** @deprecated */
  static sayHi() {
    console.log('hi');
  }
}
```

## Template Literal Types
La vrai nouvelle feature de typescript c'est les template literal Types, en gros on peut générer des types via des templates string.

```ts
type VerticalAlignment = "top" | "middle" | "bottom";
type HorizontalAlignment = "left" | "center" | "right";


function setAlignment(value: `${VerticalAlignment}-${HorizontalAlignment}`): void;

setAlignment("top-left");   // works!
setAlignment("top-middle"); // error!
```

La vous vous dites trucs de ouf mais en vrai pas temps que ça pas de `toUpperCase` ou `Capitalize` pour générer des fonction de type getUser en partant de user. Et bien si!
Les mecs de chez typescript on pensé a tout. Il existe les alias suivant: Uppercase, Lowercase, Capitalize et Uncapitalize. Avec ça les possible s'ouvre a nous.

On peut donc avoir un truc du style:
```ts
type CRUD = 'get' | 'create' | 'update' | 'delete'
type ServiceCRUD<K> = { [E in K as `${CRUD}${Capitalize<string & K>}`]: () => Array<string> };

const service: ServiceCRUD<'candidate'> = {
    createCandidate: () => [],      // works!
    getCandidate: () => [],         // works!
    updateCandidate: () => [],      // works!
    deleteCandidate: () => [],      // works!
    get: () => []                   // error!
};
```

