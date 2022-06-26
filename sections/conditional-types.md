[<- Назад](../README.md)

Оглавление
- [Условные типы (`Conditional Types`)](#условные-типы-conditional-types)
- [`Union` и `never`](#union-и-never)
- [Выведение типа `(infer)`](#выведение-типа-infer)

# Условные типы (`Conditional Types`)

Условный тип позволяет описать проверку отношений между типами и выбрать один из двух результирующих типов.

```ts
T extends U ? X : Y
```

`T extends U` - ставит условие **(Если T можно считать наследником U)**. Далее как в тернарнике

Полезен при написании утилит и дженериков.

[**Link**](https://www.typescriptlang.org/play?#code/C4TwDgpgBAwgFhAxgawAoCcIDMCWAPAHgGUoI9gIA7AEwGcpbh0dKBzAGig231PKroMmLVgD4oAXigAKALAAoKFBJkKNegAMAJAG9uuPAF8A+rsbM2hjQqVKA-Mqg3bALiiUIANwjoFASgBuBQVQSCgAFQhGSVgEFH18AgByAHEAUXDjAFUiNIAlJM5UjKTRIPlgivlQ6AAZCFYAQ0QQAEFPRuBG9BidZwBXdAAbN3MRJ3lDSpqoAHUcYDgAMWYBeikdCaUsVfVR4TYAbQBdCcNykPBoLNofAnqmlr41QSShhuaQJKgAHygkjwAd2+UgBEGB4g2zhw1DcD0+zzW-3ejy+UAclH6AFsAEY+KD7CyscpKSiNLEQQkiElQRodLroOEfJ6qJFvZlohzwlrtTrdAlCIkKQxQABkMm5IER6n+QO+DnmixWOCRbh0hj80yuUAAcuCbvipAb0Mk5WVLmFJTFjckUZ9SkA)

```ts
type CheckPrefix<S extends string, Prefix extends string> = (
  S extends `${Prefix}_${string}`
    ? S 
    : never
);

type Test = CheckPrefix<'GET_USER', 'GET'>;
```

```ts
type LegacyAvatar = {
  url: string 
}

type WithFriends = { 
  friends: string[] 
};

type User<Legacy extends 'legacy' | 'new' = 'new'> = {
  id: Legacy extends 'legacy' ? number : string;
  name: string;
  avatar: Legacy extends 'legacy' ? LegacyAvatar : string
} & (Legacy extends 'new' ? WithFriends : {})

type NewUser = User<'new'>;
type Legacy = User<'legacy'>
```

# `Union` и `never`
Если условный тип применяется к объединению типов, то условие будет проверяться для каждого из них, а на выходе получится объединение результатов.

[**Link**](https://www.typescriptlang.org/play?#code/C4TwDgpgBAqgdgSwPZygXigcgIaagHywCM9CBGABgKjgFcBbIiAJwG0BdaugG24G4AsAChhoSFADCtAM5J6AUQAeAY260AJhAA8AFQA0sAHzooOqBEXAIcddNhQA-DQgA3FlABcpwSKFjoUtLAciaBckqqGtrwyHAGQcwIcADmxsIA9OlYuOaW1rZQCUnJUKVl6MZwriwZWZgkuVY2dkUp5WVoldXMtVCUjfktwIlt7aWdfRS9dIwsHAPNhcPF45UMTGzs07S8CwWtJe0TPNxAA)

```ts
type Union = 'a' | 'b' | 10 | number[] | null;

type CusomExclude<T, U> = T extends U ? never : T;

type Custom = CusomExclude<Union, string> 
// 'a' extends string       => never
// 'b' extends string       => never
// 10 extends string        => 10
// number[] extends string  => number[]
// null extends string      => null
```
Условные типы это то место где `never` незаменим, так как он удаляется из объединения

# Выведение типа `(infer)`
Ключевой слово `ifer` позволяет указать тайпскрипту на необходимость вывести тип.

[**Link**](https://www.typescriptlang.org/play?#code/C4TwDgpgBAqgdgazgewO5wGIFc4GNgCWycUAvFABQB0NAhgE4DmAXFDkmnANoC6AlGQB8bRCnQBuALAAoGaEhQAwlgDOwZAFsAShGBZ6cACrgIAHmx4oEAB7AIcACYrYozhfxE4w8hRlQo7la29k6UAqTCBHAAZhD0UDp6BsaQfv5QAPwJuvpGJmn+rHAQAG5x6TJ8UrLS8tAAcrSEZWTZSXmQphThwnBYGgBGcYLVdUqq6hqtymqaibkpZt1CUH2Dw+JAA)

```ts
type UnknownFunction = (...arg: unknown[]) => unknown;

type CustomReturnType<Func extends UnknownFunction> = (
  Func extends () => infer ReturnType
    ? ReturnType
    : never   
);

type Native = ReturnType<() => number>; // number
type Custom = CustomReturnType<() => number>; // number
```

Ещё один интересный пример со строкамт

[**Link**](https://www.typescriptlang.org/play?#code/C4TwDgpgBA4hwAUBOEBmBLAHgHgMpQk2AgDsATAZyguCXRIHMA+KAXigAoBYAKCinyFi5KgAMAJAG96qCEijI0WAL4B9KTTqNlo3v34B+BSgyYoe-QC4oJCADc5vAJQBuXr1CQoAFQg02sPCKptgA5DAAot6qAKq4EQBKoUxAA)

```ts
type GetPrefix<S extends string> = S extends `${infer Prefix}_${string}` ? Prefix : never;

type Test = GetPrefix<'GET_USER'>  // 'GET'
```


