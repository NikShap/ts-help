[<- Назад](../README.md)

Оглавление
- [Function](#function)
- [Function Declaration](#function-declaration)
- [Function Expression](#function-expression)
- [Типизация возвращаемого значения](#типизация-возвращаемого-значения)
- [Типизация в отрыве от объявления](#типизация-в-отрыве-от-объявления)
- [Колбэки](#колбэки)
- [Допущения при проверке типов](#допущения-при-проверке-типов)
- [Перегрузка функций (Function Overloads)](#перегрузка-функций-function-overloads)
- [Rest параметры/аргументы](#rest-параметрыаргументы)

# Function

```
(a: тип_1, b: тип_2): возвращаемый_тип
(a: тип_1, b: тип_2) => возвращаемый_тип
```

[**Link**](https://www.typescriptlang.org/play?#code/PTAEDEFcDsGMBcCWB7aoAiBTWAbAhgE55KoCwAUBQGYwIpo1xa6HH0AUAHgFyjSQBbAEaYCAGlABPXv2GiJALxmCRBUAF5QABgkATTAGdYBRAAcS0APy8D8E9ADmASlABvCqFAFM8SATTu5J6enGIewZJhQcEKUcGg+kYm5vRxoAC+FJmU5CAQtBagAKKcpt4GBvQUFLCotqCMsCVlhpWoGqDs0HgCmDZ2iI4SeA59fCqiLuoAfKAABgAkrt296Z1LI5jpTnMA3NW5YABKPn5oACqSppgHtdD1dwBuovDnyAByE2qa7AbIvfAABaDBy8GAAa2gyAA7tAnMo5N9Zp9Eb9-j5gY4nPsco1Ck8Xm8AMoDRxogGY0GgCFQ2Hw0C2ewONzhby+fygElM8kYkFOLIHPKAIhBABwggH4QEWAdhBAAwggDEQEWAeRBQIAmEFAgD4QQBCIIABEEA0iDKwCsIOqNerAIwggCkQBXKwDcIPrALwgioO8CumFAAGFUM8CK9kFyQR0eUCQWDoJCYXCNLNGSCcRRBvBRFQ8LAXe7oJ7vSjVCzogHKcHQ3SEaocekgA)
# Function Declaration

```ts 
function func1(x: number, y: number, z: number = 0, description?: string) {
  return {
    x,
    y,
    z,
    description,
  }
}
```

# Function Expression

```ts
const func2 = (name: string, age: number) => `${name} (${age})`;
```

# Типизация возвращаемого значения

```ts
const convertToNumber = (something: unknown): number => Number(something);

function convertToString(something: unknown): string {
  return String(something)
}
```

# Типизация в отрыве от объявления

```ts
type ConvertToString = (something: unknown) => string;

interface ConvertToNumber {
  (something: unknown): number
}
```

# Колбэки

[**Link**](https://www.typescriptlang.org/play?#code/MYewdgzgLgBNBOBLMBzCBBe8CGBPGAvDANoCMANDAEyUDMlALALoB0AttgA4AU3iUAUzYBKQgD4YAbwCwAKBgx4AqAFd4YGAGUoSVH0Ei5AX2EBuORdnEA5KWuUaMa4CEQQAIggDhB7MZiyUATFWABbjkFbmxgYAAuGDAVNgAjAXhiJkp+IVECCRl5BRhQSFg4xOSASQNCGAA5eKT4fUzzPIVEADMYbgBCRAhq7GruEvqKzNElVXUSFhmI4Eph8oMmZvzFZTUNOdWYI3JQkiYYbAhYuuTUuTNLORs7B0oXDy8ff0CBAB5FlKYxELzwpF0gYsjkDgpCtAzqV4KM2FVajDGiIdq0Ot1ev1Bt84cJxhspsQZiw5gtzrDlqj1pMtpEdnsDpdZMIgA)

```ts
// Часто типизируются автоматически по контексту
const stringsArray = [1, 2, 3, 4].map((item) => {
  return convertToString(item)
});

// если тайпскрипт не может, то указываем явно
['1', 2, 'три', 4].reduce(
  (acc: number[], item) => {
    const numberItem = Number(item);
    if (!isNaN(numberItem)) return [...acc, numberItem];
    return acc;
  },
  [] as number[]
);

// если возможно то через дженерик
['1', 2, 'три', 4].reduce<number[]>(
  (acc, item) => {
    const numberItem = Number(item);
    if (!isNaN(numberItem)) return [...acc, numberItem];
    return acc;
  },
  []
)
```

# Допущения при проверке типов

[**Link**](https://www.typescriptlang.org/play?#code/MYewdgzgLgBNBOBLMBzCMC8MDaByAQiAEa4A0MuAggDaLACmZFAUiABZhNVhgCGuAXQDcAWABQoSLF7x4vAJ7os2bAEZyAJnIBmAeWwa9OI9gAM5czEvWBw8eMnQYKelAAy9VFDaYYACnpqegBbTygALhgAbxggrzZIsABXYKJ6eBgAXwBKTAA+GECQsIA6OJRvUTEHcCcEZDQPeN961AgS4N4ABz8Xd08Ktmyqx2lZBQgmwd8ZOUUO7t7XKe9h+2qxKHku+hgAYSIAMSSwYF8-XkSUtPhyIivU9PJgB5vyABNX9NyMAoA3ECId5VGpSGDBeTHU7nYD3fZHE7AH4FKLiGAwWF+NHomAAWV43hKcjA7xAwT82VI2PR+MJxNJ5Mp1LxBLYRN4JLJFKpYhxLLpHIZFOxazEmRBElqsHg9AgSWosCwEKhwD8ADNEVBEOB-Lw7rlUbyYDKoEl4GAYLwYABqGBEcQ5IRAA)

Вернемся к методу `map`. Предположим, у нас есть несколько разных массивов и нам нужно их промапить, но логика колбэка одна и та же. В этом случае имеет смысл вместо анонимной функции создать обычную и просто прокидывать её в `map`.

```ts
const strings = ['Bob', 'Alice', 'John', 'Anna'];
const arrays = [[1, 2, 3], [2], [], [0, 0, 0, 0, 0]];

const getLength = (element: { length: number }) => element.length;

const stringsLength = strings.map(getLength);
const arraysLength = arrays.map(getLength);
```

Функция `getLength` ожидает один параметр `element` и возвращает `number`. Но как мы знаем `map` вызывает callback с тремя аргументами `element, index, array`. 

Тайпскрипт не заставляет вас использовать все аргументы которые прокидываются в колбэк.

Так же тайпскрипт не выдаст ошибку, если ожидается функция, которая вернет `void`, а она возвращает другой тип.

```ts
type CbFunc = (a: number, b: number, c: number, d: number) => void;

const myFunc = (cb: CbFunc) => {
  cb(
    Math.random(),
    Math.random(),
    Math.random(),
    Math.random()
  );
};

const result = myFunc(function (a, b) {
  return a + b
});
```

# Перегрузка функций (Function Overloads)

[**Link**](https://www.typescriptlang.org/play?#code/LAKALgngDgpgBAYQPZIE4BMDOcC8cDaAdgK4C2ARjKgDRwkVW32WoC6A3KJLHAApIBLQmFxwA3qDhwAHgC46ZFpxBSI85lWVSAXusWbQAX2WhQAM2KEAxmAFJCcAOYww-IWAAUVlBkzzkaFgAlPJuwuxwAPSRcEgAblQANkgAhuhwmAKOhClgxKgw5pY2dg7OroLCHnIKDDRwarUstLpNVCF8lWAR0bEJqMlpGVk5eQVF1rb2Ti5hnil6dXAAPog+WLTkAPyLzXBWO22oHXPiUTECpFCJMKQwwrmlw9m5+YUq+-aYIvjStBCiAAMLSBrFEAEFUKgUhAAHQCTCQ6EQDwpIJwLZwFJweT4FKbWhWViSOAFMYOCQfKR-Emqai0uDaekfYxGUwgbyEb5wdAARlE5TmHnwACZaGK4ABmVhBZSc7noEUC2ZdDwAVloGrgatloHlInQkuVFXcHl5tElsqAA)

Если функция может работать с разным набором параметров, то использется перегрузка.
```ts
type Coords = [number, number, number];
type Point = {
  x: number;
  y: number;
  z: number;
};

function getPoint(coords: Coords): Point; // overload signature
function getPoint(x: number, y: number, z: number): Point; // overload signature
function getPoint(a: number | Coords, b?: number, c?: number): Point { // implementation signature
  const [x,y = 0,z = 0] = Array.isArray(a) ? a : [a,b,c]
  return {
    x,
    y,
    z,
  };
}

const d1 = getPoint([2, 2, 3]);
const d2 = getPoint(5, 5, 5);
const d3 = getPoint(1, 3); // No overload expects 2 arguments, but overloads do exist that expect either 1 or 3 arguments.
```
`overload signature` - тип который виден извне, например при вызове функции

`implementation signature` - тип который объединяет все вариации перегрузок и виден только внутри самой функции

За счет разделения на сигнатуру реализации и сигнатуру перегрузки, тайпскрипт не позволит смешивать перегрузки между собой. В примере выше нельзя вызвать функцию с двумя параметрами, так как в перегрузках нет такого варианта.

# Rest параметры/аргументы

[**Link**](https://www.typescriptlang.org/play?#code/GYVwdgxgLglg9mABAWxAG1gBzQTwBRgBciYIyARgKYBOANIgHRPLGkU0DaAugJSIDeAWABQiRNUpQQ1JMgbIAhpjx4AHnwC8APhKIAVInUBuEQF8REBAGcoiBYg0p0WXHgCMABnpv6AJnoAzPQALDxGQA)

Rest параметр это массив всех, не описанных ранее параметров. Идёт последним параметром и начинается с `...`

```ts
function multiply(n: number, ...m: number[]) {
  return m.map((x) => n * x);
}
const a = multiply(10, 1, 2, 3, 4);
```