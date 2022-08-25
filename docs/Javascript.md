# JavaScript Style Guide

## Table of Contents

- [JavaScript Style Guide](#javascript-style-guide)
  - [Table of Contents](#table-of-contents)
  - [Types](#types)
  - [References](#references)
  - [Objects](#objects)
  - [Arrays](#arrays)
  - [Destructuring](#destructuring)
  - [Strings](#strings)
  - [Functions](#functions)
  - [Arrow Functions](#arrow-functions)
  - [Classes & Constructors](#classes--constructors)
  - [Modules](#modules)
  - [Iterators and Generators](#iterators-and-generators)
  - [Properties](#properties)
  - [Variables](#variables)
  - [Hoisting](#hoisting)
  - [Comparison Operators & Equality](#comparison-operators--equality)
  - [Blocks](#blocks)
  - [Control Statements](#control-statements)
  - [Comments](#comments)
  - [Whitespace](#whitespace)
  - [Commas](#commas)
  - [Semicolons](#semicolons)
  - [Type Casting & Coercion](#type-casting--coercion)
  - [Naming Conventions](#naming-conventions)
  - [Accessors](#accessors)
  - [Events](#events)
  - [jQuery](#jquery)
  - [ECMAScript 5 Compatibility](#ecmascript-5-compatibility)
  - [ECMAScript 6+ (ES 2015+) Styles](#ecmascript-6-es-2015-styles)
  - [Standard Library](#standard-library)
  - [Testing](#testing)
  - [Performance](#performance)
  - [Resources](#resources)

## Types

<a name="types--primitives"></a><a name="1.1"></a>

- [1.1](#types--primitives) **值类型**: 修改值类型相当于修改了值本身.

  - `string`
  - `number`
  - `boolean`
  - `null`
  - `undefined`
  - `symbol`
  - `bigint`

  ```javascript
  const foo = 1
  let bar = foo

  bar = 9

  console.log(foo, bar) // => 1, 9
  ```

  - 注意 Symbols 和 BigInts 的浏览器兼容性.

<a name="types--complex"></a><a name="1.2"></a>

- [1.2](#types--complex) **引用类型**: 引用类型变量的赋值只赋值对对象的引用, 而不赋值对象本身.

  - `object`
  - `array`
  - `function`

  ```javascript
  const foo = [1, 2]
  const bar = foo

  bar[0] = 9

  console.log(foo[0], bar[0]) // => 9, 9
  ```

**[⬆ back to top](#table-of-contents)**

## References

<a name="references--prefer-const"></a><a name="2.1"></a>

- [2.1](#references--prefer-const) 尽量使用 `const`声明变量; 避免使用 `var`. eslint: [`prefer-const`](https://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](https://eslint.org/docs/rules/no-const-assign.html)

  > 尽量避免为一个变量重新赋值,因为这可能会导致 bug 或者让代码难以理解.

  ```javascript
  // bad
  var a = 1;
  var b = 2;

  // good
  const a = 1;
  const b = 2;
  ```

<a name="references--disallow-var"></a><a name="2.2"></a>

- [2.2](#references--disallow-var) 如果一定要为一个变量重新赋值, 使用 `let` 替代 `var`. eslint: [`no-var`](https://eslint.org/docs/rules/no-var.html)

  > `let` 是块级作用域(`var`是函数作用域)/不存在变量提升/在相同作用域内不能重复声明, 优于 `var`.

  ```javascript
  // bad
  var count = 1;
  if (true) {
      count += 1;
  }

  // good, use the let.
  let count = 1;
  if (true) {
      count += 1;
  }
  ```

<a name="references--block-scope"></a><a name="2.3"></a>

- [2.3](#references--block-scope) `let` 和 `const` 是块级作用域, 而 `var` 是函数级作用域.

  ```javascript
  // const and let only exist in the blocks they are defined in.
  {
      let a = 1;
      const b = 1;
      var c = 1;
  }
  console.log(a); // ReferenceError
  console.log(b); // ReferenceError
  console.log(c); // Prints 1
  ```

  在上面的代码中, 可以看到在控制台打印 `a` 和 `b` 将产生 ReferenceError, 而 `c` 则打印出了 number 数字. 这是因为 `a` 和 `b` 是块级作用域, 而 `c` 是函数级作用域.

**[⬆ back to top](#table-of-contents)**

## Objects

<a name="objects--no-new"></a><a name="3.1"></a>

- [3.1](#objects--no-new) 创建对象使用`对象字面量`的语法. eslint: [`no-new-object`](https://eslint.org/docs/rules/no-new-object.html)

  ```javascript
  // bad
  const item = new Object();

  // good
  const item = {};
  ```

<a name="es6-computed-properties"></a><a name="3.4"></a>

- [3.2](#es6-computed-properties) 在创建具有动态属性名的对象时使用计算属性名.

  > 允许你在一个地方定义一个对象的所有属性

  ```javascript
  function getKey(k) {
      return `a key named ${k}`;
  }

  // bad
  const obj = {
      id: 5,
      name: 'San Francisco',
  }
  obj[getKey('enabled')] = true

  // good
  const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true,
  }
  ```

<a name="es6-object-shorthand"></a><a name="3.5"></a>

- [3.3](#es6-object-shorthand) 给对象声明方法的时候,使用简略的表达方式. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

  ```javascript
  // bad
  const atom = {
      value: 1,

      addValue: function (value) {
          return atom.value + value
      },
  }

  // good
  const atom = {
      value: 1,

      addValue(value) {
          return atom.value + value
      },
  }
  ```

<a name="es6-object-concise"></a><a name="3.6"></a>

- [3.4](#es6-object-concise) 给属性赋值的时候,使用简略的表达方式. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

  ```javascript
  const lukeSkywalker = 'Luke Skywalker'

  // bad
  const obj = {
      lukeSkywalker: lukeSkywalker,
  }

  // good
  const obj = {
      lukeSkywalker,
  }
  ```

<a name="objects--grouped-shorthand"></a><a name="3.7"></a>

- [3.5](#objects--grouped-shorthand) 对象属性赋值的简写形式尽量放到对象声明的最上边.

  > 更清晰的告知哪些属性使用了简写赋值.

  ```javascript
  const anakinSkywalker = 'Anakin Skywalker'
  const lukeSkywalker = 'Luke Skywalker'

  // bad
  const obj = {
    episodeOne: 1,
    twoJediWalkIntoACantina: 2,
    lukeSkywalker,
    episodeThree: 3,
    mayTheFourth: 4,
    anakinSkywalker,
  }

  // good
  const obj = {
    lukeSkywalker,
    anakinSkywalker,
    episodeOne: 1,
    twoJediWalkIntoACantina: 2,
    episodeThree: 3,
    mayTheFourth: 4,
  }
  ```

<a name="objects--quoted-props"></a><a name="3.8"></a>

- [3.6](#objects--quoted-props) 只给有无效标识符(如:-等)的属性加引号. eslint: [`quote-props`](https://eslint.org/docs/rules/quote-props.html)

> 一般来说，我们主观上认为它更容易阅读。 它改进了语法高亮显示，也更容易被许多JS引擎优化。

```javascript
// bad
const bad = {
    'foo': 3,
    'bar': 4,
    'data-blah': 5,
}

// good
const good = {
    foo: 3,
    bar: 4,
    'data-blah': 5,
}
```

<a name="objects--prototype-builtins"></a>

- [3.7](#objects--prototype-builtins) 不要直接使用 `Object.prototype` 方法, 例如 `hasOwnProperty`, `propertyIsEnumerable`, 和 `isPrototypeOf`. eslint: [`no-prototype-builtins`](https://eslint.org/docs/rules/no-prototype-builtins)

  > 这些方法可能会被相关对象上的属性所遮蔽 - 比如 `{ hasOwnProperty: false }` - 或者, 可能是个空对象 (`Object.create(null)`).

  ```javascript
  // bad
  console.log(object.hasOwnProperty(key));

  // good
  console.log(Object.prototype.hasOwnProperty.call(object, key));

  // best
  const has = Object.prototype.hasOwnProperty; // cache the lookup once, in module scope.
  console.log(has.call(object, key));
  /* or */
  import has from 'has'; // https://www.npmjs.com/package/has
  console.log(has(object, key));
  ```

<a name="objects--rest-spread"></a>

- [3.8](#objects--rest-spread) 使用拓展运算符(...)优于使用 [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign). 也可以应用对象的拓展运算,去忽略/覆盖掉某些属性. eslint: [`prefer-object-spread`](https://eslint.org/docs/rules/prefer-object-spread)

  ```javascript
  // very bad
  const original = { a: 1, b: 2 };
  const copy = Object.assign(original, { c: 3 }); // this mutates `original` ಠ_ಠ
  delete copy.a; // so does this

  // bad
  const original = { a: 1, b: 2 };
  const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

  // good
  const original = { a: 1, b: 2 };
  const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

  const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
  ```

**[⬆ back to top](#table-of-contents)**

## Arrays

<a name="arrays--literals"></a><a name="4.1"></a>

- [4.1](#arrays--literals) 使用字面量语法去创建数组. eslint: [`no-array-constructor`](https://eslint.org/docs/rules/no-array-constructor.html)

  ```javascript
  // bad
  const items = new Array();

  // good
  const items = [];
  ```

<a name="arrays--push"></a><a name="4.2"></a>

- [4.2](#arrays--push) 使用 [Array#push](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push) 方法去给数组增加条目.

  ```javascript
  const someStack = []

  // bad
  someStack[someStack.length] = 'abracadabra';

  // good
  someStack.push('abracadabra');
  ```

<a name="es6-array-spreads"></a><a name="4.3"></a>

- [4.3](#es6-array-spreads) 使用数组的拓展运算符 `...` 来浅克隆数组.

  ```javascript
  // bad
  const len = items.length;
  const itemsCopy = [];
  let i;

  for (i = 0; i < len; i += 1) {
      itemsCopy[i] = items[i];
  }

  // good
  const itemsCopy = [...items];
  ```

<a name="arrays--from"></a>
<a name="arrays--from-iterable"></a><a name="4.4"></a>

- [4.4](#arrays--from-iterable) 将一个迭代器的对象(iterable object) 转换成数组时, 使用 `...` 优于使用 [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from).

  ```javascript
  const foo = document.querySelectorAll('.foo');

  // good
  const nodes = Array.from(foo);

  // best
  const nodes = [...foo];
  ```

<a name="arrays--from-array-like"></a>

- [4.5](#arrays--from-array-like) 使用 [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) 可以将类数组对象转换成数组,如一个对象包含`length`属性.

  ```javascript
  const arrLike = { 0: 'foo', 1: 'bar', 2: 'baz', length: 3 };

  // bad
  const arr = Array.prototype.slice.call(arrLike);

  // good
  const arr = Array.from(arrLike);
  ```

<a name="arrays--mapping"></a>

- [4.6](#arrays--mapping) 使用 [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) 替代 `...` 来map迭代数组, 因为它可以避免创建一次迭代器.

  ```javascript
  // bad
  const baz = [...foo].map(bar);

  // good
  const baz = Array.from(foo, bar);
  ```

<a name="arrays--callback-return"></a><a name="4.5"></a>

- [4.7](#arrays--callback-return) 在数组方法回调中使用return语句。 如果函数体由一条语句组成，返回一个没有副作用的表达式，那么省略return是可以的 , 遵从如下规则 [8.2](#arrows--implicit-return). eslint: [`array-callback-return`](https://eslint.org/docs/rules/array-callback-return)

  ```javascript
  // good
  ;[1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
  })

  // good
  ;[1, 2, 3].map((x) => x + 1);

  // bad - no returned value means `acc` becomes undefined after the first iteration
  [
      [0, 1],
      [2, 3],
      [4, 5],
  ].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
  })

  // good
  ;[
      [0, 1],
      [2, 3],
      [4, 5],
  ].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
      return flatten;
  })

  // bad
  inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
          return author === 'Harper Lee';
      } else {
          return false;
      }
  })

  // good
  inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
          return author === 'Harper Lee';
      }

      return false;
  })
  ```

<a name="arrays--bracket-newline"></a>

- [4.8](#arrays--bracket-newline) 如果数组有多行，则在数组左括号和右括号之前使用换行符

  ```javascript
  // bad
  const arr = [
      [0, 1], [2, 3], [4, 5],
  ];

  const objectInArray = [{
      id: 1,
  }, {
      id: 2,
  }];

  const numberInArray = [
      1, 2,
  ];

  // good
  const arr = [
      [0, 1],
      [2, 3],
      [4, 5],
  ];

  const objectInArray = [
      {
          id: 1,
      },
      {
          id: 2,
      },
  ];

  const numberInArray = [1, 2];
  ```

**[⬆ back to top](#table-of-contents)**

## Destructuring

<a name="destructuring--object"></a><a name="5.1"></a>

- [5.1](#destructuring--object) 在访问和使用对象的多个属性时使用对象解构. eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

  > 解构不必为这些属性创建临时引用，并避免重复访问对象。 重复的对象访问创建了更多重复的代码，需要更多的阅读，并创造了更多的错误机会。 解构对象还提供了块中使用的对象结构的一个定义点，而不是需要读取整个块来确定使用了什么。

  ```javascript
  // bad
  function getFullName(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
  }

  // good
  function getFullName(user) {
      const { firstName, lastName } = user;
      return `${firstName} ${lastName}`;
  }

  // best
  function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
  }
  ```

<a name="destructuring--array"></a><a name="5.2"></a>

- [5.2](#destructuring--array) 使用数组解构. eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

  ```javascript
  const arr = [1, 2, 3, 4];

  // bad
  const first = arr[0];
  const second = arr[1];

  // good
  const [first, second] = arr;
  ```

<a name="destructuring--object-over-array"></a><a name="5.3"></a>

- [5.3](#destructuring--object-over-array) 当遇到多个值返回的时候, 要使用对象, 而不是数组, 以便在解构的时候可读性更强.

  > 因为对象结构可以随时增加新的属性, 并且调整顺序, 不易出错.

  ```javascript
  // bad
  function processInput(input) {
      // then a miracle occurs
      return [left, right, top, bottom];
  }

  // the caller needs to think about the order of return data
  const [left, __, top] = processInput(input);

  // good
  function processInput(input) {
      // then a miracle occurs
      return { left, right, top, bottom };
  }

  // the caller selects only the data they need
  const { left, top } = processInput(input);
  ```

**[⬆ back to top](#table-of-contents)**

## Strings

<a name="strings--quotes"></a><a name="6.1"></a>

- [6.1](#strings--quotes) 字符串使用单引号 `''` . eslint: [`quotes`](https://eslint.org/docs/rules/quotes.html)

  ```javascript
  // bad
  const name = "Capt. Janeway";

  // bad - template literals should contain interpolation or newlines
  const name = `Capt. Janeway`;

  // good
  const name = 'Capt. Janeway';
  ```

<a name="strings--line-length"></a><a name="6.2"></a>

- [6.2](#strings--line-length) 超长字符(120个以上)的字符串不应该使用字符串连接跨多行写入.

  > 分行断开字符串使用起来比较痛苦, 而且影响搜索.

  ```javascript
  // bad
  const errorMessage =
    'This is a super long error that was thrown because \
  of Batman. When you stop to think about how Batman had anything to do \
  with this, you would get nowhere \
  fast.';

  // bad
  const errorMessage =
    'This is a super long error that was thrown because ' +
    'of Batman. When you stop to think about how Batman had anything to do ' +
    'with this, you would get nowhere fast.';

  // good
  const errorMessage =
    'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
  ```

<a name="es6-template-literals"></a><a name="6.4"></a>

- [6.3](#es6-template-literals) 在以编程方式构建字符串时，应使用模板字符串而不是连接字符串. eslint: [`prefer-template`](https://eslint.org/docs/rules/prefer-template.html) [`template-curly-spacing`](https://eslint.org/docs/rules/template-curly-spacing)

  > 模板字符串提供了更好的可读性，简洁的语法和适当的换行和字符串插值特性 .

  ```javascript
  // bad
  function sayHi(name) {
      return 'How are you, ' + name + '?';
  }

  // bad
  function sayHi(name) {
      return ['How are you, ', name, '?'].join();
  }

  // bad
  function sayHi(name) {
      return `How are you, ${name}?`;
  }

  // good
  function sayHi(name) {
      return `How are you, ${name}?`;
  }
  ```

<a name="strings--eval"></a><a name="6.5"></a>

- [6.4](#strings--eval) 非万不得已不要使用 `eval()`, 它可能造成很多的隐患. eslint: [`no-eval`](https://eslint.org/docs/rules/no-eval)

<a name="strings--escaping"></a>

- [6.5](#strings--escaping) 不要在字符串中包含非必要的转义字符. eslint: [`no-useless-escape`](https://eslint.org/docs/rules/no-useless-escape)

  > 损害可读性, 转义字符只在必要的地方出现.

  ```javascript
  // bad
  const foo = '\'this\' is "quoted"';

  // good
  const foo = '\'this\' is "quoted"';
  const foo = `my name is '${name}'`;
  ```

**[⬆ back to top](#table-of-contents)**

## Functions

<a name="functions--declarations"></a><a name="7.1"></a>

- [7.1](#functions--declarations) 使用命名函数表达式而不是函数声明. eslint: [`func-style`](https://eslint.org/docs/rules/func-style)

  > 函数声明包含状态提升, 在文件中定义函数之前可以去使用这个函数, 损害了可读性和可维护性。 如果发现函数的定义非常大或复杂，以至于影响了对文件中其余部分的理解，那么可能是时候将其提取到自己的模块中了! 不要忘记显式地为表达式命名，而不管该名称是否从包含的变量推断出来(这在现代浏览器或使用Babel等编译器时经常发生)。 这就消除了对Error调用栈的。  ([Discussion](https://github.com/airbnb/javascript/issues/794))

  ```javascript
  // bad
  function foo() {
      // ...
  }

  // bad
  const foo = function () {
      // ...
  };

  // bad
  const foo = () => {
      // ...
  };

  // good
  // lexical name distinguished from the variable-referenced invocation(s)
  const short = function longUniqueMoreDescriptiveLexicalFoo() {
      // ...
  };
  ```

<a name="functions--iife"></a><a name="7.2"></a>

- [7.2](#functions--iife) 将立即调用的函数表达式包裹在括号中. eslint: [`wrap-iife`](https://eslint.org/docs/rules/wrap-iife.html)

  ```javascript
  // immediately-invoked function expression (IIFE)
  (function () {
    console.log('Welcome to the Internet. Please follow me.')
  })();
  ```

<a name="functions--in-blocks"></a><a name="7.3"></a>

- [7.3](#functions--in-blocks) 不要在非方法块中直接声明方法 (`if`, `while`等). 可以把它赋值给一个变量. 不同浏览器对声明方法有不同的解释, 建议统一使用变量去接收. eslint: [`no-loop-func`](https://eslint.org/docs/rules/no-loop-func.html)

  ```javascript
  // bad
  for (let i=10; i; i--) {
      (function() { return i; })();
  }

  while(i) {
      const a = function() { return i; };
      a();
  }

  do {
      function a() { return i; };
      a();
  } while (i);

  let foo = 0;
  for (let i = 0; i < 10; ++i) {
      //Bad, `foo` is not in the loop-block's scope and `foo` is modified in/after the loop
      setTimeout(() => console.log(foo));
      foo += 1;
  }

  for (let i = 0; i < 10; ++i) {
      //Bad, `foo` is not in the loop-block's scope and `foo` is modified in/after the loop
      setTimeout(() => console.log(foo));
  }
  foo = 100;

  // good
  const a = function() {};

  for (let i=10; i; i--) {
      a();
  }

  for (let i=10; i; i--) {
      const a = function() {}; // OK, no references to variables in the outer scopes.
      a();
  }

  for (let i=10; i; i--) {
      const a = function() { return i; }; // OK, all references are referring to block scoped variables in the loop.
      a();
  }

  const foo = 100;
  for (let i=10; i; i--) {
      const a = function() { return foo; }; // OK, all references are referring to never modified variables.
      a();
  }
  //... no modifications of foo after this loop ...
  ```

<a name="functions--note-on-blocks"></a><a name="7.4"></a>

- [7.4](#functions--note-on-blocks) **Note:** 同上, ECMA-262 不要再非方法块 `block` 中声明方法. 方法的声明不是一个表达式.

  ```javascript
  // bad
  if (currentUser) {
      function test() {
          console.log('Nope.')
      }
  }

  // good
  let test
  if (currentUser) {
      test = () => {
          console.log('Yup.')
      }
  }
  ```

<a name="functions--arguments-shadow"></a><a name="7.5"></a>

- [7.5](#functions--arguments-shadow) 不要给参数命名为 `arguments`.

  ```javascript
  // bad
  function foo(name, options, arguments) {
    // ...
  }

  // good
  function foo(name, options, args) {
    // ...
  }
  ```

<a name="es6-rest"></a><a name="7.6"></a>

- [7.6](#es6-rest) 不要使用 `arguments`, 优先使用rest语法 `...` 替代. eslint: [`prefer-rest-params`](https://eslint.org/docs/rules/prefer-rest-params)

  > `...` 明确知道要处理哪些参数. 另外, `rest arguments` 是真正的数组, 而`arguments` 是 Array-like .

  ```javascript
  // bad
  function concatenateAll() {
    const args = Array.prototype.slice.call(arguments)
    return args.join('')
  }

  // good
  function concatenateAll(...args) {
    return args.join('')
  }
  ```

<a name="es6-default-parameters"></a><a name="7.7"></a>

- [7.7](#es6-default-parameters) 使用默认参数语法，而不是去直接修改函数参数.

  ```javascript
  // really bad
  function handleThings(opts) {
      // No! We shouldn’t mutate function arguments.
      // Double bad: if opts is falsy it'll be set to an object which may
      // be what you want but it can introduce subtle bugs.
      opts = opts || {};
      // ...
  }

  // still bad
  function handleThings(opts) {
      if (opts === void 0) {
          opts = {};
      }
      // ...
  }

  // good
  function handleThings(opts = {}) {
      // ...
  }
  ```

<a name="functions--default-side-effects"></a><a name="7.8"></a>

- [7.8](#functions--default-side-effects) 禁止在参数上引入副作用.

  > 让推算一个函数的返回值变得非常复杂

  ```javascript
  var b = 1;
  // bad
  function count(a = b++) {
      console.log(a);
  }
  count(); // 1
  count(); // 2
  count(3); // 3
  count(); // 3
  ```

<a name="functions--defaults-last"></a><a name="7.9"></a>

- [7.9](#functions--defaults-last) 把有默认值的参数放到参数列表最后. eslint: [`default-param-last`](https://eslint.org/docs/rules/default-param-last)

  ```javascript
  // bad
  function handleThings(opts = {}, name) {
    // ...
  }

  // good
  function handleThings(name, opts = {}) {
    // ...
  }
  ```

<a name="functions--constructor"></a><a name="7.10"></a>

- [7.10](#functions--constructor) 不要使用函数的构造器去创建新函数. eslint: [`no-new-func`](https://eslint.org/docs/rules/no-new-func)

  > 以这种方式创建一个函数，计算字符串的结果类似于 `eval()`, 不安全.

  ```javascript
  // bad
  var add = new Function('a', 'b', 'return a + b');

  // still bad
  var subtract = Function('a', 'b', 'return a - b');
  ```

<a name="functions--signature-spacing"></a><a name="7.11"></a>

- [7.11](#functions--signature-spacing) 函数签名中的空格. eslint: [`space-before-function-paren`](https://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks)

  > 保持代码的一致性

  ```javascript
  // bad
  const f = function(){};
  const g = function (){};
  const h = function() {};

  // good
  const x = function () {}
  const y = function a() {}
  ```

<a name="functions--mutate-params"></a><a name="7.12"></a>

- [7.12](#functions--mutate-params) 函数的参数不可变. eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

  > 操作作为参数传入的对象可能会在原始调用方中引起不必要的变量副作用

  ```javascript
  // bad
  function f1(obj) {
      obj.key = 1
  }

  // good
  function f2(obj) {
      const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1
  }
  ```

<a name="functions--reassign-params"></a><a name="7.13"></a>

- [7.13](#functions--reassign-params) 函数参数不能重新被赋值. eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

  ```javascript
  // bad
  function f1(a) {
      a = 1
      // ...
  }

  function f2(a) {
      if (!a) {
          a = 1
      }
      // ...
  }

  // good
  function f3(a) {
      const b = a || 1
      // ...
  }

  function f4(a = 1) {
      // ...
  }
  ```

<a name="functions--spread-vs-apply"></a><a name="7.14"></a>

- [7.14](#functions--spread-vs-apply) 优先使用拓展运算符的语法 `...`, 而不是去使用 `call` 或者 `apply`. eslint: [`prefer-spread`](https://eslint.org/docs/rules/prefer-spread)

  > 更清晰, 不再需要去提供一个上下文

  ```javascript
  // bad
  const x = [1, 2, 3, 4, 5]
  console.log.apply(console, x)

  // good
  const x = [1, 2, 3, 4, 5]
  console.log(...x)

  // bad
  new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]))()

  // good
  new Date(...[2016, 8, 5])
  ```

<a name="functions--signature-invocation-indentation"></a>

- [7.15](#functions--signature-invocation-indentation) 函数如果签名需要占用多行,要对齐,且最后一个闭合括号单独占一行. eslint: [`function-paren-newline`](https://eslint.org/docs/rules/function-paren-newline)

  ```javascript
  // bad
    function foo(bar,
                 baz,
                 quux) {
        // ...
    }

  // good
  function foo(
      bar,
      baz,
      quux,
  ) {
    // ...
  }

  // bad
  console.log(foo,
      bar,
      baz);

  // good
  console.log(
      foo,
      bar,
      baz,
  );

  ```

**[⬆ back to top](#table-of-contents)**

## Arrow Functions

<a name="arrows--use-them"></a><a name="8.1"></a>

- [8.1](#arrows--use-them) 当需要使用匿明函数时(比如只是一个单行返回的函数), 使用箭头函数. eslint: [`prefer-arrow-callback`](https://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](https://eslint.org/docs/rules/arrow-spacing.html)

  > 箭头函数默认包含了`this`上下文, 而且语法更简洁

  > 如果有一个相当复杂的函数，可以将该逻辑移出到它自己的命名函数表达式中

  ```javascript
  // bad
  [1, 2, 3].map(function (x) {
      const y = x + 1;
      return x * y;
  })

  // good
  [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
  })
  ```

<a name="arrows--implicit-return"></a><a name="8.2"></a>

- [8.2](#arrows--implicit-return) 如果一个函数体只包含了单行返回值的 [表达式(expression)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) ,而且没有副作用, 省略大括号，使用隐式返回. 否则, 保留括号并且使用 `return` 表达式. eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](https://eslint.org/docs/rules/arrow-body-style.html)

  > 语法糖. 当多个函数链接在一起时,

  ```javascript
  // bad
  [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      `A string containing the ${nextNumber}.`;
  });

  // good
  [1, 2, 3].map((number) => `A string containing the ${number + 1}.`);

  // good
  [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      return `A string containing the ${nextNumber}.`;
  })

  // good
  [1, 2, 3].map((number, index) => ({
      [index]: number,
  }))

  // No implicit return with side effects
  function foo(callback) {
      const val = callback();
      if (val === true) {
          // Do something if callback returns true
      }
  }

  let bool = false;

  // bad
  foo(() => (bool = true))

  // good
  foo(() => {
      bool = true;
  })
  ```

<a name="arrows--paren-wrap"></a><a name="8.3"></a>

- [8.3](#arrows--paren-wrap) 如果表达式跨越多行，请将其括在括号中以提高可读性.

  > 让函数的起止更明显

  ```javascript
  // bad
  ['get', 'post', 'put'].map((httpMethod) => Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
    )
  );

  // good
  ['get', 'post', 'put'].map((httpMethod) =>
      Object.prototype.hasOwnProperty.call(
          httpMagicObjectWithAVeryLongName,
          httpMethod,
      ),
  );
  ```

<a name="arrows--one-arg-parens"></a><a name="8.4"></a>

- [8.4](#arrows--one-arg-parens) 为了清晰和一致，参数周围一定要加圆括号. eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html)

  ```javascript
  // bad
  [1, 2, 3].map(x => x * x);

  // good
  [1, 2, 3].map((x) => x * x);

  // bad
  [1, 2, 3].map(
      number =>
        `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`,
  );

  // good
  [1, 2, 3].map(
      (number) =>
        `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`,
  );

  // bad
  [1, 2, 3].map(x => {
      const y = x + 1;
      return x * y;
  });

  // good
  [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
  });
  ```

<a name="arrows--confusing"></a><a name="8.5"></a>

- [8.5](#arrows--confusing) 当箭头函数语法 (`=>`) 和 (`<=`, `>=`)混合使用时, 避免混淆. eslint: [`no-confusing-arrow`](https://eslint.org/docs/rules/no-confusing-arrow)

  ```javascript
  // bad
  const itemHeight = (item) =>
    item.height <= 256 ? item.largeSize : item.smallSize;

  // bad
  const itemHeight = (item) =>
    item.height >= 256 ? item.largeSize : item.smallSize;

  // good
  const itemHeight = (item) =>
    item.height <= 256 ? item.largeSize : item.smallSize;

  // good
  const itemHeight = (item) => {
      const { height, largeSize, smallSize } = item;
      return height <= 256 ? largeSize : smallSize;
  }
  ```

<a name="whitespace--implicit-arrow-linebreak"></a>

- [8.6](#whitespace--implicit-arrow-linebreak) 使用隐式返回箭头函数体必须是单行函数. eslint: [`implicit-arrow-linebreak`](https://eslint.org/docs/rules/implicit-arrow-linebreak)

  ```javascript
  // bad
  (foo) =>
      bar;

  (foo) =>
      (bar);

  // good
  (foo) => bar;
  (foo) => (bar);
  (foo) => (
      bar
  )
  ```

**[⬆ back to top](#table-of-contents)**

## Classes & Constructors

<a name="constructors--use-class"></a><a name="9.1"></a>

- [9.1](#constructors--use-class) 使用 `class`. 避免直接使用 `prototype`.

  > `class` 语法更加简洁.

  ```javascript
  // bad
  function Queue(contents = []) {
      this.queue = [...contents]
  }
  Queue.prototype.pop = function () {
      const value = this.queue[0]
      this.queue.splice(0, 1)
      return value
  }

  // good
  class Queue {
      constructor(contents = []) {
          this.queue = [...contents]
      }
      pop() {
          const value = this.queue[0]
          this.queue.splice(0, 1)
          return value
      }
  }
  ```

<a name="constructors--extends"></a><a name="9.2"></a>

- [9.2](#constructors--extends) 继承时使用 `extends`.

  > 这是一种不破坏' instanceof '而继承原型功能的内置方法。 .

  ```javascript
  // bad
  const inherits = require('inherits')
  function PeekableQueue(contents) {
      Queue.apply(this, contents)
  }
  inherits(PeekableQueue, Queue)
  PeekableQueue.prototype.peek = function () {
      return this.queue[0]
  }

  // good
  class PeekableQueue extends Queue {
      peek() {
          return this.queue[0]
      }
  }
  ```

<a name="constructors--chaining"></a><a name="9.3"></a>

- [9.3](#constructors--chaining) 可以返回 `this` 以便实现链式调用.

  ```javascript
  // bad
  Jedi.prototype.jump = function () {
      this.jumping = true;
      return true;
  }

  Jedi.prototype.setHeight = function (height) {
      this.height = height;
  }

  const luke = new Jedi();
  luke.jump(); // => true
  luke.setHeight(20); // => undefined

  // good
  class Jedi {
      jump() {
          this.jumping = true;
          return this;
      }

    setHeight(height) {
        this.height = height;
        return this;
    }
  }

  const luke = new Jedi();

  luke.jump().setHeight(20);
  ```

<a name="constructors--tostring"></a><a name="9.4"></a>

- [9.4](#constructors--tostring) 可以去重写 `toString()` 方法, 但尽量不要引入副作用.

  ```javascript
  class Jedi {
      constructor(options = {}) {
          this.name = options.name || 'no name'
      }

      getName() {
          return this.name
      }

      toString() {
          return `Jedi - ${this.getName()}`
      }
  }
  ```

<a name="constructors--no-useless"></a><a name="9.5"></a>

- [9.5](#constructors--no-useless) 如果没有指定，则类有一个默认构造函数。 空的构造函数或只委托给父类的构造函数是不必要的,应该删除掉. eslint: [`no-useless-constructor`](https://eslint.org/docs/rules/no-useless-constructor)

  ```javascript
  // bad
  class Jedi {
      constructor() {}

      getName() {
          return this.name
      }
  }

  // bad
  class Rey extends Jedi {
      constructor(...args) {
          super(...args);
      }
  }

  // good
  class Rey extends Jedi {
      constructor(...args) {
          super(...args);
          this.name = 'Rey';
      }
  }
  ```

<a name="classes--no-duplicate-members"></a>

- [9.6](#classes--no-duplicate-members) 禁止重复类成员. eslint: [`no-dupe-class-members`](https://eslint.org/docs/rules/no-dupe-class-members)

  > 重复声明类成员将会造成意想不到的错误, 后声明的成员会覆盖之前的成员.

  ```javascript
  // bad
  class Foo {
      bar() {
          return 1;
      }

      bar() {
          return 2;
      }
  }

  // good
  class Foo {
      bar() {
          return 1;
      }
  }

  // good
  class Foo {
      bar() {
          return 2;
      }
  }
  ```

<a name="classes--methods-use-this"></a>

- [9.7](#classes--methods-use-this) 除非外部库或框架要求使用特定的非静态方法，否则类方法应该使用“this”或成为静态方法。 作为一个实例方法应该去使用类内部的属性或者方法. eslint: [`class-methods-use-this`](https://eslint.org/docs/rules/class-methods-use-this)

  ```javascript
  // bad
  class Foo {
      bar() {
          console.log('bar')
      }
  }

  // good - this is used
  class Foo {
      bar() {
          console.log(this.bar)
      }
  }

  // good - constructor is exempt
  class Foo {
      constructor() {
        // ...
      }
  }

  // good - static methods aren't expected to use this
  class Foo {
      static bar() {
          console.log('bar')
      }
  }
  ```

**[⬆ back to top](#table-of-contents)**

## Modules

<a name="modules--use-them"></a><a name="10.1"></a>

- [10.1](#modules--use-them) 在非标准模块系统上总是使用模块(`import`/`export`).

  > 模块是未来

  ```javascript
  // bad
  const AirbnbStyleGuide = require('./AirbnbStyleGuide')
  module.exports = AirbnbStyleGuide.es6

  // ok
  import AirbnbStyleGuide from './AirbnbStyleGuide'
  export default AirbnbStyleGuide.es6

  // best
  import { es6 } from './AirbnbStyleGuide'
  export default es6
  ```

<a name="modules--no-wildcard"></a><a name="10.2"></a>

- [10.2](#modules--no-wildcard) 不使用通配符(*)导入.

  > 确保只有一个独立的默认导出.

  ```javascript
  // bad
  import * as AirbnbStyleGuide from './AirbnbStyleGuide'

  // good
  import AirbnbStyleGuide from './AirbnbStyleGuide'
  ```

<a name="modules--no-export-from-import"></a><a name="10.3"></a>

- [10.3](#modules--no-export-from-import) 不要直接导出一个导入.

  > 虽然一行写法非常简洁, 但有明确的导入和导出方式可以使条理保持一致

  ```javascript
  // bad
  // filename es6.js
  export { es6 as default } from './AirbnbStyleGuide'

  // good
  // filename es6.js
  import { es6 } from './AirbnbStyleGuide'
  export default es6
  ```

<a name="modules--no-duplicate-imports"></a>

- [10.4](#modules--no-duplicate-imports) 从同一个模块的导入要在同一行代码执行. eslint: [`no-duplicate-imports`](https://eslint.org/docs/rules/no-duplicate-imports)

  >  从同一模块导入,如果拆分成多行代码会使代码更难维护.

  ```javascript
  // bad
  import foo from 'foo'
  // … some other imports … //
  import { named1, named2 } from 'foo'

  // good
  import foo, { named1, named2 } from 'foo'

  // good
  import foo, { named1, named2 } from 'foo'
  ```

<a name="modules--no-mutable-exports"></a>

- [10.5](#modules--no-mutable-exports) 不要导出可变的内容. eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)

  > 通常应该只导出常量引用

  ```javascript
  // bad
  let foo = 3
  export { foo }

  // good
  const foo = 3
  export { foo }
  ```

<a name="modules--prefer-default-export"></a>

- [10.6](#modules--prefer-default-export) 如果模块只有一个导出, 首选 `default export`而不是命名的 `export`.
  eslint: [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)

  > 在具有单个导出的模块中，首选`default export`而不是命名的 `export`。 鼓励更多只导出一个东西的文件，这有利于可读性和可维护性。

  ```javascript
  // bad
  export function foo() {}

  // good
  export default function foo() {}
  ```

<a name="modules--imports-first"></a>

- [10.7](#modules--imports-first) 将 `import` 放到文件最上边.
  eslint: [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)

  > 提升代码可读性

  ```javascript
  // bad
  import foo from 'foo'
  foo.init()

  import bar from 'bar'

  // good
  import foo from 'foo'
  import bar from 'bar'

  foo.init()
  ```

<a name="modules--multiline-imports-over-newlines"></a>

- [10.8](#modules--multiline-imports-over-newlines) 多行导入应该像多行数组和对象字面量一样缩进.
  eslint: [`object-curly-newline`](https://eslint.org/docs/rules/object-curly-newline)

  > 花括号遵循与样式指南中其他花括号块相同的缩进规则，后面的逗号也是如此.

  ```javascript
  // bad
  import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

  // good
  import {
      longNameA,
      longNameB,
      longNameC,
      longNameD,
      longNameE,
  } from 'path';
  ```

<a name="modules--no-webpack-loader-syntax"></a>

- [10.9](#modules--no-webpack-loader-syntax) 在模块导入语句中禁止Webpack加载器语法。 .
  eslint: [`import/no-webpack-loader-syntax`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)

  > 因为在导入中使用Webpack语法会将代码耦合到一个模块绑定器。 最好使用`webpack.config.js`中的加载器语法。

  ```javascript
  // bad
  import fooSass from 'css!sass!foo.scss'
  import barCss from 'style!css!bar.css'

  // good
  import fooSass from 'foo.scss'
  import barCss from 'bar.css'
  ```

<a name="modules--import-extensions"></a>

- [10.10](#modules--import-extensions) 不要包含JavaScript文件拓展名
  eslint: [`import/extensions`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/extensions.md)

  > 导入不需要知道文件的拓展名

  ```javascript
  // bad
  import foo from './foo.js'
  import bar from './bar.jsx'
  import baz from './baz/index.jsx'

  // good
  import foo from './foo'
  import bar from './bar'
  import baz from './baz'
  ```

**[⬆ back to top](#table-of-contents)**

## Iterators and Generators

<a name="iterators--nope"></a><a name="11.1"></a>

- [11.1](#iterators--nope) 不要使用迭代器. 优先使用 JavaScript 高阶函数去替代循环操作 `for-in` 或者 `for-of`. eslint: [`no-iterator`](https://eslint.org/docs/rules/no-iterator.html) [`no-restricted-syntax`](https://eslint.org/docs/rules/no-restricted-syntax)

  > 这就实现了我们的不可变规则。 处理返回值的纯函数比处理副作用更容易推理。

  > Use `map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some()` / ... 来迭代数组, 而 `Object.keys()` / `Object.values()` / `Object.entries()` 来迭代对象.

  ```javascript
  const numbers = [1, 2, 3, 4, 5];

  // bad
  let sum = 0;
  for (let num of numbers) {
      sum += num;
  }
  sum === 15;

  // good
  let sum = 0;
  numbers.forEach((num) => {
      sum += num;
  })
  sum === 15;

  // best (use the functional force)
  const sum = numbers.reduce((total, num) => total + num, 0);
  sum === 15;

  // bad
  const increasedByOne = [];
  for (let i = 0; i < numbers.length; i++) {
      increasedByOne.push(numbers[i] + 1);
  }

  // good
  const increasedByOne = [];
  numbers.forEach((num) => {
      increasedByOne.push(num + 1);
  })

  // best (keeping it functional)
  const increasedByOne = numbers.map((num) => num + 1);
  ```

<a name="generators--nope"></a><a name="11.2"></a>

- [11.2](#generators--nope) 不要轻易使用迭代器(generators)

  > 还不能被很好的转义成es5

<a name="generators--spacing"></a>

- [11.3](#generators--spacing) 如果一定要使用迭代器(generators), 或者如果你不同意 [our advice](#generators--nope), 确保迭代器(generators)的函数签名的间隔正确 . eslint: [`generator-star-spacing`](https://eslint.org/docs/rules/generator-star-spacing)

  > `function` 和 `*` 是相同的概念关键字的一部分 - `*` 不是 `function` 的修饰语, `function*`是一个独立的构造函数,完全不同于 `function`.

  ```javascript
  // bad
  function * foo() {
      // ...
  }

  // bad
  const bar = function *() {
      // ...
  }

  // bad
  const baz = function*() {
      // ...
  }

  // bad
  const quux = function* () {
      // ...
  }

  // bad
  function*foo() {
      // ...
  }

  // bad
  function *foo() {
      // ...
  }

  // very bad
  function
  *
  foo() {
      // ...
  }

  // very bad
  const wat = function
  *
  () {
      // ...
  };

  // good
  function* foo() {
    // ...
  }

  // good
  const foo = function* () {
    // ...
  }
  ```

**[⬆ back to top](#table-of-contents)**

## Properties

<a name="properties--dot"></a><a name="12.1"></a>

- [12.1](#properties--dot) 访问属性时使用点表示法. eslint: [`dot-notation`](https://eslint.org/docs/rules/dot-notation.html)

  ```javascript
  const luke = {
    jedi: true,
    age: 28,
  };

  // bad
  const isJedi = luke['jedi'];

  // good
  const isJedi = luke.jedi;
  ```

<a name="properties--bracket"></a><a name="12.2"></a>

- [12.2](#properties--bracket) 在访问带有变量的属性时使用方括号'[]'.

  ```javascript
  const luke = {
      jedi: true,
      age: 28,
  }

  function getProp(prop) {
      return luke[prop]
  }

  const isJedi = getProp('jedi')
  ```

<a name="es2016-properties--exponentiation-operator"></a>

- [12.3](#es2016-properties--exponentiation-operator) 在计算取幂时使用求幂运算符 `**`. eslint: [`no-restricted-properties`](https://eslint.org/docs/rules/no-restricted-properties).

  ```javascript
  // bad
  const binary = Math.pow(2, 10)

  // good
  const binary = 2 ** 10
  ```

  <a name="optional--chaining"></a>

- [12.4](#optional--chaining) 可选链操作符 (?.) 需要注意的是:如果存在一个属性名且不是函数，使用 ?. 仍然会产生一个 TypeError 异常 (`x.y is not a function`). eslint: [`no-unsafe-optional-chaining`](https://eslint.org/docs/rules/no-unsafe-optional-chaining#disallow-use-of-optional-chaining-in-contexts-where-the-undefined-value-is-not-allowed-no-unsafe-optional-chaining).

  > 检测一些使用可选链接不能防止运行时错误的情况。 特别是，它会在短路到未定义的位置标记可选的链接表达式，这些位置会导致随后抛出TypeError。

  ```javascript
  // bad
  (obj?.foo)();

  // good
  obj?.foo?.();
  ```

**[⬆ back to top](#table-of-contents)**

## Variables

<a name="variables--const"></a><a name="13.1"></a>

- [13.1](#variables--const) 永远使用 `const` 或 `let` 去声明变量. eslint: [`no-undef`](https://eslint.org/docs/rules/no-undef) [`prefer-const`](https://eslint.org/docs/rules/prefer-const)

  ```javascript
  // bad
  superPower = new SuperPower();

  // good
  const superPower = new SuperPower();
  ```

<a name="variables--one-const"></a><a name="13.2"></a>

- [13.2](#variables--one-const) 独立每个 `const` 或者 `let` 去声明变量. eslint: [`one-var`](https://eslint.org/docs/rules/one-var.html)

  > 这种方式更明确, 赋值清晰, 易于调试.

  ```javascript
  // bad
  const items = getItems(),
      goSportsTeam = true,
      dragonball = 'z'

  // bad
    const items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

  // good
  const items = getItems();
  const goSportsTeam = true;
  const dragonball = 'z';
  ```

<a name="variables--const-let-group"></a><a name="13.3"></a>

- [13.3](#variables--const-let-group) 将 `const` 变量 和 `let`变量 分组摆放.

  > 当需要给变量重新赋值, 更易于查找哪些变量可变, 哪些不可变

  ```javascript
  // bad
  let i,
      len,
      dragonball,
      items = getItems(),
      goSportsTeam = true

  // bad
  let i;
  const items = getItems();
  let dragonball;
  const goSportsTeam = true;
  let len;

  // good
  const goSportsTeam = true;
  const items = getItems();
  let dragonball;
  let i;
  let length;
  ```

<a name="variables--define-where-used"></a><a name="13.4"></a>

- [13.4](#variables--define-where-used) 在需要变量的地方赋值，但要把它们放在一个合理的位置

  >  `let` 和 `const` 是块级变量,不是函数级变量.

  ```javascript
  // bad - unnecessary function call
  function checkName(hasName) {
      const name = getName();

      if (hasName === 'test') {
          return false;
      }

      if (name === 'test') {
          this.setName('');
          return false;
      }

      return name;
  }

  // good
  function checkName(hasName) {
      if (hasName === 'test') {
          return false;
      }

      const name = getName();

      if (name === 'test') {
          this.setName('');
          return false;
      }

      return name;
  }
  ```

<a name="variables--no-chain-assignment"></a><a name="13.5"></a>

- [13.5](#variables--no-chain-assignment) 禁止链式给变量赋值. eslint: [`no-multi-assign`](https://eslint.org/docs/rules/no-multi-assign)

  > 链接变量赋值会创建隐式全局变量.

  ```javascript
    // bad
    (function example() {
        // JavaScript interprets this as
        // let a = ( b = ( c = 1 ) );
        // The let keyword only applies to variable a; variables b and c become
        // global variables.
        // 这里加括号也不可以 let a = （b = c = 1);
        let a = b = c = 1;
    }());

  console.log(a); // throws ReferenceError
  console.log(b); // 1
  console.log(c); // 1

  // good
  (function example() {
      let a = 1;
      let b = a;
      let c = a;
  })();

  console.log(a); // throws ReferenceError
  console.log(b); // throws ReferenceError
  console.log(c); // throws ReferenceError

  // the same applies for `const`
  ```

<a name="variables--unary-increment-decrement"></a><a name="13.6"></a>

- [13.6](#variables--unary-increment-decrement) 尽量避免使用一元递增和递减 (`++`, `--`). eslint [`no-plusplus`](https://eslint.org/docs/rules/no-plusplus)

  > eslint文档中，一元递增和递减语句会自动插入分号，并且会在应用程序中增加或减少值时导致静默错误。 使用' num+ = 1 '而不是' num++ '或' num++ '这样的语句来改变值也更有表现力。 禁用一元递增和递减语句还可以防止无意中预递增/预递减值，这也会导致程序中出现意外行为。

  ```javascript
  // bad

  const array = [1, 2, 3]
  let num = 1
  num++
  --num

  let sum = 0
  let truthyCount = 0
  for (let i = 0; i < array.length; i++) {
      let value = array[i]
      sum += value
      if (value) {
          truthyCount++
      }
  }

  // good

  const array = [1, 2, 3]
  let num = 1
  num += 1
  num -= 1

  const sum = array.reduce((a, b) => a + b, 0)
  const truthyCount = array.filter(Boolean).length
  ```

<a name="variables--linebreak"></a>

- [13.7](#variables--linebreak) 避免在赋值' = '之前或之后换行. 如果你的赋值违反了最大单行长度 [`max-len`](https://eslint.org/docs/rules/max-len.html), 将值用括号括起来. eslint [`operator-linebreak`](https://eslint.org/docs/rules/operator-linebreak.html).

  > 赋值 `=` 便随着换行符可能会对赋值的值产生混淆.

  ```javascript
  // bad
  const foo =
    superLongLongLongLongLongLongLongLongFunctionName();

  // bad
  const foo
    = 'superLongLongLongLongLongLongLongLongString';

  // good
    const foo = (
        superLongLongLongLongLongLongLongLongFunctionName()
    );

  // good
  const foo = 'superLongLongLongLongLongLongLongLongString'
  ```

<a name="variables--no-unused-vars"></a>

- [13.8](#variables--no-unused-vars) 不允许有变量声明了但不使用. eslint: [`no-unused-vars`](https://eslint.org/docs/rules/no-unused-vars)

  > 在代码中声明且未使用的变量很可能是由于重构不完全而导致的错误。 这样的变量会占用代码的空间，并可能导致读者的困惑。

  ```javascript
  // bad

  var some_unused_var = 42;

  // Write-only variables are not considered as used.
  var y = 10;
  y = 5;

  // A read for a modification of itself is not considered as used.
  var z = 0;
  z = z + 1;

  // Unused function arguments.
  function getX(x, y) {
      return x;
  }

  // good

  function getXPlusY(x, y) {
      return x + y;
  }

  var x = 1;
  var y = a + 2;

  alert(getXPlusY(x, y));

  // 'type' is ignored even if unused because it has a rest property sibling.
  // This is a form of extracting an object that omits the specified keys.
  var { type, ...coords } = data;
  // 'coords' is now the 'data' object without its 'type' property.
  ```

**[⬆ back to top](#table-of-contents)**

## Hoisting

<a name="hoisting--about"></a><a name="14.1"></a>

- [14.1](#hoisting--about) `var` 声明会被提升到它们最近的封闭函数作用域的顶部，但它的赋值并不会提升. `const` 和 `let` 声明被赋予了一个新概念, [Temporal Dead Zones (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#temporal_dead_zone_tdz). 这个知识很重要 - [typeof is no longer safe](https://web.archive.org/web/20200121061528/http://es-discourse.com/t/why-typeof-is-no-longer-safe/15).

  ```javascript
  // we know this wouldn’t work (assuming there
  // is no notDefined global variable)
  function example() {
      console.log(notDefined); // => throws a ReferenceError
  }

  // creating a variable declaration after you
  // reference the variable will work due to
  // variable hoisting. Note: the assignment
  // value of `true` is not hoisted.
  function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
  }

  // the interpreter is hoisting the variable
  // declaration to the top of the scope,
  // which means our example could be rewritten as:
  function example() {
      let declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
  }

  // using const and let
  function example() {
      console.log(declaredButNotAssigned); // => throws a ReferenceError
      console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
      const declaredButNotAssigned = true;
  }
  ```

<a name="hoisting--anon-expressions"></a><a name="14.2"></a>

- [14.2](#hoisting--anon-expressions) 匿名函数表达式提升它们的变量名，但不提升函数赋值.

  ```javascript
  function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function () {
          console.log('anonymous function expression');
      };
  }
  ```

<a name="hoisting--named-expresions"></a><a name="hoisting--named-expressions"></a><a name="14.3"></a>

- [14.3](#hoisting--named-expressions) 命名函数表达式提升的是变量名，而不是函数名或函数体.

  ```javascript
  function example() {
      console.log(named) // => undefined

      named() // => TypeError named is not a function

      superPower() // => ReferenceError superPower is not defined

      var named = function superPower() {
          console.log('Flying')
      }
  }

  // the same is true when the function name
  // is the same as the variable name.
  function example() {
      console.log(named) // => undefined

      named() // => TypeError named is not a function

      var named = function named() {
          console.log('named')
      }
  }
  ```

<a name="hoisting--declarations"></a><a name="14.4"></a>

- [14.4](#hoisting--declarations) 函数声明提升了它们的名称和函数体.

  ```javascript
  function example() {
      superPower() // => Flying

      function superPower() {
          console.log('Flying')
      }
  }
  ```

- 更多信息请查阅 [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting/) 作者: [Ben Cherry](http://www.adequatelygood.com/).

**[⬆ back to top](#table-of-contents)**

## Comparison Operators & Equality

<a name="comparison--eqeqeq"></a><a name="15.1"></a>

- [15.1](#comparison--eqeqeq) 使用 `===` 和 `!==` 替代 `==` 和 `!=`. eslint: [`eqeqeq`](https://eslint.org/docs/rules/eqeqeq.html)

<a name="comparison--if"></a><a name="15.2"></a>

- [15.2](#comparison--if) 条件语句，如`if`语句，通过强制使用`ToBoolean`抽象方法来计算它们的表达式，并始终遵循以下简单规则:

  - **Objects** evaluate to **true**
  - **Undefined** evaluates to **false**
  - **Null** evaluates to **false**
  - **Booleans** evaluate to **the value of the boolean**
  - **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
  - **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

  ```javascript
  if ([0] && []) {
      // true
      // an array (even an empty one) is an object, objects will evaluate to true
  }
  ```

<a name="comparison--shortcuts"></a><a name="15.3"></a>

- [15.3](#comparison--shortcuts) 布尔值使用快捷方式去比较, 但字符串和数字类型要显式比较.

  ```javascript
  // bad
  if (isValid === true) {
      // ...
  }

  // good
  if (isValid) {
      // ...
  }

  // bad
  if (name) {
      // ...
  }

  // good
  if (name !== '') {
      // ...
  }

  // bad
  if (collection.length) {
      // ...
  }

  // good
  if (collection.length > 0) {
      // ...
  }
  ```

<a name="comparison--moreinfo"></a><a name="15.4"></a>

- [15.4](#comparison--moreinfo) 更多资料: [Truth Equality and JavaScript](https://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) 作者: Angus Croll.

<a name="comparison--switch-blocks"></a><a name="15.5"></a>

- [15.5](#comparison--switch-blocks) 使用大括号在包含词法声明的 `case` 和 `default` 子句中创建块 (例如. `let`, `const`, `function`, 和 `class`). eslint: [`no-case-declarations`](https://eslint.org/docs/rules/no-case-declarations.html)

  > 词法声明在整个“switch”块中是可见的，但只有在赋值时才会初始化，这只在到达它的“case”时才会发生。 当多个“case”子句试图定义相同的东西时，这会导致问题。

  ```javascript
  // bad
  switch (foo) {
      case 1:
          let x = 1
          break
      case 2:
          const y = 2
          break
      case 3:
          function f() {
            // ...
          }
          break
      default:
          class C {}
  }

  // good
  switch (foo) {
      case 1: {
          let x = 1;
          break;
      }
      case 2: {
          const y = 2;
          break;
      }
      case 3: {
          function f() {
            // ...
          }
          break;
      }
      case 4:
          bar();
          break;
      default: {
          class C {}
      }
  }
  ```

<a name="comparison--nested-ternaries"></a><a name="15.6"></a>

- [15.6](#comparison--nested-ternaries) 三元组不应该嵌套，通常是单行表达式. eslint: [`no-nested-ternary`](https://eslint.org/docs/rules/no-nested-ternary.html)

```javascript
// bad
const foo = maybe1 > maybe2
    ? "bar"
    : value1 > value2 ? "baz" : null;

// split into 2 separated ternary expressions
const maybeNull = value1 > value2 ? 'baz' : null;

// better
const foo = maybe1 > maybe2
    ? 'bar'
    : maybeNull;

// best
const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
```

<a name="comparison--unneeded-ternary"></a><a name="15.7"></a>

- [15.7](#comparison--unneeded-ternary) 避免不必要的三元表达式. eslint: [`no-unneeded-ternary`](https://eslint.org/docs/rules/no-unneeded-ternary.html)

  ```javascript
  // bad
  const foo = a ? a : b;
  const bar = c ? true : false;
  const baz = c ? false : true;

  // good
  const foo = a || b;
  const bar = !!c;
  const baz = !c;
  ```

<a name="comparison--no-mixed-operators"></a>

- [15.8](#comparison--no-mixed-operators) 混合使用操作符的时候, 按照优先级将他们放入括号中.
  eslint: [`no-mixed-operators`](https://eslint.org/docs/rules/no-mixed-operators.html)

  > 这提高了代码可读性，并让开发者的意图更明确

  ```javascript
  // bad
  const foo = a && b < 0 || c > 0 || d + 1 === 0;

  // bad
  const bar = a ** b - 5 % d;

  // bad
  // one may be confused into thinking (a || b) && c
  if (a || b && c) {
      return d
  }

  // bad
  const bar = a + b / c * d

  // good
  const foo = (a && b < 0) || c > 0 || d + 1 === 0

  // good
  const bar = a ** b - (5 % d)

  // good
  if (a || (b && c)) {
      return d
  }

  // good
  const bar = a + (b / c) * d
  ```

**[⬆ back to top](#table-of-contents)**

## Blocks

<a name="blocks--braces"></a><a name="16.1"></a>

- [16.1](#blocks--braces) 对所有多行代码块使用大括号. eslint: [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

  ```javascript
  // bad
  if (test)
      return false;

  // good
  if (test) return false

  // good
  if (test) {
      return false
  }

  // bad
  function foo() { return false; }

  // good
  function bar() {
      return false
  }
  ```

<a name="blocks--cuddled-elses"></a><a name="16.2"></a>

- [16.2](#blocks--cuddled-elses) 如果你使用了多行代码块 `if` 和 `else`, 则把 `else` 放到和 `if` 块结束同一行的位置上. eslint: [`brace-style`](https://eslint.org/docs/rules/brace-style.html)

  ```javascript
  // bad
  if (test) {
    thing1();
    thing2();
  }
  else {
    thing3();
  }

  // good
  if (test) {
    thing1();
    thing2();
  } else {
    thing3();
  }
  ```

<a name="blocks--no-else-return"></a><a name="16.3"></a>

- [16.3](#blocks--no-else-return) 如果 `if` 块包含 `return` 语句, 后边紧跟它的 `else` 块就是非必须的. `else if` 块中的 `return` 语句 如果在一个包含 `return` 语句的块之后, 则可以被分割成多个 `if` 块. eslint: [`no-else-return`](https://eslint.org/docs/rules/no-else-return)

  ```javascript
  // bad
  function foo() {
      if (x) {
          return x;
      } else {
          return y;
      }
  }

  // bad
  function cats() {
      if (x) {
          return x;
      } else if (y) {
          return y;
      }
  }

  // bad
  function dogs() {
    if (x) {
        return x;
    } else {
        if (y) {
            return y;
        }
    }
  }

  // good
  function foo() {
    if (x) {
        return x;
    }

    return y;
  }

  // good
  function cats() {
    if (x) {
        return x;
    }

    if (y) {
        return y;
    }
  }

  // good
  function dogs(x) {
    if (x) {
        if (z) {
            return y;
        }
    } else {
        return z;
    }
  }
  ```

**[⬆ back to top](#table-of-contents)**

## Control Statements

<a name="control-statements"></a>

- [17.1](#control-statements) 如果控制语句(`if`， `while`等)太长或超过最大行长度的限制，则每个(分组)条件可以被放入一个新行。 逻辑运算符应该从该行开始

  > 要求在行的开头使用操作符可以使操作符对齐，并遵循类似于方法链接的模式。 这也提高了可读性，使它更容易直观地遵循复杂的逻辑。

    ```javascript
    // bad
    if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
        thing1();
    }

    // bad
    if (foo === 123 &&
      bar === 'abc') {
        thing1();
    }

    // bad
    if (foo === 123
      && bar === 'abc') {
        thing1();
    }

    // bad
    if (
      foo === 123 &&
      bar === 'abc'
    ) {
        thing1();
    }

    // good
    if (
      foo === 123
      && bar === 'abc'
    ) {
        thing1();
    }

    // good
    if (
      (foo === 123 || bar === 'abc')
      && doesItLookGoodWhenItBecomesThatLong()
      && isThisReallyHappening()
    ) {
        thing1();
    }

    // good
    if (foo === 123 && bar === 'abc') {
        thing1();
    }
    ```

<a name="control-statement--value-selection"></a><a name="control-statements--value-selection"></a>

- [17.2](#control-statements--value-selection) 不要在控制语句中使用选择操作符

  ```javascript
  // bad
  !isRunning && startRunning()

  // good
  if (!isRunning) {
      startRunning()
  }
  ```

**[⬆ back to top](#table-of-contents)**

## Comments

<a name="comments--multiline"></a><a name="17.1"></a>

- [18.1](#comments--multiline) 使用`/** ... */` 作为多行注释.

  ```javascript
  // bad
  // make() returns a new element
  // based on the passed in tag name
  //
  // @param {String} tag
  // @return {Element} element
  function make(tag) {
      // ...

      return element
  }

  // good
  /**
   * make() returns a new element
   * based on the passed-in tag name
   */
  function make(tag) {
      // ...

      return element
  }
  ```

<a name="comments--singleline"></a><a name="17.2"></a>

- [18.2](#comments--singleline) 使用 `//` 作为单行注释. 将单行注释放在需要注释的内容上方, 在注释上方放一个空行，除非它在块的第一行.

 ```javascript
  // bad
  const active = true;  // is current tab

  // good
  // is current tab
  const active = true;

  // bad
  function getType() {
    console.log('fetching type...');
    // set the default type to 'no type'
    const type = this.type || 'no type';

    return type;
  }

  // good
  function getType() {
    console.log('fetching type...');

    // set the default type to 'no type'
    const type = this.type || 'no type';

    return type;
  }

  // also good
  function getType() {
    // set the default type to 'no type'
    const type = this.type || 'no type';

    return type;
  }
  ```

<a name="comments--spaces"></a>

- [18.3](#comments--spaces) 为了更容易阅读, 所有的评论以空格开始。 . eslint: [`spaced-comment`](https://eslint.org/docs/rules/spaced-comment)

  ```javascript
  // bad
  //is current tab
  const active = true

  // good
  // is current tab
  const active = true

  // bad
  /**
   *make() returns a new element
   *based on the passed-in tag name
   */
  function make(tag) {
      // ...

      return element
  }

  // good
  /**
   * make() returns a new element
   * based on the passed-in tag name
   */
  function make(tag) {
      // ...

      return element
  }
  ```

<a name="comments--actionitems"></a><a name="17.3"></a>

- [18.4](#comments--actionitems) 注释内容以 `FIXME` 或者 `TODO` 开头, 可以帮助其他开发人员快速理解, 开发者在此标记了一个准备去解决的问题或者准备去做的优化. 和其他普通的注释不一样, 这种标记的注释是可操作的.动作是  `FIXME: -- 想要解决某些问题` or `TODO: -- 需要去实现某些功能`.

<a name="comments--fixme"></a><a name="17.4"></a>

- [18.5](#comments--fixme) 使用 `// FIXME:` 来标记要去解决的问题.

  ```javascript
  class Calculator extends Abacus {
      constructor() {
          super()

          // FIXME: shouldn’t use a global here
          total = 0
      }
  }
  ```

<a name="comments--todo"></a><a name="17.5"></a>

- [18.6](#comments--todo) 使用 `// TODO:` 来标识要去做的事情.

  ```javascript
  class Calculator extends Abacus {
      constructor() {
          super()

          // TODO: total should be configurable by an options param
          this.total = 0
      }
  }
  ```

**[⬆ back to top](#table-of-contents)**

## Whitespace

<a name="whitespace--spaces"></a><a name="18.1"></a>

- [19.1](#whitespace--spaces) 使用设置为`4`个空格的软制表符(空格字符)(这点是否有歧义?). eslint: [`indent`](https://eslint.org/docs/rules/indent.html)

  ```javascript
  // bad
  function foo() {
  ∙∙let name;
  }

  // bad
  function bar() {
  ∙let name;
  }

  // good
  function baz() {
  ∙∙∙∙let name;
  }
  ```

<a name="whitespace--before-blocks"></a><a name="18.2"></a>

- [19.2](#whitespace--before-blocks) 在大括号 `{` 之前留一个空格. eslint: [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks.html)

  ```javascript
  // bad
  function test(){
      console.log('test');
  }

  // good
  function test() {
      console.log('test');
  }

  // bad
  dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
  });

  // good
  dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
  });
  ```

<a name="whitespace--around-keywords"></a><a name="18.3"></a>

- [19.3](#whitespace--around-keywords) 在控制语句 (`if`, `while` 等.)中. 左括号前放一个空格.在函数调用和声明中，参数列表和函数名之间不要有空格. eslint: [`keyword-spacing`](https://eslint.org/docs/rules/keyword-spacing.html)

  ```javascript
  // bad
  if(isJedi) {
      fight ();
  }

  // good
  if (isJedi) {
      fight();
  }

  // bad
  function fight () {
      console.log ('Swooosh!');
  }

  // good
  function fight() {
      console.log('Swooosh!');
  }
  ```

<a name="whitespace--infix-ops"></a><a name="18.4"></a>

- [19.4](#whitespace--infix-ops) 使用空格来间隔操作符计算. eslint: [`space-infix-ops`](https://eslint.org/docs/rules/space-infix-ops.html)

  ```javascript
  // bad
  const x=y+5;

  // good
  const x = y + 5;
  ```

<a name="whitespace--newline-at-end"></a><a name="18.5"></a>

- [19.5](#whitespace--newline-at-end) 用一个换行符作为文件结尾. eslint: [`eol-last`](https://github.com/eslint/eslint/blob/master/docs/rules/eol-last.md)

  ```javascript
  // bad
  import { es6 } from './AirbnbStyleGuide'
  // ...
  export default es6
  ```

  ```javascript
  // bad
  import { es6 } from './AirbnbStyleGuide';
    // ...
  export default es6;↵
  ↵
  ```

  ```javascript
  // good
  import { es6 } from './AirbnbStyleGuide';
    // ...
  export default es6;↵
  ```

<a name="whitespace--chains"></a><a name="18.6"></a>

- [19.6](#whitespace--chains) 在写一个较长(超过2个方法)的方法链时, 尽量分割成多行语句, 并且使用前导点，强调这是一个方法调用，而不是一个新语句 . eslint: [`newline-per-chained-call`](https://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](https://eslint.org/docs/rules/no-whitespace-before-property)

  ```javascript
  // bad
  $('#items').find('.selected').highlight().end().find('.open').updateCount();

  // bad
  $('#items').
    find('.selected').
      highlight().
      end().
    find('.open').
      updateCount();

  // good
  $('#items')
    .find('.selected')
      .highlight()
      .end()
    .find('.open')
      .updateCount();

  // bad
  const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
      .attr('width', (radius + margin) * 2).append('svg:g')
      .attr('transform', `translate(${radius + margin},${radius + margin})`)
      .call(tron.led);

  // good
  const leds = stage.selectAll('.led')
      .data(data)
    .enter().append('svg:svg')
      .classed('led', true)
      .attr('width', (radius + margin) * 2)
    .append('svg:g')
      .attr('transform', `translate(${radius + margin},${radius + margin})`)
      .call(tron.led);

  // good
  const leds = stage.selectAll('.led').data(data);
  const svg = leds.enter().append('svg:svg');
  svg.classed('led', true).attr('width', (radius + margin) * 2);
  const g = svg.append('svg:g');
  g.attr('transform', `translate(${radius + margin},${radius + margin})`).call(tron.led);
  ```

<a name="whitespace--after-blocks"></a><a name="18.7"></a>

- [19.7](#whitespace--after-blocks) 在块之后和下一个语句之前留一个空行.

  ```javascript
  // bad
  if (foo) {
      return bar
  }
  return baz

  // good
  if (foo) {
      return bar
  }

  return baz

  // bad
  const obj = {
      foo() {},
      bar() {},
  }
  return obj

  // good
  const obj = {
    foo() {},

    bar() {},
  }

  return obj

  // bad
  const arr = [function foo() {}, function bar() {}]
  return arr

  // good
  const arr = [function foo() {}, function bar() {}]

  return arr
  ```

<a name="whitespace--padded-blocks"></a><a name="18.8"></a>

- [19.8](#whitespace--padded-blocks) Do not pad your blocks with blank lines. eslint: [`padded-blocks`](https://eslint.org/docs/rules/padded-blocks.html)

   ```javascript
  // bad
  if (foo) {
      return bar;
  }
  return baz;

  // good
  if (foo) {
      return bar;
  }

  return baz;

  // bad
  const obj = {
      foo() {
      },
      bar() {
      },
  };
  return obj;

  // good
  const obj = {
      foo() {
      },

      bar() {
      },
  };

  return obj;

  // bad
  const arr = [
      function foo() {
      },
      function bar() {
      },
  ];
  return arr;

  // good
  const arr = [
      function foo() {
      },

      function bar() {
      },
  ];

  return arr;
  ```

<a name="whitespace--no-multiple-blanks"></a>

- [19.9](#whitespace--no-multiple-blanks) 不要使用多个空白行来填充代码. eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

  <!-- markdownlint-disable MD012 -->

  ```javascript
  // bad
  class Person {
      constructor(fullName, email, birthday) {
          this.fullName = fullName;


          this.email = email;


          this.setAge(birthday);
      }


      setAge(birthday) {
          const today = new Date();


          const age = this.getAge(today, birthday);


          this.age = age;
      }


        getAge(today, birthday) {
          // ..
        }
    }

    // good
    class Person {
        constructor(fullName, email, birthday) {
            this.fullName = fullName;
            this.email = email;
            this.setAge(birthday);
        }

        setAge(birthday) {
            const today = new Date();
            const age = getAge(today, birthday);
            this.age = age;
        }

        getAge(today, birthday) {
            // ..
        }
    }
    ```

<a name="whitespace--in-parens"></a><a name="18.9"></a>

- [19.10](#whitespace--in-parens) 小括号内容两边不要加空格. eslint: [`space-in-parens`](https://eslint.org/docs/rules/space-in-parens.html)

```javascript
  // bad
  function bar( foo ) {
    return foo;
  }

  // good
  function bar(foo) {
    return foo;
  }

  // bad
  if ( foo ) {
    console.log(foo);
  }

  // good
  if (foo) {
    console.log(foo);
  }
  ```

<a name="whitespace--in-brackets"></a><a name="18.10"></a>

- [19.11](#whitespace--in-brackets) 中括号内容两边不要加空格. eslint: [`array-bracket-spacing`](https://eslint.org/docs/rules/array-bracket-spacing.html)

  ```javascript
  // bad
  const foo = [ 1, 2, 3 ];
  console.log(foo[ 0 ]);

  // good
  const foo = [1, 2, 3];
  console.log(foo[0]);
  ```

<a name="whitespace--in-braces"></a><a name="18.11"></a>

- [19.12](#whitespace--in-braces) 大括号内容两边要加空格. eslint: [`object-curly-spacing`](https://eslint.org/docs/rules/object-curly-spacing.html)

  ```javascript
  // bad
  const foo = {clark: 'kent'}

  // good
  const foo = { clark: 'kent' }
  ```

<a name="whitespace--max-len"></a><a name="18.12"></a>

- [19.13](#whitespace--max-len) 避免代码行长度超过120个字符(包括空格) . 注意:  [above](#strings--line-length), 长字符串不包含在该规则内, 并且尽量不要换行断开. eslint: [`max-len`](https://eslint.org/docs/rules/max-len.html)

  > 可读性/可维护性

  ```javascript
  // bad
    const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

    // bad
    $.ajax({ method: 'POST', url: 'https://airbnb.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

    // good
    const foo = jsonData
      && jsonData.foo
      && jsonData.foo.bar
      && jsonData.foo.bar.baz
      && jsonData.foo.bar.baz.quux
      && jsonData.foo.bar.baz.quux.xyzzy;

    // good
    $.ajax({
      method: 'POST',
      url: 'https://airbnb.com/',
      data: { name: 'John' },
    })
      .done(() => console.log('Congratulations!'))
      .fail(() => console.log('You have failed this city.'));
  ```

<a name="whitespace--block-spacing"></a>

- [19.14](#whitespace--block-spacing) 要求在同一行上的一个开放块标记和下一个标记之间有一个的空格. 该规则还强制close块标记与同一行的前一个标记之间保持一致的空格。 eslint: [`block-spacing`](https://eslint.org/docs/rules/block-spacing)

  ```javascript
  // bad
  function foo() {return true;}
  if (foo) { bar = 0;}

  // good
  function foo() { return true; }
  if (foo) { bar = 0; }
  ```

<a name="whitespace--comma-spacing"></a>

- [19.15](#whitespace--comma-spacing) 在逗号之前不要有空格, 在逗号之后要有空格. eslint: [`comma-spacing`](https://eslint.org/docs/rules/comma-spacing)

  ```javascript
  // bad
  var foo = 1,bar = 2;
  var arr = [1 , 2];

  // good
  var foo = 1, bar = 2;
  var arr = [1, 2];
  ```

<a name="whitespace--computed-property-spacing"></a>

- [19.16](#whitespace--computed-property-spacing) 注意指定属性内的空格. eslint: [`computed-property-spacing`](https://eslint.org/docs/rules/computed-property-spacing)

  ```javascript
  // bad
  obj[foo ]
  obj[ 'foo']
  var x = {[ b ]: a}
  obj[foo[ bar ]]

  // good
  obj[foo]
  obj['foo']
  var x = { [b]: a }
  obj[foo[bar]]
  ```

<a name="whitespace--func-call-spacing"></a>

- [19.17](#whitespace--func-call-spacing) 不要再函数调用名称和小括号之间使用空格. eslint: [`func-call-spacing`](https://eslint.org/docs/rules/func-call-spacing)

  ```javascript
  // bad
  func ();

  func
  ();

  // good
  func()
  ```

<a name="whitespace--key-spacing"></a>

- [19.18](#whitespace--key-spacing) 在字面量的`key:` 和 `value` 之间使用 空格. eslint: [`key-spacing`](https://eslint.org/docs/rules/key-spacing)

  ```javascript
   // bad
  var obj = { foo : 42 };
  var obj2 = { foo:42 };

  // good
  var obj = { foo: 42 };
  ```

<a name="whitespace--no-trailing-spaces"></a>

- [19.19](#whitespace--no-trailing-spaces) 避免行位有空格. eslint: [`no-trailing-spaces`](https://eslint.org/docs/rules/no-trailing-spaces)

<a name="whitespace--no-multiple-empty-lines"></a>

- [19.20](#whitespace--no-multiple-empty-lines) 避免出现多个空行，只允许在文件末尾出现一个换行符，并避免在文件开头出现换行符. eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

   ```javascript
    // bad - multiple empty lines
    var x = 1;


    var y = 2;

    // bad - 2+ newlines at end of file
    var x = 1;
    var y = 2;


    // bad - 1+ newline(s) at beginning of file

    var x = 1;
    var y = 2;

    // good
    var x = 1;
    var y = 2;

    ```

**[⬆ back to top](#table-of-contents)**

## Commas

<a name="commas--leading-trailing"></a><a name="19.1"></a>

- [20.1](#commas--leading-trailing) 逗号永远不要放到最前边. eslint: [`comma-style`](https://eslint.org/docs/rules/comma-style.html)

  ```javascript
  // bad
  const story = [
      once
    , upon
    , aTime
  ];

  // good
  const story = [
    once,
    upon,
    aTime,
  ];

  // bad
  const hero = {
      firstName: 'Ada'
    , lastName: 'Lovelace'
    , birthYear: 1815
    , superPower: 'computers'
  };

  // good
  const hero = {
    firstName: 'Ada',
    lastName: 'Lovelace',
    birthYear: 1815,
    superPower: 'computers',
  };
  ```

<a name="commas--dangling"></a><a name="19.2"></a>

- [20.2](#commas--dangling) 行尾要有空格. eslint: [`comma-dangle`](https://eslint.org/docs/rules/comma-dangle.html)

  > 使git diffs更加干净. 另外，像Babel这样的转译器会在转译代码中删除附加的尾随逗号，这意味着你不必担心 [trailing comma problem](https://github.com/airbnb/javascript/blob/es5-deprecated/es5/README.md#commas) 问题.

  ```diff
  // bad - git diff without trailing comma
  const hero = {
       firstName: 'Florence',
  -    lastName: 'Nightingale'
  +    lastName: 'Nightingale',
  +    inventorOf: ['coxcomb chart', 'modern nursing']
  };

  // good - git diff with trailing comma
  const hero = {
       firstName: 'Florence',
       lastName: 'Nightingale',
  +    inventorOf: ['coxcomb chart', 'modern nursing'],
  };
  ```

  ```javascript
    // bad
    const hero = {
        firstName: 'Dana',
        lastName: 'Scully'
    };

    const heroes = [
        'Batman',
        'Superman'
    ];

    // good
    const hero = {
        firstName: 'Dana',
        lastName: 'Scully',
    };

    const heroes = [
        'Batman',
        'Superman',
    ];

    // bad
    function createHero(
        firstName,
        lastName,
        inventorOf
    ) {
      // does nothing
    }

    // good
    function createHero(
        firstName,
        lastName,
        inventorOf,
    ) {
      // does nothing
    }

    // good (note that a comma must not appear after a "rest" element)
    function createHero(
        firstName,
        lastName,
        inventorOf,
        ...heroArgs
    ) {
      // does nothing
    }

    // bad
    createHero(
        firstName,
        lastName,
        inventorOf
    );

    // good
    createHero(
        firstName,
        lastName,
        inventorOf,
    );

    // good (note that a comma must not appear after a "rest" element)
    createHero(
        firstName,
        lastName,
        inventorOf,
        ...heroArgs
    );
    ```

**[⬆ back to top](#table-of-contents)**

## Semicolons

<a name="semicolons--required"></a><a name="20.1"></a>

- [21.1](#semicolons--required) 行结尾需要有分号. eslint: [`semi`](https://eslint.org/docs/rules/semi.html)

  > 当JavaScript遇到没有分号的换行符时，它会使用一组名为 [Automatic Semicolon Insertion](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion) 的规则来决定是否应该将换行符视为语句的结束. 但是，如果JavaScript错误地解释了你的换行符，你的代码就会中断。 随着新特性成为JavaScript的一部分，这些规则将变得更加复杂。 显式地终止语句并配置检查程序以捕获丢失的分号将有助于防止遇到问题

  ```javascript
  // bad - raises exception
  const luke = {}
  const leia = {}
  [luke, leia].forEach((jedi) => jedi.father = 'vader')

  // bad - raises exception
  const reaction = "No! That’s impossible!"
  (async function meanwhileOnTheFalcon() {
    // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
    // ...
  }())

  // bad - returns `undefined` instead of the value on the next line - always happens when `return` is on a line by itself because of ASI!
  function foo() {
    return
      'search your feelings, you know it to be foo'
  }

  // good
  const luke = {};
  const leia = {};
  [luke, leia].forEach((jedi) => {
    jedi.father = 'vader';
  });

  // good
  const reaction = "No! That’s impossible!";
  (async function meanwhileOnTheFalcon() {
    // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
    // ...
  }());

  // good
  function foo() {
    return 'search your feelings, you know it to be foo';
  }
  ```

  [Read more](https://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214#7365214).

**[⬆ back to top](#table-of-contents)**

## Type Casting & Coercion

<a name="coercion--explicit"></a><a name="21.1"></a>

- [22.1](#coercion--explicit) 在语句的开头执行类型强制转换.

<a name="coercion--strings"></a><a name="21.2"></a>

- [22.2](#coercion--strings) `String` `Number` `Boolean` 不允许使用`new`: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

  ```javascript
  // => this.reviewScore = 9;

  // bad
  const totalScore = new String(this.reviewScore) // typeof totalScore is "object" not "string"

  // bad
  const totalScore = this.reviewScore + '' // invokes this.reviewScore.valueOf()

  // bad
  const totalScore = this.reviewScore.toString() // isn’t guaranteed to return a string

  // good
  const totalScore = String(this.reviewScore)
  ```

<a name="coercion--numbers"></a><a name="21.3"></a>

- [22.3](#coercion--numbers) 使用 `Number` 作为类型的映射, 而`parseInt` 用来指定数字的进制. eslint: [`radix`](https://eslint.org/docs/rules/radix) [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

  ```javascript
  const inputValue = '4'

  // bad
  const val = new Number(inputValue)

  // bad
  const val = +inputValue

  // bad
  const val = inputValue >> 0

  // bad
  const val = parseInt(inputValue)

  // good
  const val = Number(inputValue)

  // good
  const val = parseInt(inputValue, 10)
  ```

<a name="coercion--comment-deviations"></a><a name="21.4"></a>

- [22.4](#coercion--comment-deviations) 如果使用位运算符代替 `parseInt`,可能由于性能原因([performance reasons](https://jsperf.com/coercion-vs-casting/3)), 留一下注释说明一下原因.

  ```javascript
  // good
  /**
   * parseInt was the reason my code was slow.
   * Bitshifting the String to coerce it to a
   * Number made it a lot faster.
   */
  const val = inputValue >> 0
  ```

<a name="coercion--bitwise"></a><a name="21.5"></a>

- [22.5](#coercion--bitwise) 使用位运算要小心. Numbers 是64位 [64-bit values](https://es5.github.io/#x4.3.19), 而位运算只返回32位 ([source](https://es5.github.io/#x11.7)). 当数字大于32位的时候, 位运算会出错. [Discussion](https://github.com/airbnb/javascript/issues/109). 最大的32位数字是 2,147,483,647:

  ```javascript
  2147483647 >> 0 // => 2147483647
  2147483648 >> 0 // => -2147483648
  2147483649 >> 0 // => -2147483647
  ```

<a name="coercion--booleans"></a><a name="21.6"></a>

- [22.6](#coercion--booleans) Booleans使用请参考 eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

  ```javascript
  const age = 0

  // bad
  const hasAge = new Boolean(age)

  // good
  const hasAge = Boolean(age)

  // best
  const hasAge = !!age
  ```

**[⬆ back to top](#table-of-contents)**

## Naming Conventions

<a name="naming--descriptive"></a><a name="22.1"></a>

- [23.1](#naming--descriptive) 避免使用单个字符的名称. 命名要有描述意义. eslint: [`id-length`](https://eslint.org/docs/rules/id-length)

  ```javascript
  // bad
  function q() {
    // ...
  }

  // good
  function query() {
    // ...
  }
  ```

<a name="naming--camelCase"></a><a name="22.2"></a>

- [23.2](#naming--camelCase) 在命名对象、函数和实例时使用camelCase(小驼峰). eslint: [`camelcase`](https://eslint.org/docs/rules/camelcase.html)

  ```javascript
  // bad
  const OBJEcttsssss = {}
  const this_is_my_object = {}
  function c() {}

  // good
  const thisIsMyObject = {}
  function thisIsMyFunction() {}
  ```

<a name="naming--PascalCase"></a><a name="22.3"></a>

- [23.3](#naming--PascalCase) 只有在命名构造函数或类时才使用PascalCase(大驼峰). eslint: [`new-cap`](https://eslint.org/docs/rules/new-cap.html)

  ```javascript
  // bad
  function user(options) {
      this.name = options.name
  }

  const bad = new user({
      name: 'nope',
  })

  // good
  class User {
      constructor(options) {
          this.name = options.name
      }
  }

  const good = new User({
      name: 'yup',
  })
  ```

<a name="naming--leading-underscore"></a><a name="22.4"></a>

- [23.4](#naming--leading-underscore) 命名中不要使用尾下划线或前导下划线. eslint: [`no-underscore-dangle`](https://eslint.org/docs/rules/no-underscore-dangle.html)

  > JavaScript在属性或方法方面没有隐私的概念。 虽然前面的下划线是表示“私有”的常见约定，但实际上，这些属性是完全公共的，因此，是公共API契约的一部分。 这种惯例可能会导致开发人员错误地认为更改不算作破坏，或者不需要测试。 如果想要一件东西是“私人的”，它必须是不明显的存在。

  ```javascript
  // bad
  this.__firstName__ = 'Panda'
  this.firstName_ = 'Panda'
  this._firstName = 'Panda'

  // good
  this.firstName = 'Panda'

  // good, in environments where WeakMaps are available
  // see https://kangax.github.io/compat-table/es6/#test-WeakMap
  const firstNames = new WeakMap()
  firstNames.set(this, 'Panda')
  ```

<a name="naming--self-this"></a><a name="22.5"></a>

- [23.5](#naming--self-this) 不需要保留 `this` 的引用. 使用箭头函数 或者 [函数绑定(Function#bind)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind).

  ```javascript
  // bad
  function foo() {
      const self = this
      return function () {
          console.log(self)
      }
  }

  // bad
  function foo() {
      const that = this
      return function () {
          console.log(that)
      }
  }

  // good
  function foo() {
      return () => {
          console.log(this)
      }
  }
  ```

<a name="naming--filename-matches-export"></a><a name="22.6"></a>

- [23.6](#naming--filename-matches-export) 文件名应该完全匹配其默认导出的名称.

  ```javascript
  // file 1 contents
  class CheckBox {
      // ...
  }
  export default CheckBox

  // file 2 contents
  export default function fortyTwo() {
      return 42
  }

  // file 3 contents
  export default function insideDirectory() {}

  // in some other file
  // bad
  import CheckBox from './checkBox' // PascalCase import/export, camelCase filename
  import FortyTwo from './FortyTwo' // PascalCase import/filename, camelCase export
  import InsideDirectory from './InsideDirectory' // PascalCase import/filename, camelCase export

  // bad
  import CheckBox from './check_box' // PascalCase import/export, snake_case filename
  import forty_two from './forty_two' // snake_case import/filename, camelCase export
  import inside_directory from './inside_directory' // snake_case import, camelCase export
  import index from './inside_directory/index' // requiring the index file explicitly
  import insideDirectory from './insideDirectory/index' // requiring the index file explicitly

  // good
  import CheckBox from './CheckBox' // PascalCase export/import/filename
  import fortyTwo from './fortyTwo' // camelCase export/import/filename
  import insideDirectory from './insideDirectory' // camelCase export/import/directory name/implicit "index"
  // ^ supports both insideDirectory.js and insideDirectory/index.js
  ```

<a name="naming--camelCase-default-export"></a><a name="22.7"></a>

- [23.7](#naming--camelCase-default-export) 在导出默认函数时使用camelCase,并且文件名应该与函数名相同。

  ```javascript
  function makeStyleGuide() {
      // ...
  }

  export default makeStyleGuide
  ```

<a name="naming--PascalCase-singleton"></a><a name="22.8"></a>

- [23.8](#naming--PascalCase-singleton) 在导出构造函数/类/单例/函数库/对象时使用PascalCase。

  ```javascript
  const AirbnbStyleGuide = {
    es6: {},
  }

  export default AirbnbStyleGuide
  ```

<a name="naming--Acronyms-and-Initialisms"></a>

- [23.9](#naming--Acronyms-and-Initialisms) 首字母缩略词应该全部大写，或者全部小写

  > 名称是为了可读性，而不是为了满足计算机算法

  ```javascript
  // bad
  import SmsContainer from './containers/SmsContainer'

  // bad
  const HttpRequests = [
    // ...
  ]

  // good
  import SMSContainer from './containers/SMSContainer'

  // good
  const HTTPRequests = [
    // ...
  ]

  // also good
  const httpRequests = [
    // ...
  ]

  // best
  import TextMessageContainer from './containers/TextMessageContainer'

  // best
  const requests = [
    // ...
  ]
  ```

<a name="naming--uppercase"></a>

- [23.10](#naming--uppercase) 使用纯大写命名的原因: (1) 被导出, (2) 是一个 `常量` (不能被赋值),  (3) 其他研发人员可以相信它(及其嵌套属性)永远不会改变

  > 让程序员知道他们可以信任变量(及其属性)不会被更改

  - 变量全部都是 `const`, 怎么样? - 这是不必要的，所以大写不应该用于文件中的常量。 但是，它应该用于导出的常量.
  - 导出对象(object), 如果是纯大写, 是什么含义? - 标识它的属性都无法被更改.

  ```javascript
  // bad
  const PRIVATE_VARIABLE =
    'should not be unnecessarily uppercased within a file'

  // bad
  export const THING_TO_BE_CHANGED = 'should obviously not be uppercased'

  // bad
  export let REASSIGNABLE_VARIABLE = 'do not use let with uppercase variables'

  // ---

  // allowed but does not supply semantic value
  export const apiKey = 'SOMEKEY'

  // better in most cases
  export const API_KEY = 'SOMEKEY'

  // ---

  // bad - unnecessarily uppercases key while adding no semantic value
  export const MAPPING = {
    KEY: 'value',
  }

  // good
  export const MAPPING = {
    key: 'value',
  }
  ```

**[⬆ back to top](#table-of-contents)**

## Accessors

<a name="accessors--not-required"></a><a name="23.1"></a>

- [24.1](#accessors--not-required) 属性的访问器函数不是必需的

<a name="accessors--no-getters-setters"></a><a name="23.2"></a>

- [24.2](#accessors--no-getters-setters) 不要使用JavaScript的getter /setter，因为它们会导致意想不到的副作用，并且更难测试、维护和解释. 可以使用访问器函数,如 `getVal()` 和 `setVal('hello')`.

  ```javascript
  // bad
  class Dragon {
    get age() {
        // ...
    }

    set age(value) {
        // ...
    }
  }

  // good
  class Dragon {
      getAge() {
          // ...
      }

      setAge(value) {
          // ...
      }
  }
  ```

<a name="accessors--boolean-prefix"></a><a name="23.3"></a>

- [24.3](#accessors--boolean-prefix) 如果一个属性或者方法返回值是 `boolean`, 请使用类似 `isVal()` 或者 `hasVal()`去命名.

  ```javascript
  // bad
  if (!dragon.age()) {
      return false
  }

  // good
  if (!dragon.hasAge()) {
      return false
  }
  ```

<a name="accessors--consistent"></a><a name="23.4"></a>

- [24.4](#accessors--consistent) class 可以使用通用的 `get()` 和 `set()` 函数.

  ```javascript
  class Jedi {
    constructor(options = {}) {
        const lightsaber = options.lightsaber || 'blue'
        this.set('lightsaber', lightsaber)
    }

    set(key, val) {
        this[key] = val
    }

    get(key) {
        return this[key]
    }
  }
  ```

**[⬆ back to top](#table-of-contents)**

## ECMAScript 5 Compatibility

<a name="es5-compat--kangax"></a><a name="24.1"></a>

- [25.1](#es5-compat--kangax) 参考 [Kangax](https://twitter.com/kangax/)  ES5 [compatibility table](https://kangax.github.io/es5-compat-table/).

**[⬆ back to top](#table-of-contents)**

<a name="ecmascript-6-styles"></a>

## ECMAScript 6+ (ES 2015+) Styles

<a name="es6-styles"></a><a name="25.1"></a>

- [26.1](#es6-styles) 下边是ES6的新特性

1. [Arrow Functions](#arrow-functions)
1. [Classes](#classes--constructors)
1. [Object Shorthand](#es6-object-shorthand)
1. [Object Concise](#es6-object-concise)
1. [Object Computed Properties](#es6-computed-properties)
1. [Template Strings](#es6-template-literals)
1. [Destructuring](#destructuring)
1. [Default Parameters](#es6-default-parameters)
1. [Rest](#es6-rest)
1. [Array Spreads](#es6-array-spreads)
1. [Let and Const](#references)
1. [Exponentiation Operator](#es2016-properties--exponentiation-operator)
1. [Iterators and Generators](#iterators-and-generators)
1. [Modules](#modules)

<a name="tc39-proposals"></a>

- [26.2](#tc39-proposals) 不要使用 [TC39 proposals](https://github.com/tc39/proposals).

  > [They are not finalized](https://tc39.github.io/process-document/)

**[⬆ back to top](#table-of-contents)**

## Standard Library

[Standard Library](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects)

<a name="standard-library--isnan"></a><a name="26.1"></a>

- [27.1](#standard-library--isnan) 使用 `Number.isNaN` 代替全局 的 `isNaN`.
  eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

  > 全局的 `isNaN` 将非数字强制转换为数字，对能够强制转换为NaN的任何内容返回true

  ```javascript
  // bad
  isNaN('1.2') // false
  isNaN('1.2.3') // true

  // good
  Number.isNaN('1.2.3') // false
  Number.isNaN(Number('1.2.3')) // true
  ```

<a name="standard-library--isfinite"></a>

- [27.2](#standard-library--isfinite) 使用 `Number.isFinite` 代替全局 的 `isFinite`.
  eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

  > 全局的 `isFinite` 将非数字强制转换为数字，将任何强制转换为有限数字的对象返回true

  ```javascript
  // bad
  isFinite('2e3') // true

  // good
  Number.isFinite('2e3') // false
  Number.isFinite(parseInt('2e3', 10)) // true
  ```

**[⬆ back to top](#table-of-contents)**

## Testing

<a name="testing--for-real"></a><a name="27.1"></a>

- [28.2](#testing--for-real) **遥不可及的测试规划**:
  - 无论使用什么框架, 都应该进行单元测试!
  - 努力编写许多小的纯函数，尽量减少发生突变的地方 .
  - 注意 stubs 和 mocks - 会让你的单元测试变得无比脆弱.
  - Airbnb推荐使用 [`mocha`](https://www.npmjs.com/package/mocha) 和 [`jest`](https://www.npmjs.com/package/jest). [`tape`](https://www.npmjs.com/package/tape) 可以应用于独立的小模块.
  - 100%的测试覆盖率是一个值得努力的目标，即使达到它并不总是实际的
  - 每当你修复一个错误, _编写一个回归测试_. 在没有回归测试的情况下修复的bug几乎肯定会在将来再次出现.

**[⬆ back to top](#table-of-contents)**

## Performance

- [On Layout & Web Performance](https://www.kellegous.com/j/2013/01/26/layout-performance/)
- [String vs Array Concat](https://jsperf.com/string-vs-array-concat/2)
- [Try/Catch Cost In a Loop](https://jsperf.com/try-catch-in-loop-cost/12)
- [Bang Function](https://jsperf.com/bang-function)
- [jQuery Find vs Context, Selector](https://jsperf.com/jquery-find-vs-context-sel/164)
- [innerHTML vs textContent for script text](https://jsperf.com/innerhtml-vs-textcontent-for-script-text)
- [Long String Concatenation](https://jsperf.com/ya-string-concat/38)
- [Are JavaScript functions like `map()`, `reduce()`, and `filter()` optimized for traversing arrays?](https://www.quora.com/JavaScript-programming-language-Are-Javascript-functions-like-map-reduce-and-filter-already-optimized-for-traversing-array/answer/Quildreen-Motta)
- Loading...

**[⬆ back to top](#table-of-contents)**

## Resources

**Learning ES6+**

- [Latest ECMA spec](https://tc39.github.io/ecma262/)
- [ExploringJS](http://exploringjs.com/)
- [ES6 Compatibility Table](https://kangax.github.io/compat-table/es6/)
- [Comprehensive Overview of ES6 Features](http://es6-features.org/)

**Read This**

- [Standard ECMA-262](http://www.ecma-international.org/ecma-262/6.0/index.html)

**Tools**

- Code Style Linters
  - [ESlint](https://eslint.org/) - [Airbnb Style .eslintrc](https://github.com/airbnb/javascript/blob/master/linters/.eslintrc)
  - [JSHint](http://jshint.com/) - [Airbnb Style .jshintrc](https://github.com/airbnb/javascript/blob/master/linters/.jshintrc)
- Neutrino Preset - [@neutrinojs/airbnb](https://neutrinojs.org/packages/airbnb/)

**Other Style Guides**

- [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html)
- [Google JavaScript Style Guide (Old)](https://google.github.io/styleguide/javascriptguide.xml)
- [jQuery Core Style Guidelines](https://contribute.jquery.org/style-guide/js/)
- [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwaldron/idiomatic.js)
- [StandardJS](https://standardjs.com)

**Other Styles**

- [Naming this in nested functions](https://gist.github.com/cjohansen/4135065) - Christian Johansen
- [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52) - Ross Allen
- [Popular JavaScript Coding Conventions on GitHub](http://sideeffect.kr/popularconvention/#javascript) - JeongHoon Byun
- [Multiple var statements in JavaScript, not superfluous](http://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman

**Further Reading**

- [Understanding JavaScript Closures](https://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
- [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer
- [You Might Not Need jQuery](http://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz
- [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban
- [Frontend Guidelines](https://github.com/bendc/frontend-guidelines) - Benjamin De Cock

**Books**

- [JavaScript: The Good Parts](https://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
- [JavaScript Patterns](https://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
- [Pro JavaScript Design Patterns](https://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X) - Ross Harmes and Dustin Diaz
- [High Performance Web Sites: Essential Knowledge for Front-End Engineers](https://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
- [Maintainable JavaScript](https://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
- [JavaScript Web Applications](https://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
- [Pro JavaScript Techniques](https://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
- [Smashing Node.js: JavaScript Everywhere](https://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
- [Secrets of the JavaScript Ninja](https://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
- [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
- [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
- [JSBooks](http://jsbooks.revolunet.com/) - Julien Bouquillon
- [Third Party JavaScript](https://www.manning.com/books/third-party-javascript) - Ben Vinegar and Anton Kovalyov
- [Effective JavaScript: 68 Specific Ways to Harness the Power of JavaScript](http://amzn.com/0321812182) - David Herman
- [Eloquent JavaScript](http://eloquentjavascript.net/) - Marijn Haverbeke
- [You Don’t Know JS: ES6 & Beyond](http://shop.oreilly.com/product/0636920033769.do) - Kyle Simpson

**Blogs**

- [JavaScript Weekly](http://javascriptweekly.com/)
- [JavaScript, JavaScript...](https://javascriptweblog.wordpress.com/)
- [Bocoup Weblog](https://bocoup.com/weblog)
- [Adequately Good](http://www.adequatelygood.com/)
- [NCZOnline](https://www.nczonline.net/)
- [Perfection Kills](http://perfectionkills.com/)
- [Ben Alman](http://benalman.com/)
- [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
- [nettuts](http://code.tutsplus.com/?s=javascript)

**Podcasts**

- [JavaScript Air](https://javascriptair.com/)
- [JavaScript Jabber](https://devchat.tv/js-jabber/)

**[⬆ back to top](#table-of-contents)**
