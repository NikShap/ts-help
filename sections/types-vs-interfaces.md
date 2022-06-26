[<- Назад](../README.md)

Оглавление
- [Type vs Interface](#type-vs-interface)
- [Интерфейс для Объектов](#интерфейс-для-объектов)
- [Интерфейсы сливаются, и это хорошо](#интерфейсы-сливаются-и-это-хорошо)
# Type vs Interface

Чаще всего выбор между `type` и `interface` обусловлен привычкой разработчика или общим стилем кода на проекте. Оба подхода позволяют легко решать задачу типизации объета. 

[**Link**](https://www.typescriptlang.org/play?#code/PTAEBcE8AcFMFgBQSp1ASQCagLygM7gBOAlgHYDmoAPqGQK4C2ARrEQNxIoyygCq+NrlABvJKFAlMALgyZOiCWQCGjWLMKlKCibEbKSAGw3FyFBQF8F3NABVYAD3AAFAPaFhYxZJlydER3ATLXMkKy5EVF50fQpYNw88eycE8FAAMlFxSVj1AlNKMIio0DjwGOU41OEACilgswBKXAA+DFzUiKQQSTJwNgAzZQBjBGREcn6iIdHQAHFYcAE2AEFh8BJXMizvKIbtbOhlSENXZV9ljiLxycGR3gWlwSI1ja2AYSJYZXBXIh2JDV6M9ZJdGrJHpdXpsyJYIsMth4ypcIYsoesYZ9vr9-nggc9mjg2jUvBI9qAAORzACitgA+nwAMrUgBKFIANIdjqdzrJgWwwo0IkA)

```ts
type User = {
  id: string;
  name: string;
  email: string;
};
```

```ts
interface Post {
  id: string;
  title: string;
  text: string;
  tags: string[];
  author: string;
}
```

И тут, у самых внимательных из вас уже может возникнуть вопрос, "Типизации **Объекта**?"

# Интерфейс для Объектов

Да, тут скрывается первое различие.
`interface` позволяет описать **только** объектные типы данных. В том числе функции.

```ts
interface GetUserAction {
  type: string;
  payload: User;
}

interface GetUserActionCreator {
  (user: User): GetUserAction;
}

const getUser: GetUserActionCreator = (user) => ({
  type: 'GET_USER',
  payload: user
})
```

`type` же позволяет работать и с объектами, и с примитивами, и с пересечениями, и с объединениями и возможно ещё с чем то.

```ts
type Id = string | number;

type TextPost = {
  id: Id;
  text: string;
};

type ImagePost = TextPost & {
  image: string
}

type getImagePost = (id: string) => ImagePost
```

# Интерфейсы сливаются, и это хорошо

[**Link**](https://www.typescriptlang.org/play?#code/PTAEBcE8AcFMFgBQSp1AYQIblAXlAN5KigB2mAtrAFygDO4ATgJakDmA3EgL5IoywM2PIWKhMbGmQCuFAEaxGXRN2VIQoVuEUAzTAGMEyRFt0HBAEQD2bUYhLkqtBi3bKSEqaVkKlPPiak2ox6hqDWtkT2oABuVsyGABQAlLRxzAAmyrzG+lakDKAZNrQRIlEekrQAjAA0Yo5SAOQAygAWmCwA1k310emGtCl4AHygeQVWADawAHRTNolNgMgggAwggEwgTck8QA)

Вторая особенность `interface` в том, что они мержатся если повторно использовать одно и то же имя.

```ts
interface Dog {
  name: string;
  age: number;
}

interface Dog {
  voice(): void;
}

const dog: Dog = {
  age: 1,
  name: 'Sharik',
  voice: () => console.log('Гав')
}
```
Имя используемое для `type` должно быть уникально

```ts
type Cat = { name: string }

type Cat = { age: number } // Duplicate identifier 'Cat'.
```
Слияние интерфейсов очень важный элемнт, раскрывающийся когда необходимо дополнить уже имеющийся тип, над исходником которого у вас нет власти. Например дефолтный тип Window

```ts
interface Window {
  sayMyName: () => void
  myName: string;
}

window.myName = 'Bob';
window.sayMyName = () => {
  console.log(window.myName);
};
```