## keyof 和 in 的用法

### keyof

`keyof` 操作符可以用于获取对象类型的**所有键**，其返回类型是**联合类型**。

```ts
type Point = { x: number; y: number };
type P = keyof Point; // "x" | "y"

// keyof any 表示任何类型的键，返回类型是 string | number | symbol
keyof any // string | number | symbol
```

#### 索引签名兼容性规则

```ts
type A = { k[number]:number};
keyof A // number

type B = { k[string]:number};
keyof B // string | number
```

- **字符串的索引类型签名**（`string`）表示所有 **可转化为字符串的键** 均有效。
- **数字键**（`number`）会被隐式转化为字符串（`string`），因此 TS 认为 `number` 类型的键也符合该索引签名的约束。

> JavaScript 对象的键在底层会被强制转为 **字符串** 类型。 keyof B // string | number 是 TS 对 JS 运行时行为的类型层模拟。它通过 **兼容数字键的隐式字符串化**，确保了类型系统与语言底层机制的一致性。这种设计既保持了类型安全性，有避免了因 JS 特性导致的类型漏洞。

!todo 面


### in

`in` 操作符用于**遍历**联合类型，将**联合类型**中的每个成员分别代入类型声明中

```ts
type animals = "dog" | "cat" | "bird";
type Animals = {
  [key in animals]: string; // 遍历联合类型，将联合类型中的每个成员分别代入类型声明中
};
```
