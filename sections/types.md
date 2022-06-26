[<- Назад](../README.md)

Оглавление
- [Объединения типов (Union Types)](#объединения-типов-union-types)
- [Пересечение типов (Union Types)](#пересечение-типов-union-types)
- [Псевдонимы типов (Type Aliases)](#псевдонимы-типов-type-aliases)
- [any](#any)
- [unknown](#unknown)
- [void](#void)
- [never](#never)
# Объединения типов (Union Types)

```
тип1 | тип2 | тип3
```

Если в качестве типа указано объединение, то тип переменной это один из типов в объединении. Другими словами можно воспринимать как *`тип1 ИЛИ тип2 ИЛИ тип3`*

[**Link**](https://www.typescriptlang.org/play?#code/C4TwDgpgBA0hJQLxQM7AE4EsB2BzKAPlNgK4C2ARhOgLABQ9AZidgMbCYD22UANp7gCSAEwAUAa3gAuWPACUUAN70oUVtxSdeEAHT9cE+DuCcAqmEjoAwgEMUEUXLkBuKAHo3UAArpOl0FAA5Cbmlrb2gVDCnBAoxJzAUBAAHphoUNxQoJBBpJTUgSpQmIxQotkQnKWSCIh1UABEaFh4DQrKdKqqHlCAOCCAfCCAbCCAvCD9UIAcIICCIID8IP2A3CCAMiCA7CD9gEwggAwggEIgC1CAPCCArCCb-YAsIIDSIFCTm4ACIP2AXCBFqurYmtp6AoYgxmYW1OEOTs4igBfJK8exKB7uTw1KCrfozK7jYbjXbrQ6TQDyIFBbutbsRyFRaJ0uk8Xrp9B8XMD6ECgA)

```ts
type Key = string | number

function logId(key: Key) {
  console.log(key.toUpperCase()); // Property 'toUpperCase' does not exist on type 'number'
  if (typeof key === "string") {
    // Можно использовать методы строк
    console.log(key.toUpperCase());
  } else {
    // key воспринимается как number
    console.log(key);
  }
}
```

----------------------------------------

# Пересечение типов (Union Types) 

```
тип1 & тип2 & тип3
```
Пересечение означает что тип переменной одновременно удовлетворяет всем перечисленным типам. *`тип1 И тип2 И тип3`*

[**Link**](https://www.typescriptlang.org/play?#code/C4TwDgpgBAIglgZwIYCMA2FUagXigbwFgAoKKReZdCAEwC4oUB7JjJAOwG4SBfEk0JCgBhNHADGAayzQ8RUlCbtREyQwAUASlwA+KADcmcGt2J9iA8NACS4pQCEArsGBLcIsVJlQAZLEQy3n7yZBJKAKoATmgMCMCRcOwA5qY8QA)

```ts
type Disableable = {
  isDisabled: boolean;
}

type Clickable = {
  onClick: () => void;
}

type IconButton = Clickable & Disableable & {
  iconUrl: string;
}
```

----------------------------------------

# Псевдонимы типов (Type Aliases)

```
type ИмяТипа = тип
```
Используется если: 
- тип переменной составной (объект, пересечение, объединение, функция)
- примитиву нужно задать более понятный псевдоним `DateString`, `UuidType`.
- тип будет переиспользоваться

[**Link**](https://www.typescriptlang.org/play?#code/C4TwDgpgBAqgrgSwCYBVzQLxQM7AE4IB2A5gNwCwAUFaJFAHJwC2EBAxgJKrpRaHMAjVhWqVa0FAENsAawDKwScDjZeUAOQoA8gBEt6qAB8NHegH0A6loBKAaQPH1e+gFEHGgGIBBDgBl1pFBUNDww2KxodFgA3lRQUMgAXLCI3JAi8YSSLMm4BCQiAL7BYjxSspGYULGU8UkMzKwInGkQGTiKytjJ5fKdKu3AEAAewLn4RGRUhUA)

```ts
type UuidType = string;

type NumericIdType = number;

type TaskStatus = 'TODO' | 'IN_WORK' | 'DONE' | 'FAIL'; 

type UserType = {
  id: UuidType;
  name: string;
}

type TaskType = {
  id: NumericIdType;
  status: TaskStatus;
  text: string;
}
```

----------------------------------------

# any

Тип any - это что-то вроде "надтипа" к которому может быть сведен любой другой тип. Лучший друг багов. Вы можете вызывать переменную с типом any как функцию даже если это не функция. Тайпскрипт не сможет отследить ошибку и она всплывет только в момент выполнения.

[**Link**](https://www.typescriptlang.org/play?#code/MYewdgzgLgBAhmAngRgFzyTAvDArAbgFgAoUSWBRAJnWgCcBLMAc2wxSOMuQAoBKTtwB0AW0QAxAK5hg-fEA)

```ts
const any1: any = 5;
const any2: string = any1;
any1();
any1.myFunc();
```

----------------------------------------

# unknown

Безопасный вариант типа any. Не позволит проводить над собой никакие манипуляции пока тип не будет сведет к чему то более конкретному. Используется там, где вы не можете определить тип заранее.

[**Link**](https://www.typescriptlang.org/play?#code/MYewdgzgLgBArmA1mEB3MBGAXPJL0wC8MA5AEYhkkDcAsAFCiSwLJpgBMO0ATgJZgA5kVxt0GOvQZ8AZjAAUUAJ4AHAKYg5rfJiKFiJXgMEkAlDADeDGKJ0YAdFBABVFep4BhAIYQ1805IAvkA)

```ts
const unknown1: unknown = 'bob';
const unknown2: string = unknown1; // Type 'unknown' is not assignable to type 'string'.

if (typeof unknown1 === 'string') {
  unknown1.toUpperCase();
}
```

----------------------------------------

# void

Тип пустоты. Как правило, используется как тип возвращаемого значения у функций, которые не содержат return (ничего не возвращают).

Так же, могут быть использованы для описания колбэка, если возвращаемое им значение не имеет ...эмм значения. Подробнее об этом будет в материале по функциям

```ts
type SetText = (text: string) => void;
```

----------------------------------------

# never

Тип ненаблюдаемых (невозможных) значений. В возвращаемом типе это означает, что функция выдает исключение или завершает выполнение программы.

Но куда важнеее то, что never исключается из Union типов. Подробнее об этом будет в материале по условным типам.

```ts
type NoNeverHere = string | number | never; // string | number
```