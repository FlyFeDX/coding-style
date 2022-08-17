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
  - 2.23  [Enums](#enums)  
  - 2.24  [Debugger statements](#debugger-statements)  
  - 2.25  [Decorators](#decorators)  
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

标识符只能使用ASCII字母、数字、下划线(用于常量和结构化测试方法名称)和$符号。 因此,每个有效的标识符名称都匹配正则表达式' [$\w]+ '。

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

  > Tip: 如果只需要数组(或TypeScript元组)中的一些元素,可以在解构语句中插入额外的逗号来忽略中间元素:
  
  ```ts
  const [a, , b] = [1, 5, 10]; // a <- 1, b <- 10
  ```

- **Imports**: 模块的命名空间, 引入对象 使用 `lowerCamelCase` 而文件名采用 `snake_case`, 比如:

    ```ts
    import * as fooBar from "./foo_bar";
    ```

一些库通常可能会使用违反这种命名模式的名称空间导入前缀,但常见的开源库使用会使这种违反的样式更具可读性. 例如,目前属于这种例外的库是:

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

    如果一个值在程序的生命周期内可以被实例化不止一次,或者如果用户以任何方式改变它, 必须使用`lowerCamelCase`命名.

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

当创建本地作用域的别名时, 使用被起别名对象的命名格式. 本地别名必须与源的现有名称和格式匹配. 对于变量,请使用' const '作为局部别名,对于类字段,请使用' readonly '属性。 .

```ts
const { Foo } = SomeType;
const CAPACITY = 5;

class Teapot {
    readonly BrewStateEnum = BrewStateEnum;
    readonly CAPACITY = CAPACITY;
}
```

#### Naming style

TypeScript用类型来表达信息,所以名字*不应该*用类型中包含的信息来装饰

这条规则的一些具体例子:

- 不要为私有属性或方法使用尾随或前导下划线
- 不使用`opt_`等作为可选参数的前缀.
  - 对于访问器,请参阅下面的访问器规则.
- 不要对接口使用特殊的标记 (~~`IMyInterface`~~ 或 ~~`MyFooInterface`~~) 除非它定义的环境中有特定的含义. 当为类引入接口时, 要通过类名表达为什么要实现这个接口 (比如: `class TodoItem` 和 `interface TodoItemStorage` (接口用于存储/序列化等功能)).
- `Observable`后面加上`$`是一种常见的外部约定,可以帮助解决Observable值和具体值之间的混淆.

#### Descriptive names

名称必须对研发人员具有描述性和清晰性. 不要使用模棱两可或对项目外的读者不熟悉的缩写,也不要通过删除单词中的字母来缩写.

- **例外情况**: 在10行或更少范围内的变量,包括不属于导出API的参数,可以使用较短的(例如单字母)变量名.

### File encoding: UTF-8

对于非ascii字符,使用实际的Unicode字符 (例如: `∞`). 对于不可打印字符,等效的十六进制或Unicode转义 (例如: `\u221e`) 可以与解释性注释一起使用.

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

JSDoc注释可以被工具(比如编辑器和文档生成器)理解,而普通注释只供其他人使用。

#### JSDoc rules follow the JavaScript style

一般来说,遵循JSDoc的JavaScript样式指南规则.本节的其余部分将描述这些规则的例外情况.

#### Document all top-level exports of modules

使用`/** JSDoc _/`注释将信息传达给代码的用户. 避免仅仅重述属性或参数名称. 所以需要将 `export` 的模块使用`/** JSDoc _/`注释.

#### Omit comments that are redundant with TypeScript

例如, 不要在 `@param` 或 `@return` 块中声明类型, 不要在使用 `implements`, `enum`, `private` 等关键字的代码上写入 `@implements`, `@enum`, `@private` 等关键字.

#### Do not use `@override`

不要在TypeScript代码中使用 `@override`.

`@override` 导致注释和实现不同步. 纯粹出于文档目的而包含它不是很好.

#### Make comments that actually add information

对于未导出的方法或者对象, 函数或参数额名称和类型就足够了.

- 只重复参数名称和类型的注释无效,例如:

    ```ts
    // Bad
    /** @param fooBarService The Bar service for the Foo application. */
    ```

- 因此 `@param` 和 `@return` 行只有在添加信息时才需要,否则可能会被省略.

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

参数属性是当类在单个声明中声明一个字段和一个构造函数参数时,通过在构造函数中标记一个参数. 例如: `constructor(private readonly foo: Foo)`,声明该类有一个 `foo` 字段.

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

如果需要,使用块注释内联调用站点的文档参数。
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
- 考虑将私有方法转换为同一文件中任何类之外的非导出函数,并将私有属性移动到单独的非导出类中.
- TypeScript symbols 默认是公共的. 永远不要使用 `public` 修饰符,除非在构造函数中声明非只读公共参数属性.

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

请省略掉空的构造函数或只委托给父类的构造函数,因为如果没有指定,ES2015会提供默认的类构造函数.
但是,即使构造函数体为空,也不应该省略带有形参属性、修饰符或形参修饰符的构造函数.

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

不要把一个明显的初始化式传递给类成员,而是使用TypeScript的形参属性。

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

如果类成员不是形参,则在声明它的地方初始化它,因为这有时可以让你完全删除构造函数。

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

在其包含类的词法作用域之外使用的属性,比如从模板中使用的AngularJS控制器属性,不能使用`private`可见性,因为它们是在其包含类的词法作用域之外使用的.

这些属性最好是 `public` 的, 但是 `protected` 也可以根据需要使用. 比如, Angular 和 Polymer 模板属性应该使用 `public`, 而 AngularJS 应该使用 `protected`.

#### Getters and Setters (Accessors)

通常避免使用getter和setter是一个很好的实践。

可以使用类成员的getter和setter。 getter方法必须是一个纯函数(即,结果是一致的,没有副作用)。 它们还可以用来限制内部或详细实现细节的可见性(如下所示)。

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

相反,总是使用括号表示法来初始化数组, 或者 使用 `from` 去初始化一个长度固定的 `Array` :

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

不要使用字符串连接(`'xxx' + 'xxxx'`)强制转换为string,因为要检查加号操作符的操作数是否具有匹配的类型。

使用 `Number()`去转换数字类型, 并且*必须*显式地检查其返回的“NaN”值,除非从上下文不可能解析失败.

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

不要使用 `for (... in ...)` 去迭代数组. 它将给数组提供索引(作为字符串!),而不是值的索引:

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

不要使用 `Array.prototype.forEach`, `Set.prototype.forEach`, 和 `Map.prototype.forEach`. 它们使代码更难调试,并破坏了一些有用的编译器检查(例如可达性).

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
> `x` 不是 null 而在它被访问之前不会改变. 但是编译器无法知道这个 `.forEach()` 调用没有挂起传递进来的闭包,并在稍后的某个点上调用它, 可能是在 `x` 被设为null之后, 所以编译器会将此代码标记为错误。 而使用for-of循环就不会被标记为错误了
>
> [See the error and non-error in the playground](https://www.typescriptlang.org/play/?#code/DYUwLgBAHgXBDOYBOBLAdgcwD5oK7GAgF4IByAQwCMBjUgbgCgBtAXQDoAzAeyQFFzqACwAUwgJTEAfBADeDCNDZDySAIJhhABjGMAvjoYNQkAJ5xEqTDnyESFGvQbckEYdS5pEEAPoQuHCFYJOQUTJUEVdS0DXQYgA)
>
> 在实践中,控制流分析的这种限制的变化出现在更复杂的代码路径中

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

使用拓展运算符时, 必须确保拓展的是对象和数组(需要清晰且明确的判断). 当拓展运算符作用在 `null` 或 `undefined` 上时, 会出现异常.

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

无论是单行还是多行的控制流, 都尽量要用块`{ }`包裹.

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

如果是单行的 `if` 语句, 可以接受不需要用块`{ }`包裹.

```ts
if (x) x.doFoo();
```

### Switch Statements

所有 `switch` 语句都要包含 `default` 分支(就算`default`分支没有要处理的代码).

```ts
switch (x) {
  case Y:
      doSomethingElse();
      break;
  default:
  // nothing to do.
}
```

非空的状态组 (`case ...`) 不要忽略break语句:

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

空状态组可以忽略break语句:

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

永远使用 (`===`) 和 (`!==`).

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

### Function Declarations

使用`function foo() { ... }` 声明命名函数, 包括函数嵌套(函数内部包含其他函数).

使用函数声明,而不是将函数表达式赋值给局部变量 (~~`const x = function() {...};`~~). TypeScript 不允许重新绑定函数, 因此也不要使用函数声明给 `const`变量赋值 .

**例外**: 如果函数访问外部作用域的 `this`, 应使用分配给变量的箭头函数,而不是函数声明。

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

注意这里讨论的函数声明 (`function foo() {}`) 和下面讨论的函数表达式 (~~`doSomethingWith(function() {});`~~) 的区别.

顶级的箭头函数 *可以* 用于显式的声明一个接口的实现.

```ts
interface SearchFunction {
  (source: string, subString: string): boolean;
}

const fooSearch: SearchFunction = (source, subString) => { ... };
```

### Function Expressions

#### Use arrow functions in expressions

总是使用箭头函数,而不是es6之前使用 `function` 关键字定义的函数表达式。

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

函数表达式(用 `function` 关键字定义)只能在代码必须动态地重新绑定 `this` 指针时使用,但通常代码*不应该*重新绑定 `this` 指针。常规函数中的代码(与箭头函数和方法相反)*不应该*访问 `this` 。

#### Expression bodies vs block bodies

适当地将箭头函数与表达式或块一起用作箭头函数的主体。

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

只有在实际使用函数的返回值时才使用表达式体

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

函数表达式不要使用`this`,除非它们是专门用来重新绑定`this`指针的。在大多数情况下,可以通过使用箭头函数或显式参数来避免重新绑定`this`。

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

类通常不应该包含箭头函数初始化的属性.箭头函数属性要求调用函数理解被调用方 `this` 已经绑定,这增加了对 `this` 是什么的困惑.代码*应该*总是使用箭头函数来调用实例方法(`const handler = (x) => { this.listener(x); };`),并且*不应该*获取或传递实例方法的引用(~~`const handler = this.listener; handler(x);`~~)。

注意:在某些特定的情况下,例如在模板中绑定函数时,箭头函数作为属性是很有用的,可以创建可读性更强的代码.

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

事件处理程序*可以*在不需要卸载处理程序时使用箭头函数(例如,如果事件是由类本身发出的)。如果必须卸载处理程序,箭头函数属性是正确的方法,因为它们会自动捕获`this` ,并为卸载提供稳定的引用。

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

不要在使用事件处理程序的表达式中使用`bind`,因为它会创建无法卸载的临时引用.

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

尽量自己使用分号去显式终止所有语句

### @ts-ignore

尽量不要使用 `@ts-ignore`.

### Type and Non-nullability Assertions

类型断言 (`x as SomeType`) 和非空性断言 (`y!`) 是不安全的. 它们都只是静默TypeScript编译器,但不会插入任何运行时检查来匹配这些断言,所以它们会导致程序在运行时崩溃.

因此,如果没有明显或显式的理由,*不应*使用类型和非空性断言。

```ts
// Bad
(x as Foo).foo();

y!.bar();
```

当想要断言一个类型或非空性时,最好是显式地编写一个执行该检查的运行时检查。

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

有时由于代码的某些本地属性,可以确保断言表单是安全的。在这些情况下,你*应该*补充说明,解释为什么你可以接受不安全的行为:

```ts
// x is a Foo, because ...
(x as Foo).foo();

// y cannot be null, because ...
y!.bar();
```

如果类型或非空性断言背后的推理很明显,则可能不需要注释

#### Type Assertions Syntax

> 类型断言必须使用`as`语法(而不是尖括号语法)

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

使用类型注释 (`: Foo`) 而不是类型断言 (`as Foo`) 来指定对象字面量的类型.

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

接口和类声明必须使用`;`字符分隔各个成员声明:

```ts
interface Foo {
    memberA: string;
    memberB: number;
}
```

不要使用 `,` 分隔：

```ts
// Bad
interface Foo {
    memberA: string,
    memberB: number,
}
```

> 内联对象类型声明必须使用逗号作为分隔符：

```ts
type SomeTypeAlias = {
    memberA: string,
    memberB: number
};

let someProperty: { memberC: string, memberD: number };
```

#### Optimization compatibility for property access

不要的混合使用 `(x["someField"]` 语法和 `x.someField` 语法

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

导入模块对象时,尽量直接访问模块对象的属性,而不是全部引入。
这确保了模块可以被分析和优化。将模块导入作为名称空间处理是可以的。

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

### Enums

使用 `enum` 而不是 `const enum`. TypeScript 中 `enum` 已经是不可变对象了

### Debugger statements

调试器语句不能包含在生产代码中

```ts
// Bad
function debugMe() {
  debugger;
}
```

### Decorators

装饰器语法是使用 `@` 前缀, 如 `@MyDecorator`.

不要定义新的装饰器。只使用框架定义的装饰器:

- Angular (e.g. `@Component`, `@NgModule`, etc.)
- Polymer (e.g. `@property`)

> 为什么?
>
> 通常想要避免装饰器,因为它们是一个实验性的特性,已经偏离了TC39的建议,并且已经知道了无法修复的bug。
>
> 当使用装饰器时,装饰器必须直接位于它所装饰的符号之前,中间不能有空行:
>

```ts
/** JSDoc comments go before decorators */
@Component({...}) // Note: no empty line after the decorator.
class MyComp {
    @Input() myField: string; // Decorators on fields may be on the same line...

    @Input()
    myOtherField: string; // ... or wrap.
}
```

## Source Organization

### Modules

#### Import Paths

TypeScript 代码必须使用路径导入其他 TypeScript 代码. 路径可以是相对的, 比如. 以 `.` 或者 `..`, 或者 根路径开头等等. `root/path/to/file`.

使用相对路径引入 (`./foo`) 优于使用绝对路径 `path/to/foo` 引入(当引用同一(逻辑)项目中的文件时).

考虑限制父级文件夹的数量 (`../../../`) ,因为这会使模块和路径结构难以理解.

```ts
import { Symbol1 } from "google3/path/from/root";
import { Symbol2 } from "../parent/file";
import { Symbol3 } from "./sibling";
```

#### Namespaces vs Modules

TypeScript 提供的两种组织代码的模式: *namespaces* 和 *modules*, 但尽量不要使用 namespaces. 也就是说,你的代码*必须*引用其他文件中的代码,格式为 `import {foo} from bar;`.

不要使用命名空间 `namespace Foo { ... }` 的构造函数. `namespace` 只能在需要与外部第三方代码进行引用时使用. 要在语义上命名代码,请使用单独的文件.

不要使用 `require` (同理 `import x = require(...);`) 引入文件. 使用 ES6 module 语法.

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

注: TypeScript `namespace`以前被称为内部模块, 并且经常使用 `module` 关键字, 形式为 `module Foo { ... }`. 也不要这样去使用.

### Exports

在所有代码中使用命名 `export`:

```ts
// Good
// Use named exports:
export class Foo { ... }
```

不要使用 default exports. 这确保了所有导入都遵循统一的模式(只有一个导出对象除外).

```ts
// Bad
// Do not use default exports:
export default class Foo { ... } // BAD!
```

> 为什么?
>
> `default export`不提供规范名称,这使得集中维护变得困难,对代码所有者的好处相对较少,包括可能降低可读性:
>

 ```ts
// Bad
import Foo from "./bar"; // Legal.
import Bar from "./bar"; // Also legal.
```

命名`export`的好处是,当 `import` 语句试图 `import`尚未声明的内容时,它会出错. 比如 `foo.ts` 文件中:

```ts
// Bad
const foo = "blah";
export default foo;
```

而在 `bar.ts`文件中:

```ts
// Bad
import { fizz } from "./foo";
```

会出现异常：

```ts
// Bad
error TS2614: Module "./foo" has no exported member fizz.
```

而 `bar.ts`文件如果使用 `default export`:

```ts
// Bad
import fizz from "./foo";
```

结果 `fizz === foo`, 可能会产生混淆.

此外,`default export`鼓励研发人员将所有内容放在一个大对象中,并将其命名为所有内容:

```ts
// Bad
export default class Foo {
static SOME*CONSTANT = ...
static someHelpfulFunction() { ... }
  ...
}
```

更推荐使用文件作用域(module模块的方式),以及命名 `export`:

```ts
export const SOME_CONSTANT = ...
export function someHelpfulFunction()
export class Foo {
    // only class stuff here
}
```

#### Export visibility

TypeScript不支持限制 `export` 符号的可见性. 应该只 `export` 在模块外部使用的内容.

#### Mutable Exports

export 内容一定是不可变的.

```ts
> // Bad
export let foo = 3;
// In pure ES6, foo is mutable and importers will observe the value change after a second.
// In TS, if foo is re-exported by a second file, importers will not see the value change.
window.setTimeout(() => {
    foo = 4;
}, 1000 /* ms */);
```

如果需要支持外部可访问和可变的绑定,则应该使用显式的getter函数.

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

有条件的 `export` 也是一种常见的模式.

```ts
function pickApi() {
    if (useOtherApi()){
        return OtherApi;
    }
    return RegularApi;
}
export const SomeApi = pickApi();
```

#### Container Classes

不要为了 `namespace` 而创建带有静态方法或属性的容器类.

```ts
> // Bad
export class Container {
    static FOO = 1;
    static bar() {
        return 1;
    }
}
```

相反, 应该 `export` 单独的常量和函数:

```ts
export const FOO = 1;
export function bar() {
  return 1;
}
```

### Imports

在 ES6 和 TypeScript 中 有4种 `import` 语句的变体 :

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

模块导入和解构导入都有各自的优势,这取决于具体的情况.

有时候采用模块导入也有优势,比如当`import`一个非常复杂的类时,所有`import`的属性都包含类命名空间,如果采用解构导入会导致`import`代码过长并且名称有冗余.因此, 当使用来自大型api的许多不同符号时,模块导入特别有用.

解构导入为每个导入的符号提供了局部名称。 当使用导入的符号时,它们允许更简洁的代码,这对非常常用的符号特别有用, 比如 Jasmines 的 `describe` 和 `it`.

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

代码*应该*通过使用模块导入或重命名导出本身来修复名称冲突. 如果需要, 可以重命名 `import` (`import {SomeThing as SomeOtherThing}`).

重命名有三个实例:

有必要避免与其他`import`名称冲突的时候.

`import`的名称是自动生成的时候.

如果`import`的名称描述自身能力不是特别清楚, 重命名能使`import`之后的代码逻辑更清晰.比如, 在使用RxJS时, 将' from '函数重命名为' observableFrom '可能会更易读.

#### Import & export type

不要使用 `import type ... from` 或 `export type ... from`.

`import` 和 `export` 不适用于导出类型定义, 即: `export type Foo = ...;`.

`import`和`import type`都可以导入一个类型或一个值, 但是使用`import type`导入的值, 只能在类型上下文中使用, 不能作为一个值来使用, 而`import`导入的类型和值, 都可以按照其原本定义来使用.

```ts
// Bad
import type { Foo } from "./foo";
export type { Bar } from "./bar";
```

只需要使用常规导入:

```ts
import { Foo } from "./foo";
export { Bar } from "./bar";
```

在一些场景下,TypeScript 编译器会混淆导出的究竟是一个类型还是一个值.

### Organize By Feature

按照业务或者特性组织`package`,而不是按照类型组织. 比如, 一个在线商店的 `package` 应该包含 `products`, `checkout`, `backend`, 而不是 ~`views`, `models`, `controllers`~.

### Type System

### Type Inference

代码需要依赖于TypeScript编译器实现的所有类型表达式(变量、字段、返回类型等)的类型推断

```ts
const x = 15; // Type inferred.
```

对于简单的推断类型请忽略类型注释;比如  `string`, `number`, `boolean`, `RegExp` 字面量 或者 `new` 表达式.

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

对于更复杂的表达式, 类型注释有助于提高程序的可读性. 是否需要注释是由代码审查者决定的.

#### Return types

是否包含函数和方法的返回类型注释取决于代码作者.  评审者*可能*要求注释来澄清难以理解的复杂返回类型. 项目*可能*有一个本地策略总是要求返回类型,但这不是一般的TypeScript编码规范的要求.

显式输入函数和方法的隐式返回值有两个好处:

- 更精确的文档, 有利于阅读代码.
- 如果代码更改了函数的返回类型, 那么将来可以更快地处理潜在的类型错误.

#### Null vs Undefined

TypeScript 支持 `null` 和 `undefined` 类型. Nullable 类型 可以构造为联合类型(`string|null`); `undefined`同理. 对于 `null` 和 `undefined`联合类型目前没有特殊语法.

TypeScript 可以使用 `undefined` 或 `null` 来标识没有值, 也没有很通用的规则说明哪里使用`undefined` 哪里使用 `null`. 许多 JavaScript API 使用 `undefined` (比如: `Map.get`), 而 许多 DOM 和 Google API 使用 `null` (比如: `Element.getAttribute`).

#### Nullable/undefined type aliases

使用类型别名时,  *不能* 在联合类型中包含 `|null` 或者 `|undefined`. Nullable 别名通常表明空值在应用程序的很多层中传递, 如果类型别名中包含可控类型就掩盖了导致 `null` 的原始问题的根源.

必须在实际使用别名时,去添加 `|null` 或者 `|undefined`. 综上所述, 代码应该在可能出现空值的地方处理空值.

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

TypeScript支持可选参数和字段的特殊构造,使用 `?`:

```ts
interface CoffeeOrder {
    sugarCubes: number;
    milk?: Whole|LowFat|HalfHalf;
}

function pourCoffee(volume?: Milliliter) { ... }
```

可选参数在其类型中隐式包含 `|undefined`.  但是,不同之处在于,在构造值或调用方法时可以忽略它们. 比如, `{sugarCubes: 1}` 是有效的`CoffeeOrder`, 因为 `milk` 是可选的.

在接口或类上尽可能的使用可选字段和可选参数, 而不是使用 `|undefined` 类型.

对于类来讲,最好完全避免这种模式,并初始化尽可能多的字段.

```ts
class MyClass {
    field = "";
}
```

### Structural Types vs Nominal Types

TypeScript 中的类型兼容性基于结构子类型。结构类型是一种仅基于其成员去推断类型的方法.

比如：

```ts
interface Pet {
    name: string;
}
class Dog {
    name: string;
}
let pet: Pet;
// OK, because of structural typing
pet = new Dog();
```

在 C# 或 Java 等名义类型的语言中,上述等效代码将是错误的,因为Dog该类没有明确将自己描述为Pet接口的实现者.

因此当提供基于结构的实现时,显式地在声明中包含类型(这允许更精确的类型检查和错误报告)

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

>
> 上面的 "badFoo"对象依赖于类型推断. 可以将其他字段添加到 "badFoo" 中,并根据对象本身推断类型.
>
> 当将"badFoo"传递给以"Foo"为参数的函数时,错误将出现在函数调用处,而不是对象声明处. 因此在广泛被第三方使用代码库中,这很有用.
>

```ts
interface Animal {
    sound: string;
    name: string;
}

function makeSound(animal: Animal) {}

/**
 * 'cat' has an inferred type of '{sound: string}'
 */
const cat = {
    sound: "meow",
};

/**
 * 'cat' does not meet the type contract required for the function, so the
 * TypeScript compiler errors here, which may be very far from where 'cat' is
 * defined.
 */
makeSound(cat);

/**
 * Horse has a structural type and the type error shows here rather than the
 * function call.  'horse' does not meet the type contract of 'Animal'.
 */
const horse: Animal = {
  sound: "niegh",
};

const dog: Animal = {
  sound: "bark",
  name: "MrPickles",
};

makeSound(dog);
makeSound(horse);
```

### Interfaces vs Type Aliases

TypeScript支持类型别名来命名类型表达式. 可以用来命名原始类型, 联合类型, 元组和其他任意类型.

但是, 在为对象声明类型时, 使用接口而不是对象字面量表达式的类型别名。

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

### `Array<T>` Type

对于简单类型(只包含字符串或者数字等), 使用数组语法糖, `T[]`, 而不是 `Array<T>`.

对于更复杂的类型则使用 `Array<T>`.

同样适用于 `readonly T[]` 和 `ReadonlyArray<T>`.

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

在JavaScript中,通常使用一个对象作为可索引类型(又名 "map", "hash", 或者 "dict"):

```ts
const fileSizes: { [fileName: string]: number } = {};
fileSizes["readme.txt"] = 541;
```

在TypeScript中,为键提供一个有意义的名称标签. (该标签仅仅适用于文档.)

```ts
// Bad
const users: {[key: string]: number} = ...;
```

```ts
// Good
const users: {[userName: string]: number} = ...;
```

 有时候使用 ES6 的 `Map` 和 `Set` 比使用一个普通的对象作为可索引类型能更明确的表达意图. 也可以使用 TypeScripts 内置的 `Record<Keys, ValueType>` 类型 允许使用一组定义好的键来构造类型.

### Mapped & Conditional Types

TypeScripts 映射类型 和 条件类型 允许基于其他类型指定新类型. typescript标准库包含了几种基于这些的类型操作符(`Record`, `Partial`, `Readonly` 等).

这些类型系统特性允许简洁地指定类型和构造功能强大但类型安全的抽象. 不过, 它们也有一些缺点:

- 与显式地指定属性和类型关系(例如使用接口和扩展,见下面的示例)相比,类型操作符要求代码读者在心里估算类型表达式.这可能会使程序更难以阅读,特别是结合了类型推断和跨越边界的表达式.
- 映射类型和条件类型的求值模型,尤其是与类型推断结合使用的时候,没有得到很好的定义,而且在TypeScript编译器版本中经常会发生变化.代码可以"意外地"编译或似乎给出正确的结果.这增加了未来使用类型操作符的代码的支持成本.
- 映射类型和条件类型在从复杂类型或推断类型去派生类型时最强大, 但这也是最容易创建难以理解和维护程序的时候.
- 有些语言工具不能很好地使用这些类型系统特性. 比如: 在 IDE 里查找引用, 可能很难找到在 `Pick<T, Keys>` 类型中的属性, 代码搜索也不会超链接到它们.

推荐的编码风格是:

- 总是使用能够表达的最简单的类型构造方式.
- 少量重复或者冗余的字段, 通常比复杂的类型表达式维护成本要低一些.
- 可以使用映射类型和条件类型.

比如, TypeScript 内置的 \`Pick\` 类型允许通过对另一个类型 \`T\` 进行子集设置来创建一个新类型,但是简单的接口扩展通常更容易理解.

```ts
interface User {
    shoeSize: number;
    favoriteIcecream: string;
    favoriteChocolate: string;
}

// FoodPreferences has favoriteIcecream and favoriteChocolate, but not shoeSize.
type FoodPreferences = Pick<User, "favoriteIcecream" | "favoriteChocolate">;
```

这相当于 `FoodPreferences` 上的属性:

```ts
interface FoodPreferences {
    favoriteIcecream: string;
    favoriteChocolate: string;
}
```

为了减少重复, `User` 应该拓展实现接口 `FoodPreferences`:

```ts
interface FoodPreferences {
    /* as above */
}
interface User extends FoodPreferences {
    shoeSize: number;
    // also includes the preferences.
}
```

在这里使用接口可以显式地分组属性,改进IDE支持,允许更好的优化,并使代码更容易理解.

### `any` Type

TypeScripts 的 `any` 类型是所有其他类型的超类型和子类型, 并且允许解除对所有属性的引用. 因此, `any` 是危险的 - 它可以掩盖严重的编程错误,而且它的使用首先会破坏静态类型的价值.

**思考如何不去使用 `any`.** 在需要使用 `any`的场景中, 思考一下:

- 提供一个精确的类型
- 使用 `unknown`
- 取消lint警告, 并且注释一下原因

#### Any attribute

当希望接口允许除必需和可选属性之外的任何其他属性时, 可以采用索引签名的形式来满足上述要求.

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

使用接口、内联对象类型或类型别名:

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

`any`类型允许赋值到任何其他类型,并解除对其中任何属性的引用.通常这种行为是不必要的,代码只需要表示未知的类型.
在这种情况下使用内置类型`unknown`——它表达了概念,而且更安全,因为它不允许对任意属性解除引用

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

安全的使用 `unknown` 类型, 使用类型保护缩小未知范围

```ts
let value: unknown;
let value1: unknown = value; // OK
let value2: any = value; // OK
let value3: boolean = value; // Error
let value4: number = value; // Error
```

`unknown` 类型只能被赋值给 `any` 类型 和 `unknown` 类型本身.

#### Suppressing `any` lint warnings

有时候使用`any`是合法的,例如在测试中构造一个模拟对象.

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

如果想要去创建类似 Pair 的类型, 使用元组 `tuple`:

```ts
// Bad
interface Pair {
    first: string;
    second: string;
}
function splitInHalf(input: string): Pair {
    // ...
    return {first: x, second: y};
}
```

```ts
// Good
function splitInHalf(input: string): [string, string] {
    // ...
    return [x, y];
}

// Use it like:
const [leftHalf, rightHalf] = splitInHalf('my string');
```

为属性提供有意义的名称通常更清晰.

如果感觉申明一个接口 `interface` 太重了, 可以使用内联对象字面量类型去声明:

```ts
function splitHostPort(address: string): {host: string, port: number} {
    // ...
}

// Use it like:
const address = splitHostPort(userAddress);
use(address.port);

// You can also get tuple-like behavior using destructuring:
const {host, port} = splitHostPort(userAddress);
```

### Wrapper types

要使用JavaScript的原始类型,而不是包装器的类型:

- `String`, `Boolean`, 和 `Number` 与对应的原始类型(`string`, `boolean`, 和 `number`)的含义略有不同. 要使用`string`, `boolean`, 和 `number`.
- `Object` 与 `{}` 和 `object`类似, 要求会稍微宽松一些. 对于除了 `null` 和 `undefined`之外的所有类型使用`{}`或者 `object`.

此外,永远不要将包装器类型作为构造函数调用 (使用 `new`).

### Return type only generics

避免创建只有返回泛型类型的API, 当使用具有仅返回类型泛型的现有api时, 需要显式指定泛型

## Consistency

将你的代码保持一致性非常重要, 我们要去继续完善统一规范文档, 收集大家在此文档中没有出现的新问题.
