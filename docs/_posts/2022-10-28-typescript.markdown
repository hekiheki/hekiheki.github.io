---
layout: post
title:  "Type Challenges easy questions 中的知识点总结"
date:   2022-10-11 11:27:51 +0800
categories: TypeScript
---

[Type Challenges](https://github.com/type-challenges/type-challenges)

暂时只看了`Easy`的问题，主要用到了`extends`和`infer`两个关键字, 以及[Tyep Manipualtion](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html)中的一些遍历方法。

`extends` 不仅表示继承/拓展，还可以用来约束泛型。
`infer` 表示 `extends` 条件语句中待推断的变量。

1. `Pick`
```TypeScript
type MyPick<T, K extends keyof T> = {
  [Key in K]: T[Key]
}
```
2. `ReadOnly`
```TypeScript
type ReadOnly<T> = {
  readonly [Key in keyof T]: T[Key]
}
```
3. `Tuple to Object`
```TypeScript
type TupleToObject<T extends readonly any[]> = {
  [Key in T[number]]: Key
}
```
4. `First of Array`
```TypeScript
type First<T extends any[]> = T extends [] ? never: T[0]
//type First<T extends any[]> = T extends [infer F, , ...infer Rest] ? F : never
```
5. `Length of Tuple`
```TypeScript
type Length<T extends readonly any[]> = T['length']
```
6. `Exclude`
```TypeScript
type MyExclude<T, U> = T extends U ? never : T
```
7. `Awaited`
```TypeScript
type MyAwaited<T> = T extends Promise<infer R> ? MyAwaited<R> : T;
```
8. `If`
```TypeScript
type If<C, T, F> = C extends true ? T : F
```
9. `Concat`
```TypeScript
type Concat<T extends any[], U extends any[]> = [...T, ...U];
```
10. `Includes`
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
11. `Push`
```TypeScript
type Push<T extends any[], U> = [...T, U];
```
12. `Unshift`
```TypeScript
type Unshift<T extends any[], U> = [U, ...T];
```
13. `Parameters` 
```TypeScript
type MyParameters<T extends (...args: any[]) => any> = T extends (...args: infer Args) => any ? Args : never
```

