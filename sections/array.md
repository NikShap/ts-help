[<- Назад](../README.md)

Оглавление
- [Array](#array)
- [Tuple](#tuple)
# Array


```
тип[]
Array<тип>
```

[**Link**](https://www.typescriptlang.org/play?#code/MYewdgzgLgBAhgJwXAngRhgXhgbTQGhgCZCBmAXQG4BYAKABsBTWRZFIrXA4squuqCgAOjGADkArgFsARowQQAgklQZsACmgIAlmADmMAD4ww0uQgCUOPrUEjxZ+UpUpSnZWwA8GYx2OkAPhpaIA)

```ts 
const array1 = [1, 2, 3]; // number[]
let array2 = [1, 2, 3]; // number[]
const array3 = [0, 5, 10, 15] as const; // [0, 5, 10, 15]

type NumbersArray1 = (string | number)[];
type NumbersArray2 = readonly (1| 2 | 3)[];
type NumbersArray3 = Array<1 | 2 | 3>;
type NumbersArray4 = ReadonlyArray<boolean>;

```

# Tuple

```
[тип1, тип2, тип3, тип4]
```

Tuple - подвид массива, сохраняющий порядок типов у элементов.

[**Link**](https://www.typescriptlang.org/play?#code/C4TwDgpgBAKgrmANhAggJzQQxAOTgWwCMI0BnKAXigG0A7A4tAGinqJIF0BuAWACh+AYwD2tUsCiYM2AIwAuWAmTosuBiXJVqMgAwtdLAOSAhEEACIIFYQY4A4QQJwgUQNwgVwBIggXhBbh7lCGjxk6SAAmBXgkVH88djJKGgMoGQBWbn4gA)
```ts
type TupleArrayNumbers = [number, number];

const array4: TupleArrayNumbers = [10, 10, 'третий лишний']; 
// Type '[number, number, string]' is not assignable to type 'TupleArrayNumbers'. Source has 3 element(s) but target allows only 2.
const array5: TupleArrayNumbers = [10, 15];
```

Если элемент опционален поставьте `?` после типа. Опциональные элементы не могут идти перед обязательными.

```ts
type PositionArray = [number, number, number?] // [number, number, (number | undefined)?]
```

Typescript позволяет использовать `rest` элементы

```ts
type NamedPolygon [string, ...PositionArray[]];

const Triangle: NamedPolygon = ['Triangle', [1, 1], [2,3], [0, 3]];
```