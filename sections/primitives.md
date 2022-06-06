# Примитивы

- [Примитивы](#примитивы)
  - [Null и Undefined](#null-и-undefined)
  - [Number](#number)
  - [Boolean](#boolean)
  - [String](#string)
    - [Template Literal](#template-literal)

## Null и Undefined

Если в tsconfig.json файле включён strictNullChecks, то необходимо явно указывать что сущность может быть null или undefined. 

```json
{
  "compilerOptions": {
    "strictNullChecks": true
  }
}
```

Рекомендуется работать с включенным, так как это позволяет отловить ошибки.

[**Link**](https://www.typescriptlang.org/play?#code/C4TwDgpgBAcghgW2gXigZ2AJwJYDsDmA3ALABQZANhMFLohDAK4UUBcs9UquzFJpVGnSQBVXABMIAMzwRx7eEi5RGE6bPGEgA) 

```ts
type Name = string;
let nameNull: Name = null; // Type 'null' is not assignable to type 'string'
let nameUndefined: Name = undefined; // Type 'undefined' is not assignable to type 'string'.

type NameOrNull = string | null | undefined;
let nameNull1: NameOrNull = null;
let nameUndefined2: NameOrNull = undefined;
```

----------------------------------------

## Number

[**Link**](https://www.typescriptlang.org/play?#code/MYewdgzgLgBGCuBbARgUwE4EYYF4aYAYBuAWAChRJZkQQAbVAQzGzynXlVIvGhmnQBLMAHNWMAOQAhEMgndyDWAhQYATLnzFFqarQbMNeAGaM6ELjtgDhIo5ICCdQcFTzy5SnxVp0AZgAuOCRfTUJtHioYGnomMEDo-TjNdk5uL2t2WwSbUU1pWXcyK2DVdAAWIIBWTSruJUTY5kqYU3NUTTaLet1+LNEWiScXN3zh1yKSnwwavBrGCBgMnr0msFnWswsYBaXeKBW+oVENoecJncXloA)

  ```ts 
  const number1 = 10; // 10
  let number2 = 10; // number

  const number3: number = 100; // number
  let number4: 5 = 5; // 5

  let number5 = 5 as const // 5;
  ```

----------------------------------------

## Boolean

[**Link**](https://www.typescriptlang.org/play?#code/MYewdgzgLgBGCuBbARgUwE4EYYF4aYAYBuAWAChRJZkQQAbVAQzGzynXlVIvGhmnQBLMAHNWMAOQAhEMgndyDWAhQYATLnzFFqarQbMNeAGaM6ELjtgDhIo5ICCdQcFTzy5SnxVp0AZgAuOCRfTUJtHioYGnomMEDo-TjNdk5uL2t2WwSbUU1pWXcyK2DVdAAWIIBWTSruJUTY5kqYU3NUTTaLet1+LNEWiScXN3zh1yKSnwwavBrGCBgMnr0msFnWswsYBaXeKBW+oVENoecJncXloA)

```ts 
const boolean1 = true; // true
let boolean2 = false; // boolean

const boolean3: boolean = true; // boolean
let boolean4: false = false; // false

let boolean5 = false as const // false;
```

----------------------------------------

## String

[**Link**](https://www.typescriptlang.org/play?#code/MYewdgzgLgBGCuBbARgUwE4EYYF4aYAYBuAWAChRJZkQQAbVAQzGzynXlVIvGhmnQBLMAHNWMAOQAhEMgndyDWAhQYATLnzFFqarQbMNeAGaM6ELjtgDhIo5ICCdQcFTzy5SnxVp0AZgAuOCRfTUJtHioYGnomMEDo-TjNdk5uL2t2WwSbUU1pWXcyK2DVdAAWIIBWTSruJUTY5kqYU3NUTTaLet1+LNEWiScXN3zh1yKSnwwavBrGCBgMnr0msFnWswsYBaXeKBW+oVENoecJncXloA)

```ts 
const string1 = 'Bob'; // 'Bob'
let string2 = 'Alice'; // string

const string3: string = 'Bob'; // string
let string4: 'Alice' = 'Alice'; // 'Alice'

let string5 = 'Alice' as const; // 'Alice'
```

### Template Literal

Более гибкий вариант задания строковых типов

[**Link**](https://www.typescriptlang.org/play?#code/C4TwDgpgBAshwAsD2ATKBeKByA4gUQBUsoAfbABQHkBlI07AETwBlC8sBuAWAChRIoAVQDOEAE5xEqDFAAGAEgDek5CgC+AfUHU8AJVncevftDwA7YAEtQMrADk8AdWrEyWbXs7Hw0GEjEQkqjCMgrK8KqaSuZWoGoGvN4CMACGYgDWoakZALRKZgCuALYARuLxvADGSGbCwFB1YpZmAOYAbABcsGmZmFjZ6TkAjFiJfD5Q1MBNrQBCSEgANhApZqFKJQvLqxXjAlMzLY7WCOQBAGaWAB6hANpgF9cAukqNzS27QA)

```ts
type Method = 'GET' | 'POST' | 'DELETE';
type UserMethod = `${Method}_USER`;

type Entity = 'NEWS' | 'USER';
type MoreMetods = `${Method}_${Entity}`;

type Mark = `Mark-${number}`
const string6: Mark = 'Mark-1'

type StringBoolean = `${boolean}`
type StringWithPrefix = `[prefix]${string}`
```