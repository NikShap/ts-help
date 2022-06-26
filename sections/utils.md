# Утилиты и прочее
- [Утилиты и прочее](#утилиты-и-прочее)
  - [Тип переменной (`typeof`)](#тип-переменной-typeof)
  - [Ключи объекта (`keyof`)](#ключи-объекта-keyof)
  - [Доступ по ключам / индексам (`User['id']`)](#доступ-по-ключам--индексам-userid)
  - [Record`<Keys, Type>`](#recordkeys-type)
  - [Partial`<Type>`](#partialtype)
  - [Required`<Type>`](#requiredtype)
  - [Readonly`<Type>`](#readonlytype)
  - [Pick`<Type, Keys>`](#picktype-keys)
  - [Omit`<Type, Keys>`](#omittype-keys)
  - [Extract`<Type, Union>`](#extracttype-union)
  - [Exclude`<UnionType, ExcludedMembers>`](#excludeuniontype-excludedmembers)

## Тип переменной (`typeof`)

[**Link**](https://www.typescriptlang.org/play?#code/MYewdgzgLgBA5gUygVQggTjAvDAFASwBMAuGadfMOASmwD48BvAWACgYYoBPABwVIDkAcQCiAFQD6yAMoiAShIBCATQkBJACICYAQwgxQkKABo2HHjq4AbEDpIwip1gF9qbNoegwAwgHkAMr5y0tgwLOww6Aj2AuhwAEa4AEwArCnGMAAMGZnUAk4ccFEIYIJxidkwqelZeQUw8VYArvwwsQm4lZXVdWzOuvqeUADc7qzcfDBCSKgYilxqhACCwFD44KETCCAAZvAzaOij47wIPiA26Po4W7s+AUHSw0A)

```ts
const getUser = (id: string) => ({
  type: 'GET_USER_BY_ID' as const,
  payload: id,
})

const COLORS = {
  red: 'rgb(255, 0, 0)',
  green: 'rgb(0, 255, 0)',
  blue: 'rgb(0, 0, 255)',
} as const;

type GetUserByIdAction = typeof getUser;
type Colors = typeof COLORS;
```

## Ключи объекта (`keyof`)

[**Link**](https://www.typescriptlang.org/play?#code/C4TwDgpgBAwg9gGzgJwM5QLxQN4FgBQUUyEAJgFxQDkyA5gEYAUATAKysA0UADF9wJRUOBIrRIQAdpRoNGvKG049BwwlHoIArhGl0m8+YpUioAdwAWAS2A7qeluy6Kn7Y-gC+BAqEixEKADkAQwBbaCwAawgQOAAzPyQ0AG4vfAB6NKgECGAoIMp4ROCw1J9oAAU4SwlcrABtAA9KCU0Q+ghkLhBm1vbOqAAvHraOgF0U-G9wCqqagGlo9EjouKhK6uAJggysnPVKdfnFoA)

```ts
type Colors = {
  red: 'rgb(255, 0, 0)',
  green: 'rgb(0, 255, 0)',
  blue: 'rgb(0, 0, 255)',
  white: 'rgb(255, 255, 255)',
}

type ColorName = keyof Colors; // 'red' | 'blue' | 'green' | 'white';

type Point = [x: number, y: number, z: number];

type PointKeys = keyof Point; // '0' | '1' | '2' | 'map' | 'forEach' | 'filter' | и т.д.
```

## Доступ по ключам / индексам (`User['id']`)

[**Link**](https://www.typescriptlang.org/play?#code/PTAEkIQQmEEPhBE4QRBEEEIglAMIKahGEEFIghWEEFwgiKAsAFCkAuAngA4CmoAqgM60BOoAvKAN6migBLACYAuUADsArgFsARmwDcfQUwCCAY3ICAbrTGyA9gYA2tAIbilJfuLPS9oJuVYDxAcyv8z2s+TOsxJxd3T1A3VgNJagBJUUdnVw9SAF8rChp6AHFacmY2Tn4ACmExPNYAbQByYUqAXQBKTgA+RhZWNJIqOlBVY2MygAUIulYqABUMgrLygGtaSgMAM1a2Wo6QUEBaEEBuEBxAHhAcQF4QREBpEFA9lHh4QA4QVFJ07oBZA3FyAAsAGQEnAvLxWl0rAANKBKoB6EEOqEAAiCAGRBKiDKoASEFokChKG2cIRgBwQFBQxDw0GABBBAPwgUJwGIJlRxsEpgAwQQByIIdMaCGRSEYTIIBmEEAwiBISngI6IQDyIBhYZTAHggBBFYoRgFwQaDS5mVQAoIPgUKK4Wt7p1JgB1VzkNjPV5vArG95fJzlACMoAAPqAAEz20DWx1asg6p4vd4FUDmz7fch-GTyVi1bVAA)

Подобно тому как это делается в js, в ts можно обратиться к свойству типа через квадратные скобки `[]`

```ts
type User = {
  id: number;
  isActive: boolean;
  name: string;
  avatar: string;
  groupId: string;
};

type GetUser =  (id: User['id']) => User;

type AllUserPropertyType = User[keyof User]; // string | number | boolean
```
Вытянуть типы элементов массива можно передав индексы

```ts
type MonthList = [never, 'Январь', 'Фeвраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь'];

type WinterMonth = MonthList[1 | 2 | 12];

type Month =  MonthList[number]
```

## Record`<Keys, Type>`
Полезная утилка, создаёт тип объекта с ключами из первого параметра и типом значений из второго

[**Link**](https://www.typescriptlang.org/play?#code/C4TwDgpgBAggdiA8gIwFYQMbCgXigJUwHsAnAEwB4BDBAGihpAD4BYAKFEigGEiAbUgGdcBYuQoByEhDISoAHygTkfAK4Q5iiQHNpEOJqUB3ABYBLYBvqDgJM3G2s27TtACqgiCREBvdlCgzMgAuKDhVAFtkLwBuf0DBGCwzADcIUOQifggaOLYAuCoI9KgbOwc8gKoUqmAqElCy+21KqF0iVTAASRDS22a8gF88l3BoAHESDsgyDy9hPEIMUko5kgBtHSnOnokAXXo1piA)

```ts
type AnyObject = Record<any, any>
type Colors = Record<'red' | 'blue' | 'green' | 'white', string>

type User = {
  id: number;
  isActive: boolean;
  name: string;
  avatar: string;
  groupId: string;
};

type GroupedUsers = Record<User['groupId'], User>
```

## Partial`<Type>`
Утилита возвращающая тип у которого все свойства опциональны.

[**Link**](https://www.typescriptlang.org/play?#code/JYOwLgpgTgZghgYwgAgAoHsDOZkG8CwAUMsmMGADYQBcy2UoA5gNxEmQAeYt9TrxyYAFs4jCAH4eYBiBZtSozFJmMA2gF1+AXyJEE6ENjoQwGbABE4YOMgC8yABQgIAd0vXaqOFDJwKAHjMwAD4ASjtgxwIBMABPAAcaZAByAGUAUQAVAH1UAHlUnPMAQUzi5IAaeXi4WIp0OAATWmc3Kzgqwi1Q3UJMEyD3OAdo9kVaVWSwTEqUgFcyCnIZ9SJuoA)

```ts
interface Post {
  title: string;
  text: string;
  image?: string;
  tags: string[];
}

const setPostData = (newData: Partial<Post>) => ({
  type: 'SET_POST_DATA',
  payload: newData,
})

setPostData({
  tags: ['ts', 'utilits'] // так как все свойства опциональны, можно поменять только часть данных поста
})
```

## Required`<Type>`

Утилита возвращающая тип объекта у которого все свойства обязательны. Допустим в UI пользователю не обязательно указывать image, но сервер требует это поле в теле запроса. 

[**Link**](https://www.typescriptlang.org/play?#code/JYOwLgpgTgZghgYwgAgAoHsDOZkG8CwAUMsmMGADYQBcy2UoA5gNxEmQAeYt9TrxyYAFs4jCAH4eYBiBZEAvkSIJ0IbHTgA3CBmwBBAA7BkAXmQAKA1m7IAShACOAV2BQIAEwA8usAD4AlKa+eGzIAPRhyIDoIIAMIID8IIACIIB8IICCIMiAvCAxyClJgKIgCTHpgKwg6YAcIEXIcamAQiAxCvzKquqYWjrWphZW2LQ+gSbBBAIt2j6GwOaDJCQAdLNdYAA0oSTCojTI89OrYsgAPrvIAOSHSwLy-vVAA)

```ts
interface Post {
  title: string;
  text: string;
  image?: string;
}

const savePostApi = (post: Required<Post>) => {
  // Запрос на сохранение поста
};

const savePost = (post: Post) => {
  savePostApi({
    ...post,
    image: post.image || '', // image всегда будет в теле запроса
  })
};
```

## Readonly`<Type>`

Создаёт тип, свойства которого нельзя переназначить, только читать. Как пример использования, можно привести объект props в копонентах. Как оказалось, есть люди, которые пытаются менять что-то в этом объекте. 

[**Link**](https://www.typescriptlang.org/play?#code/JYOwLgpgTgZghgYwgAgApQPYAcDOyDeAsAFDLLAAmAXMjmFKAOYDcJZYwYANhDXQyBZtkkAB5g+9Jq1LkAtnEYQA-JIFDZGEABEIPSDQAUlNUwCUyALwA+ZADcMlGQF8SJBFrpoMdAMJwoCitkQyxMXBoAJQg4Ci0uAE8AHnRsHGsLGwJhMLSAOg5uFEtkAHJAXBBAPhBAJhBAaRBAThBkQHYQQAYQQGYQSsBuEBrKwC4QUpkyKAgwAFcoEGQAeinkACkAZQANEmcZElBIWEQUVNwAJmzZLB8JI7IL8mpaKUFBy5FOHlM74UuxM-5pN4vgBSVVDd1Pc1sItLp9LwQiYgeYrLYHE5Vm5iB4QF5UKd-IFDiVQuEcFEYnEQIkUgT9hl4edkLlcHkTnQCk9imUqnVGq0Ot1qn0BiihiNxpMZvNlqtmEA)

```ts
interface Props {
  id: string;
  title: string;
  text: string;
  image?: string;
  onDelete: (id: string) => void;
}

const PostCard = (props: Readonly<Props>) => {
  props.title = 'Новый заголовок'; // Cannot assign to 'title' because it is a read-only property

  return // JSX
};
```

Важно помнить что Readonly не распространяется на все уровни вложенности.

```ts
interface Props {
  post: {
      id: string;
      title: string;
      text: string;
      image?: string;
  };
  onDelete: (id: string) => void;
}

const PostCard = (props: Readonly<Props>) => {
  props.post.title = 'Новый заголовок'; // Это добром не кончится

  return // JSX
};
```

## Pick`<Type, Keys>`

Конструирует тип объекта, включая свойства с указанными ключами из другого типа.

[**Link**](https://www.typescriptlang.org/play?#code/JYOwLgpgTgZghgYwgAgAoHsDOZkG8CwAUMssACYBcy2UoA5gNxEljBgA2EVN9TxykAB5huYWiEbNSAWzh0IAflHjJ-OAFcwAC3RRlvIgF8iRMAE8ADigzZUUCADdgEAO7IAvGmAIA1gB4bMAAaZAByVg4IUOQAHzChMFCAPiA)

```ts
interface Post {
  id: string;
  title: string;
  text: string;
  image?: string;
  author: string;
}

type PostPreview = Pick<Post, 'title' | 'text'>
```

## Omit`<Type, Keys>`

Утилита обратная по функционалу утилите `Pick`.

Конструирует тип объекта, исключая свойства с указанными ключами из другого типа.

[**Link**](https://www.typescriptlang.org/play?#code/JYOwLgpgTgZghgYwgAgCoHsAm7kG8CwAUMsmMGADYQBcyAzmFKAOYDcRJmEdCTADmXQhaDJiDYdkCdAFs+VSJloAjdOipwQ7YlKgQ4igIJhaIAK4zl0bQF8iRMAE8+KDNgAKegG7AIAd2QAXmQAeRlyAB43dAAaZAAiLh5+QRB4gD5WIA)

```ts
interface Todo {
  title: string;
  description: string;
  completed: boolean;
  createdAt: number;
}

type TodoPreview = Omit<Todo, "description">;
```

## Extract`<Type, Union>`

Конструирует тип включая указанные типы из объединения

[**Link**](https://www.typescriptlang.org/play?#code/C4TwDgpgBAqgdgSwPZwCrmgXigZ2AJwTgHMoAfKAcgENLyo4BXAWwCMJ96BtARgBoATHwDMAXXoBvKAhwB5SHAgATAFxRWSJABsI1OFAC+AbgCwAKHOhIUAOoJgACwAyEEo6jYAogA8C1AMbAADzwyGgYfFBSOm4OakxsHIYAfEA)

```ts
type UnionType = string | 'a' | number | [1,2,3] | { isOpened: boolean };

type WithLength = Extract<UnionType, { length: number }> // string | [1, 2, 3]
```


## Exclude`<UnionType, ExcludedMembers>`

Конструирует тип исключая указанные типы из объединения

[**Link**](https://www.typescriptlang.org/play?#code/C4TwDgpgBAqgdgSwPZwCrmgXigZ2AJwTgHMoAfKAcgENLyo4BXAWwCMJ96BtARgBoATHwDMAXXoBvKAhwB5SHAgATAFxRWSJABsI1OFAC+AbgCwAKHOhIUAHJJgAQQDKBIqWwBRAB4BjLYyUIAB54ZDQMPlxXEgA+IA)

```ts
type UnionType = string | 'a' | number | [1,2,3] | { isOpened: boolean };

type NotAString = Exclude<UnionType, string> // number | [1, 2, 3] | { isOpened: boolean; }
```


