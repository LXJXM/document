## extends 用法总结

### 1. 接口继承

```ts
interface T1 {
  name: string;
}

interface T2 {
  age: number;
}

interface T3 extends T1, T2 {
  sex: 1;
}

// T3 继承了 T1 和 T2 的属性
// T3 必须包含 T1 和 T2 的属性
const t3: T3 = {
  name: "xxx",
  age: 18,
  sex: 1,
};
```

### 2. 条件判断

> 普通用法

```ts
interface T1 {
  name: string;
}

interface T2 extends T1 {
  age: number;
}
// 若 extends 前面的类型能够赋值给后面的类型，则返回 true，否则返回 false
type A = T2 extends T1 ? string : number;

const a: A = "this is string";
```

> 泛型用法

- 分配条件类型

```ts
type A1 = "x" extends "x" ? string : number; // string
type A2 = "x" | "y" extends "x" ? string : number; // number

type P<T> = T extends "x" ? string : number;
type A3 = P<"x"|"y">  = P<"x"> | P<"y"> = string | number; // string | number
```

- 特殊的 never

```ts
// never是所有类型的子类型
type A1 = never extends "x" ? string : number; // string

// 这里的 never 被当作空的联合类型，由于没有可分配的联合类型，所以返回 never
type A4 = P<never>; // never;
```

- 防止条件判断中的分配

```ts
type P<T> = [T] extends ["x"] ? string : number;
type A3 = P<"x" | "y">; // number
type A4 = P<never>; // string;

// 在条件判断类型的定义中，将泛型参数使用[]括起来，即可阻断条件判断类型的分配，此时，传入参数T的类型将被当做一个整体，不再分配
```

### 3. 在高级类型中的应用

> Exclude: 从联合类型中`排除`指定`类型`

```ts
// 类型申明
type Exclude<T, U> = T extends U ? never : T;

// eg: 从联合类型 "a" | "b" | "c" 中排除 "a"
type A = Exclude<"a" | "b" | "c", "a">; // "b" | "c"
// 等价于
type A = Exclude<"a", "a"> | Exclude<"b", "a"> | Exclude<"c", "a">;
// => never 是所有类型的子集
type A = never | "b" | "c";
// =>
type A = "b" | "c";
```

> Extract: 从联合类型中`提取`指定`类型`

```ts
// 类型申明
type Extract<T, U> = T extends U ? T : never;

// eg: 从联合类型 "a" | "b" | "c" 中提取 "a"
type A = Extract<"a" | "b" | "c", "a">; // "a"
// 等价于
type A = Extract<"a", "a"> | Extract<"b", "a"> | Extract<"c", "a">;
// =>
type A = "a" | never | never;
// =>
type A = "a";
```

> Pick: 从类型中`提取`指定`属性`

```ts
type Pick<T, K extends keyof T> = {
  [P in K]: T[P];
};

// eg:
interface A {
  name: string;
  age: number;
  sex: number;
}Z
type B = Pick<A, "name" | "age">; // { name: string; age: number; }
```
