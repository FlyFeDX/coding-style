# React/JSX Style Guide

_目前 React 和 JSX 最合理的编码方式_

React 编码风格指南主要是基于 JavaScript 中目前流行的标准，尽管一些约定(比如 async/await 或者 静态的类属性)仍然可以根据具体情况包含或禁止.

## Table of Contents

1. [Basic Rules](#basic-rules)
1. [Class vs `React.createClass` vs stateless](#class-vs-reactcreateclass-vs-stateless)
1. [Mixins](#mixins)
1. [Naming](#naming)
1. [Declaration](#declaration)
1. [Alignment](#alignment)
1. [Quotes](#quotes)
1. [Spacing](#spacing)
1. [Props](#props)
1. [Refs](#refs)
1. [Parentheses](#parentheses)
1. [Tags](#tags)
1. [Methods](#methods)
1. [Hooks](#hooks)
1. [Ordering](#ordering)
1. [`isMounted`](#ismounted)

## Basic Rules

- 每个文件只包含一个 React 组件.
- 但是, 在一个文件里包含多个 [Stateless, or Pure, Components 组件](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions)是允许的. eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
- 坚持使用 JSX 语法.
- 不要用 `React.createElement`, 除了初始化 app 的文件.

## Class vs `React.createClass` vs stateless

- 如果组件内部包含 state 和/或 refs, 推荐使用 `class extends React.Component` 而不是 `React.createClass`. eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md) [`react/prefer-stateless-function`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)

  ```jsx
  // bad
  const Listing = React.createClass({
    // ...
    render() {
      return <div>{this.state.hello}</div>
    },
  })

  // good
  class Listing extends React.Component {
    // ...
    render() {
      return <div>{this.state.hello}</div>
    }
  }
  ```

  如果组件内部不包含 state 或者 refs, 推荐使用函数式组件:

  ```jsx
  // bad
  class Listing extends React.Component {
    render() {
      return <div>{this.props.hello}</div>
    }
  }

  // bad (relying on function name inference is discouraged)
  const Listing = ({ hello }) => <div>{hello}</div>

  // good
  function Listing({ hello }) {
    return <div>{hello}</div>
  }
  ```

## Mixins

- [不要使用 mixins](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html).

> mixins 会引入一些隐含依赖，导致命名冲突，会导致滚雪球式的复杂度。大多数情况下，mixins 都可以通过组件，高阶组件 HOC 或者工具模块更好的实现。

## Naming

- **拓展名**: 用 `.jsx` 或 `.tsx` 作为 React 组件 的拓展名. eslint: [`react/jsx-filename-extension`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-filename-extension.md)
- **文件命名**: 使用大驼峰命名. 比如, `reservationCard.jsx`.
- **文件夹命名**: 如果是文件夹包含的组件簇, 每个单词小写, 使用中划线命名规则. 比如, `checkbox-number/*`
- **引用命名**: React 组件使用大驼峰命名,组件的实例用小驼峰命名. eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md).

  ```jsx
  // bad
  import reservationCard from './ReservationCard'

  // good
  import ReservationCard from './ReservationCard'

  // bad
  const ReservationItem = <ReservationCard />

  // good
  const reservationItem = <ReservationCard />
  ```

- **组件命名**: 使用文件名作为组件名. 例如, `ReservationCard.jsx` 应该用 `ReservationCard`作为组件名. 但是, 对于一个文件夹里的根组件, 需要用 `index.jsx` 作为文件名，同时用文件夹名的大驼峰形式作为组件名:

  ```jsx
  // bad
  import Footer from './footer-component/Footer'

  // bad
  import Footer from './footer-component/index'

  // good
  import Footer from './footer-component'
  ```

- **高阶组件命名**: 用高阶组件名和传入的组件名组合作为生成的组件的 displayName. 举个例子，一个高阶组件 withFoo(),当传入一个组件 Bar 应该生成一个新的组件，他的 displayName 属性是 withFoo(Bar).

  > 组件的 displayName 可以用于开发者工具或者错误信息中，同时还有一个值可以清晰的表达这种组件关系，这可以帮助大家理解到底发生了什么

  ```jsx
  // bad
  export default function withFoo(WrappedComponent) {
    return function WithFoo(props) {
      return <WrappedComponent {...props} foo />
    }
  }

  // good
  export default function withFoo(WrappedComponent) {
    function WithFoo(props) {
      return <WrappedComponent {...props} foo />
    }

    const wrappedComponentName =
      WrappedComponent.displayName || WrappedComponent.name || 'Component'

    WithFoo.displayName = `withFoo(${wrappedComponentName})`
    return WithFoo
  }
  ```

- **Props 命名**: 避免用 DOM 组件的属性名表达不同的意义.

  > 不要使用 `style` 或者 `className`这种内置的 props. 会使代码的可读性降低, 维护性降低, 并可能导致错误.

  ```jsx
  // bad
  <MyComponent style="fancy" />

  // bad
  <MyComponent className="fancy" />

  // good
  <MyComponent variant="fancy" />
  ```

## Declaration

- 不要使用 `displayName` 去命名一个组件. 最好直接为组件命名.

  ```jsx
  // bad
  export default React.createClass({
    displayName: 'ReservationCard',
    // stuff goes here
  })

  // good
  export default class ReservationCard extends React.Component {}
  ```

## Alignment

- 对 JSX 语法使用这些对齐风格. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md) [`react/jsx-closing-tag-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-tag-location.md)

  ```jsx
  // bad
  <Foo superLongParam="bar"
       anotherSuperLongParam="baz" />

  // good
  <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
  />

  // if props fit in one line then keep it on the same line
  <Foo bar="bar" />

  // children get indented normally
  <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
  >
      <Quux />
  </Foo>

  // bad
  {showButton &&
      <Button />
  }

  // bad
  {
    showButton &&
        <Button />
  }

  // good
  {showButton && (
      <Button />
  )}

  // good
  {showButton && <Button />}

  // good
  {someReallyLongConditional
      && anotherLongConditional
      && (
          <Foo
              superLongParam="bar"
              anotherSuperLongParam="baz"
          />
      )
  }

  // good
  {someConditional ? (
      <Foo />
  ) : (
      <Foo
          superLongParam="bar"
          anotherSuperLongParam="baz"
      />
  )}
  ```

## Quotes

- 在 JSX 中 的属性 使用双引号 (`"`) , 在 JS 中的 属性使用单引号 (`'`). eslint: [`jsx-quotes`](https://eslint.org/docs/rules/jsx-quotes)

  > 通常 HTML 属性也通常使用双引号而不是单引号，所以 JSX 属性也使用这个约定。

  ```jsx
  // bad
  <Foo bar='bar' />

  // good
  <Foo bar="bar" />

  // bad
  <Foo style={{ left: "20px" }} />

  // good
  <Foo style={{ left: '20px' }} />
  ```

## Spacing

- 在自闭和标签内空一格. eslint: [`no-multi-spaces`](https://eslint.org/docs/rules/no-multi-spaces), [`react/jsx-tag-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-tag-spacing.md)

  ```jsx
  // bad
  <Foo/>

  // very bad
  <Foo                 />

  // bad
  <Foo
   />

  // good
  <Foo />
  ```

- JSX 里的大括号不要空格. eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

  ```jsx
  // bad
  <Foo bar={ baz } />

  // good
  <Foo bar={baz} />
  ```

## Props

- props 用小驼峰.

  ```jsx
  // bad
  <Foo
      UserName="hello"
      phone_number={12345678}
  />

  // good
  <Foo
      userName="hello"
      phoneNumber={12345678}
      component={SomeComponent}
  />
  ```

- 如果 prop 的值是 true 可以忽略这个值，直接写 prop 名就可以. eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

  ```jsx
  // bad
  <Foo
      hidden={true}
  />

  // good
  <Foo
      hidden
  />

  // good
  <Foo hidden />
  ```

- <img> 标签通常会设置 alt 属性. eslint: [`jsx-a11y/alt-text`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/alt-text.md)

  ```jsx
  // bad
  <img src="hello.jpg" />

  // good
  <img src="hello.jpg" alt="Me waving hello" />

  // good
  <img src="hello.jpg" alt="" />
  ```

- 不要在 <img> 的 alt 属性里用类似 "image"， "photo"， "picture" 这些单词. eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)

  > Why? 因为屏幕阅读器已经将 img 声明为图片了，所以这个信息就不需要出现在 alt 文本里了。

  ```jsx
  // bad
  <img src="hello.jpg" alt="Picture of me waving hello" />

  // good
  <img src="hello.jpg" alt="Me waving hello" />
  ```

- 避免使用数组索引作为组件的 `key` 属性, 要使用 `稳定` 的 ID. eslint: [`react/no-array-index-key`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-array-index-key.md)

```jsx
// bad
{
  todos.map((todo, index) => <Todo {...todo} key={index} />)
}

// good
{
  todos.map((todo) => <Todo {...todo} key={todo.id} />)
}
```

- 有节制的使用扩展运算符 {...props} 去赋值一个组件的属性
  > 因为有可能会将一些没必要的属性赋值给组件 [pass invalid HTML attributes to the DOM](https://reactjs.org/blog/2017/09/08/dom-attributes-in-react-16.html).

例外:

- 高阶组件

```jsx
function HOC(WrappedComponent) {
  return class Proxy extends React.Component {
    Proxy.propTypes = {
      text: PropTypes.string,
      isLoading: PropTypes.bool
    };

    render() {
      return <WrappedComponent {...this.props} />
    }
  }
}
```

- 可以使用拓展运算符去拓展一个已知的对象, 如下:

```jsx
export default function Foo {
  const props = {
    text: '',
    isPublished: false
  }

  return (<div {...props} />);
}
```

```jsx
// bad
render() {
  const { irrelevantProp, ...restProps } = this.props;
  return <WrappedComponent {...this.props} />
}

// good
render() {
  const { irrelevantProp, ...restProps } = this.props;
  return <WrappedComponent {...restProps} />
}
```

## Refs

- 推荐在类组件中 用 ref callback 函数. eslint: [`react/no-string-refs`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-string-refs.md)

  ```jsx
  // bad
  <Foo
    ref="myRef"
  />

  // good
  <Foo
    ref={(ref) => { this.myRef = ref; }}
  />
  ```

## Parentheses

- 当 JSX 标签有多行时，用圆括号包起来. eslint: [`react/jsx-wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-wrap-multilines.md)

  ```jsx
  // bad
  render() {
    return <MyComponent variant="long body" foo="bar">
             <MyChild />
           </MyComponent>;
  }

  // good
  render() {
    return (
      <MyComponent variant="long body" foo="bar">
        <MyChild />
      </MyComponent>
    );
  }

  // good, when single line
  render() {
    const body = <div>hello</div>;
    return <MyComponent>{body}</MyComponent>;
  }
  ```

## Tags

- 当没有子元素时，最好用自闭合标签. eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

  ```jsx
  // bad
  <Foo variant="stuff"></Foo>

  // good
  <Foo variant="stuff" />
  ```

- 如果组件有多行属性，用它的闭合标签单独作为结束行. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

  ```jsx
  // bad
  <Foo
    bar="bar"
    baz="baz" />

  // good
  <Foo
    bar="bar"
    baz="baz"
  />
  ```

## Methods

- 使用箭头函数作为属性传递给一个组件. 当需要向事件处理程序传递额外的数据时，它很方便. 注意, 确保 [不要去损耗太多性能](https://www.bignerdranch.com/blog/choosing-the-best-approach-for-react-event-handlers/), 特别是要传递给 `PureComponents` 时 , 因为它可能触发多次重复渲染.

  ```jsx
  function ItemList(props) {
    return (
      <ul>
        {props.items.map((item, index) => (
          <Item
            key={item.key}
            onClick={(event) => {
              doSomethingWith(event, item.name, index)
            }}
          />
        ))}
      </ul>
    )
  }
  ```

- 不要在 React 组件里使用下划线作为内部方法名前缀.也不需要使用下划线来标记私有变量.

  ```jsx
  // bad
  React.createClass({
    _onClickSubmit() {
      // do stuff
    },

    // other stuff
  });

  // good
  class extends React.Component {
    onClickSubmit() {
      // do stuff
    }

    // other stuff
  }
  ```

- `render` 方法一定要有返回值. eslint: [`react/require-render-return`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/require-render-return.md)

  ```jsx
  // bad
  render() {
    (<div />);
  }

  // good
  render() {
    return (<div />);
  }
  ```

- 当一个方法需要处理一些事情的时候,名称需要以 `handle` 开头. 而组件内部 export 的事件,需要以`on`开头.

  ```tsx
  // bad
  <Button onClick={onClick} />

  // good
  <Button onClick={handleClick} />

  // bad
  const handleClick = () => {
    props.methodFromParentComponent();
  }

  // good
  const handleClick = () => {
    props.onClick();
  }
  ```

## Hooks

- 只能在函数组件的顶级作用域使用.比如: 不能在循环/条件语句/嵌套函数等中使用 hooks.

  > React 依赖于调用 hook 的顺序

- 只能在函数组件或者其他 Hooks 中使用.

- useState 中,设置值的方法必须用 `set` 开头. 例如 `const [count, setCount] = useState(0);`
- 不要忘记一些 `Hooks` 的依赖, 如 `useCallback` 和 `useEffect` 等等.
- 当在`useEffect`中添加事件监听器时，在返回函数中删除事件监听器是很重要的。同理: `setTimeout`, `setInterval` 和 `requestAnimationFrame`.

## Ordering

- 类组件内部属性定义的顺序 `class extends React.Component`:

1. (可选) `static` 方法
1. `constructor`
1. `static getDerivedStateFromProps`
1. `componentDidMount`
1. _从服务端获取数据的方法_ 比如: `getTableData()` or `getFormData()`
1. `shouldComponentUpdate`
1. `componentDidUpdate`
1. `componentWillUnmount`
1. _事件处理函数, 以 'handle'开头的函数_ 比如: `handleSubmit()` or `handleChangeDescription()`
1. _事件响应函数, 以'on'开头的函数_ 比如: `onClickSubmit()` or `onChangeDescription()`
1. _可选的其他 render 方法_ like `renderNavigation()` or `renderProfilePicture()`
1. `render`

- 如何去定义 `propTypes`, `defaultProps`, `contextTypes`, 等等...

  ```jsx
  import React from 'react'
  import PropTypes from 'prop-types'

  const propTypes = {
    id: PropTypes.number.isRequired,
    url: PropTypes.string.isRequired,
    text: PropTypes.string,
  }

  const defaultProps = {
    text: 'Hello World',
  }

  class Link extends React.Component {
    static methodsAreOk() {
      return true
    }

    render() {
      return (
        <a href={this.props.url} data-id={this.props.id}>
          {this.props.text}
        </a>
      )
    }
  }

  Link.propTypes = propTypes
  Link.defaultProps = defaultProps

  export default Link
  ```

* 在 TypeScript 中如何去 Props 和 states 定义类型.

  ```tsx
  import React from 'react'

  interface IProps {
    id: number
    url: string
    text?: string
  }

  interface IStates {}

  class Link extends React.Component<IProps, IStates> {
    static methodsAreOk() {
      return true
    }

    render() {
      const { id, url, text = 'Hello World' } = this.props
      return (
        <a href={url} data-id={id}>
          {text}
        </a>
      )
    }
  }

  export default Link
  ```

* 在类组件中不要使用如下废弃的生命周期:

1. `UNSAFE_componentWillMount`
2. `UNSAFE_componentWillReceiveProps`
3. `UNSAFE_componentWillUpdate`

- 函数式组件内部属性定义的顺序

1. `useContext`
1. `useState`
1. `useRef`
1. `useImperativeHandle`
1. `useEffect`
1. `useLayoutEffect`
1. `useMemo`
1. _从服务端获取数据的方法_ 比如 `getTableData()` or `getFormData()`
1. _事件处理函数, 以 'handle'开头的函数_ 比如: `handleSubmit()` or `handleChangeDescription()`
1. _事件响应函数, 以'on'开头的函数_ like `onClickSubmit()` or `onChangeDescription()`
1. _可选的其他 render 方法_ like `renderNavigation()` or `renderProfilePicture()`
1. `return JSX`

举例:

```tsx
const Link = forwardRef(function (props, ref) {
  const {} = useContext(context)
  const [count, setCount] = useState(0)
  const wrapper = useRef(null)

  useImperativeHandle(ref, () => ({
    resetCount: () => {
      setCount(0)
    },
  }))

  const getXXX = () => {}

  useEffect(() => {
    // do something ...
  }, [])

  useLayoutEffect(() => {
    // do something ...
  }, [])

  const momiziedValue = useMemo(() => {
    // calculate some value
    return 0
  }, [])

  const handleXXX = () => {}

  const onXXX = () => {}

  const renderXXX = () => {}

  return <div>{momiziedValue}</div>
})

export default Link
```

## `isMounted`

- 不要使用 `isMounted`. eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

> Why? [`isMounted` 是反模式][anti-pattern], 这个在 ES6 class 里不允许的, 并且已经被官方废弃.

[anti-pattern]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

**[⬆ back to top](#table-of-contents)**
