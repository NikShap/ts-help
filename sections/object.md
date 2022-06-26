[<- Назад](../README.md)

Оглавление
- [Objects](#objects)
- [Модификаторы](#модификаторы)
- [Сигнатура индексов](#сигнатура-индексов)
- [Mapped types](#mapped-types)
- [Наследование](#наследование)
# Objects

[**Link**](https://www.typescriptlang.org/play?#code/MYewdgzgLgBCBGArApsKBGGBeGBvAsAFAwkwAeAXDOgAwA0RpMAnlQEz1EC+RRoksBCjRtseRqQBOyACZUA5AGIAYsprqa8hsVIBzacjALF61eq0SS8ADYBXZMY01VFwlxgBDCDH7QA3LyEUMwADsgwAAogAJZgsDgEOpQwYLYAtvDIkgE6rCnpmdncgbFQWQBmHsDhAKoQWeI6JNFy+RlZOUxgHmkOMNCSsbqdpMhpHtHWVANDOTyEQA)
```
{
  ключ1: тип1;
  ключ2: тип2;
}
```
```ts 
const object1 = {
  x: 10,
  y: 20,
}
/*
{
    x: number;
    y: number;
}
*/

const object2 = {
    red: '#FF0000',
    green: '#00FF00',
    blue: '#0000FF',
  } as const;
/*
{
    readonly red: "#FF0000";
    readonly green: "#00FF00";
    readonly blue: "#0000FF";
}
*/

type Point = {
  x: number;
  y: number;
}

interface User {
    id: number;
    name: string;
    email: string;
}
```

# Модификаторы

Помимо типа, свойствам объекта можно задать модификаторы. Их два: 

- Опциональность `(?)`
- Доступность для записи `(readonly)`

[**Link**](https://www.typescriptlang.org/play?#code/C4TwDgpgBACg9gSwHbCgXigbwLACgpQBOEAhgCZxIA2IUCZAXFEgK4C2ARhIQNx4EAPJq07c++KCGHsuvflABeAfmmi5uAL548AY0oBnVGEQoAgk3jJUGHBIL0mARgA08wU4AMru5KYBaABZvLVw8YytTADoBdCgPcXCzSIVYgFYEk2Ao+liAJh4gA)

```ts
type Point = {
  readonly id: number;
  x: number;
  y: number;
  z?: number;
}

// {
//     readonly id: number;
//     x: number;
//     y: number;
//     z?: number | undefined;
// }

const pointA: Point = {
    id: 1,
    x: 10,
    y: -4,
}

pointA.x = 0;
pointA.z = 5;
pointA.id = 2; // Cannot assign to 'id' because it is a read-only property.
```

# Сигнатура индексов

[**Link**](https://www.typescriptlang.org/play?#code/JYOwLgpgTgZghgYwgAgKoGdrIN4FgBQyRywAJgFzIgCuAtgEbQDcBxVctEl6YUoA5i0LEItOMAA23XgKFsEAe2rgoATwD80viEEEAvgQIB6I8gDax022zJFy3qsoByOE+R6ANMktsctpSqOyE70bp7e+Ca+NnaBzqHuXj7W-vZqzghhSZFWxDEBDs6uiRFRKbGFwaRZyXmpccEJnj4AuoY5OLVEcJRm2OH9LR5dyPS9-V4TOHpDIwjjM8MdbBTmg0tRBvgEoJCwiCgA4lBKAA4QpBjQ6J3CZsdnWgItlFdQZi1CekA)

Если конкретные имена свойств неизвестны, то в ход идет `Index Signatures` или утилита `Record`

```ts
type GroupedUsers = {
  [Group: string]: User[];
}

type GroupedUsers2 = Record<string, User[]>
```

Стоит отметить, что сигнатуру индексов можно совмещать с обычными свойствами, но тип свойств должен быть сводимым к типу сигнатуры индексов. Как вариант с помощью `Union` 

```ts
type GroupedUsers = {
  [Group: string]: User[];
  ungrouped: User[];
  count: number; // Property 'count' of type 'number' is not assignable to 'string' index type 'User[]'.
}

type GroupedUsersFixed = {
  [Group: string]: User[] | number;
  ungrouped: User[];
  count: number;
}

```

Модификатор `readonly` работает и с сигнатурой
индексов

```ts
type GroupedUsers = {
  readonly [Group: string]: User[];
}
```

# Mapped types

[**Link**](https://www.typescriptlang.org/play?#code/C4TwDgpgBASg4gIQGoEMA2BXCBnKBeKAbQDsMBbAIwgCcAaKUym+xq6gXQG4BYAKD9CQoAZTQBLAMYQAJgGEA9mnnVcBAN58oUMNTFkU1EAC5YiVJhy1NUbBAnzi0gyAD8J+MnRZsV3loooEgDWAObU8hiObqaeFj58AL58fAD0KVAavGla2rr6hiYA5NQhFAAUAJSFvtlatvaOzkUl5VU16f6BoeGR0s2lldWp6Un8vILQCkoq+BnWhFPKUGLEUEEQIPIAZiLiUnKKytjsJtjAusQhPLwJQA)

Ингода вам необходимо описать тип объекта, ключи в котором базируются на ключах другого объекта. В таком случае используется перебор типа. 

Синтаксис схож с сигнатурой индексов, но используется ключевое слово `in`

```ts
type RGBArray = [number, number, number];

type SlicedColors = {
  primary: RGBArray,
  secondary?: RGBArray,
  background?: RGBArray,
}

type Colors = {
  [Color in keyof SlicedColors]: string;
}
// {
//     primary: string;
//     secondary?: string | undefined;
//     background?: string | undefined;
// }
```
Как вы можете заметить, при маппинге типа, модификаторы его свойств сохраняются. Для манипуляций с модификаторами используются символы `+` и `-`.

Знак `-` перед модификатором убирает его.

Знак `+` или отсутствие знака добавляет модификатор. 

```ts
type Colors = {
  primary: string,
  secondary?: string,
  background?: string,
}

type StrictColors = {
  [Color in keyof Colors]-?: string
}

// {
//     primary: string;
//     secondary: string;
//     background: string;
// }
```

Помимо этого можно модифицировать ключи используя `as`

```ts
type RGBArray = [number, number, number];

type SlicedColors = {
  primary: RGBValues,
  secondary?: RGBValues,
  background?: RGBValues,
}

type Colors = {
  [Color in keyof SlicedColors as `${Color}RGB`]: string;
}

// {
//     primaryRGB: string;
//     secondaryRGB: string;
//     backgroundRGB: string;
// }
```

# Наследование

Если типы нескольких объектов имеют ряд повторяющихся свойств возможно стоит применить наследование. 

- Для интерфейсов используется ключевое свойство `extends`.
- Для типов - пересечение типов **`&`** (Intersection Types)

[**Link**](https://www.typescriptlang.org/play?#code/JYOwLgpgTgZghgYwgAgJLmoswBuKDeAsAFDLLADOAIpXAEYA2EAJgPwBcydA9t03CADcJAL4kSYAJ4AHFAGF+Uek2QBeZEVLJuIBRDhQOyABQBKNQD5kObsGbDiY4iVCRYiFAGVgAL2UESMgpfCCMAcgoAWzDkAB9kMOi4hIYY+LCAD1TRcWJXaHgkNBBpAFcwAAUobmkKZAgMyBBmOvQ3LFwIABpkbz9GbuQ9A38NQOs4BlKITgowKFAAcwcyHTkACwFFmZMcSenZ+aXzVSsbOwcnCRkUdDLK6tqAJjVi9oRsPGQAMl7fUd+wyUAx+Yy0eymOzmCxAy3Ga02sJ2xghB2Q0OOlmstnsoiAA)

```ts
interface Interactive {
  isDisabled?: boolean;
}

type Clearable = {
  onClear?: () => void;
}

interface Sizable {
  size?: 'sm' | 'm' | 'l' | 'xl'
}

interface InputProps extends Interactive, Sizable, Clearable {
  value: string;
  onChange: (value: string) => void;
}

type InputProps2 = Interactive & Sizable & Clearable & {
  value: string;
  onChange: (value: string) => void;
}
```
Стоит отметить, что между `extends` и `&` есть разница. Они по разному разрешают конфликты.

[**Link**](https://www.typescriptlang.org/play?#code/JYOwLgpgTgZghgYwgAgJIDkCuBbARtAFQHsBlMKUAcwGEiQA3aSKZAbwFgAoZZBOxqGABcyABT04AG0wQRIHPigBKZAF4AfMgDO5KgG4uAXy5dQzeEjQAhYABNgUCAjDA6UshRCUseaLQZM0MgQAB6QILZaaD6KxB5U-gLMbFw8fAGCIuJSMiI6npQqGsjyvlAGnMacXGAAngAOKDGEpLpeiYFQBA0oqincvPxMWRLSsiUK0EWa+fpGJpx1jcgAChTYwC6McW00Q4JBfc1drQUdB109yABk-Wn7wmKjuci4RESSEHAgyAA+E5JJH9kJgIhAYKAILZptpdhUqlx0jpBhlmCI1sANlsIDszg9DnceCikiMcuNSopgW8Pl8fv95IDgaDbODIdC1JoOAMiUTHGBMFAfvEvNkxkpUkSqoYgA)
```ts
interface INumberToStringConverter {
  convert: (value: number) => string;
}

interface IBidirectionalStringNumberConverter extends INumberToStringConverter {
  convert: (value: string) => number;
}
/*
Interface 'IBidirectionalStringNumberConverter' incorrectly extends interface 'INumberToStringConverter'.
  Types of property 'convert' are incompatible.
    Type '(value: string) => number' is not assignable to type '(value: number) => string'.
      Types of parameters 'value' and 'value' are incompatible.
        Type 'number' is not assignable to type 'string'.
*/

type NumberToStringConverterType = {
  convert: (value: number) => string;
}

type PrimitiveToStringConverter = NumberToStringConverterType & {
  convert: (value: boolean | null | undefined) => string;
}

const converter: PrimitiveToStringConverter = {
    convert: (value: number | boolean | null | undefined) => {
        return String(value)
    }
}
```