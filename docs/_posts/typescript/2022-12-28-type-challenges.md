---
layout: post
title:  "Type Challenges easy questions 中的知识点总结"
categories: TypeScript
---

> [Type Challenges](https://github.com/type-challenges/type-challenges)

暂时只看了 `Easy` 的问题，主要用到了 `extends` 和 `infer` 两个关键字, 以及[Tyep Manipualtion](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html)中的一些遍历方法。

`extends` 不仅表示继承/拓展，还可以用来约束泛型。
`infer` 表示 `extends` 条件语句中待推断的变量。

### Pick

```TypeScript
type MyPick<T, K extends keyof T> = {
  [Key in K]: T[Key]
}
```

### ReadOnly

```TypeScript
type ReadOnly<T> = {
  readonly [Key in keyof T]: T[Key]
}
```

### Tuple to Object

```TypeScript
type TupleToObject<T extends readonly any[]> = {
  [Key in T[number]]: Key
}
```

### First of Array

```TypeScript
type First<T extends any[]> = T extends [] ? never: T[0]
//type First<T extends any[]> = T extends [infer F, , ...infer Rest] ? F : never
```

### Length of Tuple

```TypeScript
type Length<T extends readonly any[]> = T['length']
```

### Exclude

```TypeScript
type MyExclude<T, U> = T extends U ? never : T
```

### Awaited

```TypeScript
type MyAwaited<T> = T extends Promise<infer R> ? MyAwaited<R> : T;
```

### If

```TypeScript
type If<C, T, F> = C extends true ? T : F
```

### Concat

```TypeScript
type Concat<T extends any[], U extends any[]> = [...T, ...U];
```

### Includes

```TypeScript
type Includes<T extends readonly any[], U> = T extends []
  ? false
  : T extends [infer Head, ...infer Tail]
  // Head extends U 无法识别一些特殊情况
  ? (<T>() => T extends Head ? 1 : 0) extends <T>() => T extends U ? 1 : 0
    ? true
    : Includes<Tail, U>
  : never
```

### Push

```TypeScript
type Push<T extends any[], U> = [...T, U];
```

### Unshift

```TypeScript
type Unshift<T extends any[], U> = [U, ...T];
```

### Parameters

```TypeScript
type MyParameters<T extends (...args: any[]) => any> = T extends (...args: infer Args) => any ? Args : never
```
