## 类型系统

* 变量未初始化及未标注类型时将被解释为 `any` 类型。`any` 类型变量将不做类型检查，可赋予任何类型的值。
* `unknown` 类型和 `any` 相似，它可以接受任何类型的值。但与 `any` 不同的是，`unknown` 是类型安全的，在对 `unknown` 类型的值进行操作前，必须先进行类型检查或断言。

```typescript
let a: unknown;
a = 42;
a = true;

// 类型检查
if (typeof a == "string") {
    // ...
}

// 类型断言
let b = a as boolean;
```

* 返回类型为 `void` 的函数

```typescript
function foo(): void {
    return; // ok
    return undefined; //ok
    // return null; // no
}

function bar(): null {
    return null; // ok
}
```

* 返回类型为 `never` 的函数意味着该函数会抛出异常，该函数的调用处后的代码将视为不可达。

* `==` 是“宽松相等”

  * 会进行**类型转换**，然后再比较值。

  * 如果两个值类型不同，JavaScript 会尝试将它们转换为相同的类型后再比较。

```javascript
console.log(1 == "1");     // true — 字符串 "1" 被转为数字 1
console.log(true == 1);    // true — 布尔 true 被转为 1
console.log(null == undefined); // true — 特殊规则
console.log(0 == false);   // true — 0 和 false 都转为 0
```

* `===` 是“严格相等”（Strict Equality）

  * **不进行类型转换**。

  * 类型不同 → 直接返回 `false`。

  * 只有当**类型和值都相同**时才返回 `true`。

```javascript
console.log(1 === "1");    // false — 类型不同（number vs string）
console.log(true === 1);   // false — 类型不同（boolean vs number）
console.log(null === undefined); // false — 类型不同
console.log(0 === false);  // false — 类型不同
console.log(1 === 1);      // true — 类型和值都相同
```

* 检查 `null` 或 `undefined`的惯用范式：

```javascript
if (value == null) {
  // 相当于 value === null || value === undefined
}
```

* 可以往 `tuple` 中 `push` 新的值，但是在解包时不能使用添加的值。

```typescript
let t: [string, number, boolean] = ["hello", 10, true];
t.push("world");
// t => ["hello", 10, true, "world"]

let [a, b, c] = t; // ok
// let [a, b, c, d] = t; // no
```

* 字面量也可以作为类型（有点类似 LuaCATS）

```typescript
// value 的值只能是这三者
let value: "a" | "b" | "c";

// VALUE 的类型将自动被推导为 "hello"，因为是常量
const VALUE = "hello"

// 由于最终会被编译到 Javascript，因此字面量类型最终还是会被推导回字面量本身的类型
console.log(typeof VALUE); // => string
```

