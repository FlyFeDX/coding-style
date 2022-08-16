# TypeScript style guide

*Typescript 开发实践*

## Table of Contents

- 1 [Syntax](#syntax)  
  - 1.1 [Identifiers](#identifiers)  
    - 1.1.1 [Aliases](#aliases)  
    - 1.1.2 [Naming style](#naming-style)  
    - 1.1.3 [Descriptive names](#descriptive-names)  
  - 1.2 [File encoding: UTF-8](#file-encoding-utf-8)  
  - 1.3 [Source Code Formatting](#source-code-formatting)  
  - 1.4 [Comments & Documentation](#comments--documentation)  
    - 1.4.1 [JSDoc vs comments](#jsdoc-vs-comments)  
    - 1.4.2 [JSDoc rules follow the JavaScript style](#jsdoc-rules-follow-the-javaScript-style)  
    - 1.4.3 [Document all top-level exports of modules](#document-all-top-level-exports-of-modules)  
    - 1.4.4 [Omit comments that are redundant with TypeScript](#omit-comments-that-are-redundant-with-typeScript)  
    - 1.4.5 [Do not use `@override`](#do-not-use-@override)  
    - 1.4.6 [Make comments that actually add information](#make-comments-that-actually-add-information)  
    - 1.4.7 [Parameter property comments](#parameter-property-comments)  
    - 1.4.8 [Comments when calling a function](#comments-when-calling-a-function)  
    - 1.4.9 [Place documentation prior to decorators](#place-documentation-prior-to-decorators)  
- 2 [Language Rules](#language-rules)  
  - 2.1 [Visibility](#visibility)
  - 2.2  [Constructors](#constructors)
  - 2.3  [Class Members](#class-members)  
    - 2.3.1 [No `#private` fields](#no-private-fields)
    - 2.3.2 [Use `readonly`](#use-readonly)
    - 2.3.3 [Parameter properties](#parameter-properties)  
    - 2.3.4 [Field initializers](#field-initializers)  
    - 2.3.5 [Properties used outside of class lexical scope](#properties-used-outside-of-class-lexical-scope)  
    - 2.3.6 [Getters and Setters (Accessors)](#getters-and-setters-accessors)  
  - 2.4  [Primitive Types & Wrapper Classes](#primitive-types--wrapper-classes)  
  - 2.5  [Array constructor](#array-constructor)  
  - 2.6  [Type coercion](#type-coercion)  
  - 2.7  [Variables](#variables)  
  - 2.8  [Exceptions](#exceptions)  
  - 2.9  [Iterating objects](#iterating-objects)  
  - 2.10  [Iterating containers](#iterating-containers)  
  - 2.11 [Using the spread operator](#using-the-spread-operator)  
  - 2.12  [Control flow statements & blocks](#control-flow-statements--blocks)  
  - 2.13  [Switch Statements](#switch-statements)  
  - 2.14  [Equality Checks](#equality-checks)  
  - 2.15  [Function Declarations](#function-declarations)  
  - 2.16  [Function Expressions](#function-expressions)  
  - 2.17  [Use arrow functions in expressions](#use-arrow-functions-in-expressions)  
  - 2.18  [Expression bodies vs block bodies](#expression-bodies-vs-block-bodies)  
    - 2.18.1 [Rebinding `this`](#rebinding-this)
    - 2.18.2 [Arrow functions as properties](#arrow-functions-as-properties)
    - 2.18.3 [Event Handlers](#event-handlers)
  - 2.19  [Automatic Semicolon Insertion](#automatic-semicolon-insertion)  
  - 2.20  [@ts-ignore](#@ts-ignore)  
  - 2.21  [Type and Non-nullability Assertions](#type-and-non-nullability-assertions)  
    - 2.21.1 [Type Assertions Syntax](#type-assertions-syntax)  
    - 2.21.2 [Type Assertions and Object Literals](#type-assertions-and-object-literals)  
  - 2.22  [Member property declarations](#member-property-declarations)
    - 2.22.1 [Optimization compatibility for property access](#optimization-compatibility-for-property-access)  
  - 2.23  [Exception](#exception)  
  - 2.24  [Enums](#enums)  
  - 2.25  [Debugger statements](#debugger-statements)  
  - 2.26  [Decorators](#decorators)  
- 3 [Source Organization](#source-organization)  
  - 3.1 [Modules](#modules)  
    - 3.1.1 [Import Paths](#import-paths)  
    - 3.1.2 [Namespaces vs Modules](#namespaces-vs-modules)  
  - 3.2 [Exports](#exports)  
    - 3.2.1 [Export visibility](#export-visibility)  
    - 3.2.2 [Mutable Exports](#mutable-exports)  
    - 3.2.3 [Container Classes](#container-classes)  
  - 3.3 [Imports](#imports)  
    - 3.3.1 [Module versus destructuring imports](#module-versus-destructuring-imports)  
    - 3.3.2 [Renaming imports](#renaming-imports)  
    - 3.3.3 [Import & export type](#import--export-type)  
  - 3.4 [Organize By Feature](#organize-by-feature)  
  - 3.5 [Type System](#type-system)  
  - 3.6 [Type Inference](#type-inference)  
    - 3.6.1 [Return types](#return-types)  
    - 3.6.2 [Null vs Undefined](#null-vs-undefined)  
    - 3.6.3 [Nullable/undefined type aliases](#nullable/undefined-type-aliases)  
    - 3.6.4 [Optionals vs `|undefined` type](#optionals-vs-undefined-type)  
  - 3.7 [Structural Types vs Nominal Types](#structural-types-vs-nominal-types)  
  - 3.8 [Interfaces vs Type Aliases](#interfaces-vs-type-aliases)  
  - 3.9 [`Array<T>` Type](#arrayT-type)  
  - 3.10 [Indexable (`{[key: string]: number}`) Type](#indexable-key-string-number-type)  
  - 3.11 [Mapped & Conditional Types](#mapped--conditional-types)  
  - 3.12 [`any` Type](#any-type)  
    - 3.12.1 [Providing a more specific type](#providing-a-more-specific-type)  
    - 3.12.2 [Using `unknown` over `any`](#using-unknown-over-any`)  
    - 3.12.3 [Suppressing `any` lint warnings](#suppressing-any-lint-warnings)  
  - 3.13 [Tuple Types](#tuple-types)  
  - 3.14 [Wrapper types](#wrapper-types)  
  - 3.15 [Return type only generics](#return-type-only-generics)  
- 4 [Consistency](#consistency)  

## Syntax

### Identifiers

标识符只能使用ASCII字母、数字、下划线(用于常量和结构化测试方法名称)和$符号。 因此，每个有效的标识符名称都匹配正则表达式' [$\w]+ '。

| Style | Category |
| --- | --- |
| `UpperCamelCase` | class / interface / type / enum / decorator / type / parameters |
| `lowerCamelCase` | variable / parameter / function / method / property / module alias |
| `CONSTANT_CASE` | global constant values, including enum values |
| `#ident` | 不要使用私有标识符 |

- **Abbreviations**: 把缩写像名字中的首字母缩略词一样当成一个完整的单词 , 比如: 使用 `loadHttpUrl`, 不要用 ~`loadHTTPURL`~, 除非一些特定的纯大写标识 (比如 `XMLHttpRequest`).
- **Dollar sign**: 变量标识不要使用 `$`, 除非与第三方框架的命名约定保持一致.
- **Type parameters**: 类型参数, 比如 `Array<T>`, 可以使用单个大写字符 (`T`) 或者 `UpperCamelCase`.
- **`_` prefix/suffix**: 标识符不要使用` _ `作为前缀或后缀.

  > Tip: 如果只需要数组(或TypeScript元组)中的一些元素，可以在解构语句中插入额外的逗号来忽略中间元素:
  
  ```ts
  const [a, , b] = [1, 5, 10]; // a <- 1, b <- 10
  ```

- **Imports**: 模块的命名空间, 引入对象 使用 `lowerCamelCase` 而文件名采用 `snake_case`, 比如:

    ```ts
    import * as fooBar from "./foo_bar";
    ```

一些库通常可能会使用违反这种命名模式的名称空间导入前缀，但常见的开源库使用会使这种违反的样式更具可读性. 例如,目前属于这种例外的库是:

- jquery, using the `$` prefix
- threejs, using the `THREE` prefix

- **Constants**: `CONSTANT_CASE` 标识变量不能被修改.例如:

    ```ts
    const UNIT_SUFFIXES = {
      milliseconds: "ms",
      seconds: "s",
    };
    // Even though per the rules of JavaScript UNIT_SUFFIXES is
    // mutable, the uppercase shows users to not modify it.
    ```

    常量也可以是类的`静态只读`属性

    ```ts
    class Foo {
      private static readonly MY_SPECIAL_NUMBER = 5;

      bar() {
        return 2 * Foo.MY_SPECIAL_NUMBER;
      }
    }
    ```

    如果一个值在程序的生命周期内可以被实例化不止一次，或者如果用户以任何方式改变它, 必须使用`lowerCamelCase`命名.

    如果值是实现接口的箭头函数, 也要使用`lowerCamelCase`命名.

- **React Components**: 当组件使用 JSX 时, 必须是 `UpperCamelCase` 命名方式, 无论组件是如何实现的 (比如: function, es6 class, `React.createClass`).

    ```ts
    // Good
    function GoLink(props: GoLinkProps) {
      return <a href={...}>...;
    }

    class MyDialog extends React.Component {
      ...
    }

    // Bad
    // Lowercased functions cannot be used as JSX elements because the
    // transpiler cannot distinguish them from HTML tags.
    function goLink(props: GoLinkProps) {
      return <a href={...}>...;
    }
    ```

#### Aliases

当创建本地作用域的别名时, 使用被起别名对象的命名格式. 本地别名必须与源的现有名称和格式匹配. 对于变量，请使用' const '作为局部别名，对于类字段，请使用' readonly '属性。 .

```ts
const { Foo } = SomeType;
const CAPACITY = 5;

class Teapot {
    readonly BrewStateEnum = BrewStateEnum;
    readonly CAPACITY = CAPACITY;
}
```

#### Naming style

TypeScript用类型来表达信息，所以名字*不应该*用类型中包含的信息来装饰

这条规则的一些具体例子:

- 不要为私有属性或方法使用尾随或前导下划线
- 不使用`opt_`等作为可选参数的前缀.
  - 对于访问器，请参阅下面的访问器规则.
- 不要对接口使用特殊的标记 (~~`IMyInterface`~~ 或 ~~`MyFooInterface`~~) 除非它定义的环境中有特定的含义. 当为类引入接口时, 要通过类名表达为什么要实现这个接口 (比如: `class TodoItem` 和 `interface TodoItemStorage` (接口用于存储/序列化等功能)).
- `Observable`后面加上`$`是一种常见的外部约定，可以帮助解决Observable值和具体值之间的混淆.

#### Descriptive names

名称必须对研发人员具有描述性和清晰性. 不要使用模棱两可或对项目外的读者不熟悉的缩写，也不要通过删除单词中的字母来缩写.

- **例外情况**: 在10行或更少范围内的变量，包括不属于导出API的参数，可以使用较短的(例如单字母)变量名.

### File encoding: UTF-8

对于非ascii字符，使用实际的Unicode字符 (例如: `∞`). 对于不可打印字符，等效的十六进制或Unicode转义 (例如: `\u221e`) 可以与解释性注释一起使用.

```ts
// Good
// Perfectly clear, even without a comment.
const units = "μs";

// Use escapes for non-printable characters.
const output = "\\ufeff" + content; // byte order mark

// Bad
// Hard to read and prone to mistakes, even with the comment.
const units = "\\u03bcs"; // Greek letter mu, 's'

// The reader has no idea what this is.
const output = "\\ufeff" + content;
```

### Source Code Formatting

代码格式化可以完全自动化. 我们不应该浪费时间争论代码检查中逗号的位置等情况.

### Comments & Documentation

#### JSDoc vs comments

有两种类型的注释, JSDoc (`/** ... _/`) 和 普通注释 (`// ...` or `/_ ... \*/`).

- 使用 `/** JSDoc _/` 为文档注释, 比如: 代码用户应该阅读的注释
- 使用 `// line comments` 为实现注释, 比如: 只涉及代码本身实现的注释

```ts
// Bad
const active: boolean = true;  // is current tab

// Good
// is current tab
const active: boolean = true;
```

JSDoc注释可以被工具(比如编辑器和文档生成器)理解，而普通注释只供其他人使用。

#### JSDoc rules follow the JavaScript style

一般来说，遵循JSDoc的JavaScript样式指南规则.本节的其余部分将描述这些规则的例外情况.

#### Document all top-level exports of modules

使用`/** JSDoc _/`注释将信息传达给代码的用户. 避免仅仅重述属性或参数名称. 所以需要将导出的模块使用`/** JSDoc _/`注释.

#### Omit comments that are redundant with TypeScript

例如, 不要在 `@param` 或 `@return` 块中声明类型, 不要在使用 `implements`, `enum`, `private` 等关键字的代码上写入 `@implements`, `@enum`, `@private` 等关键字.

#### Do not use `@override`

不要在TypeScript代码中使用 `@override`.

`@override` 导致注释和实现不同步. 纯粹出于文档目的而包含它不是很好.

#### Make comments that actually add information

对于未导出的方法或者对象, 函数或参数额名称和类型就足够了.

- 只重复参数名称和类型的注释无效，例如:

    ```ts
    // Bad
    /** @param fooBarService The Bar service for the Foo application. */
    ```

- 因此 `@param` 和 `@return` 行只有在添加信息时才需要，否则可能会被省略.

    ```ts
    // Good
    /**
     * POSTs the request to start coffee brewing.
     * @param amountLitres The amount to brew. Must fit the pot size!
     */
    brew(amountLitres: number, logger: Logger) {
      // ...
    }
    ```

#### Parameter property comments

参数属性是当类在单个声明中声明一个字段和一个构造函数参数时，通过在构造函数中标记一个参数. 例如: `constructor(private readonly foo: Foo)`,声明该类有一个 `foo` 字段.

让这些字段文档化, 使用 JSDocs `@param` 声明. 编辑器在构造函数调用和属性访问上显示描述.

```ts
/** This class demonstrates how parameter properties are documented. */
class ParamProps {
  /**
   * @param percolator The percolator used for brewing.
   * @param beans The beans to brew.
   */
  constructor(
    private readonly percolator: Percolator,
    private readonly beans: CoffeeBean[]
  ) {}
}

/** This class demonstrates how ordinary fields are documented. */
class OrdinaryClass {
    /** The bean that will be used in the next call to brew(). */
    nextBean: CoffeeBean;

    constructor(initialBean: CoffeeBean) {
    this.nextBean = initialBean;
}
}
```

#### Comments when calling a function

如果需要，使用块注释内联调用站点的文档参数。
还要考虑使用对象字面量和解构的命名参数。 注释的确切格式和位置没有规定。

```ts
// Inline block comments for parameters that'd be hard to understand:
new Percolator().brew(/_ amountLitres= _/ 5);
// Also consider using named arguments and destructuring parameters (in brew's declaration):
new Percolator().brew({ amountLitres: 5 });
```

```ts
/** An ancient {@link CoffeeBrewer} */
export class Percolator implements CoffeeBrewer {
  /**
   * Brews coffee.
   * @param amountLitres The amount to brew. Must fit the pot size!
   */
  brew(amountLitres: number) {
    // This implementation creates terrible coffee, but whatever.
    // TODO(b/12345): Improve percolator brewing.
  }
}
```

#### Place documentation prior to decorators

当一个类、方法或属性同时拥有 `@Component` 和 JsDoc 这样的装饰器时, 请确保在装饰器之前写JsDoc。 .

- 不要在Decorator和Decorator语句之间写JsDoc。

    ```ts
    // Bad
    @Component({
      selector: "foo",
      template: "bar",
    })
    /** Component that prints "bar". */
    export class FooComponent {}
    ```

- 在Decorator之前编写JsDoc块

    ```ts
    // Good
    /** Component that prints "bar". */
    @Component({
      selector: "foo",
      template: "bar",
    })
    export class FooComponent {}
    ```

## Language Rules

### Visibility

限制属性、方法和整个类型的可见性有助于保持代码的解耦.

- 尽可能的限制 symbol 的可见性.
- 考虑将私有方法转换为同一文件中任何类之外的非导出函数，并将私有属性移动到单独的非导出类中.
- TypeScript symbols 默认是公共的. 永远不要使用 `public` 修饰符，除非在构造函数中声明非只读公共参数属性.

    ```ts
    class Foo {
        public bar = new Bar(); // BAD: public modifier not needed

        constructor(public readonly baz: Baz) {} // BAD: readonly implies it's a property which defaults to public
    }
    ```

    ```ts
    class Foo {
        bar = new Bar(); // GOOD: public modifier not needed

        constructor(public baz: Baz) {} // public modifier allowed
    }
    ```

下方 [export visibility](#Export-visibility).

### Constructors

即使没有传递参数, 构造函数调用也必须使用圆括号:

  ```ts
  // Bad
  const x = new Foo;
  ```

  ```ts
  // Good
  const x = new Foo();
  ```

请省略掉空的构造函数或只委托给父类的构造函数，因为如果没有指定，ES2015会提供默认的类构造函数.
但是，即使构造函数体为空，也不应该省略带有形参属性、修饰符或形参修饰符的构造函数.

  ```ts
  // Bad
  class UnnecessaryConstructor {
      constructor() {}
  }
  ```

  ```ts
  // Bad
  class UnnecessaryConstructorOverride extends Base {
      constructor(value: number) {
      super(value);
      }
  }
  ```

  ```ts
  // Good
  class DefaultConstructor {}

  class ParameterProperties {
      constructor(private myService) {}
  }

  class ParameterDecorators {
      constructor(@SideEffectDecorator myService) {}
  }

  class NoInstantiation {
      private constructor() {}
  }
  ```

### Class Members

#### No `#private` fields

不要使用#作为私有字段的标识符:

  ```ts
  // Bad
  class Clazz {
      #ident = 1;
  }
  ```

显式声明private字段:

  ```ts
  // Good
  class Clazz {
      private ident = 1;
  }
  ```

#### Use `readonly`

使用 `readonly` 修饰符标记从未在构造函数外部重新赋值的属性(这些不需要是深度不可变的)

#### Parameter properties

不要把一个明显的初始化式传递给类成员，而是使用TypeScript的形参属性。

```ts
// Bad
class Foo {
  private readonly barService: BarService;

  constructor(barService: BarService) {
      this.barService = barService;
  }
}
```

```ts
// Good
class Foo {
    constructor(private readonly barService: BarService) {}
}
```

如果参数属性需要文档, 使用`@param` [JSDoc](#parameter-property-comments) 标记.

#### Field initializers

如果类成员不是形参，则在声明它的地方初始化它，因为这有时可以让你完全删除构造函数。

```ts
// Bad
class Foo {
  private readonly userList: string[];
  constructor() {
      this.userList = [];
  }
}
```

```ts
// Good
class Foo {
    private readonly userList: string[] = [];
}
```

#### Properties used outside of class lexical scope

在其包含类的词法作用域之外使用的属性，比如从模板中使用的AngularJS控制器属性，不能使用`private`可见性，因为它们是在其包含类的词法作用域之外使用的.

这些属性最好是 `public` 的, 但是 `protected` 也可以根据需要使用. 比如, Angular 和 Polymer 模板属性应该使用 `public`, 而 AngularJS 应该使用 `protected`.

#### Getters and Setters (Accessors)

通常避免使用getter和setter是一个很好的实践。

可以使用类成员的getter和setter。 getter方法必须是一个纯函数(即，结果是一致的，没有副作用)。 它们还可以用来限制内部或详细实现细节的可见性(如下所示)。

```ts
class Foo {
  constructor(private readonly someService: SomeService) {}

  get someMember(): string {
      return this.someService.someVariable;
  }

  set someMember(newValue: string) {
      this.someService.someVariable = newValue;
  }
}
```

声明访问器的作用尽量不只是为了保护一个属性的私密性,而且提供更丰富的访问器能力.如果单纯为了保护属性的可访问性, 使用`private`或者`public`即可.如下所示:

```ts
// Good
class Foo {
  private wrappedBar = "";
  get bar() {
      return this.wrappedBar || "bar";
  }

  set bar(wrapped: string) {
      this.wrappedBar = wrapped.trim();
  }
}
```

```ts
// Bad
class Bar {
    private barInternal = "";

  // Neither of these accessors have logic, so just make bar public.
  get bar() {
      return this.barInternal;
  }

  set bar(value: string) {
      this.barInternal = value;
  }
}
```

### Primitive Types & Wrapper Classes

> 不要实例化TypeScript代码原始类型 `String`, `Boolean`, 和 `Number` 的包装类.

```ts
// Bad
const s = new String("hello");
const b = new Boolean(false);
const n = new Number(5);
```

```ts
// Good
const s = "hello";
const b = false;
const n = 5;
```

### Array constructor

不要使用 `Array()` 构造函数, 无论有还是没有 `new`. 它有令人困惑和矛盾的用法:

```ts
// Bad
const a = new Array(2); // [undefined, undefined]
const b = new Array(2, 3); // [2, 3];
```

相反，总是使用括号表示法来初始化数组, 或者 使用 `from` 去初始化一个长度固定的 `Array` :

```ts
// Good
const a = [2];
const b = [2, 3];

// Equivalent to Array(2):
const c = [];
c.length = 2;

// [0, 0, 0, 0, 0]
Array.from<number>({ length: 5 }).fill(0);
```

### Type coercion

可以使用 `String()` 和 `Boolean()` (注意: 没有 `new`!) 函数, 字符串字面量模板, 或者 `!!` 强制类型.

```ts
const bool = Boolean(false);
const str = String(aNumber);
const bool2 = !!str;
const str2 = `result: ${bool2}`;
```

不要使用字符串连接(`'xxx' + 'xxxx'`)强制转换为string，因为要检查加号操作符的操作数是否具有匹配的类型。

使用 `Number()`去转换数字类型, 并且*必须*显式地检查其返回的“NaN”值，除非从上下文不可能解析失败.

注意: `Number()`, `Number( )`, 和 `Number(\t)` 将会返回 `0` 而不是 `NaN`. `Number(Infinity)` 和 `Number(-Infinity)` 将会分别返回`Infinity` 和 `-Infinity`. 这些情况可能需要特殊处理.

```ts
const aNumber = Number('123');
if (isNaN(aNumber)) throw new Error(...); // Handle NaN if the string might not contain a number
assertFinite(aNumber, ...); // Optional: if NaN cannot happen because it was validated before.
```

不要使用意愿加号 (`+`) 将字符串强制转换为数字.

```ts
// Bad
const x = +y;
```

也不要使用 `parseInt` 或者 `parseFloat` 去转换数字. 这两个函数都忽略字符串中的末尾字符.

```ts
//bad
const n = parseInt(someString, 10); // Error prone,
const f = parseFloat(someString); // regardless of passing a radix.
```

如果要使用`parseInt`转换进制, 必须先检查转换字符串中是否包含字符.

```ts
// Good
if (!/^[a-fA-F0-9]+$/.test(someString)) throw new Error(...);
// Needed to parse hexadecimal.
// tslint:disable-next-line:ban
const n = parseInt(someString, 16); // Only allowed for radix != 10
```

使用 `Number()` 紧跟着 `Math.floor` 或者 `Math.trunc` 来转换整型数字:

```ts
// Good
let f = Number(someString);
if (isNaN(f)) handleError();
f = Math.floor(f);
```

不要在含有隐式布尔强制的条件从句中使用显式布尔强制转换`!!`. 比如在 `if`, `for` 和 `while` 语句中的条件.

```ts
// Bad
const foo: MyInterface|null = ...;
if (!!foo) {...}
while (!!foo) {...}
```

```ts
// Good
const foo: MyInterface|null = ...;
if (foo) {...}
while (foo) {...}
```

代码应该使用显式比较:

```ts
// Explicitly comparing > 0 is OK:
if (arr.length > 0) {...}
// so is relying on boolean coercion:
if (arr.length) {...}
```

### Variables

永远使用 `const` 或 `let` 来声明变量. 默认使用 `const`, 除非变量要被重新赋值. 否则不要使用 `var`.

```ts
const foo = otherValue; // Use if "foo" never changes.
let bar = someValue; // Use if "bar" is ever assigned into later on.
```

`const` 和 `let` 是块级作用域. `var` 在JS中是函数级作用域, 这可能导致难以理解的bug.

```ts
var foo = someValue; // Don't use - var scoping is complex and causes bugs.
```

变量在声明之前不能使用

### Exceptions

实例化一个异常的时候应该总是使用 `new Error()`, 不要去调用 `Error()`方法. 这两种形式都是去创建一个 `Error` 实例, 但使用 `new` 与其他实例化对象的方式更加一致.

```ts
// Good
throw new Error("Foo is not a valid bar.");
```

```ts
// Bad
throw Error("Foo is not a valid bar.");
```

### Iterating objects

迭代对象使用 `for (... in ...)` 容易出错. 它将包括`prototype`中的可枚举属性

不要使用未过滤原型链的 `for (... in ...)` 语句:

```ts
// Bad
for (const x in someObj) {
  // x could come from some parent prototype!
}
```

使用 `if` 语句显式地筛选值 , 或者使用 `for (... of Object.keys(...))`.

```ts
// Good
for (const x in someObj) {
  if (!someObj.hasOwnProperty(x)) continue;
  // now x was definitely defined on someObj
}
for (const x of Object.keys(someObj)) {
  // note: for _of_!
  // now x was definitely defined on someObj
}
for (const [key, value] of Object.entries(someObj)) {
  // note: for _of_!
  // now key was definitely defined on someObj
}
```

### Iterating containers

不要使用 `for (... in ...)` 去迭代数组. 它将给数组提供索引(作为字符串!)，而不是值的索引:

```ts
// Bad
for (const x in someArray) {
    // x is the index!
}
```

使用 `for (... of someArr)`或者 `for` 带索引的循环遍历数组.

```ts
// Good
for (const x of someArr) {
  // x is a value of someArr.
}

for (let i = 0; i < someArr.length; i++) {
  // Explicitly count if the index is needed, otherwise use the for/of form.
  const x = someArr[i];
  // ...
}
for (const [i, x] of someArr.entries()) {
  // Alternative version of the above.
}
```

不要使用 `Array.prototype.forEach`, `Set.prototype.forEach`, 和 `Map.prototype.forEach`. 它们使代码更难调试，并破坏了一些有用的编译器检查(例如可达性).

```ts
// Bad
someArr.forEach((item, index) => {
    someFn(item, index);
});
```

> 为什么?
>
> 看下这段代码:
>
> ```ts
> let x: string | null = "abc";
> myArray.forEach(() => {
>     x.charAt(0);
> });
> ```
>
> `x` 不是 null 而在它被访问之前不会改变. 但是编译器无法知道这个 `.forEach()` 调用没有挂起传递进来的闭包，并在稍后的某个点上调用它, 可能是在 `x` 被设为null之后, 所以编译器会将此代码标记为错误。 而使用for-of循环就不会被标记为错误了
>
> [See the error and non-error in the playground](https://www.typescriptlang.org/play/?#code/DYUwLgBAHgXBDOYBOBLAdgcwD5oK7GAgF4IByAQwCMBjUgbgCgBtAXQDoAzAeyQFFzqACwAUwgJTEAfBADeDCNDZDySAIJhhABjGMAvjoYNQkAJ5xEqTDnyESFGvQbckEYdS5pEEAPoQuHCFYJOQUTJUEVdS0DXQYgA)
>
> 在实践中，控制流分析的这种限制的变化出现在更复杂的代码路径中

### Using the spread operator

使用拓展运算符 `[...foo]; {...bar}` 去浅克隆数组和对象. When using the spread operator on objects, later values replace earlier values at the same key.

```ts
const foo = {
  num: 1,
};

const foo2 = {
  ...foo,
  num: 5,
};

const foo3 = {
  num: 5,
  ...foo,
};

foonum === 5;
foonum === 1;
```

When using the spread operator, the value being spread must match what is being created. That is, when creating an object, only objects may be used with the spread operator; when creating an array, only spread iterables. Primitives, including `null` and `undefined`, may never be spread.

```ts
// Bad
const foo = { num: 7 };
const bar = { num: 5, ...(shouldUseFoo && foo) }; // might be undefined

const fooStrings = ["a", "b", "c"]; // Creates {0: 'a', 1: 'b', 2: 'c'} but has no length
const ids = { ...fooStrings };
```

```ts
// Good
const foo = shouldUseFoo ? { num: 7 } : {};
const bar = { num: 5, ...foo };
const fooStrings = ["a", "b", "c"];
const ids = [...fooStrings, "d", "e"];
```

### Control flow statements & blocks

Control flow statements spanning multiple lines always use blocks for the containing code.

```ts
// Good
for (let i = 0; i < x; i++) {
  doSomethingWith(i);
  andSomeMore();
}
if (x) {
  doSomethingWithALongMethodName(x);
}
```

```ts
// Bad
if (x) x.doFoo();
for (let i = 0; i < x; i++) doSomethingWithALongMethodName(i);
```

The exception is that `if` statements fitting on one line may elide the block.

```ts
if (x) x.doFoo();
```

### Switch Statements

All `switch` statements must contain a `default` statement group, even if it contains no code.

```ts
switch (x) {
  case Y:
    doSomethingElse();
  break;
  default:
  // nothing to do.
}
```

Non-empty statement groups (`case ...`) may not fall through (enforced by the compiler):

```ts
// Bad
switch (x) {
  case X:
    doSomething();
  // fall through - not allowed!
  case Y:
  // ...
}
```

Empty statement groups are allowed to fall through:

```ts
switch (x) {
  case X:
  case Y:
    doSomething();
  break;
  default: // nothing to do.
}
```

### Equality Checks

Always use triple equals (`===`) and not equals (`!==`). The double equality operators cause error prone type coercions that are hard to understand and slower to implement for JavaScript Virtual Machines. See also the JavaScript equality table.

```ts
// Bad
if (foo == "bar" || baz != bam) {
  // Hard to understand behaviour due to type coercion.
}
```

```ts
// Good
if (foo === "bar" || baz !== bam) {
  // All good here.
}
```

**Exception**: Comparisons to the literal `null` value may use the `==` and `!=` operators to cover both `null` and `undefined` values.

```ts
if (foo == null) {
  // Will trigger when foo is null or undefined.
}
```

### Function Declarations

Use`function foo() { ... }` to declare named functions, including functions in nested scopes, e.g. within another function.

Use function declarations instead of assigning a function expression into a local variable (~~`const x = function() {...};`~~). TypeScript already disallows rebinding functions, so preventing overwriting a function declaration by using `const` is unnecessary.

Exception: Use arrow functions assigned to variables instead of function declarations if the function accesses the outer scopes `this`.

```ts
function foo() { ... }
```

```ts
// Bad
// Given the above declaration, this won't compile:
foo = () => 3; // ERROR: Invalid left-hand side of assignment expression.

// So declarations like this are unnecessary.
const foo = function() { ... }
```

Note the difference between function declarations (`function foo() {}`) discussed here, and function expressions (~~`doSomethingWith(function() {});`~~) discussed below.

Top level arrow functions *may* be used to explicitly declare that a function implements an interface.

```ts
interface SearchFunction {
  (source: string, subString: string): boolean;
}

const fooSearch: SearchFunction = (source, subString) => { ... };
```

### Function Expressions

#### Use arrow functions in expressions

Always use arrow functions instead of pre-ES6 function expressions defined with the `function` keyword.

```ts
// Good
bar(() => {
  this.doSomething();
});
```

```ts
// Bad
bar(function() { ... })
```

Function expressions (defined with the `function` keyword) may only be used if code has to dynamically rebind the `this` pointer, but code *should not* rebind the `this` pointer in general. Code in regular functions (as opposed to arrow functions and methods) *should not* access `this`.

#### Expression bodies vs block bodies

Use arrow functions with expressions or blocks as their body as appropriate.

```ts
// Top level functions use function declarations.
function someFunction() {
  // Block arrow function bodies, i.e. bodies with => { }, are fine:
  const receipts = books.map((b: Book) => {
    const receipt = payMoney(b.price);
    recordTransaction(receipt);
    return receipt;
  });

// Expression bodies are fine, too, if the return value is used:
  const longThings = myValues
    .filter((v) => v.length > 1000)
    .map((v) => String(v));

  function payMoney(amount: number) {
    // function declarations are fine, but don't access `this` in them.
  }
}
```

Only use an expression body if the return value of the function is actually used.

```ts
// BAD: use a block ({ ... }) if the return value of the function is not used.
myPromise.then((v) => console.log(v));
```

```ts
// GOOD: return value is unused, use a block body.
myPromise.then((v) => {
  console.log(v);
});
// GOOD: code may use blocks for readability.
const transformed = [1, 2, 3].map((v) => {
  const intermediate = someComplicatedExpr(v);
  const more = acrossManyLines(intermediate);
  return worthWrapping(more);
});
```

#### Rebinding `this`

Function expressions must not use `this` unless they specifically exist to rebind the `this` pointer. Rebinding `this` can in most cases be avoided by using arrow functions or explicit parameters.

```ts
function clickHandler() {
  // Bad: what's `this` in this context?
  this.textContent = "Hello";
}
// Bad: the `this` pointer reference is implicitly set to document.body.
document.body.onclick = clickHandler;
```

```ts
// Good: explicitly reference the object from an arrow function.
document.body.onclick = () => {
  document.body.textContent = "hello";
};
// Alternatively: take an explicit parameter
const setTextFn = (e: HTMLElement) => {
  e.textContent = "hello";
};
document.body.onclick = setTextFn.bind(null, document.body);
```

#### Arrow functions as properties

Classes usually *should not* contain properties initialized to arrow functions. Arrow function properties require the calling function to understand that the callees `this` is already bound, which increases confusion about what `this` is, and call sites and references using such handlers look broken (i.e. require non-local knowledge to determine that they are correct). Code *should* always use arrow functions to call instance methods (`const handler = (x) => { this.listener(x); };`), and *should not* obtain or pass references to instance methods (~~`const handler = this.listener; handler(x);`~~).

Note: in some specific situations, e.g. when binding functions in a template, arrow functions as properties are useful and create much more readable code. Use judgement with this rule. Also, see the `Event Handlers` section below.

```ts
// Bad
class DelayHandler {
  constructor() {
    // Problem: `this` is not preserved in the callback. `this` in the callback
    // will not be an instance of DelayHandler.
    setTimeout(this.patienceTracker, 5000);
  }
  private patienceTracker() {
    this.waitedPatiently = true;
  }
}
```

```ts
// Arrow functions usually should not be properties.
class DelayHandler {
  constructor() {
    // Bad: this code looks like it forgot to bind `this`.
    setTimeout(this.patienceTracker, 5000);
  }
  private patienceTracker = () => {
    this.waitedPatiently = true;
  };
}
```

```ts
// Good
// Explicitly manage `this` at call time.
class DelayHandler {
  constructor() {
    // Use anonymous functions if possible.
    setTimeout(() => {
      this.patienceTracker();
    }, 5000);
  }
  private patienceTracker() {
    this.waitedPatiently = true;
  }
}
```

#### Event Handlers

Event handlers *may* use arrow functions when there is no need to uninstall the handler (for example, if the event is emitted by the class itself). If the handler must be uninstalled, arrow function properties are the right approach, because they automatically capture `this` and provide a stable reference to uninstall.

```ts
// Event handlers may be anonymous functions or arrow function properties.
class Component {
  onAttached() {
    // The event is emitted by this class, no need to uninstall.
    this.addEventListener("click", () => {
      this.listener();
    });
    // this.listener is a stable reference, we can uninstall it later.
    window.addEventListener("onbeforeunload", this.listener);
  }
  onDetached() {
    // The event is emitted by window. If we don't uninstall, this.listener will
    // keep a reference to `this` because it's bound, causing a memory leak.
    window.removeEventListener("onbeforeunload", this.listener);
  }
  // An arrow function stored in a property is bound to `this` automatically.
  private listener = () => {
    confirm("Do you want to exit the page?");
  };
}
```

Do not use `bind` in the expression that installs an event handler, because it creates a temporary reference that cant be uninstalled.

```ts
// Bad
// Binding listeners creates a temporary reference that prevents uninstalling.
class Component {
  onAttached() {
    // This creates a temporary reference that we won't be able to uninstall
    window.addEventListener("onbeforeunload", this.listener.bind(this));
  }
  onDetached() {
    // This bind creates a different reference, so this line does nothing.
    window.removeEventListener("onbeforeunload", this.listener.bind(this));
  }
  private listener() {
    confirm("Do you want to exit the page?");
  }
}
```

### Automatic Semicolon Insertion

Do not rely on Automatic Semicolon Insertion (ASI). Explicitly terminate all statements using a semicolon. This prevents bugs due to incorrect semicolon insertions and ensures compatibility with tools with limited ASI support (e.g. clang-format).

### @ts-ignore

Do not use `@ts-ignore`. It superficially seems to be an easy way to "fix" a compiler error, but in practice, a specific compiler error is often caused by a larger problem that can be fixed more directly.

For example, if you are using `@ts-ignore` to suppress a type error, then its hard to predict what types the surrounding code will end up seeing. For many type errors, the advice in how to best use `any` is useful.

### Type and Non-nullability Assertions

Type assertions (`x as SomeType`) and non-nullability assertions (`y!`) are unsafe. Both only silence the TypeScript compiler, but do not insert any runtime checks to match these assertions, so they can cause your program to crash at runtime.

Because of this, you *should not* use type and non-nullability assertions without an obvious or explicit reason for doing so.

Instead of the following:

```ts
// Bad
(x as Foo).foo();

y!.bar();
```

When you want to assert a type or non-nullability the best answer is to explicitly write a runtime check that performs that check.

```ts
// Good
// assuming Foo is a class.
if (x instanceof Foo) {
  x.foo();
}

if (y) {
  y.bar();
}
```

Sometimes due to some local property of your code you can be sure that the assertion form is safe. In those situations, you *should* add clarification to explain why you are ok with the unsafe behavior:

```ts
// x is a Foo, because ...
(x as Foo).foo();

// y cannot be null, because ...
y!.bar();
```

If the reasoning behind a type or non-nullability assertion is obvious, the comments may not be necessary. For example, generated proto code is always nullable, but perhaps it is well-known in the context of the code that certain fields are always provided by the backend. Use your judgement.

#### Type Assertions Syntax

> Type assertions must use the `as` syntax (as opposed to the angle brackets syntax). This enforces parentheses around the assertion when accessing a member.

```ts
// Bad
const x = (<Foo>z).length;
const y = z.length;
```

```ts
// Good
const x = (z as Foo).length;
```

#### Type Assertions and Object Literals

Use type annotations (`: Foo`) instead of type assertions (`as Foo`) to specify the type of an object literal. This allows detecting refactoring bugs when the fields of an interface change over time.

```ts
// Bad
interface Foo {
  bar: number;
  baz?: string; // was "bam", but later renamed to "baz".
}

const foo = {
  bar: 123,
  bam: "abc", // no error!
} as Foo;

function func() {
  return {
    bar: 123,
    bam: "abc", // no error!
  } as Foo;
}
```

```ts
// Good
interface Foo {
  bar: number;
  baz?: string;
}

const foo: Foo = {
  bar: 123,
  bam: "abc", // complains about "bam" not being defined on Foo.
};

function func(): Foo {
  return {
    bar: 123,
    bam: "abc", // complains about "bam" not being defined on Foo.
  };
}
```

### Member property declarations

Interface and class declarations must use the `;` character to separate individual member declarations:

```ts
interface Foo {
  memberA: string;
  memberB: number;
}
```

Interfaces specifically must not use the `,` character to separate fields, for symmetry with class declarations:

```ts
// Bad
interface Foo {
  memberA: string,
  memberB: number,
}
```

> Inline object type declarations must use the comma as a separator:

```ts
type SomeTypeAlias = {
  memberA: string;
  memberB: number;
};

let someProperty: { memberC: string; memberD: number };
```

#### Optimization compatibility for property access

Code must not mix quoted property access with dotted property access:

```ts
// Bad: code must use either non-quoted or quoted access for any property
// consistently across the entire application:
console.log(x["someField"]);
console.log(x.someField);
```

```ts
// Good: declaring an interface
declare interface ServerInfoJson {
  appVersion: string;
  user: UserJson;
}
const data = JSON.parse(serverResponse) as ServerInfoJson;
console.log(data.appVersion); // Type safe & renaming safe!
```

#### Optimization compatibility for module object imports

When importing a module object, directly access properties on the module object rather than passing it around. This ensures that modules can be analyzed and optimized. Treating module imports as namespaces is fine.

```ts
// Good
import { method1, method2 } from "utils";
class A {
  readonly utils = { method1, method2 };
}
```

```ts
// Bad
import _ as utils from "utils";
class A {
  readonly utils = utils;
}
```

### Exception

This optimization compatibility rule applies to all web apps. It does not apply to code that only runs server side (e.g. in NodeJS for a test runner). It is still strongly encouraged to always declare all types and avoid mixing quoted and unquoted property access, for code hygiene.

### Enums

Always use `enum` and not `const enum`. TypeScript enums already cannot be mutated; `const enum` is a separate language feature related to optimization that makes the enum invisible to JavaScript users of the module.

### Debugger statements

Debugger statements must not be included in production code.

```ts
// Bad
function debugMe() {
  debugger;
}
```

### Decorators

Decorators are syntax with an `@` prefix, like `@MyDecorator`.

Do not define new decorators. Only use the decorators defined by frameworks:

- Angular (e.g. `@Component`, `@NgModule`, etc.)
- Polymer (e.g. `@property`)

> Why?
>
> We generally want to avoid decorators, because they were an experimental feature that have since diverged from the TC39 proposal and have known bugs that wont be fixed.
>
> When using decorators, the decorator must immediately precede the symbol it decorates, with no empty lines between:
>
> ```ts
> /** JSDoc comments go before decorators */
> @Component({...}) // Note: no empty line after the decorator.
> class MyComp {
>   @Input() myField: string; // Decorators on fields may be on the same line...
> 
>   @Input()
>   myOtherField: string; // ... or wrap.
> }

```

## Source Organization

### Modules

#### Import Paths

TypeScript code must use paths to import other TypeScript code. Paths may be relative, i.e. starting with `.` or `..`, or rooted at the base directory, e.g. `root/path/to/file`.

Code _should_ use relative imports (`./foo`) rather than absolute imports `path/to/foo` when referring to files within the same (logical) project.

Consider limiting the number of parent steps (`../../../`) as those can make module and path structures hard to understand.

```ts
import { Symbol1 } from "google3/path/from/root";
import { Symbol2 } from "../parent/file";
import { Symbol3 } from "./sibling";
```

#### Namespaces vs Modules

TypeScript supports two methods to organize code: *namespaces* and *modules*, but namespaces are disallowed. google3 code must use TypeScript *modules* (which are ECMAScript 6 modules). That is, your code *must* refer to code in other files using imports and exports of the form `import {foo} from bar;`

Your code must not use the `namespace Foo { ... }` construct. `namespace`s may only be used when required to interface with external, third party code. To semantically namespace your code, use separate files.

Code must not use `require` (as in `import x = require(...);`) for imports. Use ES6 module syntax.

```ts
// Bad: do not use namespaces:
namespace Rocket {
  function launch() { ... }
}

// Bad: do not use <reference>
/// 

// Bad: do not use require()
import x = require('mydep');
```

NB: TypeScript `namespace`s used to be called internal modules and used to use the `module` keyword in the form `module Foo { ... }`. Dont use that either. Always use ES6 imports.

### Exports

Use named exports in all code:

```ts
// Good
// Use named exports:
export class Foo { ... }
```

Do not use default exports. This ensures that all imports follow a uniform pattern.

```ts
// Bad
// Do not use default exports:
export default class Foo { ... } // BAD!
```

> Why?
>
> Default exports provide no canonical name, which makes central maintenance difficult with relatively little benefit to code owners, including potentially decreased readability:
>
> ```ts
> // Bad
> import Foo from "./bar"; // Legal.
> import Bar from "./bar"; // Also legal.
> ```
>
> Named exports have the benefit of erroring when import statements try to import something that hasnt been declared. In `foo.ts`:
>
> ```ts
> // Bad
> const foo = "blah";
> export default foo;
> ```
>
> And in `bar.ts`:
>
> ```ts
> // Bad
> import { fizz } from "./foo";
> ```
>
> Results in
>
> ```ts
> // Bad
> error TS2614: Module "./foo" has no exported member fizz.
> ```
>
> While `bar.ts`:
>
> ```ts
> // Bad
> import fizz from "./foo";
> ```
>
> Results in `fizz === foo`, which is probably unexpected and difficult to debug.
>
> Additionally, default exports encourage people to put everything into one big object to namespace it all together:
>
> ```ts
> // Bad
> export default class Foo {
> static SOME*CONSTANT = ...
> static someHelpfulFunction() { ... }
>   ...
> }
> ```
>
> With the above pattern, we have file scope, which can be used as a namespace. We also have a perhaps needless second scope (the class `Foo`) that can be ambiguously used as both a type and a value in other files.
>
> Instead, prefer use of file scope for namespacing, as well as named exports:
>
> ```ts
> export const SOME_CONSTANT = ...
> export function someHelpfulFunction()
> export class Foo {
>   // only class stuff here
> }
> ```

#### Export visibility

TypeScript does not support restricting the visibility for exported symbols. Only export symbols that are used outside of the module. Generally minimize the exported API surface of modules.

#### Mutable Exports

Regardless of technical support, mutable exports can create hard to understand and debug code, in particular with re-exports across multiple modules. One way to paraphrase this style point is that `export let` is not allowed.

```ts
> // Bad
export let foo = 3;
// In pure ES6, foo is mutable and importers will observe the value change after a second.
// In TS, if foo is re-exported by a second file, importers will not see the value change.
window.setTimeout(() => {
  foo = 4;
}, 1000 /* ms */);
```

If one needs to support externally accessible and mutable bindings, they should instead use explicit getter functions.

```ts
let foo = 3;
window.setTimeout(() => {
  foo = 4;
}, 1000 /* ms */);
// Use an explicit getter to access the mutable export.
export function getFoo() {
  return foo;
}
```

For the common pattern of conditionally exporting either of two values, first do the conditional check, then the export. Make sure that all exports are final after the modules body has executed.

```ts
function pickApi() {
if (useOtherApi()) return OtherApi;
  return RegularApi;
}
export const SomeApi = pickApi();
```

#### Container Classes

Do not create container classes with static methods or properties for the sake of namespacing.

```ts
> // Bad
export class Container {
  static FOO = 1;
  static bar() {
    return 1;
  }
}
```

Instead, export individual constants and functions:

```ts
export const FOO = 1;
export function bar() {
  return 1;
}
```

### Imports

There are four variants of import statements in ES6 and TypeScript:

| Import type | Example | Use for |
| --- | --- | --- |
| module | \`import * as foo from | TypeScript imports |
| destructuring | \`import {SomeThing} from | TypeScript imports |
| default | \`import SomeThing from | Only for other external code that requires them |
| side-effect | `import '...';` | Only to import libraries for their side-effects on load (such as custom elements) |

```ts
// Good: choose between two options as appropriate (see below).
import * as ng from "@angular/core";
import { Foo } from "./foo";

// Only when needed: default imports.
import Button from "Button";

// Sometimes needed to import libraries for their side effects:
import "jasmine";
import "@polymer/paper-button";
```

#### Module versus destructuring imports

Both module and destructuring imports have advantages depending on the situation.

Despite the `_`, a module import is not comparable to a "wildcard" import as seen in other languages. Instead, module imports give a name to the entire module and each symbol reference mentions the module, which can make code more readable and gives autocompletion on all symbols in a module. They also require less import churn (all symbols are available), fewer name collisions, and allow terser names in the module thats imported. Module imports are particularly useful when using many different symbols from large APIs.

Destructuring imports give local names for each imported symbol. They allow terser code when using the imported symbol, which is particularly useful for very commonly used symbols, such as Jasmines `describe` and `it`.

```ts
// Bad: overlong import statement of needlessly namespaced names.
import {TableViewItem, TableViewHeader, TableViewRow, TableViewModel, TableViewRenderer} from './tableview';
let item: TableViewItem = ...;
```

```ts
// Better: use the module for namespacing.
import _ as tableview from './tableview';
let item: tableview.Item = ...;
```

```ts
import * as testing from './testing';

// All tests will use the same three functions repeatedly.
// When importing only a few symbols that are used very frequently, also
// consider importing the symbols directly (see below).
testing.describe('foo', () => {
  testing.it('bar', () => {
    testing.expect(...);
    testing.expect(...);
  });
});
```

```ts
// Better: give local names for these common functions.
import {describe, it, expect} from './testing';

describe('foo', () => {
  it('bar', () => {
    expect(...);
    expect(...);
  });
});
...
```

#### Renaming imports

Code *should* fix name collisions by using a module import or renaming the exports themselves. Code *may* rename imports (`import {SomeThing as SomeOtherThing}`) if needed.

Three examples where renaming can be helpful:

If it's necessary to avoid collisions with other imported symbols.

If the imported symbol name is generated.

If importing symbols whose names are unclear by themselves, renaming can improve code clarity. For example, when using RxJS the `from` function might be more readable when renamed to `observableFrom`.

#### Import & export type

Do not use `import type ... from` or `export type ... from`.

Note: this does not apply to exporting type definitions, i.e. `export type Foo = ...;`.

```ts
// Bad
import type { Foo } from "./foo";
export type { Bar } from "./bar";
```

Instead, just use regular imports:

```ts
import { Foo } from "./foo";
export { Bar } from "./bar";
```

TypeScript tooling automatically distinguishes symbols used as types vs symbols used as values and only generates runtime loads for the latter.

> Why?
>
> TypeScript tooling automatically handles the distinction and does not insert runtime loads for type references. This gives a better developer UX: toggling back and forth between `import type` and `import` is bothersome. At the same time, `import type` gives no guarantees: your code might still have a hard dependency on some import through a different transitive path.
>
> If you need to force a runtime load for side effects, use `import ...;`. See the imports.
>
> `export type` might seem useful to avoid ever exporting a value symbol for an API. However it does not give guarantees either: downstream code might still import an API through a different path. A better way to split & guarantee type vs value usages of an API is to actually split the symbols into e.g. `UserService` and `AjaxUserService`. This is less error prone and also better communicates intent.

### Organize By Feature

Organize packages by feature, not by type. For example, an online shop *should* have packages named `products`, `checkout`, `backend`, not ~`views`, `models`, `controllers`~.

### Type System

### Type Inference

Code may rely on type inference as implemented by the TypeScript compiler for all type expressions (variables, fields, return types, etc). The google3 compiler flags reject code that does not have a type annotation and cannot be inferred, so all code is guaranteed to be typed (but might use the `any` type explicitly).

```ts
const x = 15; // Type inferred.
```

Leave out type annotations for trivially inferred types: variables or parameters initialized to a `string`, `number`, `boolean`, `RegExp` literal or `new` expression.

```ts
// Bad
const x: boolean = true; // Bad: 'boolean' here does not aid readability
```

```ts
// Bad: 'Set' is trivially inferred from the initialization
const x: Set<string> = new Set();
```

```ts
const x = new Set<string>();
```

For more complex expressions, type annotations can help with readability of the program. Whether an annotation is required is decided by the code reviewer.

#### Return types

Whether to include return type annotations for functions and methods is up to the code author. Reviewers *may* ask for annotations to clarify complex return types that are hard to understand. Projects *may* have a local policy to always require return types, but this is not a general TypeScript style requirement.

There are two benefits to explicitly typing out the implicit return values of functions and methods:

- More precise documentation to benefit readers of the code.
- Surface potential type errors faster in the future if there are code changes that change the return type of the function.

#### Null vs Undefined

TypeScript supports `null` and `undefined` types. Nullable types can be constructed as a union type (`string|null`); similarly with `undefined`. There is no special syntax for unions of `null` and `undefined`.

TypeScript code can use either `undefined` or `null` to denote absence of a value, there is no general guidance to prefer one over the other. Many JavaScript APIs use `undefined` (e.g. `Map.get`), while many DOM and Google APIs use `null` (e.g. `Element.getAttribute`), so the appropriate absent value depends on the context.

#### Nullable/undefined type aliases

Type aliases *must not* include `|null` or `|undefined` in a union type. Nullable aliases typically indicate that null values are being passed around through too many layers of an application, and this clouds the source of the original issue that resulted in `null`. They also make it unclear when specific values on a class or interface might be absent.

Instead, code *must* only add `|null` or `|undefined` when the alias is actually used. Code *should* deal with null values close to where they arise, using the above techniques.

```ts
// Bad
type CoffeeResponse = Latte|Americano|undefined;

class CoffeeService {
  getLatte(): CoffeeResponse { ... };
}
```

```ts
// Better
type CoffeeResponse = Latte|Americano;

class CoffeeService {
  getLatte(): CoffeeResponse|undefined { ... };
}
```

```ts
// Best
type CoffeeResponse = Latte | Americano;

class CoffeeService {
  getLatte(): CoffeeResponse {
    return assert(fetchResponse(), "Coffee maker is broken, file a ticket");
  }
}
```

#### Optionals vs `|undefined` type

In addition, TypeScript supports a special construct for optional parameters and fields, using `?`:

```ts
interface CoffeeOrder {
  sugarCubes: number;
  milk?: Whole|LowFat|HalfHalf;
}

function pourCoffee(volume?: Milliliter) { ... }
```

Optional parameters implicitly include `|undefined` in their type. However, they are different in that they can be left out when constructing a value or calling a method. For example, `{sugarCubes: 1}` is a valid `CoffeeOrder` because `milk` is optional.

Use optional fields (on interfaces or classes) and parameters rather than a `|undefined` type.

For classes preferably avoid this pattern altogether and initialize as many fields as possible.

```ts
class MyClass {
  field = "";
}
```

### Structural Types vs Nominal Types

TypeScripts type system is structural, not nominal. That is, a value matches a type if it has at least all the properties the type requires and the properties types match, recursively.

Use structural typing where appropriate in your code. Outside of test code, use interfaces to define structural types, not classes. In test code it can be useful to have mock implementations structurally match the code under test without introducing an extra interface.

When providing a structural-based implementation, explicitly include the type at the declaration of the symbol (this allows more precise type checking and error reporting).

```ts
// Good
const foo: Foo = {
  a: 123,
  b: "abc",
};
```

```ts
// Bad
const badFoo = {
  a: 123,
  b: "abc",
};
```

> Why?
>
> The "badFoo" object above relies on type inference. Additional fields could be added to "badFoo" and the type is inferred based on the object itself.
>
> When passing a "badFoo" to a function that takes a "Foo", the error will be at the function call site, rather than at the object declaration site. This is also useful when changing the surface of an interface across broad codebases.
>
> ```ts
> interface Animal {
>   sound: string;
>   name: string;
> }
> 
> function makeSound(animal: Animal) {}
> 
> /**
>  * 'cat' has an inferred type of '{sound: string}'
>  */
> const cat = {
>   sound: "meow",
> };
> 
> /**
>  * 'cat' does not meet the type contract required for the function, so the
>  * TypeScript compiler errors here, which may be very far from where 'cat' is
>  * defined.
>  */
> makeSound(cat);
> 
> /**
>  * Horse has a structural type and the type error shows here rather than the
>  * function call.  'horse' does not meet the type contract of 'Animal'.
>  */
> const horse: Animal = {
>   sound: "niegh",
> };
> 
> const dog: Animal = {
>   sound: "bark",
>   name: "MrPickles",
> };
> 
> makeSound(dog);
> makeSound(horse);
> ```

### Interfaces vs Type Aliases

TypeScript supports type aliases for naming a type expression. This can be used to name primitives, unions, tuples, and any other types.

However, when declaring types for objects, use interfaces instead of a type alias for the object literal expression.

```ts
// Good
interface User {
  firstName: string;
  lastName: string;
}
```

```ts
// Bad
type User = {
  firstName: string;
  lastName: string;
};
```

> Why?
>
> These forms are nearly equivalent, so under the principle of just choosing one out of two forms to prevent variation, we should choose one. Additionally, there also interesting technical reasons to prefer interface. That page quotes the TypeScript team lead: "Honestly, my take is that it should really just be interfaces for anything that they can model. There is no benefit to type aliases when there are so many issues around display/perf."

### `Array<T>` Type

For simple types (containing just alphanumeric characters and dot), use the syntax sugar for arrays, `T[]`, rather than the longer form `Array<T>`.

For anything more complex, use the longer form `Array<T>`.

This also applies for `readonly T[]` vs `ReadonlyArray<T>`.

```ts
const a: string[];
const b: readonly string[];
const c: ns.MyObj[];
const d: Array<string | number>;
const e: ReadonlyArray;
```

```ts
// Bad
const f: Array<string>; // the syntax sugar is shorter
const g: ReadonlyArray;
const h: { n: number; s: string }[]; // the braces/parens make it harder to read
const i: (string | number)[];
const j: readonly (string | number)[];
```

### Indexable (`{[key: string]: number}`) Type

In JavaScript, its common to use an object as an associative array (aka "map", "hash", or "dict"):

```ts
const fileSizes: { [fileName: string]: number } = {};
fileSizes["readme.txt"] = 541;
```

In TypeScript, provide a meaningful label for the key. (The label only exists for documentation; its unused otherwise.)

```ts
// Bad
const users: {[key: string]: number} = ...;
```

```ts
// Good
const users: {[userName: string]: number} = ...;
```

 Rather than using one of these, consider using the ES6 `Map` and `Set` types instead. JavaScript objects have surprising undesirable behaviors and the ES6 types more explicitly convey your intent. Also, `Map`s can be keyed by—and `Set`s can contain—types other than `string`.

TypeScripts builtin `Record<Keys, ValueType>` type allows constructing types with a defined set of keys. This is distinct from associative arrays in that the keys are statically known. See advice on that below.

### Mapped & Conditional Types

TypeScripts mapped types and conditional types allow specifying new types based on other types. TypeScripts standard library includes several type operators based on these (`Record`, `Partial`, `Readonly` etc).

These type system features allow succinctly specifying types and constructing powerful yet type safe abstractions. They come with a number of drawbacks though:

- Compared to explicitly specifying properties and type relations (e.g. using interfaces and extension, see below for an example), type operators require the reader to mentally evaluate the type expression. This can make programs substantially harder to read, in particular combined with type inference and expressions crossing file boundaries.
- Mapped & conditional types' evaluation model, in particular when combined with type inference, is underspecified, not always well understood, and often subject to change in TypeScript compiler versions. Code can "accidentally" compile or seem to give the right results. This increases future support cost of code using type operators.
- Mapped & conditional types are most powerful when deriving types from complex and/or inferred types. On the flip side, this is also when they are most prone to create hard to understand and maintain programs.
- Some language tooling does not work well with these type system features. E.g. your IDE's find references (and thus rename property refactoring) will not find properties in a `Pick<T, Keys>` type, and Code Search won't hyperlink them.

The style recommendation is:

- Always use the simplest type construct that can possibly express your code.
- A little bit of repetition or verbosity is often much cheaper than the long term cost of complex type expressions.
- Mapped & conditional types may be used, subject to these considerations.

For example, TypeScript's builtin \`Pick\` type allows creating a new type by subsetting another type \`T\`, but simple interface extension can often be easier to understand.

```ts
interface User {
  shoeSize: number;
  favoriteIcecream: string;
  favoriteChocolate: string;
}

// FoodPreferences has favoriteIcecream and favoriteChocolate, but not shoeSize.
type FoodPreferences = Pick<User, "favoriteIcecream" | "favoriteChocolate">;
```

This is equivalent to spelling out the properties on `FoodPreferences`:

```ts
interface FoodPreferences {
  favoriteIcecream: string;
  favoriteChocolate: string;
}
```

To reduce duplication, `User` could extend `FoodPreferences`, or (possibly better) nest a field for food preferences:

```ts
interface FoodPreferences {
  /* as above */
}
interface User extends FoodPreferences {
  shoeSize: number;
  // also includes the preferences.
}
```

Using interfaces here makes the grouping of properties explicit, improves IDE support, allows better optimization, and arguably makes the code easier to understand.

### `any` Type

TypeScripts `any` type is a super and subtype of all other types, and allows dereferencing all properties. As such, `any` is dangerous - it can mask severe programming errors, and its use undermines the value of having static types in the first place.

**Consider *not* to use `any`.** In circumstances where you want to use `any`, consider one of:

- Provide a more specific type
- Use `unknown`
- Suppress the lint warning and document why

#### Any attribute

Sometimes we want an interface to allow any other attributes in addition to the required and optional attributes. At this time, we can use the form of index signature to meet the above requirements.

```ts
interface Person {
  name: string;
  age?: number;
  [propName: string]: any;
}

const p1 = { name: "semlinker" };
const p2 = { name: "lolo", age: 5 };
const p3 = { name: "kakuqo", sex: 1 };
```

#### Providing a more specific type

Use interfaces, an inline object type, or a type alias:

```ts
// Use declared interfaces to represent server-side JSON.
declare interface MyUserJson {
  name: string;
  email: string;
}

// Use type aliases for types that are repetitive to write.
type MyType = number | string;

// Or use inline object types for complex returns.
function getTwoThings(): { something: number; other: string } {
  // ...
  return { something, other };
}

// Use a generic type, where otherwise a library would say `any` to represent
// they don't care what type the user is operating on (but note "Return type
// only generics" below).
function nicestElement<T>(items: T[]): T {
  // Find the nicest element in items.
  // Code can also put constraints on T, e.g. .
}
```

#### Using `unknown` over `any`

The `any` type allows assignment into any other type and dereferencing any property off it. Often this behaviour is not necessary or desirable, and code just needs to express that a type is unknown. Use the built-in type `unknown` in that situation — it expresses the concept and is much safer as it does not allow dereferencing arbitrary properties.

```ts
// Good
// Can assign any value (including null or undefined) into this but cannot
// use it without narrowing the type or casting.
const val: unknown = value;
```

```ts
// Bad
const danger: any = value; /_ result of an arbitrary expression _/
danger.whoops(); // This access is completely unchecked!
```

To safely use `unknown` values, narrow the type using a type guard

```ts
let value: unknown;
let value1: unknown = value; // OK
let value2: any = value; // OK
let value3: boolean = value; // Error
let value4: number = value; // Error
```

`unknown` type can only be assigned to `any` type and `unknown` type itself.

#### Suppressing `any` lint warnings

Sometimes using `any` is legitimate, for example in tests to construct a mock object. In such cases, add a comment that suppresses the lint warning, and document why it is legitimate.

```ts
// This test only needs a partial implementation of BookService, and if
// we overlooked something the test will fail in an obvious way.
// This is an intentionally unsafe partial mock
// tslint:disable-next-line:no-any
const mockBookService = ({
  get() {
    return mockBook;
  },
} as any) as BookService;
// Shopping cart is not used in this test
// tslint:disable-next-line:no-any
const component = new MyComponent(
  mockBookService,
  /* unused ShoppingCart */ null as any
);
```

### Tuple Types

If you are tempted to create a Pair type, instead use a tuple type:

```ts
// Bad
interface Pair {
  first: string;
  second: string;
}
function splitInHalf(input: string): Pair {
  ...
  return {first: x, second: y};
}
```

```ts
// Good
function splitInHalf(input: string): [string, string] {
  ...
  return [x, y];
}

// Use it like:
const [leftHalf, rightHalf] = splitInHalf('my string');
```

However, often its clearer to provide meaningful names for the properties.

If declaring an `interface` is too heavyweight, you can use an inline object literal type:

```ts
function splitHostPort(address: string): {host: string, port: number} {
  ...
}

// Use it like:
const address = splitHostPort(userAddress);
use(address.port);

// You can also get tuple-like behavior using destructuring:
const {host, port} = splitHostPort(userAddress);
```

### Wrapper types

There are a few types related to JavaScript primitives that should never be used:

- `String`, `Boolean`, and `Number` have slightly different meaning from the corresponding primitive types `string`, `boolean`, and `number`. Always use the lowercase version.
- `Object` has similarities to both `{}` and `object`, but is slightly looser. Use `{}` for a type that include everything except `null` and `undefined`, or lowercase `object` to further exclude the other primitive types (the three mentioned above, plus `symbol` and `bigint`).

Further, never invoke the wrapper types as constructors (with `new`).

### Return type only generics

Avoid creating APIs that have return type only generics. When working with existing APIs that have return type only generics always explicitly specify the generics.

## Consistency

For any style question that isnt settled definitively by this specification, do what the other code in the same file is already doing ("be consistent"). If that doesnt resolve the question, consider emulating the other files in the same directory.
