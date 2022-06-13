# Перечисление (`Enum`)

- [Перечисление (`Enum`)](#перечисление-enum)
  - [Числовые перечисления](#числовые-перечисления)
  - [Строковые перечисления](#строковые-перечисления)
  - [Перечисление как тип](#перечисление-как-тип)
  - [Перечисление как константа](#перечисление-как-константа)
  - [Объект вместо `Enum`](#объект-вместо-enum)

Enum - это набор именованных констант. Ключевой момент тут в том, что это уже не просто типизация. Это работа со значениями. Вы не можете описать тип `Enum` используя псеводимы типов (`type`). Создавая перечисление вы задаёте значение и тип одновременно.

## Числовые перечисления

Разберем на примере перечисления месяцев. Для создания числового перечисления достаточно просто указать через запятую названия месяцев. Typescript автоматически присвоит каждому месяцу число в качестве значения.

[**Link**](https://www.typescriptlang.org/play?#code/KYOwrgtgBAsg9iALgCwM4FFzQN4FgBQUUAUgIYgA0BRAYsAEZWGykBOTRAggA7vUsBPDiTCV+xMABthnMAHNhAZWDdhAeQDGiYQDk4AN2EARYBqYBfANwECiAd2BRFiMABMBABUnkoAXih4zESoiGyIAJYgcvBIyABcsAgoGFgEVjb4GgghUCFunt4gCc75Xj7+gUTBoawRUTEoCQ1omJAAdMrcaZZAA)

```ts
enum MonthsEnum {
  Jan, // 0
  Feb, // 1
  Mar, // 2 
  Apr, // 3 
  May, // 4
  Jun, // 5
  Jul, // 6
  Aug, // 7
  Sep, // 8
  Oct, // 9
  Nov, // 10
  Dec, // 11
};
```
Доступ к значениям осуществляется через точку `MonthsEnum.Jan`

По умолчанию нумерация начинается с 0, поэтому при использовании в контексте месяцев это может путать

```ts
const studyPlan = {
    startingMonth: MonthsEnum.Sep // 8, но мы привыкли что 9
};
```

Исправить это можно если явно указать число, с которого нужно начать нумерацию

```ts
enum MonthsEnum {
  Jan = 1,
  Feb,
  Mar,
  Apr,
  May,
  Jun,
  Jul,
  Aug,
  Sep,
  Oct,
  Nov,
  Dec,
}
```

Можно использовать несколько точек отсчета

[**Link**](https://www.typescriptlang.org/play?#code/KYOwrgtgBAygLgQzmAzgYQPYBNgoKLjQDeAsAFBSVQDyA0lALxQBMADKwDRXeXndoAnYEmBYuVPlQCCAYxnAADnFHievClQBCCLACVgARzC44jKABZ2XSZQCqIBGDgALDAICWALxU8bUAAoIAJ4QoHD6Ru5CYurcAGJuAEbuWDggqr4alAByGHAJYCAxaiWZ3ACSIMoCDgA2MMACAG6NeAICbmYArFaxVLlw5RAKtcChVT6l3H7aWADiIgDuwRlT02QAvkA)
```ts
enum StatusCodesEnum {
    OK = 200,                   // 200
    Created,                    // 201
    Accepted,                   // 202
    BadRequest = 400,           // 400
    Unauthorized,               // 401
    PaymentRequired,            // 402
    Forbidden,                  // 403
    NotFound,                   // 404
    InternalServerError = 500,  // 500
    NotImplemented,             // 501
    BadGateway,                 // 502
}
```

## Строковые перечисления

В случае строковых перечислений, тайпскрипт не может автоматически вычислять значения, поэтому их необходимо явно указывать

[**Link**](https://www.typescriptlang.org/play?#code/KYOwrgtgBAIglgJ2AYwC5wPYigbwLABQUUAqgA5QC8UARCQAo0A0hxMGA7ttTTAPIB1AHLNWUADLAAZqiq1xAUQBiAFVFEoAJTgBzABayemgJIBxABJqWBAL5A)

```ts
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT",
}
```

## Перечисление как тип

Если указать что переменная принадлежит к типу перечисления, то значением переменной может быть любое значение из пересечения

```ts
type StudyPlan = {
    startingMonth: MonthsEnum
};
```

Если необходимо, то можно ограничить конкретным значением или рядом значений

```ts
type FirstMonth = MonthsEnum.Jan;

type SummerMonths = MonthsEnum.Jun | MonthsEnum.Jul | MonthsEnum.Aug;
```

Важно помнить, что если вы указали как тип строковое перечисление, вы должны использовать значения непосредственно из этого перечисления

```ts
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT",
}

function move(direction: Direction): void {
    // ...
}

move('UP'); // Argument of type '"UP"' is not assignable to parameter of type 'Direction'.
move(Direction.Up);
```

## Перечисление как константа

Если использовать enum как в примерах выше, то тайпскрипт сгенерирует большую и не очень красивую конструкцию при переходе в js

```ts
// TS
enum MonthsEnum {
  // ...
}

const StudyPlan = {
    startingMonth: MonthsEnum.Sep
};
```

```js
// JS
var MonthsEnum;
(function (MonthsEnum) {
    MonthsEnum[MonthsEnum["Jan"] = 1] = "Jan";
    MonthsEnum[MonthsEnum["Feb"] = 2] = "Feb";
    MonthsEnum[MonthsEnum["Mar"] = 3] = "Mar";
    MonthsEnum[MonthsEnum["Apr"] = 4] = "Apr";
    MonthsEnum[MonthsEnum["May"] = 5] = "May";
    MonthsEnum[MonthsEnum["Jun"] = 6] = "Jun";
    MonthsEnum[MonthsEnum["Jul"] = 7] = "Jul";
    MonthsEnum[MonthsEnum["Aug"] = 8] = "Aug";
    MonthsEnum[MonthsEnum["Sep"] = 9] = "Sep";
    MonthsEnum[MonthsEnum["Oct"] = 10] = "Oct";
    MonthsEnum[MonthsEnum["Nov"] = 11] = "Nov";
    MonthsEnum[MonthsEnum["Dec"] = 12] = "Dec";
})(MonthsEnum || (MonthsEnum = {}));

const StudyPlan = {
    startingMonth: MonthsEnum.Sep
};
```

И эта конструкция будет в финальном коде, даже если вы используете только одно значение из перечисления.

Если же перед ключевым словом `enum` поставить `const`, то в js останутся только константы с комментариями

```ts
// TS
const enum MonthsEnum {
  // ...
}

const StudyPlan = {
    startingMonth: MonthsEnum.Sep
};
```

```js
// JS
const StudyPlan = {
    startingMonth: 9 /* MonthsEnum.Sep */
};
```

## Объект вместо `Enum`

На самом деле, в большинстве случаев можно обойтись объектами с указанием `as const`.

[**Link**](https://www.typescriptlang.org/play?#code/MYewdgzgLgBApmArgWxgUQCIEsBOdhRbgwDeAsAFAwwCqADgDSXUYgDuYTVMAMnAGZQu1AEpYA5gAshlAL7NKoSLADy2PASJgYAXlLNadAFwwADMJisOJgIwW+gkwCYLYqVBMBmLrJgBDCBglaABuIA)

```ts
const enum EDirection {
  Up,
  Down,
  Left,
  Right,
}
 
const ODirection = {
  Up: 0,
  Down: 1,
  Left: 2,
  Right: 3,
} as const;
```
 Это более понятный подход, который не создаёт дополнительный сгенерированный код и опирается на уже имеющиеся в js концепции