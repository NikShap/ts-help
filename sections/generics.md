- [Generics](#generics)
  - [Как объявить?](#как-объявить)
  - [Пример использования](#пример-использования)
  - [Ограничение типа-переменной](#ограничение-типа-переменной)
  - [Автоматическое выведение типа](#автоматическое-выведение-типа)
    - [Попытка объяснить номер 1:](#попытка-объяснить-номер-1)
    - [Попытка объяснить номер 2:](#попытка-объяснить-номер-2)

# Generics

Дженерики - это возможность писать более обобщенную типизацию, которая подстраивается под ситуацию.

## Как объявить?
Интерфейс
```ts
  interface A<T> {/*...*/}
```
Тип
```ts
  type B<T> = /*...*/
```
Функция
```ts
  function C1<T>() {/*...*/}
  const C2 = <T>() => {/*...*/}
```

## Пример использования
Предположим, что сервер возвращает вам данные в формате

[**Link**](https://www.typescriptlang.org/play?#code/C4TwDgpgBAqgzhATlAvFKBvAsAKHegSwBMAuKOYRAgOwHMBuXfKagQwFsIyKq7G98rAG6tgrRN0o0GuAL65coSFACCYAgCUIcMAHtqCVJiZROcOK1pdyUviaSJdEqACNdugDYRW1fuiKirGQArtQA1tS6AO6+cgo4AMb6FFCIEACOwdrARgAUYKIAFpK8tACUZGqa2noG0CgAfMYC6AD0rVAAdN0m6GnAwYjUzcz+gWQA2gC6ADS9+A5OZABmrB4Icy34ZhZWZADkAPIA0vubzPI4l7hJBjkYUAFiIQiIcFCyRmmZ2bn7rcFXnB9mV+LcUg8nkEoHoUp80N8shQ-q1YcBWgAmEFg5L3R7jKBJdicajAd7w1IZJHAFFozGtIkksnY+LgnKApBwACSRHeaA5b067FYYFyuQexA+ZVQTWIoKAA)

```ts
type ApiResponse = {
  message: string;
  error: boolean;
  data: unknown;
}

const request = (path: string): ApiResponse => {
    // ...
}
```
Не важно что вы бодете запрашивать, пользователей, посты или комментарии. свойство data в ответе всегда будет восприниматься как unknown.

```ts
const { data: users } = request('/users');
const { data: post } = request('/post/2');
const { data: comments } = request('/post/2/comments');

const usersIds = users.map(({ id }) => id); // (property) data: unknown. Object is of type 'unknown'.
```

Было бы неплохо иметь возможность указывать тип ответа в каждом конкретном случае, при вызове функции. И дженерик дает такую возможность.

Ключевым элементом в дженериках являются типы-переменные. 

В случае с `ApiResponse` нам необходимо сделать свойство `data` более гибким. Определим для этого тип-переменную `Data`.
```ts
type ApiResponse<Data> = {
  message: string;
  error: boolean;
  data: Data;
}
```

Теперь при указании типа `ApiResponse` будет необходимо указать тип свойства `data`

```ts
type UsersApiResponse = ApiResponse<User[]>
// {
//     message: string;
//     error: boolean;
//     data: User[];
// }
type UnknownResponse = ApiResponse; // Generic type 'ApiResponse' requires 1 type argument(s).
```

Чтобы сохранить возможность использования типа без прокидывания уточняющего типа, укажем типу-переменной `Data` значение по умолчанию;

```ts
type ApiResponse<Data = unknown> = {
  message: string;
  error: boolean;
  data: Data;
}

type UnknownResponse = ApiResponse;
// {
//     message: string;
//     error: boolean;
//     data: unknown;
// }
```

Окей, у нас есть гибкий тип объекта ответа. Теперь нужно как то применить его в функции.

На самом деле всё просто. И решается так же дженериком. Объявим тип-переменную `T` для дженерик функции и используем её как аргумент дженерик типа ответа
```ts
const request = <T = unknown>(path: string): ApiResponse<T> => {
    // ...
}
```
 
Готво

```ts
type User =  {
    id: string;
    name: string;
    avatar: string;
}

type ApiResponse<Data = unknown> = {
  message: string;
  error: boolean;
  data: Data;
}

const request = <T = unknown>(path: string): ApiResponse<T> => {
    return {} as any;
}

const { data } = request<User[]>('/users');
const usersIds = data.map(({ id }) => id)
```

## Ограничение типа-переменной

[**Link**](https://www.typescriptlang.org/play?#code/C4TwDgpgBAaghgGwJYBMAidhwCrmgXigFcA7AaxIHsB3EgbQF0oAfKAJQgGNKAnFAHgDOwHkhIBzADTFyVWgD4A3AFgAUGtCQoAQTBIOgsJRKCI-DFigQAHsAgkUg2IlQWceeVEIBvNVCgAthCCgnDiEABcUMKiEiqq-hA8PLxRAEaUlAgQcCTx-iiYcFFu8QC+QA)

Если вы точно знаете, что `data` в ответе всегда будет либо массивом, либо объектом, вы можете ограничить тип-переменную `Data` с помощью `extends`.

```ts
type ValidDataType = unknown[] | Record<string, unknown>;

type ApiResponse<Data extends ValidDataType> = {
  message: string;
  error: boolean;
  data: Data;
}
```
Значение по умолчанию при этом тоже подпадает под ограничение, поэтому можно установить само же ограничение `ValidDataType` как значение по умолчанию.

type ApiResponse<Data extends ValidDataType = ValidDataType> = {
  message: string;
  error: boolean;
  data: Data;
}


## Автоматическое выведение типа

[**Link**](https://www.typescriptlang.org/play?#code/MYewdgzgLgBIgiAEMAOBLKCA2KBeBTGAvDABTQBOAXDOSmAOYCUhAfDAN4CwAUDLzKJFjAAFgjIRC1KGQB0EJFijEA5MoYBubn36jxAbQAMAXUkixEQ0ZlQQAVSRJcZAMIIIuYhu5a+ZXFABXMjAdcxkAKxBaFTVNLgBfby4BaBgA9zIAOQQAW3wiRFR0LDwVACMQMtigA)

У дженериков есть супер крутая особенность. Они могут определять тип-переменную на основе аргументов функции.

Рассмотрим на примере функции сapitalize

```ts
const сapitalize = (str: string) => {
    const chars = str.split('');
    chars[0] = chars[0].toUpperCase();

    return chars.join('');
}

const userName = сapitalize('bob'); // string
```

Было бы круто, если бы тайпскрипт понимал что на выходе будет `'Bob'`. В тайпскрипте даже есть утилита `Capitalize` для работы со строками. Всё что нам нужно это сделать функцию дженериком.

```ts
const сapitalize = <T extends string>(str: T): Capitalize<T> => {
    const chars = str.split('');
    chars[0] = chars[0].toUpperCase();

    return chars.join('') as Capitalize<T>;
}

const userName = сapitalize('bob'); // 'Bob'
```
### Попытка объяснить номер 1:
- `<T extends string>` - это ограничение типа `T`. Если дословно, то `T` должно наследовать свойства абстрактного типа `string`. 
- `(str: T)` - тип `str` будет зависеть от переданного значения, но при этом ограничен вышеупомянутым `string`
- `Capitalize<T>` - будет брать динамический `T` и капитализировать
- `return chars.join('') as Capitalize<T>` - тайпскирит не понимает что наши манипуляции над строкой реально привели к тому что строка будет капитализирована, поэтому мы затыкаем ошибку и говорим, что знаем лучше; Будте осторожны с `as`, такое затыкание ошибки может вызвать баги, если что-то пойдет не по плану.

### Попытка объяснить номер 2: 

Когда мы пишем реализацию функции нам нет дела до конкретной строки которая будет передана в неё, всё что нам нужно, это ограничить параметр строковым типом данных, поэтому `T extends string`. Другими словами внутри самой функции str воспринимается как абстрактная строка. Это даёт возможность вызывать метод строки `split('')`.

Когда функция вызывается, мы уже точно можем определить, что за строка будет туда передана `сapitalize('bob')`. Тайпскрипт знает что аргумент это динамический тип `T` который используется в `Capitalize<T>` поэтому он просто заменяет `T` на `'bob'` и возвращает результат `Capitalize<'bob'>`

