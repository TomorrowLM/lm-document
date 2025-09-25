---
slug: react技术栈
title: react技术栈
description: react学习
keywords: react,redux,router,hooks,生命周期,next
category: react技术栈
tags: [react,redux,router,hooks,生命周期,next]
author: liming
date: 25-September-2020
sticky: 6
---

# 核心依赖

## react

react包含了生成虚拟DOM的函数react.createElement，及Component类。

### createElement

在 react 中，如要要创建 DOM 元素了，只能使用 React 提供的 JS API 来创建，不能【直接】像 Vue 中那样，手写 HTML 元素

React.createElement() 方法，用于创建 虚拟DOM 对象，它接收 3个及以上的参数

- 参数1： 是个字符串类型的参数，表示要创建的元素类型

- 参数2： 是一个属性对象，表示 创建的这个元素上，有哪些属性

- 参数3： 元素的子节点

```react
var myDiv = React.createElement('div', { title: 'this is a div', id: 'mydiv' }, '这是一个div', myH1)
//<div title="this is a div" id="mydiv">这是一个div</div>
```

由于，React官方，发现，如果直接让用户手写 JS 代码创建元素，用户会疯掉的，然后，用户就开始寻找新的前端框架了，于是，React 官方，就提出了一套 JSX 语法规范，能够让我们在 JS 文件中，书写类似于 HTML 那样的代码，快速定义虚拟DOM结构；

问题： JSX（符合 XML 规范的 JS 语法）的原理是什么？

JSX内部在运行的时候，也是先把类似于HTML 这样的标签代码，转换为了 React.createElement 的形式；（JSX是一个对程序员友好的语法糖）

在JSX创建DOM的时候，所有的节点，必须有唯一的根元素进行包裹；

## react-dom

包的核心功能就是把这些虚拟DOM渲染到文档中变成实际DOM

### render

全局定义将组件App放置在`id='App'`的容器里

```react
import React from 'react'
import ReactDom from 'react-dom'
import App from './app.jsx'
// ReactDOM.render('要渲染的虚拟DOM元素', '要渲染到页面上的哪个位置中')
// 注意： ReactDOM.render() 方法的第二个参数，和vue不一样，不接受 "#app" 这样的字符串，而是需要传递一个原生的 DOM 对象
ReactDom.render(
    <App/>,
    document.getElementById('App')
)
```

## react-router-dom

HashRouter, Route, Link





# 组件的生命周期

https://react.docschina.org/docs/react-component.html

https://m.html.cn/qa/react/14367.html

https://juejin.cn/post/7285540804734468150#heading-0

生命周期的描述如下：

- 挂载期：一个组件实例初次创建的过程。
- 更新期：组件在创建后再次渲染的过程。
- 卸载期：组件在使用完后被销毁的过程。

![img](img/前端/react/6df08ee6e7a72d4ae31e561afef66f57.png)

## 组件初始化阶段

**组件实例创建阶段的生命周期函数，在组件的一辈子中，只执行一次**；

### constructor(props)

实例过程中自动调用的方法，在方法内部通过 super 关键字获取来自父组件的 props

通常用于初始化组件的状态和绑定方法。

- this.state初始化
- 为事件处理函数绑定实例

```
constructor(props) { 
		//调用父类React Component的构造函数，⽤来将⽗组件传来的 props 绑定到这个类中
    super(props);
    this.state = { count: 0 }; 
    this.handleClick = this.handleClick.bind(this); 
}

```

### ~~componentWillMount~~

组件将要被挂载，此时还没有开始渲染虚拟DOM，无法获取到页面上的任何元素，因为虚拟DOM和页面都还没有开始渲染呢。

- 进⾏ajax请求，作者一开始也喜欢在React的willMount函数中进行异步获取数据（认为这可以减少白屏的时间），后来发现其实应该在didMount中进行。

- 可以修改state

### getDerivedStateFromProps（新增）

### render

> 它是一个纯函数，其中不应该包含任何副作用或改变状态的操作。

创建虚拟dom，当render执行完，内存中就有了完整的虚拟DOM了

### componentDidMount

这个函数是在组件挂载到DOM后执行的，可以在这里获取数据、进行一些异步请求或DOM操作。

- 网络请求获取数据
- 对DOM进行操作
- 这个方法是比较适合添加订阅的地方。如果添加了订阅，请不要忘记在 `componentWillUnmount()` 里取消订阅  

## 组件更新阶段

### 触发更新条件

- 组件的state
- 组件的props
- 父组件重新render

### ~~componentWillReceiveProps~~

组件将要接收新属性props，此时，只要这个方法被触发，就证明父组件为当前子组件传递了新的属性值；

如果我们使用 this.props 来获取属性值，这个属性值，不是最新的，是上一次的旧属性值

```jsx
componentWillReceiveProps(nextProps){    
    console.log(this.props.pmsg + ' ---- ' + nextProps.pmsg
);}
```

### getDerivedStateFromProps（新增）

### shouldComponentUpdate

组件是否需要被更新，此时，组件尚未被更新，但是，state 和 props 肯定是最新的。 首次渲染或使用 `forceUpdate()` 时不会调用该方法。 

```
shouldComponentUpdate(nextProps, nextState)
```

### ~~componentWillUpdate~~

组件将要被更新，此时，尚未开始更新，内存中的虚拟DOM树还是旧的，页面上的 DOM 元素 也是旧的

### render

根据最新的 state 和 props 重新渲染一棵内存中的 虚拟DOM树，当 render 调用完毕，内存中的旧DOM树，已经被新DOM树替换了！**此时页面还是旧的**

### getSnapshotBeforeUpdate

### componentDidUpdate

`componentDidUpdate(prevProps,prevState,snapshot)`：它在组件更新（即`render()` 方法执行后）后被调用。它接收三个参数：`prevProps`、`prevState`、`snapshot`。与旧的钩子函数相比，多了一个参数`snapshot`。

```jsx
componentDidUpdate(prevProps, prevState, snapshot) {
    if (this.props.data !== prevProps.data) {
      console.log('this.props中的数据变了');
    }

    // 使用getSnapshotBeforeUpdate()的返回值
    if (snapshot !== null) {
      console.log('Snapshot from getSnapshotBeforeUpdate:', snapshot);
    }
}
```

> 说明：如果`getSnapshotBeforeUpdate()`有返回值，它会成为`componentDidUpdate()`的第三个参数，你可以在这里使用它。



## 组件销毁阶段

这个函数是在组件卸载前执行的，此时组件还可以正常使用；可以在这里进行一些清理工作，比如取消订阅、清除定时器、取消异步请求或移除事件监听器等。

**也有一个显著的特点，一辈子只执行一次**

### componentWillUnmount

## 新旧生命周期对比

![react生命周期(新).png](img/前端/react/40b4f2d0b995423184a0840446)

当组件实例被创建并插入 DOM 中时，其生命周期调用顺序如下：

- constructor()
- static getDerivedStateFromProps()
- render()
- componentDidMount()

当组件的 props 或 state 发生变化时会触发更新。组件更新的生命周期调用顺序如下：

- static getDerivedStateFromProps()
- shouldComponentUpdate()
- render()
- getSnapshotBeforeUpdate()
- componentDidUpdate()

### 废弃的生命周期

#### 为什么废弃

`React`废弃`componentWillMount`和`componentWillReceiveProps`这两个[生命周期方法](https://so.csdn.net/so/search?q=生命周期方法&spm=1001.2101.3001.7020)的原因主要涉及到`React的内部机制变更、性能优化以及未来特性的支持`。以下是对这两个问题的详细解答

- **与Fiber架构的调和过程不兼容**：**fiber算法是异步渲染**，异步的渲染，很可能因为高优先级任务的出现而打断现有任务导致它们就可能执行多次，具体是在`render()`生成虚拟 `dom` 阶段可以打断重来， 这就会导致在dom挂载之前或是被更新之前的所有任务都会重复操作，所以`componentWillMount()`、·`componentWillReceiveProps()` `componentWIllUpdate()`方法可能会执行多次。

- **容易引起混淆和使用不当**：旧版Will生命周期方法在执行异步操作（如数据请求、订阅等）时，**由于不明确的调用时机，可能导致状态更新不一致或副作用不可预测**。这增加了代码的复杂性和出错的可能性，使得开发者难以维护和调试。

  

#### componentWillMount

**新版本中官方推荐将初始化的操作放在`constructor()`中， 将请求异步数据、订阅事件源、监听事件的操作放在`componentDidMount()`**

> 不推荐使用的原因主要是它的执行时机可能会导致一些问题。具体来说，如果你在`componentWillMount()` 中触发了异步操作，可能会导致在组件卸载前仍然执行未完成的操作，这可能会引发潜在的错误。

#### componentWillUpdate

> 与`componentWillReceiveProps()`类似，这个方法也容易导致状态不一致。

#### componentWillReceiveProps

在老版本的`React`中，如果组件自身的state与其props密切相关的话，我们就会用到`componentWillReceiveProps(nextProps)`。

这个生命周期方法被不推荐使用，因为它容易导致状态不一致的问题。常见的业务场景比如，tabs的激活状态，一般我们会在组件自身内通过state维持，但是当我们从其他页面返回时，想要保持离开之前时的tabs状态，这时我们可以通过props来传递，（**破坏了数据源的单一性**）

```react
//previous 
componentWillReceiveProps(nextProps, nextContext) {
   if(nextProps.activeIndex !== this.state.activeIndex) {
     this.setState({activeIndex: nextProps.activeIndex})
     this.fetchData() //因异步中断，可能会重复操作
   }
 }

/** next 
*将更新state与触发逻辑的操作分成两部分来执行，state更新部分在getDerivedStateFromProps中完成， 逻辑部分操作
*在componentDidUpdate()中完成
*/
static getDerivedStateFromProps(nextProps, prevState) { //此方法不能获取组件实例
   if(nextProps.activeIndex !== prevState.activeIndex) {
     return {activeIndex: nextProps.activeIndex}
   }
 }
componentDidUpdate() {
  this.fetchData() //可以确保只执行一次
}
```

该生命周期函数按照上面图谱中应该是在props属性改变之后调用，但其实只要父组件重新渲染，无论子组件的props有没有更新，子组件都会调用`componentWillReceiveProps` **注意这里可能会造成死循环，即当子组件在该方法中调用了父组件通过props传递过来的函数时， 恰巧该函数中有能让父组件重新渲染的逻辑，就会造成死循环**

```react
class Parent extends Component {
    render() {
        return (
            <div>
          			{/* 迫使父组件更新, 子组件就会调用componentWillReceiveProps */}
                <div onClick={() => this.forceUpdate()}> re-render </div> 
                <Child parentFun={() => this.setState({})} />
            </div>
        )
    }
}

class Child extends Component {
    componentWillReceiveProps(nextProps, nextContext) {
      {/*  子组件调用了父组件通过props传递过来的函数，该函数会使父组件重新渲染，造成死循环 */}
        nextProps.parentFun()
    }
    render() {
        return (
            <div> child component </div>
        );
    }
}
```



### 新增

#### getDerivedStateFromProps

> **使用getDerivedStateFromProps代替了旧的componentWillReceiveProps**

`getDerivedStateFromProps` 是 React 生命周期中的一个静态方法，**主要用于在组件接收到新的 props 时更新 state**。这个方法在组件的初始渲染和后续的每次更新（即每次接收到新的 props 或 state）时都会被调用。

- 使用方式
  - **静态方法**：这意味着你不能在这个方法中使用 `this` 关键字。它的第一个参数是新的 props，第二个参数是当前的 state。
  - **不触发副作用**：与`componentDidUpdate()` 不同，`getDerivedStateFromProps()` 不应执行副作用，如发起网络请求。它只用于计算state。
- **返回值**：`getDerivedStateFromProps` 必须返回一个对象来更新 state，或者返回 `null` 表示不需要更新 state。
- **作用**：这个方法的主要作用是确保组件的 state 总是与 props 保持一致。这是一个非常罕见的需求，因为通常我们希望 props 只是初始数据来源，而不是 state 的来源。然而，在某些特殊的场景中，可能需要根据 props 的变化来更新 state。

nextProps：与componentWillReceiveProps的nextProps参数相同。即改变后的Props

preState：发生改变前的state中各数据的值。

```
class MyComponent extends React.Component { 
    static getDerivedStateFromProps(nextProps, prevState) { 
        // 根据 nextProps 和 prevState 计算并返回新的 state 
        if (nextProps.value !== prevState.value) { 
            return { value: nextProps.value }; 
        } 
        return null; // 如果不需要更新 state，返回 null 
    } 
    
    constructor(props) { 
        super(props); 
        this.state = { 
            value: props.value, 
        }; 
    }
    
    render() { 
        return <div>{this.state.value}</div>; 
    } 
}

```

#### getSnapshotBeforeUpdate

> getSnapshotBeforeUpdate代替了旧的componentWillUpdate。

`getSnapshotBeforeUpdate(nextProps,prevState)`：它在组件更新之前触发。它允许你**捕获组件更新前**的一些信息（例如，滚动位置），并在组件更新后使用这些信息。

示例中，`getSnapshotBeforeUpdate()` 用于捕获滚动位置，然后在`componentDidUpdate()` 中使用snapshot来恢复滚动位置，以确保用户在滚动列表时不会在更新后失去滚动位置。

```react
class MyComponent extends React.Component {
	constructor(props) {
	  super(props);
	  this.myRef = React.createRef();
	}
  
	getSnapshotBeforeUpdate(prevProps, prevState) {
	  // 捕获组件更新前的滚动位置
	  if (prevProps.items.length < this.props.items.length) {
		const scrollHeight = this.myRef.current.scrollHeight;
		const scrollTop = this.myRef.current.scrollTop;
		return { scrollHeight, scrollTop };
	  }
	  return null;
	}
  
	componentDidUpdate(prevProps, prevState, snapshot) {
	  // 使用snapshot来恢复滚动位置
	  if (snapshot !== null) {
		this.myRef.current.scrollTop = snapshot.scrollTop + (this.myRef.current.scrollHeight - snapshot.scrollHeight);
	  }
	}
  
	render() {
	  // 使用ref来获取DOM元素的引用
	  return <div ref={this.myRef}>{/* 组件内容 */}</div>;
	}
}

```

主要特点和用法：

> - **触发时机**：`getSnapshotBeforeUpdate()` 在`render()` 方法被调用后、组件DOM更新前触发，通常用于在更新前捕获一些DOM信息。
> - **接收两个参数**：这个生命周期方法接收两个参数：`prevProps`、`prevState`。你可以使用这些参数来比较前后的props和state。
> - **返回值**：`getSnapshotBeforeUpdate()` 方法应该返回一个值（通常是一个对象），它将成为`componentDidUpdate()` 方法的第三个参数。这个返回值通常用于保存一些DOM相关的信息，比如滚动位置。
> - **通常和componentDidUpdate()一起使用**：`getSnapshotBeforeUpdate()` 结合`componentDidUpdate(prevProps, prevState, snapshot)` 使用，snapshot参数是`getSnapshotBeforeUpdate()` 的返回值。你可以在`componentDidUpdate()` 中使用snapshot来执行DOM操作或其他一些操作。

作

#### getDerivedStateFromError

```
static getDerivedStateFromError(error)
```

此生命周期会在后代组件抛出错误后被调用。 它将抛出的错误作为参数，并返回一个值以更新 state

```
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {    // 更新 state 使下一次渲染可以显降级 UI    return { hasError: true };  }
  render() {
    if (this.state.hasError) {      // 你可以渲染任何自定义的降级  UI      return <h1>Something went wrong.</h1>;    }
    return this.props.children; 
  }
}
```

## forceUpdate

默认情况下，当组件的 state 或 props 发生变化时，组件将重新渲染。如果 `render()` 方法依赖于其他数据，则可以调用 `forceUpdate()` 强制让组件重新渲染。

调用 `forceUpdate()` 将致使组件调用 `render()` 方法，**此操作会跳过该组件的 `shouldComponentUpdate()`。但其子组件会触发正常的生命周期方法，包括 `shouldComponentUpdate()` 方法**。如果标记发生变化，React 仍将只更新 DOM。

## useEffect模拟生命周期

| 类组件生命周期方法      | useEffect 实现方式                         |
| :---------------------- | :----------------------------------------- |
| `componentDidMount`     | `useEffect(fn, [])`                        |
| `componentDidUpdate`    | `useEffect(fn)` 或 `useEffect(fn, [deps])` |
| `componentWillUnmount`  | `useEffect(() => { return () => {} }, [])` |
| `shouldComponentUpdate` | `React.memo` + `useMemo`                   |

# 核心概念

## jsx

### 条件渲染

```ts
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}
//或者
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  return (
  	{isLoggedIn?<UserGreeting />:<GuestGreeting />}
  )
}
ReactDOM.render(
  // Try changing to isLoggedIn={true}:
  <Greeting isLoggedIn={false} />,
  document.getElementById('root')
);
```

### 列表 & Key

key为每个元素加上唯一的标识，这样在执行diff的时候会加快位置的确定

  ```js
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
  ```

 **在 JSX 中嵌入 map()**

  ```js
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()}
                  value={number} />
      )}
    </ul>
  );
}
  ```

key 帮助 React 识别哪些元素改变了，比如被添加或删除。因此你应当给数组中的每一个元素赋予一个确定的标识。一个元素的 key 最好是这个元素在列表中拥有的一个独一无二的字符串。

**如果使用数组索引，那么在对dom进行添加或删除，会出问题**：

<img src="img/前端/react/arr-delete-start.png" style="zoom: 50%;" />

<img src="img/前端/react/arr-delete-end.png" style="zoom:50%;" />

页面渲染好了之后，3 个 input 输入框依次输入随机内容，当我们用 index 作为 key 的时候，点击删除第一项按钮会发现，左侧文字正确改变，input 输入框最后一项没了，这不是我们希望的样子。 因为当我们使用 index 作为 key 时，此时 key 为 0、1、2，删掉第一项后 key 变为 0、1，此时 react 在执行 diff 算法过程中，任务 key=0 存在，只需要**更新**子节点的值，所以左侧的 name 成功改变，而 **input 的值非受控，不会更新**。同时在对比计算中少了 key=2 这项，删除了最后一项。

### 添加样式的方式

**第一种：行内样式**

想给虚拟dom添加行内样式，需要使用表达式传入样式对象的方式来实现：

```javascript
// 注意这里的两个括号，第一个表示我们在要JSX里插入JS了，第二个是对象的括号
 <p style={{color:'red', fontSize:'14px'}}>Hello world</p>
```

**动态添加样式**

```javascript
<div style={{display: (index===this.state.currentIndex) ? "block" : "none"}}>此标签是否隐藏</div>
```

```javascript
<div className={index===this.state.currentIndex?"active":null}>此标签是否选中</div>
```

**第二种：内嵌样式**

```javascript
<style>{`.operafor4{margin-top:42px !important}`}</style>
```

**第三种：css modules**

https://www.ruanyifeng.com/blog/2016/06/css_modules.html

CSS的规则都是全局的，任何一个组件的样式规则，都对整个页面有效。例如：父组件和子组件使用相同的class，父组件的class会覆盖子组件的样式

产生局部作用域的唯一方法，就是使用一个独一无二的`class`的名字，不会与其他选择器重名。这就是 CSS Modules 的做法。

css Modules 添加多个className

```css
<div className={`${styles.sAll} ${styles.s1}`}>aaaaaa</div>
```

CSS Modules 允许使用`:global(.className)`的语法，声明一个全局规则。

```css
.title {
  color: red;
}

:global(.title) {
  color: green;
}
```

**第四种：样式组件（styled-components）**

styled-components是针对React写的一套css-in-js框架，简单来讲就是在js中写css。
styled-components是一个第三方包，要安装。**Material框架**中的样式也是如此

## css

### class动态

#### 方法1：使用内联样式

如果你的类名变化是基于某些条件，你可以直接在JSX中使用内联样式，并通过条件表达式来决定是否应用某个样式类。

```jsx
const MyComponent = ({ isActive }) => {
  return (
    <div style={{ backgroundColor: isActive ? 'green' : 'red' }}>
      {isActive ? 'Active' : 'Inactive'}
    </div>
  );
};
```

#### 方法2：使用条件渲染

对于更复杂的类名变化，你可以在JSX中直接使用条件渲染来添加或移除类名。

```jsx
const MyComponent = ({ isActive }) => {
  return (
    <div className={isActive ? 'active' : 'inactive'}>
      {isActive ? 'Active' : 'Inactive'}
    </div>
  );
};
```

#### 方法3：使用`classnames`库

`classnames`库可以帮助你更方便地处理多个条件，尤其是当你有多个类名需要基于多个条件变化时。首先，你需要安装`classnames`：

```bash
npm install classnames
```

然后在你的组件中使用它：

```jsx
import classNames from 'classnames';
 
const MyComponent = ({ isActive, hasError }) => {
  return (
    <div className={classNames({
      'active': isActive,
      'error': hasError,
      'default': !(isActive || hasError) // 默认类名，当没有其他类名激活时使用
    })}>
      {isActive ? 'Active' : hasError ? 'Error' : 'Default'}
    </div>
  );
};
```

#### 方法4：使用模板字符串动态生成类名

如果你需要根据更复杂的逻辑动态生成类名，你可以使用ES6的模板字符串功能。

```jsx
const MyComponent = ({ status }) => {
  const className = `status-${status}`; // 根据status动态生成类名
  return (
    <div className={className}>Status: {status}</div>
  );
};
```

#### 方法5：使用CSS Modules或Styled Components

对于更高级的样式需求，你可以使用CSS Modules或Styled Components来动态地应用样式。这些方法允许你以更组件化的方式处理样式。

#### 使用Styled Components:

首先，安装Styled Components：

```bash
npm install styled-components
```

然后，你可以这样使用它：

```jsx
import styled from 'styled-components';
 
const StatusDiv = styled.div`
  background-color: ${props => props.isActive ? 'green' : 'red'}; // 动态背景色
`;
 
const MyComponent = ({ isActive }) => {
  return (
    <StatusDiv isActive={isActive}>{isActive ? 'Active' : 'Inactive'}</StatusDiv>
  );
};
```

### CSS 引入方式

#### 在组件内直接使用

#### 组件中引入.css 文件

#### 引入.module.css 文件

![image-20250506160420630](img/前端/react/image-20250506160420630.png)

这种方式能够解决局部作用域问题，但也有一定的缺陷：

不方便动态来修改某些样式，依然需要使用内联样式的方式；



#### CSS in JS

CSS-in-JS，是指一种模式，其中 CSS 由 JavaScript 生成而不是在外部文件中定义

此功能并不是 React 的一部分，而是由第三方库提供，例如：

- styled-components 
- emotion 
- glamorous

下面主要看看 styled-components 的基本使用



## 表单和受控组件

https://juejin.cn/post/6858276396968951822#heading-7

- 非受控组件
  - 由DOM本身维护 state
  - 控制能力较弱，但更加方便快捷，代码量少

- 受控组件：大部分时候推荐使用受控组件来实现表单

  - 表单数据由 React 组件负责处理，通常保存在组件的 state 属性中，并且只能通过使用 [`setState()`](https://react.docschina.org/docs/react-component.html#setstate)来更新。

    **对于受控组件来说，输入的值始终由 React 的 state 驱动**

  - 性能弱，可控制强

#### 受控组件

```tsx
class TestComponent extends React.Component {
  constructor (props) {
    super(props);
    this.state = { username: 'lindaidai' };
  }
  render () {
    return <input name="username" value={this.state.username} />
  }
}

```

输入框的内容取决的是`input`中的`value`属性，那么我们可以在`this.state`中定义一个名为`username`的属性，并将`input`上的`value`指定为这个属性

但是这时候你会发现`input`的内容是只读的，因为`value`会被我们的`this.state.username`所控制，当用户输入新的内容时，`this.state.username`并不会自动更新，这样的话`input`内的内容也就不会变了。可以用一个`onChange`事件来监听输入内容的改变并使用`setState`更新`this.state.username`

```
class TestComponent extends React.Component {
  constructor (props) {
    super(props);
    this.state = {
      username: "lindaidai"
    }
  }
  onChange (e) {
    console.log(e.target.value);
    this.setState({
      username: e.target.value
    })
  }
  render () {
    return <input name="username" value={this.state.username} onChange={(e) => this.onChange(e)} />
  }
}

```

#### 非受控组件

对于受控组件，我们需要为每个`状态更新`(例如`this.state.username`)编写一个`事件处理程序`(例如`this.setState({ username: e.target.value })`)。

那么还有一种场景是：我们仅仅是想要获取某个表单元素的值，而不关心它是如何改变的。

**可以用获取`DOM`元素信息的方式来获取表单元素的值**

```
import React, { Component } from 'react';

export class UnControll extends Component {
  constructor (props) {
    super(props);
    this.inputRef = React.createRef();
  }
  handleSubmit = (e) => {
    console.log('我们可以获得input内的值为', this.inputRef.current.value);
    e.preventDefault();
  }
  render () {
    return (
      <form onSubmit={e => this.handleSubmit(e)}>
        <input defaultValue="lindaidai" ref={this.inputRef} />
        <input type="submit" value="提交" />
      </form>
    )
  }
}

```

#### 特殊的文件file标签

**对于file类型的表单控件它始终是一个不受控制的组件，因为它的值只能由用户设置，而不是以编程方式设置。**

例如我现在想要通过状态更新来控制它：

```jsx
jsx 代码解读复制代码import React, { Component } from 'react';

export default class UnControll extends Component {
  constructor (props) {
    super(props);
    this.state = {
      files: []
    }
  }
  handleSubmit = (e) => {
    e.preventDefault();
  }
  handleFile = (e) => {
    console.log(e.target.files);
    const files = [...e.target.files];
    console.log(files);
    this.setState({
      files
    })
  }
  render () {
    return (
      <form onSubmit={e => this.handleSubmit(e)}>
        <input type="file" value={this.state.files} onChange={(e) => this.handleFile(e)} />
        <input type="submit" value="提交" />
      </form>
    )
  }
}
```

在选择了文件之后，我试图用`setState`来更新，结果却报错了：

![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/33381befaff74f39adf7bc30e4896801~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

所以我们应当使用非受控组件的方式来获取它的值，可以这样写：

```jsx
jsx 代码解读复制代码import React, { Component } from 'react';

export default class FileComponent extends Component {
  constructor (props) {
    super(props);
    this.fileRef = React.createRef();
  }
  handleSubmit = (e) => {
    console.log('我们可以获得file的值为', this.fileRef.current.files);
    e.preventDefault();
  }
  render () {
    return (
      <form onSubmit={e => this.handleSubmit(e)}>
        <input type="file" ref={this.fileRef} />
        <input type="submit" value="提交" />
      </form>
    )
  }
}
```



## props

```jsx
function Welcome(props) {  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;ReactDOM.render(
  element,
  document.getElementById('root')
);
```

### defaultProps

无论是函数组件还是 class 组件，都拥有 `defaultProps` 属性。可以通过配置特定的 `defaultProps` 属性来定义 `props` 的默认值

```jsx
class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

// 指定 props 的默认值：
Greeting.defaultProps = {
  name: 'Stranger'
};

// 渲染出 "Hello, Stranger"：
ReactDOM.render(
  <Greeting />,
  document.getElementById('example')
);
```

### PropTypes类型检查

PropTypes 进行类型检查,可用于确保组件接收到的props数据类型是有效的

导入包

```jsx
import PropTypes from 'prop-types';
```

编写组件

```jsx
class Greeting extends React.Component {
    render() {
        return <h1>Hello, {this.props.name}</h1>
    }
}
```

新增类型检查

```cpp
Greeting.propTypes = {
// 你可以将属性声明为 JS 原生类型，默认情况下
  // 这些属性都是可选的。
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,

  // 任何可被渲染的元素（包括数字、字符串、元素或数组）
  // (或 Fragment) 也包含这些类型。
  optionalNode: PropTypes.node,

  // 一个 React 元素。
  optionalElement: PropTypes.element,

  // 一个 React 元素类型（即，MyComponent）。
  optionalElementType: PropTypes.elementType,

  // 你也可以声明 prop 为类的实例，这里使用
  // JS 的 instanceof 操作符。
  optionalMessage: PropTypes.instanceOf(Message),

  // 你可以让你的 prop 只能是特定的值，指定它为
  // 枚举类型。
  optionalEnum: PropTypes.oneOf(['News', 'Photos']),

  // 一个对象可以是几种类型中的任意一个类型
  optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
  ]),

  // 可以指定一个数组由某一类型的元素组成
  optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

  // 可以指定一个对象由某一类型的值组成
  optionalObjectOf: PropTypes.objectOf(PropTypes.number),

  // 可以指定一个对象由特定的类型值组成
  optionalObjectWithShape: PropTypes.shape({
    color: PropTypes.string,
    fontSize: PropTypes.number
  }),
  
  // An object with warnings on extra properties
  optionalObjectWithStrictShape: PropTypes.exact({
    name: PropTypes.string,
    quantity: PropTypes.number
  }),   

  // 你可以在任何 PropTypes 属性后面加上 `isRequired` ，确保
  // 这个 prop 没有被提供时，会打印警告信息。
  requiredFunc: PropTypes.func.isRequired,

  // 任意类型的数据
  requiredAny: PropTypes.any.isRequired,

  // 你可以指定一个自定义验证器。它在验证失败时应返回一个 Error 对象。
  // 请不要使用 `console.warn` 或抛出异常，因为这在 `onOfType` 中不会起作用。
  customProp: function(props, propName, componentName) {
    if (!/matchme/.test(props[propName])) {
      return new Error(
        'Invalid prop `' + propName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  },

  // 你也可以提供一个自定义的 `arrayOf` 或 `objectOf` 验证器。
  // 它应该在验证失败时返回一个 Error 对象。
  // 验证器将验证数组或对象中的每个值。验证器的前两个参数
  // 第一个是数组或对象本身
  // 第二个是他们当前的键。
  customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if (!/matchme/.test(propValue[key])) {
      return new Error(
        'Invalid prop `' + propFullName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  })
}
```

### this.props.children 

> 类型vue的插槽

如果当前组件没有子节点，它就是 undefined ;

如果有一个子节点，数据类型是 Object；

如果有多个子节点，数据类型就是 Array。

优点

1. 组件全部放在顶层，数据来源直接在顶层定义，方便快捷
2. 结构更加明确，使用简单，没有更多层次的组件引用
3. 充分的认识 React 本质和 React.Children + props.children API

```
import React from 'react';
 
const Wrapper = (props) => {
  return (
    <div className="wrapper">
      {props.children}
    </div>
  );
};
 
const App = () => {
  return (
    <Wrapper>
      <h1>Hello, World!</h1>
      <p>This is a sample text.</p>
    </Wrapper>
  );
};
 
export default App;
```

`Wrapper` 组件使用 `props.children` 来访问并渲染其子元素。无论传递什么元素，`props.children` 都可以接收到并在组件内进行渲染。

```
// main.js
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";

// components
import About from "./components/About";

ReactDOM.render(
  <React.StrictMode>
    <App>
      <About name='name'></About>
      <About name='fisrtName'></About>
    </App>
  </React.StrictMode>,
  document.getElementById("root")
);

```

```
import React from 'react';

export default function About(props){
  return <div>{props.name}-{props.haxi}</div>
}
```



## setState

https://juejin.cn/post/6850418109636050958

https://juejin.cn/post/6959885030063603743#heading-0

https://juejin.cn/post/7062162951108558855

### 回调函数

setState提供了一个回调函数供开发者使用，在回调函数中，我们可以实时的获取到更新之后的数据。还是以刚才的例子做示范：

```
state = {
    number:1
};
componentDidMount(){
    this.setState({number:3},()=>{
        console.log(this.state.number)
    })
}
```

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/7/11/1733ca3cdfc5be38~tplv-t2oaga2asx-watermark.awebp)

### 执行机制

**setState 只在合成事件和钩子函数中是“异步”的，在原生事件和 setTimeout 中都是同步的。**

**合成事件：就是react 在组件中的onClick等都是属于它自定义的合成事件。**

**原生事件：比如通过addeventListener添加的，dom中的原生事件。**

#### 异步执行

这里的“异步”并不是说内部由异步代码实现，其实本身执行的过程和代码都是同步的**，只是合成事件和钩子函数的调用顺序在更新之前**，导致在合成事件和钩子函数中没法立马拿到更新后的值，形式了所谓的“异步”。

```
state = {
    number:1
};
componentDidMount(){
    this.setState({number:3})
    console.log(this.state.number)
}
```

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/7/11/1733ca3cdf9d950d~tplv-t2oaga2asx-watermark.awebp)

setState是一个异步方法，如果每次调用setState都会触发更新，那么性能消耗就大，异步操作是为了提高性能，React会将多个setState的调用合并一起进行**批量更新**，减少re-render调用。 **将 `setState()` 视为请求而不是立即更新组件的命令。** 

```
for ( let i = 0; i < 100; i++ ) {
    this.setState( { num: this.state.num + 1 } ); // num 是 0 所以 setState({num:0 + 1})。
}
```

如果setState是一个同步执行的机制，那么这个状态会被重新渲染100次，这对性能是一个相当大的消耗。

#### 同步执行

在setTimeout，Promise.then异步事件，原生dom事件中，setState和useState是同步执行的，`render` 会执行多次

```
changeText() {
  setTimeout(() => {
    this.setState({
      message: "你好啊"
    });
    console.log(this.state.message); //你好啊
  }, 0);
}
```

```
componentDidMount() {
const btnEl = document.getElementById("btn");
btnEl.addEventListener( 'click', () => {
this.setState({
message: "你好啊 ,李银河 "
});
console.log(this.state.message); // 你好啊 ,李银河
})
}
```



## ref

### 原生JS获取Dom

```jsx
import {Component} from "react";
class App extends Component {
    //定义获取Dom的函数
    handleGetDom(){
        let title = document.querySelector('#title');
        console.log(title);
        title.style.background = 'skyblue'
    }
    render() {
        return (
            <>
                <h1 id="title">测试节点</h1>
                <button onClick={this.handleGetDom}>点击操作Dom</button>
            </>
        )
    }
}
export default App;
```



### 使用场景

- 对Dom元素的焦点控制、内容选择、控制
- 对Dom元素的内容设置及媒体播放
- 对Dom元素的操作和对组件实例的操作
- 集成第三方 DOM 库

### 回调 Ref

> 支持在函数组件和类组件内部使用

回调ref是子组件绑定ref并将ref注入到子组件中的某个元素上，从而在父组件中获取子组件元素的操作

```
 <FatherWatchChild iptRef={(el) => (this.ref3 = el)} />
 
 export default function FatherWatchChild(props) {
  return (
    <div>
      <Input name="iptRef" type="text" ref={ props.iptRef }/>
    </div>
  );
}
```

### createRef

> **支持在类组件和元素dom中使用**

```jsx
import React from 'react';
export default class MyInput extends React.Component {
    constructor(props) {
        super(props);
        //分配给实例属性
        this.inputRef = React.createRef(null);
    }
    componentDidMount() {
        //通过 this.inputRef.current 获取对该节点的引用
        this.inputRef && this.inputRef.current.focus();
    }
    render() {
        //把 <input> ref 关联到构造函数中创建的 `inputRef` 上
        return (
            <input type="text" ref={this.inputRef}/>
        )
    }
}
```

**React.createRef() 创建的 ref 的值根据节点的类型而有所不同：**

- 当 `ref` 属性用于 HTML 元素时，接收底层 DOM 元素作为其 `current` 属性。

- 当 `ref` 属性用于自定义 class 组件时，`ref` 接收**组件实例**作为其 `current` 属性

- **不能挂载到函数组件上，因为函数组件没有实例（instance）**

  但是，你可以在函数式组件中使用ref属性，就像你引用DOM元素和类组件一样。

### useRef

**只能在函数组件中使用**

useRef 用法类似于React.createRef()，区别：https://zhuanlan.zhihu.com/p/105276393

作用

- 访问 DOM 元素
- `useRef` 可以存储任何可变值，且更新时不会导致组件重新渲染，或者导致useRef存储的值被再次初始化。**createRef 每次渲染都会返回一个新的引用，而 useRef 每次都会返回相同的引用**

```jsx
import React, { createRef, useEffect, useState } from "react";
import { Button, Input } from "antd";
const Different = () => {
  const [renderIndex, setRenderIndex] = React.useState(1);
  const refFromUseRef = React.useRef();
  const refFromCreateRef = createRef();
  //renderIndex改变后再次render，refFromUseRef.current的值是不会重置
  if (!refFromUseRef.current) {
    refFromUseRef.current = renderIndex;
  }
  if (!refFromCreateRef.current) {
    refFromCreateRef.current = renderIndex;
  }
  return (
    <div>
      <p>Current render index: {renderIndex}</p>
      <p>
        <b>refFromUseRef</b> value: {refFromUseRef.current}
      </p>
      <p>
        <b>refFromCreateRef</b> value:{refFromCreateRef.current}
      </p>
      <Button onClick={() => setRenderIndex((prev) => prev + 1)}>
        Cause re-render
      </Button>
      {refFromCreateRef.current
        ? "可以看出createRef 每次渲染都会返回一个新的引用，而 useRef 每次都会返回相同的引用"
        : ""}
    </div>
  );
};

export default Different;

```



### forwardRef 转发/传递

https://github.com/pro-collection/interview-question/issues/741

forwardRef 是一个函数，它接收一个[渲染函数](https://so.csdn.net/so/search?q=渲染函数&spm=1001.2101.3001.7020)作为参数。这个渲染函数接收 props 和 ref 作为参数，并返回一个 React 节点。

```js
React.forwardRef((props, ref) => {
  return <div ref={ref} />;
});
```

`forwardRef` 的作用

- **访问子组件的 DOM 节点：** 当需要直接访问子组件中的 DOM 元素（例如，需要管理焦点或测量尺寸）时，可以使用 `forwardRef`。
- **在高阶组件（HOC）中转发 refs:** 封装组件时，通过 `forwardRef` 可以将 ref 属性透传给被封装的组件，这样父组件就能够通过 ref 访问到实际的子组件实例或 DOM 节点。
- **在函数组件中使用 refs(React 16.8+）：** 在引入 Hook 之前，函数组件不能直接与 refs 交互。但是，引入了 `forwardRef` 和 `useRef` 之后，函数组件可以接受 ref 并将它透传给子节点。



#### 使用场景

##### 1. 访问子组件的 DOM 节点

假设你有一个 `FancyButton` 组件，你想从父组件中直接访问这个按钮的 DOM 节点。

```
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));

// 现在你可以从父组件中直接获取DOM引用
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
```



##### 2. 在高阶组件中转发 refs

一个常见的模式是为了抽象或修改子组件行为的高阶组件（HOC）。`forwardRef`可以用来确保 ref 可以传递给包装组件：

```
function logProps(Component) {
  class LogProps extends React.Component {
    componentDidUpdate(prevProps) {
      console.log("old props:", prevProps);
      console.log("new props:", this.props);
    }

    render() {
      const { forwardedRef, ...rest } = this.props;

      // 将自定义的 prop 属性 "forwardedRef" 定义为 ref
      return <Component ref={forwardedRef} {...rest} />;
    }
  }

  // 注意：React.forwardRef 回调的第二个参数 "ref" 传递给了LogProps组件的props.forwardedRef
  return React.forwardRef((props, ref) => {
    return <LogProps {...props} forwardedRef={ref} />;
  });
}
```



##### 3. 在函数组件中使用 ref

在 Hook 出现之前，函数组件不能够直接与 `ref` 交云。现在可以这样做：

```
const MyFunctionalComponent = React.forwardRef((props, ref) => {
  return <input type="text" ref={ref} />;
});

const ref = React.createRef();
<MyFunctionalComponent ref={ref} />;
```



当你需要在父组件中控制子组件中的 DOM 元素或组件实例的行为时，`forwardRef` 是非常有用的工具。不过，如果可行的话，通常最好通过状态提升或使用 context 来管理行为，只在没有其他替代的情况下才选择使用 refs。

#### useImperativeHandle

> 在forwarRef中使用

```javascript
useImperativeHandle(ref, createHandle, [deps])
```

useImperativeHandle使用简单总结:

- 作用:  **减少暴露给父组件获取的DOM元素属性, 只暴露给父组件需要用到的DOM方法**
- 参数1: 父组件传递的ref属性
- 参数2: 返回一个对象, 以供给父组件中通过ref.current调用该对象中的方法

```jsx
import React, { useRef, forwardRef, useImperativeHandle } from 'react'

const JMInput = forwardRef((props, ref) => {
  const inputRef = useRef()
  // 作用: 减少父组件获取的DOM元素属性,只暴露给父组件需要用到的DOM方法
  // 参数1: 父组件传递的ref属性
  // 参数2: 返回一个对象,父组件通过ref.current调用对象中方法
  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current.focus()
    },
  }))
  return <input type="text" ref={inputRef} />
})

export default function ImperativeHandleDemo() {
  // useImperativeHandle 主要作用:用于减少父组件中通过forward+useRef获取子组件DOM元素暴露的属性过多
  // 为什么使用: 因为使用forward+useRef获取子函数式组件DOM时,获取到的dom属性暴露的太多了
  // 解决: 使用uesImperativeHandle解决,在子函数式组件中定义父组件需要进行DOM操作,减少获取DOM暴露的属性过多
  const inputRef = useRef()

  return (
    <div>
      <button onClick={() => inputRef.current.focus()}>聚焦</button>
      <JMInput ref={inputRef} />
    </div>
  )
}
```

```jsx
import React, { forwardRef, useImperativeHandle, useEffect, useRef } from 'react'

const TestRef = forwardRef((props, ref) => {
  useImperativeHandle(ref, () => ({
    open() {
      console.log("open")
    }
  }))
})
//或者
const TestRef = ((props) => {
  const { ref } = props;
  useImperativeHandle(ref, () => ({
    open() {
      console.log("open")
    }
  }))
})
function App () {
  const ref = useRef()
  useEffect(() => {
    ref.current.open()
  },[])
  
  return(
    <>
      <div>石小阳</div>
      <TestRef ref={ref}></TestRef>
    </>
  )
}
export default App
```

### ~~findDOMNode()~~

当组件加载到页面上之后（mounted），你都可以通过 `react-dom` 提供的 `findDOMNode()` 方法拿到组件对应的 DOM 元素。

```javascript
import { findDOMNode } from 'react-dom';

// Inside Component class
componentDidMound() {
  const el = findDOMNode(this);
}
```

`findDOMNode()` 不能用在无状态组件上。

## 事件处理

在 JavaScript 中，class 的方法默认不会[绑定](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) `this`。如果你忘记绑定 `this.handleClick` 并把它传入了 `onClick`，当你调用这个函数的时候 `this` 的值为 `undefined`。

### 绑定this

- 在constructor中用bind绑定

- 定义阶段使用箭头函数

- render中使用箭头函数

  ```
  class LoggingButton extends React.Component {
    handleClick() {
      console.log('this is:', this);
    }
  
    render() {
      // 此语法确保 `handleClick` 内的 `this` 已被绑定。
      return (
        <button onClick={() => this.handleClick()}>
          Click me
        </button>
      );
    }
  }
  ```

### 事件顺序

![image-20250506155847800](img/前端/react/image-20250506155847800.png)

```
原生事件：子元素 DOM 事件监听！
原生事件：父元素 DOM 事件监听！
React 事件：子元素事件监听！
React 事件：父元素事件监听！
原生事件：document DOM 事件监听！
```

可以得出以下结论：

- 会先执行原生事件，然后处理 React 事件
  - 当真实DOM元素触发事件，会冒泡到document对象后，再处理React事件
  - React所有事件都挂载在document对象上

- 最后真正执行 document 上挂载的事件

## 错误处理

当渲染过程，生命周期，或子组件的构造函数中抛出错误时，会调用如下方法：

抛出错误后，请使用 static getDerivedStateFromError（）渲染备用 UI，使用 componentDidCatch（）打印错误信息

- try,catch

- static getDerivedStateFromError()
- componentDidCatch()

```
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }


  static getDerivedStateFromError(error) {
    // 更新 state 使下-次渲染能够显示降级后的UI
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // 你同样可以将错误日志上报给服务器
    logErrorToMyService(error, errorInfo);
  }
  render() {

    if (this.state.hasError) {
      // 你可以自定义降级后的  UI 并渲染
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children;
  }
}	
```



# 组件

## 命名规则

组件的首字母必须是大写

React在解析所有的标签的时候，是以标签的首字母来区分的，如果标签的首字母是小写，那么就按照 普通的 HTML 标签来解析，如果 首字母是大写，则按照 组件的形式去解析渲染

## 状态组件

### 状态组件对比

使用 function 创建的组件，叫做【无状态组件】；使用 class 创建的组件，叫做【有状态组件】

有状态组件和无状态组件，最本质的区别：

- 状态管理：有无 state 状态属性；在 hooks 出来之前，函数组件就是无状态组件，不能保管组件的状态，不像类组件中调用 setState。如果想要管理 state 状态，可以使用 useState
  - 使用 function 构造函数创建的组件，内部没有 state 私有数据，只有一个props来接收外界传递过来的数据

  - 使用 class创建的组件，内部，除了有 this.props 这个只读属性之外，还有一个 专门用于存放自己私有数据的this.state 属性，这个 state 是可读可写的！

- 生命周期：class 创建的组件，有自己的生命周期函数，但是，function 创建的 组件，没有自己的生命周期函数；

问题来了：什么时候使用 有状态组件，什么时候使用无状态组件呢？？？

1. 如果一个组件需要存放自己的私有数据，或者需要在组件的不同阶段执行不同的业务逻辑，此时，非常适合用 class 创建出来的有状态组件；

   2. 如果一个组件，只需要根据外界传递过来的 props，渲染固定的页面结构就完事儿了，此时，非常适合使用 function 创建出来的 无状态组件；（使用无状态组件的小小好处： 由于剔除了组件的生命周期，所以，运行速度会相对快一丢丢）

### class组件

**用class关键字创建出来的组件：“有状态组件”**

```js
// 使用 class 创建的类，通过 extends 关键字，继承了 React.Component 之后，这个类，就是一个组件的模板了
// 如果想要引用这个组件，可以把 类的名称， 以标签形式，导入到 JSX 中使用
export default class Hello2 extends React.Component {
  constructor(props) {
    // 注意： 如果使用 extends 实现了继承，那么在 constructor 的第一行，一定要显示调用一下 super()
    //  super() 表示父类的构造函数
    super(props)
    // 在 constructor 中，如果想要访问 props 属性，不能直接使用 this.props， 而是需要在 constructor 的构造器参数列表中，显示的定义 props 参数来接收，才能正常使用；
    // 注意： 这是固定写法，this.state 表示 当前组件实例的私有数据对象，就好比 vue 中，组件实例身上的 data(){ return {} } 函数  
    this.state = {
      msg: '这是 Hello2 组件的私有msg数据',
      info: '瓦塔西***'
    }
  }
  // 保存信息1： No `render` method found on the returned component instance: you may have forgotten to define `render`.
  // 通过分析以上报错，发现，提示我们说，在 class 实现的组件内部，必须定义一个 render 函数
  render() {
    // 报错信息2： Nothing was returned from render. This usually means a return statement is missing. Or, to render nothing, return null.
    // 通过分析以上报错，发现，在 render 函数中，还必须 return 一个东西，如果没有什么需要被return 的，则需要 return null

    // 虽然在 React dev tools 中，并没有显示说 class 组件中的 props 是只读的，但是，经过测试得知，其实 只要是 组件的 props，都是只读的；
    // this.props.address = '123'
    return <div>
      <h1>这是 使用 class 类创建的组件</h1>
      <h3>外界传递过来的数据是： {this.props.address} --- {this.props.info}</h3>
      <h5>{this.state.msg}</h5>

      //React中，提供的事件绑定机制，使用的 都是驼峰命名
      //     在为 React 事件绑定 处理函数的时候，需要通过 this.函数名， 来把 函数的引用交给 事件 
      <input type="button" value="修改 msg" id="btnChangeMsg" onClick={this.changeMsg} />
      <br />
    </div>
  }

  changeMsg = () => {
    // 注意： 这里不是传统网页，所以 React 已经帮我们规定死了，在 方法中，默认this 指向 undefined，并不是指向方法的调用者

    // 直接使用 this.state.msg = '123' 为 state 上的数据重新赋值，可以修改 state 中的数据值，但是，页面不会被更新；
    // 所以这种方式，React 不推荐，以后尽量少用；
    // 如果要为 this.state 上的数据重新赋值，那么，React 推荐使用 this.setState({配置对象}) 来重新为 state 赋值
    // 注意： this.setState 方法，只会重新覆盖那些 显示定义的属性值，如果没有提供最全的属性，则没有提供的属性值，不会被覆盖；
    /* this.setState({
      msg: '123'
    }) */

    // this.setState 方法，也支持传递一个 function，如果传递的是 function，则在 function 内部，必须return 一个 对象；
    // 在 function 的参数中，支持传递两个参数，其中，第一个参数是 prevState，表示为修改之前的 老的 state 数据
    // 第二个参数，是 外界传递给当前组件的 props 数据
    this.setState(function (prevState, props) {
      return {
        msg: '123'
      }
    }, function () {
      // 由于 this.setState 是异步执行的，所以，如果想要立即拿到最新的修改结果，最保险的方式， 在回调函数中去操作最新的数据
      console.log(this.state.msg)
    })
  }
}
```

### 函数组件

函数/无状态组件是一个纯函数，它可接受接受参数，并返回react元素。这些都是**没有任何副作用的纯函数**。这些组件没有状态或生命周期方法

```js
// 组件的首字母必须是大写
function Hello(props) {
  // 在组件中，如果想要使用外部传递过来的数据，必须，显示的在 构造函数参数列表中，定义 props 属性来接收；
  // 通过 props 得到的任何数据都是只读的，不能从新赋值
  return <div>
    <h1>这是在Hello组件中定义的元素 --- {props.name}</h1>
  </div>
}
```



## 组件渲染

### 渲染条件

#### 初始渲染

当组件首次被挂载到DOM中时，`render`方法会被调用来生成初始的UI结构，通常发生在`componentDidMount`生命周期方法之前。

#### props或state的变化

组件的props或state发生变化，React会重新调用`render`方法来根据新的props和state生成新的UI结构。

#### 上下文（Context）变化

```
const ThemeContext = createContext('light');

function ThemedButton() {
  const theme = useContext(ThemeContext);
  // 当 Context 值变化时重新渲染
}
```



#### 父组件重新渲染

即使子组件的 props 没有变化，当父组件重新渲染时，默认情况下子组件也会重新渲染。

#### 强制更新

- 在React类组件中，你可以通过调用这个方法，强制重渲染一个组件：

  this.forceUpdate();

- **使用React Hooks时的变化**在React函数组件中，`forceUpdate`函数是无法使用的。你可以使用如下方式强制更新组件，并且不更改组件的state：

  ```
  const [state, updateState] = React.useState();
  const forceUpdate = React.useCallback(() => updateState({}), []);
  ```

  

### 判断更新和渲染优化

#### shouldComponentUpdate (用于类组件)

https://blog.csdn.net/deng1456694385/article/details/88746797

```jsx
class demo extent Component {
    state = {
        name: ''
    }
    componentDidMount() {
        this.setState({name: ''})
    }
    render() {
        console.log('render')
        return <div>haha</div>
    }
}
```

上面的组件会在`this.setState`调用后就会重新传染一次,但是我们可以看出`name`状态并没有没被我们用到,也没有改变,这种渲染就是无效渲染,所以为了优化我们通常会使用钩子函数`shouldComponentUpdate`来做一些逻辑判断,来确定是否要重新`render`一次

```jsx
class demo extent Component {    
    state = {        
        name: ''    
    }
    componentDidMount() {	
        this.setState({name: ''})
    }
    shouldComponentUpdate(nextProps,nextState) {    
        if(this.state.name === nextState.name) {        
            return false    
        }else {        
            return true    
        }
    }
	render <div>haha</div>
}
```

这样就可以避免无效渲染,优化性能,但是如果这种判断逻辑多到一定程度,光判断逻辑就很复杂,而且每次都要判断也会影响性能,所以才有了 `PureComponent`

#### PureComponent(用于类组件)

当使用component时，父组件的state或prop更新时，无论子组件的state、prop是否更新，都会触发子组件的更新，这会形成很多没必要的render，浪费很多性能；pureComponent的优点在于：pureComponent在shouldComponentUpdate只进行浅层的比较，只要外层对象没变化，就不会触发render,减少了不必要的render，当遇到复杂数据结构时，可以将一个组件拆分成多个pureComponent，以这种方式来实现复杂数据结构

```
import React, { PureComponent } from 'react'
export default class List extends PureComponent {
    render() {
        console.log('list render')
        return(
            <div>{this.props.list.title}</div>
        )
    }
}

```



##### 浅比较

```
if (this._compositeType === CompositeTypes.PureClass) {
	shouldUpdate = !shallowEqual(prevProps, nextProps) || ! shallowEqual(in st.state, nextState);
}
```

浅比较通过一个`shallowEqual`函数来完成：

```js
function is(x, y) {
  if (x === y) {
    return x !== 0 || y !== 0 || 1 / x === 1 / y;
  } else {
    return x !== x && y !== y;
  }
}
function shallowEqual(objA: mixed, objB: mixed): boolean {
  // 首先对基本数据类型的比较
  // !! 若是同引用便会返回 true
  //其中is函数是自己实现的一个Object.is的功能，排除了===两种不符合预期的情况：
  // +0 === -0  // true
  // NaN === NaN // false
  if (is(objA, objB)) {
    return true;
  }
  // 只有一种情况是误判的，那就是object,所以在判断两个对象都不是object
  // 之后，就可以返回false了
  if (typeof objA !== 'object' || objA === null || typeof objB !== 'object' || objB === null) {
    return false;
  }
  // 过滤掉基本数据类型之后，就是对对象的比较了
  const keysA = Object.keys(objA);
  const keysB = Object.keys(objB);

  // 首先拿出key值，对key的长度进行对比
  if (keysA.length !== keysB.length) {
    return false;
  }

  // key相等的情况下，在去循环比较
  for (let i = 0; i < keysA.length; i++) {
    // key值相等的时候
    // 借用原型链上真正的 hasOwnProperty 方法，判断ObjB里面是否有A的key的key值
    // 属性的顺序不影响结果也就是{name:'daisy', age:'24'} 跟{age:'24'，name:'daisy' }是一样的
    // 最后，对对象的value进行一个基本数据类型的比较，返回结果
    if (!hasOwnProperty.call(objB, keysA[i]) || !is(objA[keysA[i]], objB[keysA[i]])) {
      return false;
    }
  }
  return true;
}
```



#### memo (用于函数组件)

**针对函数组件的**,减少组件的不必要更新。 `React.memo` 仅检查 props 变更。如果函数组件被 `React.memo` 包裹，且其实现中拥有 [`useState`](https://zh-hans.reactjs.org/docs/hooks-state.html)，[`useReducer`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usereducer) 或 [`useContext`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usecontext) 的 Hook，当 state 或 context 发生变化时，它仍会重新渲染。 

```js
const TextCell = memo(function(props:any) {
  console.log('我重新渲染了')
  return (
    <p onClick={props.click}>ffff</p>
  )
})

//父组件
const fatherComponent = () => {
const [number,setNumber] = useState(0);
 return(
    <div>
      模块{number}
      <TextCell/>
      <Button onClick={()=>setNumber(number => number + 1)}>加加加</Button>
    </div>
  )
}
```

在这里如果没有用到memo 每次父组件重新setNumber,子组件都会重新渲染一次,加上了后**只会在初始化的时候渲染(useMemo会在页面初始化的时候执行一次,并把执行的结果缓存一份)**,减少了子组件渲染的次数

默认情况下其只会对复杂对象做**浅层对比**，如果你想要控制对比过程，那么请将自定义的比较函数通过第二个参数传入来实现。 

```jsx
function MyComponent(props) {
  /* 使用 props 渲染 */
}
function areEqual(prevProps, nextProps) {
  /*
  如果把 nextProps 传入 render 方法的返回结果与
  将 prevProps 传入 render 方法的返回结果一致则返回 true，
  否则返回 false
  */
}
export default React.memo(MyComponent, areEqual);
```

#### useMemo 和 useCallback (优化性能)

当父组件更新时，即使子组件使用`React.memo`进行了优化，**如果传递给子组件的函数props每次都是新创建的，子组件仍然会重新渲染**。

核心问题：函数引用的变化

解决：使用`useCallback`缓存函数

在JavaScript中，函数是引用类型。每次父组件重新渲染时，如果直接在渲染函数中定义函数，会创建一个全新的函数引用：

```
function Parent() {
  const [count, setCount] = useState(0);
  
  // 每次Parent渲染都会创建新的handleClick函数
  const handleClick = () => {
    console.log('Clicked');
  };
  
  return (
    <div>
      <button onClick={() => setCount(c => c + 1)}>Count: {count}</button>
      <Child onClick={handleClick} /> {/* 每次都会重新渲染 */}
    </div>
  );
}

const Child = React.memo(function Child({ onClick }) {
  console.log('Child渲染了'); // 每次Parent更新都会打印
  return <button onClick={onClick}>Click Me</button>;
});
```



## 内置组件

### Fragment

无论是函数组件还是类组件，return 的 React 元素的语法必须是由一个标签包裹起来的所有虚拟 DOM 内容

一种是使用一个 div 标签将其包裹起来，另外一种方式就是使用 React 提供的 `<React.Fragment>` 将其包裹起来。但是我们不期望，增加额外的`dom`节点，所以`react`提供`Fragment`碎片概念，能够让一个组件返回多个元素。

```
render() {
  return (
    <React.Fragment>
      Some text.
      <h2>A heading</h2>
    </React.Fragment>
  );
}
```

### Suspense

允许在子组件完成加载前展示后备方案。

```
<Suspense fallback={<Loading />}>
  <SomeComponent />
</Suspense>
```

## 组件通信

### 父组件->子组件

父组件将需要传递的参数通过`key={xxx}`方式传递至子组件，子组件通过`this.props.key`获取参数.

### 子组件->父组件

利用 props callback 通信，父组件传递一个 callback 到子组件，当事件触发时将参数放置到 callback 带回给父组件.

```react
// 父组件
import React from 'react'
import Son from './son'
class Father extends React.Component {
  constructor(props) {
    super(props)
  }
  state = {
    info: '',
  }
  callback = (value) => {
    // 此处的value便是子组件带回
    this.setState({
      info: value,
    })
  }
  render() {
    return (
      <div>
        <p>{this.state.info}</p>
        <Son callback={this.callback} />
      </div>
    )
  }
}
export default Father

// 子组件
import React from 'react'
interface IProps {
  callback: (string) => void
}
class Son extends React.Component<IProps> {
  constructor(props) {
    super(props)
    this.handleChange = this.handleChange.bind(this)
  }
  handleChange = (e) => {
    // 在此处将参数带回
    this.props.callback(e.target.value)
  }
  render() {
    return (
      <div>
        <input type='text' onChange={this.handleChange} />
      </div>
    )
  }
}
export default Son
```

### 后代组件Context

https://zh-hans.reactjs.org/docs/context.html

数据是通过 props 属性自上而下（由父及子）进行传递的 ，需要显式地通过组件树的逐层传递 props。Context 设计目的是为了共享那些对于一个组件树而言是“全局”的数据 

```jsx
// context.js
import React from 'react'
//创建一个 Context 对象，并暴露Consumer和Provide
const { Consumer, Provider } = React.createContext(null) 
export { Consumer, Provider }

//Father.js
import { Provider } from './context'
import React from 'react'
import Son from './son'
<Provider value={this.state.info}>
    <div>
        <p>{this.state.info}</p>
        <Son />
    </div>
</Provider>
    
//Son.js
import React from 'react'
import GrandSon from './grandson'
import { Consumer } from './context'
<Consumer>
{(info) => (
// 通过Consumer直接获取父组件的值
    <div>
    <p>父组件的值:{info}</p>
    <GrandSon />
    </div>
    )}
</Consumer>
```

当 Provider 的 `value` 值发生变化时，它内部的所有消费组件都会重新渲染。从 Provider 到其内部 consumer 组件（包括 [.contextType](https://zh-hans.reactjs.org/docs/context.html#classcontexttype) 和 [useContext](https://zh-hans.reactjs.org/docs/hooks-reference.html#usecontext)）的传播不受制于  `shouldComponentUpdate` 函数，因此当 consumer 组件在其祖先组件跳过更新的情况下也能更新。 

### 跨级

状态管理器

## 高阶函数与组件

高阶组件即`高阶函数`，前面我们讲到，React遵循函数式开发，而高阶组件这个概念其实是React社区繁衍出来的概念。把通用的逻辑放在高阶组件中，对组件实现一致的处理，从而实现代码的复用

在这里我们要谨记这一句话，**组件 = 函数**。高阶函数，通俗的讲，就是把函数当作参数，传入另外一个函数当中，再返回一个函数。

### 实际应用场景

#### 权限按钮

```react
import React, { FC } from 'react';
import { useAccess } from '../../../hooks/useAccess';
import { message } from 'antd';

/**
 * 权限高阶组件，使用示例：
 * 
 * import WithAccess from '@components/WithAccess';
 * 
 * const WithAccessBtn = WithAccess(你的组件, 可选'button' | 'menu' 默认为button);
 * 
 * <WithAccessBtn permission='permission' />
 * 
 * @param Comp 组件
 * @param type 鉴权类型 按钮：button，菜单：menu
 * @returns 
 */
const WithAccess = (Comp, type = 'button') => {
  const Access = props => {
    const { getPermission } = useAccess();
    const { permission, name, icon, onClick } = props;
    //showVisible是否展示, available是否有权限
    const { showVisible, available } = getPermission(permission, type) || {};
    let initProps = props
    console.log(props);
    const config = () => {
      if (available === 0) {
        return {
          onClick: () => {
            message.info('按钮没有权限')
          }
        }
      }
    }
    return showVisible ? <Comp {...initProps} {...config()}>{name}</Comp> : null;
  }

  return Access;
}

export default WithAccess;
```

使用高阶组件

```react
import React from "react";
import usePermissionModel from "../../hox/access";
import WithAccess from './components'
import { Button, message } from 'antd';
import { LaptopOutlined } from "@ant-design/icons";

const WithAccessBtnYes = WithAccess(Button)
const WithAccessBtnNo = WithAccess(Button)
export default function AHooks(props) {
  const { menus, set } = usePermissionModel();
  console.log(menus, set)
  return <div>
    <WithAccessBtnYes permission='account:authorization:yes' name='按钮' icon={<LaptopOutlined />} onClick={() => { message.success('按钮有权限') }}></WithAccessBtnYes>
    <WithAccessBtnNo permission='account:authorization:no' name='按钮' icon={<LaptopOutlined />} onClick={() => { message.success('按钮有权限') }}></WithAccessBtnNo>
  </div>;
}
```

# Hooks函数

http://www.ruanyifeng.com/blog/2019/09/react-hooks.html

- 纯函数组件**没有状态**
- 纯函数组件**没有生命周期**
- 纯函数组件没有`this`

这就注定，我们所推崇的函数组件，只能做UI展示的功能，涉及到状态的管理与切换，我们不得不用类组件或者redux，但我们知道类组件的也是有缺点的，比如，遇到简单的页面，你的代码会显得很重，并且每创建一个类组件，都要去继承一个React实例，至于Redux,更不用多说，很久之前Redux的作者就说过，“能用React解决的问题就不用Redux”,等等一系列的话。关于**React类组件r**edux的作者又有话说

> - 大型组件很难拆分和重构，也很难测试。
> - 业务逻辑分散在组件的各个方法之中，导致重复逻辑或关联逻辑。
> - 组件类引入了复杂的编程模式，比如 render props 和高阶组件。

**Hooks 优势**

1. 能优化类组件的三大问题
2. 能在无需修改组件结构的情况下复用状态逻辑（自定义 Hooks ）
3. 能将组件中相互关联的部分拆分成更小的函数（比如设置订阅或请求数据）

**React Hooks 的意思是，组件尽量写成纯函数，如果需要外部功能和副作用，就用钩子把外部代码"钩"进来。**而React Hooks 就是我们所说的“钩子”。

## useState():状态钩子

用于为函数组件引入状态（state）。纯函数不能有状态，所以把状态放在钩子里面。

### 使用

```
const [state, setState] = useState(initialState);
```

- `initialState`: 状态的初始值，可以是任何类型（数字、字符串、对象、数组等）
- `state`: 当前的状态值
- `setState`: 更新状态的函数

#### 更新状态的几种方式

1. **直接设置新值**:

   javascript

   ```
   setCount(10);
   ```

2. **基于前一个状态更新**:

   javascript

   ```
   setCount(prevCount => prevCount + 1);
   ```

3. **更新对象或数组**:

   javascript

   ```
   const [user, setUser] = useState({ name: 'John', age: 30 });
   
   // 更新对象时需要展开旧状态
   setUser(prevUser => ({ ...prevUser, age: 31 }));
   ```

#### mutation

在 React 中，"mutation"（突变）指的是直接修改数据或状态，而不是创建新的副本。理解这个概念对于正确管理 React 的状态非常重要。

Mutation 是指直接更改对象或数组的内容，而不是创建一个新的对象或数组。在 React 中，**直接突变状态是被禁止的**，因为这会导致不可预测的行为和性能问题。

**为什么 React 反对 Mutation**

1. **不可预测性**：直接突变可能导致组件不按预期重新渲染
2. **性能优化**：React 依赖不可变性来高效地确定何时需要重新渲染
3. **时间旅行调试**：像 Redux 这样的工具依赖不可变性来实现调试功能
4. **并发模式**：React 的未来功能依赖于状态更新的可预测性

**避免 Mutation 的工具**

1. **展开运算符 (`...`)**：用于对象和数组的浅拷贝

2. **数组方法**：`map`, `filter`, `concat`, `slice` 等不改变原数组的方法

3. **第三方库**：

   - Immer：简化不可变更新逻辑

     ```
     import produce from 'immer';
     
     const [user, setUser] = useState({
       name: 'Alice',
       profile: { age: 25 }
     });
     
     // 使用 Immer 可以"看似"直接修改，但实际上创建了新对象
     setUser(produce(draft => {
       draft.profile.age = 26;
     }));
     ```

     

   - Immutable.js：提供持久化数据结构

#### 更新 state 中的对象

state 中可以保存任意类型的 JavaScript 值，包括对象。但是，你不应该直接修改存放在 React state 中的对象。相反，当你想要更新一个对象时，**你需要创建一个新的对象（或者将其拷贝一份），然后将 state 更新为此对象。**



#### 更新数组

在 JavaScript 中，数组只是另一种对象。[同对象一样](https://react.docschina.org/learn/updating-objects-in-state)，**你需要将 React state 中的数组视为只读的**。这意味着你不应该使用类似于 `arr[0] = 'bird'` 这样的方式来重新分配数组中的元素，也不应该使用会直接修改原始数组的方法，例如 `push()` 和 `pop()`。

相反，每次要更新一个数组时，你需要把一个**新的数组**传入 state 的 setting 方法中。为此，你可以通过使用像 `filter()` 和 `map()` 这样不会直接修改原始值的方法，从原始数组生成一个新的数组。然后你就可以将 state 设置为这个新生成的数组。

| 避免使用 (会改变原始数组) | 推荐使用 (会返回一个新数组）  |                                                              |
| ------------------------- | ----------------------------- | ------------------------------------------------------------ |
| 添加元素                  | `push`，`unshift`             | `concat`，`[...arr]` 展开语法（[例子](https://react.docschina.org/learn/updating-arrays-in-state#adding-to-an-array)） |
| 删除元素                  | `pop`，`shift`，`splice`      | `filter`，`slice`（[例子](https://react.docschina.org/learn/updating-arrays-in-state#removing-from-an-array)） |
| 替换元素                  | `splice`，`arr[i] = ...` 赋值 | `map`（[例子](https://react.docschina.org/learn/updating-arrays-in-state#replacing-items-in-an-array)） |
| 排序                      | `reverse`，`sort`             | 先将数组复制一份（[例子](https://react.docschina.org/learn/updating-arrays-in-state#making-other-changes-to-an-array)） |

### 原理

```jsx
//useState模拟1.0
//因为每次调用myUseState时会重置state的值。经过改进，必须将state写在函数的外面。
let _state;
function myUseState(initialValue) {
  _state = _state===undefined? initialValue:_state;
  const setState = (newValue) => {
    _state = newValue; //更新state值，
    render(); //触发重新渲染
  };
  return [_state, setState];
}
/* 粗糙的渲染 */
const render = () => {
  ReactDOM.render(<App />, document.getElementById("root"));
};
// 使用myUseState
const App = () => {
  const [n, setN] = myUseState(0);
  return (
	  <div classNam="App">
		 <p>n:{n}</p>
		 <button onClick={()=>{setN(n+1)}}>n+1</button> 
	  </div>
	  );
};
ReactDOM.render(<App />, document.getElementById("root"));

```

```jsx
//一个组件用了两个useState怎么办？useState模拟2.0
let _state=[];
let index=0;
function myUseState(initialValue) {
  int currentIndex=index;	//引入中间变量currentIndex就是为了保存当前操作的下标index。
  _state[currentIndex] = _state[currentIndex]===undefined? initialValue:_state[currentIndex];
  const setState = (newValue) => {
    _state[currentIndex] = newValue; 
    render(); 
  };
  index+=1;// 每次更新完state值后，index值+1
  return [_state[currentIndex], setState];
}
const render = () => {
  index=0;	//重要的一步，必须在渲染前后将index值重置为0，不然index会一种增加1
  ReactDOM.render(<App />, document.getElementById("root"));
};
// 使用myUseState
const App = () => {
  const [n, setN] = myUseState(0);
  const [m, setM] = myUseState(0);
  return (
	  <div classNam="App">
		 <p>n:{n}</p>
		 <button onClick={()=>{setN(n+1)}}>n+1</button> 
		 <p>m:{m}</p>
		 <button onClick={()=>{setM(m+1)}}>n+1</button> 
	  </div>
	  );
};
ReactDOM.render(<App />, document.getElementById("root"));
```

- 在正常的react的事件流里（如onClick等）

  - setState和useState是**异步执行**的（不会立即更新state的结果，所以console数据没有更新）
    
  - 多次执行setState和useState，只会调用一次重新渲染render
  
  - 不同的是，setState会进行state的合并，而useState会进行state的覆盖
  
- 在setTimeout，Promise.then等异步事件中

  - setState和useState是**同步执行**的（立即更新state的结果，**react17之后还是会批处理**）

  - 多次执行setState和useState，每一次的执行setState和useState，都会调用一次render

### 批处理

batch批量处理：在每次执行 useState 的时候，组件都要重新 render 一次，会造成无效渲染，浪费时间（因为最后一次渲染会覆盖掉前面所有的渲染效果）。 所以 react 会把一些可以一起更新的 useState/setState 放在一起，只渲染一次。

在React16版本及以前，React 会对所有React内部触发的事件监听函数中的更新（比如onClick函数）做批处理，如果是绕过react组件，如addEventListenr，或者异步调用如异步请求或者setTimeout等，不会进行批处理。在React17版本及之后，React会对所有的更新做批处理。

**unstable_batchedUpdates手动批处理**

```jsx
  function handleClick3() {
    // 手动批处理
    setTimeout(() => {
      unstable_batchedUpdates(() => {
        setCount1(count1 + 1);
        console.log(count1);
        setFlag((f) => !f);
      });
    }, 10);
    // React 只会在最后重新渲染一次（这是批处理！）
  }
```

### Tip

- react中useState更新了组件，但是页面上的组件没有刷新

  原因：useState更新的数据，是一个多层次的数据，[react](https://so.csdn.net/so/search?q=react)监听的时候，是浅层监听(默认开启 类 Object.is 的浅层比较，所以指向的地址不变)，所以不一定及时刷新页面

  解决办法:深拷贝，把需要更新的数据深拷贝一份，再使用useState 存储，就能实现每次都及时更新页面

## useContext():共享状态钩子

如果需要在层层组件之间共享状态，可以使用`useContext()`。

第一步就是使用 React Context API，在组件外部建立一个 Context。

```javascript
 const AppContext = React.createContext({});
```

组件封装代码如下。

```
// 使用一个 Provider 来将当前的 theme 传递给以下的组件树。
// 无论多深，任何组件都能读取这个值。
<AppContext.Provider value={{
username: 'superawesome'
}}>
<div className="App">
<Navbar/>
<Messages/>
</div>
</AppContext.Provider>
```

上面代码中，`AppContext.Provider`提供了一个 Context 对象，这个对象可以被子组件共享。

Navbar 组件的代码如下。

```
const Navbar = () => {
const { username } = useContext(AppContext);
return (
 <div className="navbar">
   <p>AwesomeSite</p>
   <p>{username}</p>
 </div>
);
}
```



### **在传递对象和函数时优化重新渲染** 

你可以通过 context 传递任何值，包括对象和函数。

```
function MyApp() {
  const [currentUser, setCurrentUser] = useState(null);
  function login(response) {
    storeCredentials(response.credentials);
    setCurrentUser(response.user);
  }
  return (
    <AuthContext.Provider value={{ currentUser, login }}>
      <Page />
    </AuthContext.Provider>
  );
}
```

此处，context value 是一个具有两个属性的 JavaScript 对象，其中一个是函数。每当 `MyApp` 出现重新渲染（例如，路由更新）时，这里将会是一个 **不同的** 对象指向 **不同的** 函数，因此 React 还必须重新渲染树中调用 `useContext(AuthContext)` 的所有组件。

在较小的应用程序中，这不是问题。但是，如果基础数据如 `currentUser` 没有更改，则不需要重新渲染它们。为了帮助 React 利用这一点，你可以使用 [`useCallback`](https://reactjs.p2hp.com/reference/react/useCallback) 包装 `login` 函数，并将对象创建包装到 [`useMemo`](https://reactjs.p2hp.com/reference/react/useMemo) 中。这是一个性能优化的例子：

```
import { useCallback, useMemo } from 'react';



function MyApp() {

  const [currentUser, setCurrentUser] = useState(null);



  const login = useCallback((response) => {

    storeCredentials(response.credentials);

    setCurrentUser(response.user);

  }, []);



  const contextValue = useMemo(() => ({

    currentUser,

    login

  }), [currentUser, login]);



  return (

    <AuthContext.Provider value={contextValue}>

      <Page />

    </AuthContext.Provider>

  );

}
```

根据以上改变，即使 `MyApp` 需要重新渲染，调用 `useContext(AuthContext)` 的组件也不需要重新渲染，除非 `currentUser` 发生了变化。

## useReducer()钩子

**useReducer适用于引用类型，而useState适合值类型**

```
const [state, dispatch] = useReducer(reducer, initialArg, init)
```

- useReducer 接收三个参数，**第一个参数为一个 reducer 函数，第二个参数是reducer的初始值，第三个参数为可选参数，值为一个函数，可以用来惰性提供初始状态。**

  reducer 接受两个参数一个是 state 另一个是 action ，用法原理和 redux 中的 reducer 一致

- useReducer 返回一个数组，数组中包含一个 state 和 dispath，state 是返回状态中的值，而 dispatch 是一个可以发布事件来更新 state 的函数。

### 原理

**useReucer 也是 useState 的内部实现**

```jsx
let memoizedState
function useReducer(reducer, initialArg, init) {
    let initState = void 0
    if (typeof init === 'function') {
        initState = init(initialArg)
    } else {
        initState = initialArg
    }
    function dispatch(action) {
        memoizedState = reducer(memoizedState, action)
        // React的渲染
        // render()
    }
    memoizedState = memoizedState || initState
    return [memoizedState, dispatch]
}

function useState(initState) {
    return useReducer((oldState, newState) => {
        if (typeof newState === 'function') {
            return newState(oldState)
        }
        return newState
    }, initState)
}

```

### 实例

```js
const initialState = {count: 0};

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}

```

## useEffect()：副作用钩子

纯函数只能进行数据计算，那些不涉及计算的操作（比如ajax 请求、访问原生dom 元素、本地持久化缓存、绑定/解绑事件、添加订阅、设置定时器、记录日志）应该写在哪里呢？

**`useEffect()`的用法如下：**

参数 

- `setup`：处理 Effect 的函数。setup 函数选择性返回一个 **清理（cleanup）** 函数。当组件被添加到 DOM 的时候，React 将运行 setup 函数。在每次依赖项变更重新渲染后，React 将首先**使用旧值运行 cleanup 函数**（如果你提供了该函数），然后**使用新值运行 setup 函数**。在组件从 DOM 中移除后，React 将最后一次运行 cleanup 函数。
  - cleanup 主要功能
    1. **清理副作用：在组件卸载或依赖项变化导致 effect 重新执行前，清除上一次 effect 产生的副作用**
    2. **防止内存泄漏**：取消未完成的异步操作，避免组件卸载后仍执行状态更新
    3. **取消订阅**：移除事件监听器、定时器等
- **可选** `dependencies`：`setup` 代码中引用的所有响应式值的列表。响应式值包括 props、state 以及所有直接在组件内部声明的变量和函数。如果你的代码检查工具 [配置了 React](https://reactjs.p2hp.com/learn/editor-setup#linting)，那么它将验证是否每个响应式值都被正确地指定为一个依赖项。依赖项列表的元素数量必须是固定的，并且必须像 `[dep1, dep2, dep3]` 这样内联编写。React 将使用 [`Object.is`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/is) 来比较每个依赖项和它先前的值。如果省略此参数，则在每次重新渲染组件之后，将重新运行 Effect 函数。如果你想了解更多，请参见 [传递依赖数组、空数组和不传递依赖项之间的区别](https://reactjs.p2hp.com/reference/react/useEffect#examples-dependencies)。

```
useEffect(()  =>  {
 // Async Action
 //return 则是在页面被卸载时调用.返回一个函数来指定如何“清除”副作用
 return fn;
}, [dependencies])
```

`useEffect()`接受两个参数。

- 第一个参数是一个函数，异步操作的代码放在里面。
- 第二个参数是一个数组，用于给出 Effect 的依赖项，只要这个数组发生变化，`useEffect()`就会执行。第二个参数可以省略，这时每次组件渲染时，就会执行`useEffect()`。

**它的常见用途有下面几种：**

- 获取数据（data fetching）
- 事件监听或订阅（setting up a subscription）
- 改变 DOM（changing the DOM）
- 输出日志（logging）

**tips**

- **它在第一次渲染之后*和*每次更新之后都会执行**

- 使用`useEffect()`时，有一点需要注意。如果有多个副效应，应该调用多个`useEffect()`，而不应该合并写在一起。

- 在useEffect中，不仅会请求后端的数据，还会通过调用setData来更新本地的状态，这样会触发view的更新。

  但是，运行这个程序的时候，会出现无限循环的情况。useEffect在组件**mount**时执行，但也会在组件**更新**时执行。因为我们在每次请求数据之后都会设置本地的状态，所以组件会更新，因此useEffect会再次执行，因此出现了无限循环的情况。**我们只想在组件mount时请求数据。**我们可以传递一个空数组作为useEffect的第二个参数，这样就能避免在组件更新执行useEffect，只会在组件mount时执行。

  ```js
  import React, { useState, useEffect } from 'react';
  import axios from 'axios';
  
  function App() {
    const [data, setData] = useState({ hits: [] });
  
    useEffect(async () => {
      const result = await axios(
        'http://localhost/api/v1/search?query=redux',
      );
  
      setData(result.data);
    }, []);
  
    return (
      <ul>
        {data.hits.map(item => (
          <li key={item.objectID}>
            <a href={item.url}>{item.title}</a>
          </li>
        ))}
      </ul>
    );
  }
  
  export default App;
  ```

  升级加载loading 

  ```js
  import React, { Fragment, useState, useEffect } from 'react';
  import axios from 'axios';
  
  function App() {
    const [data, setData] = useState({ hits: [] });
    const [query, setQuery] = useState('redux');
    const [url, setUrl] = useState(
      'http://hn.algolia.com/api/v1/search?query=redux',
    );
    const [isLoading, setIsLoading] = useState(false);
  
    useEffect(() => {
      const fetchData = async () => {
        setIsLoading(true);
  
        const result = await axios(url);
  
        setData(result.data);
        setIsLoading(false);
      };
  
      fetchData();
    }, [url]);
    return (
      <Fragment>
        <input
          type="text"
          value={query}
          onChange={event => setQuery(event.target.value)}
        />
        <button
          type="button"
          onClick={() =>
            setUrl(`http://localhost/api/v1/search?query=${query}`)
          }
        >
          Search
        </button>
  
        {isLoading ? (
          <div>Loading ...</div>
        ) : (
          <ul>
            {data.hits.map(item => (
              <li key={item.objectID}>
                <a href={item.url}>{item.title}</a>
              </li>
            ))}
          </ul>
        )}
      </Fragment>
    );
  }
  
  export default App;
  ```



## uselayoutEffect

https://juejin.cn/post/7462618506350641161

`useEffect` 和 `useLayoutEffect` 是 React 中用于处理副作用的两个 Hook，它们的主要区别在于**执行时机**和**使用场景**。理解它们的区别对于优化性能和避免 UI 问题非常重要。

------

1. **共同点**

- 两者都用于在函数组件中执行副作用操作（如数据获取、DOM 操作、订阅等）。
- 两者的 API 完全相同，接收两个参数：
  - 一个副作用函数。
  - 一个依赖项数组（可选）。

**区别**

1. 执行时机

- **`useEffect`**：
  - 副作用函数在浏览器完成**渲染之后异步执行**。
  - 不会阻塞浏览器的渲染过程。
  - 适合大多数副作用操作，尤其是那些不需要立即更新 DOM 的场景。
- **`useLayoutEffect`**：
  - 副作用函数在浏览器完成**渲染之前同步执行**。
  - 会阻塞浏览器的渲染过程，直到副作用函数执行完毕。
  - 适合需要**同步更新 DOM** 的场景，例如在渲染之前测量 DOM 元素或更新布局。

2. 使用场景

- **`useEffect`**：
  - 数据获取（如调用 API）。
  - 订阅事件。
  - 不需要立即更新 DOM 的操作。
- **`useLayoutEffect`**：
  - 需要同步更新 DOM 的操作（如调整元素尺寸或位置）。
  - 在渲染之前测量 DOM 元素。
  - 避免 UI 闪烁（例如，在渲染之前更新样式）。

**性能影响**：

- `useLayoutEffect` 是同步执行的，可能会阻塞浏览器的渲染，导致性能问题。除非必要，否则应优先使用 `useEffect`。

**服务端渲染（SSR）**：

- 在服务端渲染时，`useLayoutEffect` 不会执行，因为此时没有 DOM。如果需要在 SSR 中使用，可以考虑使用 `useEffect` 或在 `useLayoutEffect` 中添加条件判断。

**避免 UI 闪烁**：

- 如果某些操作（如更新样式）在 `useEffect` 中执行会导致 UI 闪烁，可以尝试将其移到 `useLayoutEffect` 中。

## useCallback和useMemo

https://www.xiaye0.com/?p=113

https://www.jianshu.com/p/014ee0ebe959

**useCallback和useMemo**都会在组件第一次渲染的时候执行，之后会在其**依赖的变量**发生改变时再次执行；并且这两个hooks都返回**缓存**，**useMemo返回缓存的变量，useCallback返回缓存的函数。**

```js
type DependencyList = ReadonlyArray<any>;

function useCallback<T extends (...args: any[]) => any>(callback: T, deps: DependencyList): T;

function useMemo<T>(factory: () => T, deps: DependencyList | undefined): T;
```

> **React 中当组件的 props 或 state 变化时，会重新渲染视图**

### useCallback

父组件给子组件传递属性（**函数**），父组件重新渲染，会重新创建函数，对应函数地址改变，即传给子组件的属性发生了变化，导致子组件渲染。

useCallback参数 

- `fn`：想要缓存的函数。此函数可以接受任何参数并且返回任何值。React 将会在初次渲染而非调用时返回该函数。当进行下一次渲染时，如果 `dependencies` 相比于上一次渲染时没有改变，那么 React 将会返回相同的函数。否则，React 将返回在最新一次渲染中传入的函数，并且将其缓存以便之后使用。React 不会调用此函数，而是返回此函数。你可以自己决定何时调用以及是否调用。
- `dependencies`：有关是否更新 `fn` 的所有响应式值的一个列表。响应式值包括 props、state，和所有在你组件内部直接声明的变量和函数。如果你的代码检查工具 [配置了 React](https://reactjs.p2hp.com/learn/editor-setup#linting)，那么它将校验每一个正确指定为依赖的响应式值。依赖列表必须具有确切数量的项，并且必须像 `[dep1, dep2, dep3]` 这样编写。React 使用 [`Object.is`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/is) 比较每一个依赖和它的之前的值。

```js
const TextCell = memo(function(props:any) {
  console.log('我重新渲染了')
  return (
    <p onClick={props.click}>ffff</p>
  )
})

//父组件
const fatherComponent = () => {
const [number,setNumber] = useState(0);

const handleClick = useCallback(()=>{
   console.log(33)
},[])
 return(
    <div>
      模块{number}
      <TextCell click={handleClick}/>

      <Button onClick={()=>setNumber(number => number + 1)}>加加加</Button>
    </div>
  )
}
```

这里如果不使用useCallback,哪怕子组件用memo包裹了 也还是会更新子组件,因为子组件的绑定的函数click在父组件更新的时候也会更新**引用地址**,导致子组件的更新,但是这个其实是没必要的更新,绑定的函数并不需要子组件更新,useCallback就是阻止这类没必要的更新而存在的

这里需要注意的是 如果是有参数需要传递,则需要这样写

```js
<TextCell click={useCallback(()=>handleClick(‘传递的参数’),[])}/>
```

**作用**

- 防止死循环

  ```jsx
  // 用于记录 getData 调用次数
  let count = 0;
  
  function App() {
    const [val, setVal] = useState("");
  
    function getData() {
      setTimeout(() => {
        setVal("new data " + count);
        count++;
      }, 500);
    }
    return <Child val={val} getData={getData} />;
  }
  
  function Child({val, getData}) {
    useEffect(() => {
      getData();
    }, [getData]);
  
    return <div>{val}</div>;
  }
  ```

  执行过程：

  1. `App`渲染`Child`，将`val`和`getData`传进去
  2. `Child`使用`useEffect`获取数据。因为对`getData`有依赖，于是将其加入依赖列表
  3. `getData`执行时，调用`setVal`，导致`App`重新渲染
  4. `App`重新渲染时生成新的`getData`方法，传给`Child`
  5. `Child`发现`getData`的引用变了，又会执行`getData`
  6. 3 -> 5 是一个死循环

### useMemo

> 类似于vue的computed

## 调试

useEventListener

如果你发现自己使用useEffect添加了许多事件监听，那你可能需要考虑将这些逻辑封装成一个通用的hook。

useWhyDidYouUpdate

这个hook让你更加容易观察到是哪一个prop的改变导致了一个组件的重新渲染。

useLockBodyScroll

有时候当一些特别的组件在你们的页面中展示时，你想要阻止用户滑动你的页面（想一想modal框或者移动端的全屏菜单）。

# 状态管理器



## 区别

https://blog.csdn.net/weixin_45644335/article/details/138888155

Redux

- **优势**:
  - 一致的状态管理模式，有利于代码的可维护性和可预测性。
  - 方便状态共享和同步，使得大型应用中的状态管理变得容易。
- **劣势**:
  - 可能会产生较多的样板代码。
  - 状态的更新通常需要遵循一定的流程，可能导致更新流程变得复杂。
- **适用场景**:
  - 大型应用或需求强一致性的应用。

hox

- 特点：

  - 基于 React Hooks：Hox 的状态管理是基于 React Hooks 的，与 React 的编程模型高度一致。


  - 简洁：Hox 的 API 非常简洁，只需要了解几个函数和概念就可以开始使用。

  - 轻量：Hox 的代码量非常小，对项目的影响较小。

- 优点：

  - 易于上手：Hox 的 API 简单直观，学习曲线平缓。

  - 与 React 集成：Hox 的 API 设计和 React Hooks 非常相似，可以与 React 无缝集成。

  - 状态共享：Hox 支持跨组件共享状态，可以轻松地管理全局状态。

- 缺点：

  - 社区和生态：相较于一些成熟的状态管理库（如 Redux、MobX），Hox 的社区和生态相对较小。

  - 不适用于非 React 项目：Hox 是专为 React 设计的，无法在非 React 项目中使用。



## Context+useReducer

useReducer 本身是组件级别的状态管理，但通过 React Context 可以将其提升为全局状态管理。

### 1. 创建全局状态上下文

javascript

```
import React, { createContext, useContext, useReducer } from 'react';

// 初始状态
const initialState = {
  user: null,
  theme: 'light',
  cart: [],
  notifications: []
};

// reducer 函数
function appReducer(state, action) {
  switch (action.type) {
    case 'SET_USER':
      return { ...state, user: action.payload };
    case 'SET_THEME':
      return { ...state, theme: action.payload };
    case 'ADD_TO_CART':
      return { ...state, cart: [...state.cart, action.payload] };
    case 'REMOVE_FROM_CART':
      return { ...state, cart: state.cart.filter(item => item.id !== action.payload) };
    case 'ADD_NOTIFICATION':
      return { ...state, notifications: [...state.notifications, action.payload] };
    case 'CLEAR_NOTIFICATION':
      return { ...state, notifications: state.notifications.filter(n => n.id !== action.payload) };
    default:
      return state;
  }
}

// 创建 Context
const AppStateContext = createContext();
const AppDispatchContext = createContext();

// Provider 组件
export function AppProvider({ children }) {
  const [state, dispatch] = useReducer(appReducer, initialState);

  return (
    <AppStateContext.Provider value={state}>
      <AppDispatchContext.Provider value={dispatch}>
        {children}
      </AppDispatchContext.Provider>
    </AppStateContext.Provider>
  );
}

// 自定义 Hook - 获取状态
export function useAppState() {
  const context = useContext(AppStateContext);
  if (!context) {
    throw new Error('useAppState must be used within AppProvider');
  }
  return context;
}

// 自定义 Hook - 获取 dispatch
export function useAppDispatch() {
  const context = useContext(AppDispatchContext);
  if (!context) {
    throw new Error('useAppDispatch must be used within AppProvider');
  }
  return context;
}
```

### 2. 在应用顶层使用 Provider

javascript

```
import React from 'react';
import ReactDOM from 'react-dom';
import { AppProvider } from './AppState';
import App from './App';

ReactDOM.render(
  <React.StrictMode>
    <AppProvider>
      <App />
    </AppProvider>
  </React.StrictMode>,
  document.getElementById('root')
);
```

### 3. 在组件中使用全局状态

javascript

```
import React from 'react';
import { useAppState, useAppDispatch } from './AppState';

function Header() {
  const { user, theme } = useAppState();
  const dispatch = useAppDispatch();

  const toggleTheme = () => {
    dispatch({
      type: 'SET_THEME',
      payload: theme === 'light' ? 'dark' : 'light'
    });
  };

  return (
    <header style={{ background: theme === 'light' ? '#fff' : '#333' }}>
      <h1>Welcome, {user?.name || 'Guest'}</h1>
      <button onClick={toggleTheme}>
        Switch to {theme === 'light' ? 'Dark' : 'Light'} Mode
      </button>
    </header>
  );
}

function Cart() {
  const { cart } = useAppState();
  const dispatch = useAppDispatch();

  const removeFromCart = (productId) => {
    dispatch({ type: 'REMOVE_FROM_CART', payload: productId });
  };

  return (
    <div>
      <h2>Shopping Cart ({cart.length} items)</h2>
      {cart.map(item => (
        <div key={item.id}>
          {item.name} - ${item.price}
          <button onClick={() => removeFromCart(item.id)}>Remove</button>
        </div>
      ))}
    </div>
  );
}
```

### 与专业状态库的对比

#### useReducer + Context 的优势：

javascript

```
// 1. 零依赖，React 内置
// 2. 学习成本低
// 3. 适合中小型应用
// 4. 类型安全（配合 TypeScript）
```

#### 局限性：

javascript

```
// 1. 性能问题：任何状态变化都会导致所有订阅组件重渲染
// 2. 缺乏中间件生态系统
// 3. 调试工具支持有限
// 4. 异步处理需要手动实现
```

### 何时选择 useReducer + Context

#### 适合的场景：

javascript

```
// 中小型应用
// 状态结构相对简单
// 不需要复杂的时间旅行调试
// 团队已经熟悉 React Hooks
```

#### 不适合的场景：

javascript

```
// 大型复杂应用
// 需要高性能优化
// 需要丰富的中间件支持
// 需要高级调试功能
```

## Redux

Redux 是最传统的状态管理库，强调单一数据源、不可变数据和纯函数更新。使用`dispatch`来触发action，通过reducers处理action并返回新的state。[React-Redux](https://so.csdn.net/so/search?q=React-Redux&spm=1001.2101.3001.7020)提供了与React集成的桥梁。



<img src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/12/15/16f09a0b5196a2dd~tplv-t2oaga2asx-watermark.awebp" alt="img" style="zoom:50%;" />



### 三大原则

#### 单一数据源

**整个应用的state被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个store 中。**

#### State 是只读的

**唯一改变 state 的方法就是触发 action，action 是一个用于描述已发生事件的普通对象。**

这样确保了视图和网络请求都不能直接修改 state，相反它们只能表达想要修改的意图。**action就是改变state的指令，有多少操作state的动作就会有多少action。**

```js
//添加todo任务的 action 是这样的：
const ADD_TODO = 'ADD_TODO'

//action创建函数，返回一个action对象 
function addTodo(text) {
  return{
  type: ADD_TODO,//执行的动作
  text: 'Build my first Redux app'，
  index：5，//用户完成任务的动作序列号
}
}

//Redux 中只需把 action 创建函数的结果传给 dispatch() 方法即可发起一次dispatch 过程。
dispatch(addTodo(text))
//或者创建一个被绑定的 action 创建函数来自动 dispatch：
const boundAddTodo = text => dispatch(addTodo(text))
boundAddTodo(text);
//store 里能直接通过 store.dispatch() 调用 dispatch() 方法，但是多数情况下你会使用 react-redux 提供的 connect() 帮助器来调用。
```

#### 使用纯函数来执行修改

**reducer 就是一个纯函数，接收旧的 state 和 action，返回新的 state。**

```js
(previousState, action) => newState
```

之所以将这样的函数称之为reducer，是因为这种函数与被传入 [`Array.prototype.reduce(reducer, ?initialValue)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce) 里的回调函数属于相同的类型。保持 reducer 纯净非常重要。**永远不要**在 reducer 里做这些操作：

- 修改传入参数；
- 执行有副作用的操作，如 API 请求和路由跳转；
- 调用非纯函数，如 `Date.now()` 或 `Math.random()`。

**这是一个redux的经典案例**

- 通过createStore创建store

- actions 定义指令
- 调用store.dispatch()发出修改state的命令
- 定义reducer函数根据action的类型改变state

```js
import { createStore } from 'redux';
//这里一个技巧是使用 ES6 参数默认值语法 来精简代码。
const reducer = (state = {count: 0}, action) => {
  switch (action.type){
    case 'INCREASE': return {count: state.count + 1};
    case 'DECREASE': return {count: state.count - 1};
    default: return state;
  }
}
const actions = {
  increase: () => ({type: 'INCREASE'}),
  decrease: () => ({type: 'DECREASE'})
}
// 创建 Redux store 来存放应用的状态。
// API 是 { subscribe, dispatch, getState }。
let store = createStore(counter);

// 可以手动订阅更新，也可以事件绑定到视图层。
store.subscribe(() =>
  console.log(store.getState())
);

// 改变内部 state 惟一方法是 dispatch 一个 action。
// action 可以被序列化，用日记记录和储存下来，后期还可以以回放的方式执行
store.dispatch(actions.increase()) // {count: 1}
store.dispatch(actions.increase()) // {count: 2}
store.dispatch(actions.increase()) // {count: 3}
```

### store构建

#### 目录结构

[![屏幕截图](https://z3.ax1x.com/2021/01/19/sgpjbR.png)](https://imgchr.com/i/sgpjbR)

#### action

**存放描述行为的数据结构(本质上是 JavaScript 普通对象),一般来说你会通过 store.dispatch() 将 action 传到 store。**

我们约定，action 内必须使用一个字符串类型的 `type` 字段来表示将要执行的动作。多数情况下，`type` 会被定义成字符串常量。当应用规模越来越大时，建议使用单独的模块或文件来存放 action。

```js
//	./actions/counter.js
export const INCREMENT = 'INCREMENT';
export const DECREMENT = 'DECREMENT';

export const increment = ()=>{
  {type:INCREMENT}
}
export const decrement = ()=>{
  {type:DECREMENT}
}
```

注意：当我们表示用户完成任务的动作序列号时，我们还需要再添加一个 action index 来，所以我们通过下标 `index` 来引用特定的任务。而实际项目中一般会在新建数据的时候生成唯一的 ID 作为数据的引用标识。

#### Reducer

**Reducers** 指定了应用状态的变化如何响应 [actions](https://www.redux.org.cn/docs/basics/Actions.html) 并发送到 store 的。

```js
//	./reducers/counter.js
import {INCREMENT, DECREMENT} from "../actions/counter"
export default function(state = 0, action){
    switch (action.type) {
        case INCREMENT:
          return state + 1;
        case DECREMENT:
          return state - 1;
        default:
          return state;
        }
}
```

```js
//	./reducers/index.js
import { combineReducers } from 'redux'
import counter from './counter'

export default combineReducers({
	counter
})

```

#### store

**注意：Redux 应用只有一个单一的 store**

我们学会了使用 action 来描述“发生了什么”，和使用 reducers 来根据 action 更新 state 的用法。

**Store** 就是把它们联系到一起的对象。Store 有以下职责：

- 维持应用的 state；
- 提供 [`getState()`](https://www.redux.org.cn/docs/api/Store.html#getState) 方法获取 state；
- 提供 [`dispatch(action)`](https://www.redux.org.cn/docs/api/Store.html#dispatch) 方法更新 state；
- 通过 [`subscribe(listener)`](https://www.redux.org.cn/docs/api/Store.html#subscribe) 注册监听器;
- 通过 [`subscribe(listener)`](https://www.redux.org.cn/docs/api/Store.html#subscribe) 返回的函数注销监听器。

https://zhuanlan.zhihu.com/p/258017257

```js
import { createStore, applyMiddleware, compose } from 'redux'
import { createLogger } from 'redux-logger'
import thunk from 'redux-thunk'
import reducers from './reducers'

function configureStore() {
  const logger = createLogger({})

  const middlewares = [thunk]

  if (process.env.NODE_ENV !== 'production') {
    middlewares.push(logger)
  }

  const composeEnhancers =
    typeof window === 'object' && window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__
      ? window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({})
      : compose
  const enhancer = composeEnhancers(applyMiddleware(...middlewares))
//createStore() 的第二个参数是可选的, 用于设置 state 初始状态。这对开发同构应用时非常有用，服务器端 redux 应用的 state 结构可以与客户端保持一致, 那么客户端可以将从网络接收到的服务端 state 直接用于本地数据初始化。
  return createStore(reducers, enhancer)
}

export default configureStore()

```

### redux 异步请求

https://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_two_async_operations.html

![img](https://upload-images.jianshu.io/upload_images/18616547-35a9f5f3f9956a6b.png?imageMogr2/auto-orient/strip|imageView2/2/w/800/format/webp)

Action 发出以后，Reducer 立即算出 State，这叫做同步；Action 发出以后，过一段时间再执行 Reducer，这就是异步。
在实际的开发中，redux中管理的很多数据可能来自服务器，我们需要进行异步的请求，再将数据保存到redux中。就是说在异步的网络请求中通过dispatch action来更新state中的数据。这时候就需要用到Redux中间件**(指这个框架允许我们在某个流程的执行中间插入我们自定义的一段代码)**。

[Thunk middleware](https://github.com/gaearon/redux-thunk) 并不是 Redux 处理异步 action 的唯一方式：

- 你可以使用 [redux-promise](https://github.com/acdlite/redux-promise) 或者 [redux-promise-middleware](https://github.com/pburtchaell/redux-promise-middleware) 来 dispatch Promise 来替代函数。
- 你可以使用 [redux-observable](https://github.com/redux-observable/redux-observable) 来 dispatch Observable。
- 你可以使用 [redux-saga](https://github.com/yelouafi/redux-saga/) 中间件来创建更加复杂的异步 action。
- 你可以使用 [redux-pack](https://github.com/lelandrichardson/redux-pack) 中间件 dispatch 基于 Promise 的异步 Action。

### API

#### Provider 组件

`<Provider store>` 使组件层级中的 `connect()` 方法都能够获得 Redux store。正常情况下，你的根组件应该嵌套在 `<Provider>` 中才能使用 `connect()` 方法。

React-Redux 提供`Provider`组件，可以让容器组件拿到`state`。

> ```javascript
> import { Provider } from 'react-redux'
> import { createStore } from 'redux'
> import todoApp from './reducers'
> import App from './components/App'
> 
> let store = createStore(todoApp);
> 
> render(
> <Provider store={store}>
> <App />
> </Provider>,
> document.getElementById('root')
> )
> ```

上面代码中，`Provider`在根组件外面包了一层，这样一来，`App`的所有子组件就默认都可以拿到`state`了。

**它的原理是`React`组件的[`context`](https://facebook.github.io/react/docs/context.html)属性**，请看源码。

> ```javascript
> class Provider extends Component {
> getChildContext() {
> return {
> store: this.props.store
> };
> }
> render() {
> return this.props.children;
> }
> }
> 
> Provider.childContextTypes = {
> store: React.PropTypes.object
> }
> ```

上面代码中，`store`放在了上下文对象`context`上面。然后，子组件就可以从`context`拿到`store`，代码大致如下。

> ```js
> class VisibleTodoList extends Component {
> componentDidMount() {
> const { store } = this.context;
> this.unsubscribe = store.subscribe(() =>
> this.forceUpdate()
> );
> }
> 
> render() {
> const props = this.props;
> const { store } = this.context;
> const state = store.getState();
> // ...
> }
> }
> 
> VisibleTodoList.contextTypes = {
> store: React.PropTypes.object
> }
> ```

`React-Redux`自动生成的容器组件的代码，就类似上面这样，从而拿到`store`。

#### connect

React-Redux 提供`connect`方法，用于从 UI 组件生成容器组件。`connect`的意思，就是将这两种组件连起来。

> ```javascript
> import { connect } from 'react-redux'
> const VisibleTodoList = connect()(TodoList);
> ```

上面代码中，`TodoList`是 UI 组件，`VisibleTodoList`就是由 React-Redux 通过`connect`方法自动生成的容器组件。

但是，因为没有定义业务逻辑，上面这个容器组件毫无意义，只是 UI 组件的一个单纯的包装层。为了定义业务逻辑，需要给出下面两方面的信息。

> （1）输入逻辑：外部的数据（即`state`对象）如何转换为 UI 组件的参数
>
> （2）输出逻辑：用户发出的动作如何变为 Action 对象，从 UI 组件传出去。

因此，`connect`方法的完整 API 如下。

> ```javascript
> import { connect } from 'react-redux'
> 
> const VisibleTodoList = connect(
> mapStateToProps,
> mapDispatchToProps
> )(TodoList)
> ```

上面代码中，`connect`方法接受两个参数：`mapStateToProps`和`mapDispatchToProps`。它们定义了 UI 组件的业务逻辑。前者负责输入逻辑，即将`state`映射到 UI 组件的参数（`props`），后者负责输出逻辑，即将用户对 UI 组件的操作映射成 Action。

##### mapStateToProps()

`mapStateToProps`是一个函数。它的作用就是像它的名字那样，建立一个从（外部的）`state`对象到（UI 组件的）`props`对象的映射关系。也就是说， **把state映射到props中去** 

作为函数，`mapStateToProps`执行后应该返回一个对象，里面的每一个键值对就是一个映射。请看下面的例子。

> ```javascript
> const mapStateToProps = (state) => {
> return {
>  todos: getVisibleTodos(state.todos, state.visibilityFilter)
> }
> }
> ```

上面代码中，`mapStateToProps`是一个函数，它接受`state`作为参数，返回一个对象。这个对象有一个`todos`属性，代表 UI 组件的同名参数，后面的`getVisibleTodos`也是一个函数，可以从`state`算出 `todos` 的值。

下面就是`getVisibleTodos`的一个例子，用来算出`todos`。

> ```javascript
> const getVisibleTodos = (todos, filter) => {
> switch (filter) {
> case 'SHOW_ALL':
> return todos
> case 'SHOW_COMPLETED':
> return todos.filter(t => t.completed)
> case 'SHOW_ACTIVE':
> return todos.filter(t => !t.completed)
> default:
> throw new Error('Unknown filter: ' + filter)
> }
> }
> ```

`mapStateToProps`会订阅 Store，每当`state`更新的时候，就会自动执行，重新计算 UI 组件的参数，从而触发 UI 组件的重新渲染。

`mapStateToProps`的第一个参数总是`state`对象，还可以使用第二个参数，代表容器组件的`props`对象。

> ```javascript
> // 容器组件的代码
> //    <FilterLink filter="SHOW_ALL">
> //      All
> //    </FilterLink>
> 
> const mapStateToProps = (state, ownProps) => {
> return {
> active: ownProps.filter === state.visibilityFilter
> }
> }
> ```

使用`ownProps`作为参数后，如果容器组件的参数发生变化，也会引发 UI 组件重新渲染。

`connect`方法可以省略`mapStateToProps`参数，那样的话，UI 组件就不会订阅Store，就是说 Store 的更新不会引起 UI 组件的更新。

##### mapDispatchToProps()

`mapDispatchToProps`是`connect`函数的第二个参数，**用来建立各种dispatch变成props，让你可以直接使用  UI 组件的参数到`store.dispatch`方法的映射**。也就是说，**把各种dispatch变成了props让你可以直接使用** 

如果`mapDispatchToProps`是一个函数，会得到`dispatch`和`ownProps`（容器组件的`props`对象）两个参数。

> ```javascript
> const mapDispatchToProps = (
> dispatch,
> ownProps
> ) => {
> return {
> onClick: () => {
> dispatch({
>   type: 'SET_VISIBILITY_FILTER',
>   filter: ownProps.filter
> });
> }
> };
> }
> ```

从上面代码可以看到，`mapDispatchToProps`作为函数，应该返回一个对象，该对象的每个键值对都是一个映射，定义了 UI 组件的参数怎样发出 Action。

如果`mapDispatchToProps`是一个对象，它的每个键名也是对应 UI 组件的同名参数，键值应该是一个函数，会被当作 Action creator ，返回的 Action 会由 Redux 自动发出。举例来说，上面的`mapDispatchToProps`写成对象就是下面这样。

> ```javascript
> const mapDispatchToProps = {
> onClick: (filter) => {
> type: 'SET_VISIBILITY_FILTER',
> filter: filter
> };
> }
> ```



##### 实例：计数器

我们来看一个实例。下面是一个计数器组件，它是一个纯的 UI 组件。

> ```js
> import React from "react";
> import { connect } from "react-redux";
> import { increment, decrement } from "../../store/actions/counter";
> 
> const Home = function (props) {
> //生成props
> const { count, onincrement, ondecrement} = props;
> // console.log(props);
> return (
> <div>
>   <Button
>     variant="contained"
>     color="primary"
>     onClick={onincrement}
>   >
>     increment
>   </Button>
>   <Button
>     variant="contained"
>     color="primary"
>     onClick={ondecrement}
>     style={{marginLeft:'30px'}}
>   >
>     decrement
>   </Button>
>   <p style={{fontSize:'30px'}}>{count}</p>
> </div>
> );
> };
> ```

上面代码中，这个 UI 组件有三个参数：count和 onincrement, ondecrement。前者需要从`state`计算得到，后者需要向外发出 Action。

接着，定义`count`到`state`的映射，以及`onincrement, ondecrement`到`dispatch`的映射。

> ```javascript
> function mapStateToProps(state) {
> console.log(state)
> return {
> count: state.counter.count,
> };
> }
> function mapDispatchToProps(dispatch) {
> return {
>  onincrement: () => dispatch(increment()),
> ondecrement: () => dispatch(decrement())
> };
> }
> 
> ```

然后，使用`connect`方法生成容器组件。

> ```javascript
> export default connect(mapStateToProps, mapDispatchToProps)(Home);
> ```

然后，定义这个组件的 Reducer。

> ```javascript
> // Reducer
> import {INCREMENT, DECREMENT} from "../actions/counter"
> export default function(state = { count: 0}, action){
> const count = state.count
> switch (action.type) {
>   case INCREMENT:
>     return {count:count + 1};
>   case DECREMENT:
>     return {count:count - 1};
>   default:
>     return {count:count};
>   }
> }
> ```

最后，生成`store`对象，并使用`Provider`在根组件外面包一层。

> ```js
> import React from "react";
> import route from "../route/index.js";
> import { Provider } from "react-redux";
> import store from "../store";
> export default function Menu() {
> const classes = useStyles();
> return (
> <div className={classes.root}>
> <Provider store={store}>
> </Provider>
> </div>
> );
> }
> 
> ```

#### createStore

`createStore(reducer, [preloadedState], enhancer)`

创建一个 Redux [store](https://www.redux.org.cn/docs/api/Store.html) 来以存放应用中所有的 state。
应用中应有且仅有一个 store。

**参数**

1. `reducer` *(Function)*: 接收两个参数，分别是当前的 state 树和要处理的 [action](https://www.redux.org.cn/docs/Glossary.html#action)，返回新的 [state 树](https://www.redux.org.cn/docs/Glossary.html#state)。
2. [`preloadedState`] *(any)*: 初始时的 state。 在同构应用中，你可以决定是否把服务端传来的 state 水合（hydrate）后传给它，或者从之前保存的用户会话中恢复一个传给它。如果你使用 [`combineReducers`](https://www.redux.org.cn/docs/api/combineReducers.html) 创建 `reducer`，它必须是一个普通对象，与传入的 keys 保持同样的结构。否则，你可以自由传入任何 `reducer` 可理解的内容。
3. `enhancer` *(Function)*: Store enhancer 是一个组合 store creator 的**高阶函数**，返回一个新的强化过的 store creator。这与 middleware 相似，它也允许你通过复合函数改变 store 接口。

**返回值**

([*`Store`*](https://www.redux.org.cn/docs/api/Store.html)): 保存了应用所有 state 的对象。改变 state 的惟一方法是 [dispatch](https://www.redux.org.cn/docs/api/Store.html#dispatch) action。你也可以 [subscribe 监听](https://www.redux.org.cn/docs/api/Store.html#subscribe) state 的变化，然后更新 UI。

```js
import { createStore } from 'redux'

function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return state.concat([action.text])
    default:
      return state
  }
}

let store = createStore(todos, ['Use Redux'])

store.dispatch({
  type: 'ADD_TODO',
  text: 'Read the docs'
})

console.log(store.getState())
// [ 'Use Redux', 'Read the docs' ]
```

- 应用中不要创建多个 store！相反，使用 [`combineReducers`](https://www.redux.org.cn/docs/api/combineReducers.html) 来把多个 reducer 创建成一个根 reducer。
- 要使用多个 store 增强器的时候，你可能需要使用 [compose](https://www.redux.org.cn/docs/api/compose.html)

#### Store 方法

Store 就是用来维持应用所有的 state 树 的一个对象。 改变 store 内 state 的惟一途径是对它 dispatch 一个 action。

- getState()
- dispatch(action)
- subscribe(listener)
- replaceReducer(nextReducer)

#### combineReducers

随着应用变得越来越复杂，可以考虑将 [reducer 函数](https://www.redux.org.cn/docs/Glossary.html#reducer) 拆分成多个单独的函数，拆分后的每个函数负责独立管理 [state](https://www.redux.org.cn/docs/Glossary.html#state) 的一部分。

```
import { combineReducers } from 'redux'
import counter from './counter'

export default combineReducers({
	counter
})
```

combineReducers把一个由多个不同 reducer 函数作为 value 的 object，合并成一个最终的 reducer 函数，然后就可以对这个 reducer 调用 createStore 方法。

合并后的 reducer 可以调用各个子 reducer，并把它们返回的结果合并成一个 state 对象。

#### applyMiddleware

https://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_two_async_operations.html

**applyMiddleware(...middlewares)**

使用包含自定义功能的 middleware 来扩展 Redux 是一种推荐的方式。Middleware 可以让你包装 store 的 dispatch 方法来达到你想要的目的。同时， middleware 还拥有“可组合”这一关键特性。多个 middleware 可以被组合到一起使用，形成 middleware 链。其中，每个 middleware 都不需要关心链中它前后的 middleware 的任何信息。

Middleware 最常见的使用场景是实现异步 actions。这种方式可以让你像 dispatch 一般的 actions 那样 **dispatch 异步 actions**。

**示例: 自定义 Logger Middleware**

```js
import { createStore, applyMiddleware } from 'redux'
import todos from './reducers'

function logger({ getState }) {
  return (next) => (action) => {
    console.log('will dispatch', action)

    // 调用 middleware 链中下一个 middleware 的 dispatch。
    let returnValue = next(action)

    console.log('state after dispatch', getState())

    // 一般会是 action 本身，除非
    // 后面的 middleware 修改了它。
    return returnValue
  }
}

let store = createStore(
  todos,
  [ 'Use Redux' ],
  applyMiddleware(logger)
)

store.dispatch({
  type: 'ADD_TODO',
  text: 'Understand the middleware'
})
// (将打印如下信息:)
// will dispatch: { type: 'ADD_TODO', text: 'Understand the middleware' }
// state after dispatch: [ 'Use Redux', 'Understand the middleware' ]
```

####   ` compose(...functions)`

从右到左来组合多个函数。

这是函数式编程中的方法，为了方便，被放到了 Redux 里。
当需要把多个 [store 增强器](https://www.redux.org.cn/docs/Glossary.html#store-enhancer) 依次执行的时候，需要用到它。

**参数**

1. (*arguments*): 需要合成的多个函数。预计每个函数都接收一个参数。它的返回值将作为一个参数提供给它左边的函数，以此类推。例外是最右边的参数可以接受多个参数，因为它将为由此产生的函数提供签名。（译者注：`compose(funcA, funcB, funcC)` 形象为 `compose(funcA(funcB(funcC())))`）

**返回值**

(*Function*): 从右到左把接收到的函数合成后的最终函数。

```js
import { createStore, combineReducers, applyMiddleware, compose } from 'redux'
import thunk from 'redux-thunk'
import DevTools from './containers/DevTools'
import reducer from '../reducers/index'

const store = createStore(
  reducer,
  compose(
    applyMiddleware(thunk),
    DevTools.instrument()
  )
)
```

## Mobx



## hox

**定义 Model**： 用 `createModel` 包装后，就变成了持久化，且全局共享的数据。 

```javascript
import { createModel } from 'hox';

/* 任意一个 custom Hook */
function useCounter() {
  const [count, setCount] = useState(0);
  const decrement = () => setCount(count - 1);
  const increment = () => setCount(count + 1);
  return {
    count,
    decrement,
    increment
  };
}

export default createModel(useCounter)
```

**使用 Model**：`createModel` 返回值是个 Hook，你可以按 React Hooks 的用法正常使用它。

```javascript
import { useCounterModel } from "../models/useCounterModel";

function App(props) {
  const counter = useCounterModel();
  return (
    <div>
      <p>{counter.count}</p>
      <button onClick={counter.increment}>Increment</button>
    </div>
  );
}
```



# 路由

| 相关组件            | 功能                                                         |
| ------------------- | ------------------------------------------------------------ |
| react-router        | 实现了路由的核心功能，用作下面几个包的运行时依赖项(peer dependency)。 |
| react-router-dom    | 基于 `react-router` 添加了浏览器运行环境的一些组件和功能。   |
| react-router-native | 适用于 `React Native`                                        |
| react-router-redux  | React Router 和 Redux 的集成。                               |
| eact-router-config  | 提供可配置化的路由                                           |

## 路由创建

### 组件创建（V5）

使用HashRouter包裹整个应用，**一个项目中只会有一个Router**，使用`Route`指定路由规则(哪个路径展示哪个组件) 

```
import React from 'react'
import ReactDom from 'react-dom'
import { HashRouter, Route, Link } from 'react-router-dom'
import Search from './pages/Search.jsx'
import Comment from './pages/Comment.jsx'
export default function App () {
  return (
    <div>
      <h1>react路由基本使用</h1>
      <Link to="/comment">评论</Link>
      <Link to="/search">搜索</Link>
      <HashRouter>
        <Route path="/comment" component={Comment} />
        <Route path="/search" component={Search} />
      </HashRouter>
    </div>
  )
}
ReactDom.render(<App />, document.getElementById('root'))

```

### API创建（V6）

- createBrowserRouter
- createHashRouter

```jsx
import { createBrowserRouter,createHashRouter, RouterProvider } from 'react-router-dom'

//使用createBrowserRouter创建router实例对象并且配置路由对应关系
const router = createBrowserRouter([
  {
    path: '/login',
    element: <div>我是登录页</div>,
  },
  {
    path: '/home',
    element: <div>我是首页</div>,
  },
])

//使用RouterProvider组件全局注入router实例
<RouterProvider router={router}>
	<App />
</RouterProvider>
```

### 嵌套路由

> 注意：如果在父路由中开启 exact 匹配，就会导致子组件加载不出来

```jsx
//根路由
<Switch>
    <Route path="router" component={Router}></Route>
</Switch>

//Router.jsx
<span>router</span>
<ul>
    <li>
        <Link to="/router/second/1">1</Link>
    </li>
    <li>
        <Link to="/router/second/12">2</Link>
    </li>
</ul>
//子路由的配置分散到各组件中
<Switch>
    <Route exact path="/router/second/:id" component={Second}></Route>
</Switch>
```

### 路由懒加载

[路由懒加载](https://so.csdn.net/so/search?q=路由懒加载&spm=1001.2101.3001.7020)是指路由的 js 资源只有在背访问时才会动态获取，为了优化项目首次打开的时间。

```jsx
import { createBrowserRouter } from 'react-router-dom'
import { lazy, Suspense } from 'react'
import Login from '@/pages/Login'
import Layout from '@/pages/Layout'

import AuthRoute from '@/components/Auth'

// 1. lazy 函数对组件进行导入
const Publish = lazy(() => import('@/pages/Publish'))
const Article = lazy(() => import('@/pages/Article'))
const Home = lazy(() => import('@/pages/Article'))
```

```jsx
// 2. suspense 组件依次对三个组件进行包裹， callback 占位，在渲染完成之前的显示
const router = createBrowserRouter([
  {
    path: '/',
    element: (
      <AuthRoute>
        <Layout />
      </AuthRoute>
    ),
    children: [
      {
        index: true,
        element: (
          <Suspense fallback={'加载中'}>
            <Home />
          </Suspense>
        )
      },
      {
        path: 'article',
        element: (
          <Suspense fallback={'加载中'}>
            <Article />
          </Suspense>
        )
      },
      {
        path: 'publish',
        element: (
          <Suspense fallback={'加载中'}>
            <Publish />
          </Suspense>
        )
      },
    ],
  },
  {
    path: '/login',
    element: <Login />,
  },
])

export default router
```

### 路由嵌套路由懒加载

https://zh-hans.reactjs.org/docs/code-splitting.html

**Suspense和lazy**

如果我们项目有三个模块，用户管理（UserManage）、资产管理（AssetManage）、考勤管理（AttendanceManage）。当我们进入首页的时候由于没有进入任何一个模块，为了提高响应效率是不需要进行模块资源加载的，同时当我们进入用户管理的时候只需要加载用户管理路由对应的模块资源，进入其他模块亦然。这时候我们就需要对代码进行拆分，React.lazy可以结合Router来对模块进行懒加载。

```jsx
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import React, { Suspense, lazy } from 'react';

// 懒加载引入组件 在用到路由组件时才发送请求
// 通过React的lazy函数配合import()函数动态加载路由组件 ===> 路由组件代码会被分开打包 

const Home = lazy(() => import('./routes/Home'));
const UserManage = lazy(() => import('./routes/UserManage'));
const AssetManage = lazy(() => import('./routes/AssetManage'));
const AttendanceManage = lazy(() => import('./routes/AttendanceManage'));

const App = () => (
  <Router>
     {/* 用Suspense包含所有需要注册的路由 fallback为响应未回来时显示的内容 */}
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/userManage" component={UserManage}/>
        <Route path="/assetManage" component={AssetManage}/>
        <Route path="/attendanceManage" component={AttendanceManage}/>
      </Switch>
    </Suspense>
  </Router>
)
```



## 组件

### 路由匹配器Route

#### Swtich(v5)

`Swtich` 就近匹配路由，只会渲染**第一个**匹配的组件

```jsx
/// 假设你访问的URL为 /dog
<Route path='/dog' component={Dog}></Route> // 虽然这里匹配了，但不会停止查找
<Route path="/:dog" component={Husky}></Route> // 这个路由依然会被匹配，这样两个组件都会被渲染
  ...
<Switch>
  <Route path='/dog' component={Dog}></Route> // Switch 匹配一个路由后就不会再去查找下一个路由，那么下面的路由就不会被匹配
  <Route path="/:dog" component={Husky}></Route>
</Switch>
```

#### Routes(v6)

Routes是创建路由管理器

- **v6版本中移出了先前的`<Switch>`，引入了新的替代者：`<Routes>`。**
- `<Routes>` 和 `<Route>`要配合使用，且必须要用`<Routes>`包裹`<Route>`。
- `<Route>` 相当于一个 if 语句，如果其路径与当前 URL 匹配，则呈现其对应的组件

```
 <Routes>
   {/* path定义路径，可以省略path前的/；element定义当前路径对应的组件 */}
   <Route path='/' element={<Home />} />
    
   {/* 
   路由嵌套：路由嵌套要写在路由配置页，渲染嵌套的路由组件的位置使用Outlet 
   friend 是一级路由，路径为 /friend 
   chat是二级路由，对应路径为 /friend/chat/张三
   */}
   <Route path='friend' element={<Friends />} />
     <Route path='chat/:name' element={<Chat />} />
   </Route>
 ​
   <Route path='setting' element={<Settings />} />
     
   {/* Route也可以不写element属性, 这时就是用于展示嵌套的路由，所对应的路径是/users/login */}
   <Route path="user">
     <Route path="login" element={<Login />} />
   </Route>
 ​
   {/* 当没有其他路由匹配该URL时，你可以使用path="*"渲染一个"未找到"的路由。这条路由将匹配任何URL，但优先级最低，因此路由器只有在没有其他路由匹配的情况下才会选择它 */}
   <Route path='*' element={<NotFound />} />
 </Routes>

```

#### Route

Route是创建路由对象

```
const First = () => <p>页面一的页面内容</p> 
<Router> 
  <div className="App"> 
    <Link to="/first">页面一</Link> 
    <Route path="/first" component={First}></Route> 
  </div> 
</Router>
```

- <a id="exact">exact</a> 是否进行精确匹配，路由 `/a` 可以和 `/a/、/a` 匹配

  当exact为false时，根据路由匹配所有组件，例如/a/b/c 能匹配到/、/a、/a/b、/a/b/c 且匹配还是按顺序的
  
  例如路由设置的前后顺序为:
  1./ ；
  2./a；
  3./a/b ; 
  4./a/b/c
  且前3个路径都没有设置 exact，这样前3个组件**都会被渲染**并且默认将2当作1的子页面，3当作2的子页面

- `strict` 是否进行严格匹配，指明路径只匹配以斜线结尾的路径，路由`/a`可以和`/a`匹配，不能和`/a/`匹配，相比 `exact` 会更严格些

- `path (string)` 标识路由的路径,`path`属性可以使用通配符。

  ```
  <Route path="/hello/:name">
  ```

  通配符的规则如下:
  
  - **paramName**
  
    `:paramName`匹配URL的一个部分，直到遇到下一个`/`、`?`、`#`为止。这个路径参数可以通过`this.props.params.paramName`取出。
  
  - ()
  
    `()`表示URL的这个部分是可选的。
  
  - *
  
    `*`匹配任意字符，直到模式里面的下一个字符为止。匹配方式是非贪婪模式。
  
  - \**
  
    `**` 匹配任意字符，直到下一个`/`、`?`、`#`为止。匹配方式是贪婪模式。

- `component` 表示路径对应显示的组件
- `location (object)` 除了通过 path 传递路由路径，也可以通过传递 location 对象可以匹配
- `sensitive (boolean)` 匹配路径时，是否区分大小写

### 链接

#### Link

`<Link>`修改URL，且不发送网络请求（路由链接）。

```
<Link to={item.path} key={item.path}>
{item.title}
</Link>
```

#### NavLink

`NavLink组件`和`Link组件`的功能是一致的，区别在于可以判断其`to属性`是否是当前匹配到的路由，可实现导航的“高亮”效果

`NavLink组件`的`style`或`className`可以接收一个函数，函数接收一个`isActive`参数，可根据该参数调整样式，处理高亮状态，NavLink默认类名是active，可以通过自定义类名或者样式实现高亮：

- 自定义类名

```javascript
<NavLink className={({ isActive }) => (isActive ? 'active' : '')} to={item.path} key={item.path}>
   {item.title}
 </NavLink>
```

- 自定义样式

```javascript
javascript复制代码 <NavLink style={({ isActive }) => ({backgroundColor: isActive ? 'lightblue': ''})} to={item.path} key={item.path}>
   {item.title}
 </NavLink>
```

#### Redirect(v5)

**重定向，新位置将覆盖历史堆栈中的当前位置**

`from (string)` 需要重定向的路径，可以包括动态参数

`push (boolean)` 为 true 时，重定向会将新条目推入历史记录，而不是替换当前条目

`to (string | object)` 重定向到的路径

`exact (boolean)` 是否要对 from 进行精确匹配

`strict (boolean)` 是否要对 from 进行严格匹配

`sensitive (boolean)` 匹配 from 时是否区分大小写

#### Navigate(v6)

只要`<Navigate>`组件被渲染，就会修改路径，切换视图（重定向）。

`<Navigate>`的`replace`属性用于控制跳转模式（push 或 replace，默认是push）。

```javascript
import { useState } from 'react';
import { Navigate } from 'react-router-dom';
 
 const Home = props => {
     const [show, setShow] = useState(false);
     return (
         <div id='home' className='w'>
             <h1>首页</h1>
             {/* 根据show的值决定是否切换视图 */}
             {show && <Navigate to='/setting' replace={true} />}
             <button onClick={() => setShow(true)}>按钮</button>
         </div>
     );
 };
```



### IndexRoute和IndexRedirect

#### Index Routes

通常情况下，我们会建立如下情况的路由：

```jsx
<Router>
  <Route path="/" component={App}>
    <Route path="accounts" component={Accounts}/>
    <Route path="statements" component={Statements}/>
  </Route>
</Router>
```

当用户访问 `/` 时, App 组件被渲染，但组件内的子元素却没有， `App` 内部的 `this.props.children` 为 undefined 。 你可以简单地使用 `{this.props.children ||}` 来渲染一些默认的 UI 组件。

```jsx
<Router>
  <Route path="/" component={App}>
    <IndexRoute component={Home}/>
    <Route path="accounts" component={Accounts}/>
    <Route path="statements" component={Statements}/>
  </Route>
</Router>
```

如此配置后，我们再次访问 `/` 路由，你会发现页面渲染了 Home 组件的内容。这就是 IndexRoute 的功能，指定一个路由的默认页。

#### Index Redirects

上面这种情况比较常见，还有一种非常常见的方式就是当我们尝试访问 `/` 这个路由时，我们想让其直接跳转到 ‘/Accounts’，直接免去了默认页 Home，这样来的更加直接。由此我们就需要 `IndexRedirect` 功能。考虑如下路由：

```
<Router>
  <Route path="/" component={App}>
    <IndexRedirect to="/accounts"/>
    <Route path="accounts" component={Accounts}/>
    <Route path="statements" component={Statements}/>
  </Route>
</Router>
```

这样设计路由后，我们再次访问 `/` 时，系统默认会跳转到 `/accounts` 路由。

#### 总结

以上就是 IndexRoute 和 IndexRedirect 的功能介绍，让我们来总结一下他们两个的区别。

- IndexRoute 一般情况下用于设计一个默认页且不改变 URL 地址，而 IndexRedirect 则是跳转默认地址且地址会发生改变。
- IndexRoute 指定一个组件作为默认页，而 IndexRedirect 指定一个路由地址作为跳转地址。



### BrowserRouter

`<BrowserRouter>`使用常规的URL路径。但它们要求正确配置服务器。具体来说，您的Web服务器需要在所有由React Router客户端管理的URL上提供相同的页面

BrowserRouter提供了如下属性

- `basename (string)` 当前位置的基准 URL。当应用程序放置于服务器上子目录中时，可以设置，比如 `/public` 。
- `forceRefresh (boolean)`，在导航的过程中整个页面是否刷新
- `getUserConfirmation (func)`，当导航需要确认时执行的函数。默认是：window.confirm
- `keyLength (number)`  location.key 的长度。默认是 6
- `children (node)` 要渲染的子节点

```
 import React from "react";
 import ReactDOM from 'react-dom/client';
 import { BrowserRouter } from "react-router-dom";
 ​
 const root = ReactDOM.createRoot(document.getElementById('root'));
 root.render(
   <BrowserRouter>
     {/* 整体结构（通常为App组件） */}
   </BrowserRouter>
 );

```

### HashRouter

`<HashRouter>`将当前位置存储在[URL](https://developer.mozilla.org/en-US/docs/Web/API/HTMLHyperlinkElementUtils/hash)[的`hash`一部分中](https://developer.mozilla.org/en-US/docs/Web/API/HTMLHyperlinkElementUtils/hash)，因此URL看起来像`http://example.com/#/your/page`。由于哈希从不发送到服务器，因此这意味着不需要特殊的服务器配置(**在任意的路由进行页面的刷新都不会是 404**)。

HashRouter提供了如下属性

- `basename: string`, 同 `<BrowserRouter>` 的 `basename`。
- `getUserConfirmation: function`, 同 `<BrowserRouter>` 的 `getUserConfirmation`。
- `hashType: string`, Hash 编码类型，可选值 `'slash'(默认) | 'noslash' | 'hashbang'` 。
  - `slash`, 创建像 `#/`, `#/user/1` 这样的 hash 地址，默认值。
  - `noslash`, 创建像 `#`, `#user/1` 这样的 hash 地址
  - `hashbang`, 创建像 `#!/`, `#!/user/1` 这样的 ajax crawlable(已被 Google 遗弃) 的 hash 地址
- `children: node`, 同 `<BrowserRouter>` 的 `children: node`。

### Outlet

<Outlet/>作用类似于Vue中的`router-view`

```js
import { NavLink, Outlet } from "react-router-dom"; 
<Content style={{ height: '90vh' }}>
    <Outlet></Outlet>
</Content>
```

## 路由导航跳转

### 声明式导航Link

`<Link>` 组件被用来在**页面之间**进行导航，它其实就是 HTML 中的 `<a>` 标签的上层封装，不过在其源码中使用 `event.preventDefault` 禁止了其默认行为，然后使用 [history API](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FHistory_API) 自己实现了跳转。我们都知道，如果使用 `<a>` 标签去进行导航的话，整个页面都会被刷新，这是我们不希望看到的。所以我们使用 `<Link>` 组件来导航到一个目标 URL，可以在不刷新页面的情况下重新渲染页面

#### 参数

- to（string | object | function）

  - 为 string 时 就是一个明确的路径地址

    ```
    <Link to="/courses?sort=name" />
    ```

  - 为 object 时有如下属性（就是一个location对象）

    ```
    <Link
      to={{
      	pathname: "/courses",
        search: "?sort=name",
        hash: "#the-hash",
        state: { fromDashboard: true }
      }}
    />
    ```

    - pathname：URL路径。
    - search：URl中查询字符串。
    - hash：URL的hash分段，例如#a-hash。
    - state：表示location中的状态

  - 为 function 时，就是一个函数接收当前 location 为参数，然后以字符串或对象的形式返回位置形式

- `replace (boolean)`，当为 `true` 时，单击链接将替换历史堆栈中的当前记录，而不是添加一个新记录。

### 编程式导航

#### UseNavigate

```
import { useNavigate } from 'react-router-dom';

function Chat(props) {
  const navigate = useNavigate();
	const goBack = () => {
    // 第一种使用方式：传入数值进行前进或后退，类似 history.go()方法
		// navigate(-1);
    
		// 第二种使用方式：指定具体的路径
		navigate('/friend', {
			replace: false,
			state: { a: 1, b: 2 },
		});
	};
	return (
		<div id="chat" className="w">
			<h2>chat页面 </h2>
      <button onClick={goBack}>返回</button>
		</div>
	);
}

```

### 路由传参

#### param动态路由传参 

```jsx
<Route path='/path/:name' component={Path}/>
<link to={ '/user/' + '2' }>xxx</Link>
this.props.history.push({pathname:"/path/" + name});

//读取参数用:this.props.match.params.name
```

优点：
1、传参和接收都比较简单
2、刷新页面参数不会丢失
缺点：
1、 当复杂数据对象或数组需要传参时，这样做比较麻烦，需要通过json字符串的方式进行处理

```jsx
// 定义路由匹配
<Route path="/user/:data" component={Component} />;
let data = {
  id: 3,
  name: "tom",
  age: 25,
};
let path = JSON.Stringify(data);

// 传递路由参数
<Link to={path}>用户</Link>;
this.props.history.push(path);

// 使用路由参数
const { id, name, age } = this.props.match.params.data;
```

2、多个参数的传递，url 会又长又不美观
3、参数会出现在url上，不够安全 

#### search传参

```jsx
<Route path='/web/departManange' component={DepartManange}/>
<link to="web/departManange?tenantId=12121212">xxx</Link>
this.props.history.push({pathname:"/web/departManange?tenantId" + row.tenantId});

//读取参数用: this.props.location.search
```

优点：
1、传参和接收都比较简单
2、刷新页面参数不会丢失
3、可以传递多个参数
缺点：
1、当复杂数据对象或数组需要传参时，这样做比较麻烦，需要通过json字符串的方式进行处理
2、参数会出现在url上，不够安全 

#### query传参 

```jsx
<Route path='/query' component={Query}/>
<Link to={{ path : ' /query' , query : { name : 'sunny' }}}>
this.props.history.push({pathname:"/query",query: { name : 'sunny' }});

//读取参数用: this.props.location.query.name
```

优点：
1、传参和接收都比较简单
2、可以传递多个参数
3、传递对象数组等复杂参数方便
4、不会暴露给用户，比较安全
缺点：
1、如果手动刷新当前路由时，数据参数有可能会丢失 

#### state传参

```c
<Link to={{
    pathname: 'about',
    state: {
        name: 'dx'
    }
}}>关于</Link>

this.props.location.state
```

优点：
1、传参和接收都比较简单
2、可以传递多个参数
3、传递对象数组等复杂参数方便
4、不会暴露给用户，比较安全
缺点：
1、如果手动刷新当前路由时，数据参数有可能会丢失 

在[react](https://so.csdn.net/so/search?q=react&spm=1001.2101.3001.7020)中，最外层包裹了BrowserRouter时，不会丢失,但如果使用的时HashRouter，刷新当前页面时，会丢失state中的数据 

## Hooks

https://juejin.cn/post/7229493617365712953#heading-5

### withRouter

本质: 高阶组件

作用: 可以在非路由组件中注入路由对象

在没有路由指向(就是没有Route对象)的组件默认this.props当中没有路由所需要的参数，使用withRouter可以添加

```react
import React from 'react';
import BackHome from './backhome';
export default class Test extends React.Component {
 render () {
  console.log(this.props)
  return (
   <div>
    这是测试的内容
	//返回首页的按钮不是通过route标签渲染的，所以该子组件的this.props中没有路由参数
    <BackHome>返回首页</BackHome> 
   </div>
  )
 }
}
```

```react
import React from 'react';
//导入withRoute
import {withRouter} from 'react-router-dom';
class BackHome extends React.Component {
 goHome = () => {
  //必须在使用withRouter的情况下，该组件在this.props中才有路由参数和方法
  //否则，会报错
  this.props.history.push({
   pathname: '/home',
   state: {
    name: 'dx' //同样，可以通过state向home路由对应的组件传递参数
   }
  })
 }
 render () {
  return (
   <button onClick={this.goHome}>this.props.children</button>
  )
 }
}
//导出的时候，用withRouter标签将backHome组件以参数形式传出
export default withRouter(BackHome)

```

### useRoutes

新钩子useRoutes代替react-router-config

作用：根据路由表，动态创建`<Routes>`和`<Route>`。它的作用是动态地配置路由，它接收一个路由数组，并使用匹配到的路由来渲染相应的组件。

`useRoutes()` 在功能上等同于 `<Routes>`，但它使用 JavaScript 对象而不是元素来定义路由。这些对象具有与 `<Route>` 组件相同的属性。

`useRoutes()`的返回值是可用于呈现路由树的有效 React 元素。

```javascript
javascript复制代码//路由表配置：src/routes/index.jsx
import { Navigate } from 'react-router-dom';

import Home from '../views/Home';
import Friend from '../views/Friend';
import Setting from '../views/Setting';
import NotFound from '../views/NotFound';
import Chat from '../views/Chat';

const routes = [
	{ path: '/', element: <Navigate to='/home' /> },
	{ path: '/home', element: <Home /> },
	{
		path: '/friend',
		element: <Friend />,
		children: [{ path: 'chat/:name', element: <Chat /> }],
	},
	{ path: '/setting', element: <Setting /> },
	{ path: '*', element: <NotFound /> },
];

export default routes;
   
javascript复制代码// App.jsx

import { useState } from 'react';
import { NavLink, useRoutes } from 'react-router-dom';
import routes from './routes';

const App = () => {
  // 根据路由表生成对应的路由规则
  const ElementRouter = useRoutes(routes)
	const [items] = useState([
		{ path: '/home', title: '首页' },
		{ path: '/friend', title: '好友' },
		{ path: '/setting', title: '设置' },
	]);

	return (
		<div className='app'>
			<nav className='nav'>
				<div className='w'>
					{items.map(item => (
						<NavLink className={({ isActive }) => (isActive ? 'active' : '')} to={item.path} key={item.path}>
							{item.title}
						</NavLink>
					))}
				</div>
			</nav>
			{/* 注册路由 */}
			{ElementRouter}
		</div>
	);
};
```



### 属性的隐式传递

this.props.history/match/location

| 所属     | 属性                   | 类型     | 含义                                              |
| -------- | ---------------------- | -------- | ------------------------------------------------- |
| history  | length                 | number   | 表示history堆栈的数量                             |
|          | action                 | string   | 表示当前的动作。比如pop、replace或push            |
|          | location               | object   | 表示当前的位置                                    |
|          | push(path, [state])    | function | 在history堆栈顶加入一个新的条目                   |
|          | replace(path, [state]) | function | 替换在history堆栈中的当前条目                     |
|          | go(n)                  | function | 将history堆栈中的指针向前移动                     |
|          | goBack()               | function | 等同于go(-1)                                      |
|          | goForward()            | function | 等同于go(1)                                       |
|          | block(promt)           | function | 阻止跳转                                          |
|          |                        |          |                                                   |
| match    | params                 | object   | 表示路径参数，通过解析URL中动态的部分获得的键值对 |
|          | isExact                | boolean  | 为true时，表示精确匹配                            |
|          | path                   | string   | 用来做匹配的路径格式                              |
|          | url                    | string   | URL匹配的部分                                     |
|          |                        |          |                                                   |
| location | pathname               | string   | URL路径                                           |
|          | search                 | string   | URl中查询字符串                                   |
|          | hash                   | string   | URL的hash分段                                     |
|          | state                  | string   | 表示location中的状态                              |

### useNavigate(v6)

用useNavigate代替useHistory

```
import { useNavigate } from 'react-router-dom';

function Chat(props) {
  const navigate = useNavigate();
	const goBack = () => {
    // 第一种使用方式：传入数值进行前进或后退，类似 history.go()方法
		// navigate(-1);
    
		// 第二种使用方式：指定具体的路径
		navigate('/friend', {
			replace: false,
			state: { a: 1, b: 2 },
		});
	};
	return (
		<div id="chat" className="w">
			<h2>chat页面 </h2>
      <button onClick={goBack}>返回</button>
		</div>
	);
}

```



### useHistory(v5)

用以获取history对象，进行编程式的导航

```react
const Husky = props => {
  console.log(useHistory()); // 与 props.history 结果一致
  console.log(props.history);
  return <div>哈士奇</div>;
};
...
<Route path="/dog" component={Dog}></Route> // 必须这么写，props 才能拿到相关值
...
<Route path="/husky">
	<Husky />
</Route> // 这样写的话 useHistory 可以正常取值，但是 props 不行
```

### useLocation

用以获取location对象，可以查看当前路由信息

```react
const Husky = props => {
  console.log(useLocation()); // 与 props.location 结果一致
  console.log(props.location);
  return <div>哈士奇</div>;
};
```

### useParams

useParams和props.match.params可以获取路由参数

```react
<Route path="/blog/:eat">
    <Husky />
</Route>

const Husky = props => {
    console.log(useParams()) // 与 props.match.params 结果一致，但明显更简洁
    console.log(props.match.params)
    const {eat} = props.match.params;
    return (
    	<div>哈士奇 吃 {eat}</div>
    );
}
```

### useSearchParams()

作用：用于读取和修改当前位置的 URL 中的查询字符串。

返回一个包含两个值的数组，内容分别为：当前的seaech参数、更新search的函数。

```ini
ini复制代码function Chat(props) {
	const [search, setSearch] = useSearchParams();
	const name = search.get('name');
	const age = search.get('age');

	return (
		<div id='chat' className='w'>
			<h2>chat页面</h2>
			<button onClick={() => setSearch('name=张三&age=18')}>点击更新一下收到的search参数</button>
			<p> name={name} </p>
			<p> age={age} </p>
		</div>
	);
}
```



### useRouteMatch

`useRouteMatch`，接受一个**path字符串**作为参数。当参数的path与当前的路径相匹配时，useRouteMatch会返回match对象，否则返回null。

`useRouteMatch`在对于一些，**不是路由级别的组件**。但是**组件自身的显隐却和当前路径相关的组件时**，非常有用。

比如，你在做一个后台管理系统时，网页的Header只会在登录页显示，登录完成后不需要显示，这种场景下就可以用到`useRouteMatch`。

```react
const Home = () => {
  return (
    <div>Home</div>
  )
}
// Header组件只会在匹配`/detail/:id`时出现
const Header = () => {
  // 只有当前路径匹配`/detail/:id`时，match不为null
  const match = useRouteMatch('/detail/:id')
  return (
    match && <div>Header</div>
  )
}
const Detail = () => {
  return (
    <div>Detail</div>
  )
}
function App() {
  return (
    <div className="App">
      <Router>
        <Header/>
        <Switch>
          <Route exact path="/" component={Home}/>
          <Route exact path="/detail/:id" component={Detail}/> 
        </Switch>
      </Router>
    </div>
  );
}

```

## 实战



# 性能优化

### 懒加载组件

从工程方面考虑 ,    webpack  存在代码拆分能力 ,   可以为应用创建多个包 ,   并在运行时动态加载 ,   减少初始包的大小

而在  react  中使用到了  Suspense  和   lazy  组件实现代码拆分功能 ,   基本使用如下:

```
1   const johanComponent = React.lazy(() => import(/* webpackChunkName: "johanC omponent" */ './myAwesome.component'));
2
3   export const johanAsyncComponent = props => (
4     <React.Suspense fallback={<Spinner />}>
5       <johanComponent {...props} />
7   );
```



# TS与React

## jsx转ts

https://blog.yangteng.me/articles/2021/migrate-react-project-to-typescript/

### 配置TS

```bash
yarn add typescript
```

然后加入 TypeScript 的配置文件：将 tsconfig.json 放到项目的根目录下。

```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "experimentalDecorators": true,
    //有的项目webpack会设置一些module的别名alias，ts也得配一下防止找不到
    "baseUrl": "./",
    "paths": {
      "@@/*": ["./*"],
      "@/*": ["src/*"],
      "@api/*": ["src/api/*"],
      "@assets/*": ["src/assets/*"],
      "@common/*": ["src/common/*"],
      "@enum/*": ["src/enum/*"],
      "@context/*": ["src/context/*"],
      "@components/*": ["src/components/*"],
      "@models/*": ["src/models/*"],
      "@hooks/*": ["src/hooks/*"],
      "@pages/*": ["src/pages/*"],
      "@store/*": ["src/store/*"],
      "@utils/*": ["src/utils/*"]
    },
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": false,
    "forceConsistentCasingInFileNames": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react"
  },
  "include": ["src"]
}
```

### package

```
"devDependencies": {
    "@babel/core": "^7.15.8",
    "@babel/plugin-transform-runtime": "^7.15.0",
    "@babel/preset-env": "^7.15.8",
    "@babel/preset-react": "^7.14.5",
    "@babel/preset-typescript": "^7.15.0",
    "@types/react-redux": "^7.1.23",
    "babel-loader": "^8.2.2",
    "typescript": "^4.6.3",
}
"dependencies": {
    "@types/node": "^17.0.23",
    "@types/react": "^17.0.43",
    "@types/react-dom": "^17.0.14",
    "@types/react-router": "^5.1.16",
    "@types/react-router-dom": "^5.1.7",
    "less-loader": "^10.2.0",
}
```

### 配置 babel 和 webpack

将 babel 的 TypeScript 预设加入项目依赖中，并添加到 babel 的配置文件里。

```bash
yarn add @babel/preset-typescript --dev
```

```json
// .babelrc

{
  "presets": [
    // other presets
    // ...
    "@babel/typescript"
  ]
  // other settings
  // ...
}
```

 修改 webpack 的配置，将 TypeScript 文件加入 `resolve` 和`babel-loader` 的 match 规则中。 

```js
// webpack.config.js

export default {
  // other settings
  // ...
  resolve: {
    extensions: ['.js', '.jsx', '.json', '.ts', '.tsx'],
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx|ts|tsx)$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            // ...
          },
        },
      },
    ],
  },
}
```

### 引入类型定义的 package

上一步完成后，实际上已经可以在代码中使用 TypeScript 了。但这时候如果你去写一个 React 组件，就会发现类似 `Cannot find module 'react'.` 的报错。这就需要将一些你用到的 library 的类型定义加进来了。

```bash
yarn add @types/react @types/react-dom @types/node #@types/<package-used-in-your-project>
```

### TIP

- **antd,css**

  ```
      {
              loader: 'less-loader', // 编译 Less 为 CSS
              options: {
                lessOptions: {
                  javascriptEnabled: true,
                },
              },
            },
  ```

  

## React.FC

`React.FC`是函数式组件，是在TypeScript使用的一个**泛型**。FC是FunctionComponent的缩写，`React.FC`可以写成`React.FunctionComponent`。这个类型定义了默认的 props(如 children)以及一些静态属性(如 defaultProps)

```
import React, { FC } from 'react';

/**
 * 声明Props类型
 */
export interface MyComponentProps {
  className?: string;
  style?: React.CSSProperties;
}

export const MyComponent: FC<MyComponentProps> = props => {
  return <div>hello react</div>;
};
```

# NEXT

https://www.nextjs.cn/

## 背景

要从头开始使用 React 构建一个完整的 Web 应用程序，需要考虑许多重要的细节：

- 必须使用打包程序（例如 webpack）打包代码，并使用 Babel 等编译器进行代码转换。
- 你需要针对生产环境进行优化，例如代码拆分。
- 你可能需要对一些页面进行预先渲染以提高页面性能和 SEO。你可能还希望使用服务器端渲染或客户端渲染。
- 你可能必须编写一些服务器端代码才能将 React 应用程序连接到数据存储。

**Next.js：React 开发框架**

- 直观的、 [基于页面](https://www.nextjs.cn/docs/basic-features/pages) 的路由系统（并支持 [动态路由](https://www.nextjs.cn/docs/routing/dynamic-routes)）
- [预渲染](https://www.nextjs.cn/docs/basic-features/pages#pre-rendering)。支持在页面级的 [静态生成](https://www.nextjs.cn/docs/basic-features/pages#static-generation-recommended) (SSG) 和 [服务器端渲染](https://www.nextjs.cn/docs/basic-features/pages#server-side-rendering) (SSR)
- 自动代码拆分，提升页面加载速度
- 具有经过优化的预取功能的 [客户端路由](https://www.nextjs.cn/docs/routing/introduction#linking-between-pages)
- [内置 CSS](https://www.nextjs.cn/docs/basic-features/built-in-css-support) 和 [Sass 的支持](https://www.nextjs.cn/docs/basic-features/built-in-css-support#sass-support)，并支持任何 [CSS-in-JS](https://www.nextjs.cn/docs/basic-features/built-in-css-support#css-in-js) 库
- 开发环境支持 [快速刷新](https://www.nextjs.cn/docs/basic-features/fast-refresh)
- 利用 Serverless Functions 及 [API 路由](https://www.nextjs.cn/docs/api-routes/introduction) 构建 API 功能
- 完全可扩展

## 创建

```
npx create-next-app nextjs-blog --use-npm --example "https://github.com/vercel/next-learn-starter/tree/master/learn-starter"
```

```
cd nextjs-blog
```

```
npm run dev
```

在浏览器中打开 [http://localhost:3000](http://localhost:3000/) 。

## 页面

### 客户端导航

在 Next.js 中，页面是从[`pages`目录中](https://www.nextjs.cn/docs/basic-features/pages)的文件导出的 React 组件。

页面与基于其**文件名**的路由相关联。例如，在开发中：

- `pages/index.js`与`/`路由相关联。
- `pages/posts/first-post.js`与`/posts/first-post`路由相关联。

**在页面之间导航**

```react
import Link from 'next/link'
```

```react
Read <Link href="/posts/first-post"><a>this page!</a></Link>
```

该[`Link`](https://www.nextjs.cn/docs/api-reference/next/link)组件支持在同一个 Next.js 应用程序中的两个页面之间进行**客户端导航**。

客户端导航意味着页面转换*使用 JavaScript 进行*，这比浏览器执行的默认导航更快。



该[`Link`](https://www.nextjs.cn/docs/api-reference/next/link)组件支持在同一个 Next.js 应用程序中的两个页面之间进行**客户端导航**。

客户端导航意味着页面转换*使用 JavaScript 进行*，这比浏览器执行的默认导航更快。

这是您可以验证的简单方法：

- 使用浏览器的开发人员工具将`background`CSS 属性更改`<html>`为`yellow`。
- 单击链接可在两个页面之间来回切换。
- 您会看到黄色背景在页面转换之间持续存在。

这表明浏览器*未*加载完整页面并且客户端导航正在工作。

<img src="https://www.nextjs.cn/static/images/learn/navigate-between-pages/client-side.gif" alt="Links" style="zoom:50%;" />

如果您使用了`<a href="…">`代替`<Link href="…">`并执行了此操作，则链接点击时背景颜色将被清除，因为浏览器会完全刷新。

### 动态路由

Next.js 支持具有动态路由的 pages（页面）。例如，如果你创建了一个命名为 `pages/posts/[id].js` 的文件，那么就可以通过 `posts/1`、`posts/2` 等类似的路径进行访问。

- `pages/blog/[slug].js` → `/blog/:slug` (`/blog/hello-world`)
- `pages/[username]/settings.js` → `/:username/settings` (`/foo/settings`)
- `pages/post/[...all].js` → `/post/*` (`/post/2020/id/title`)

### 代码拆分和预取

Next.js 会自动进行代码拆分，因此每个页面只加载该页面所需的内容。这意味着在呈现主页时，最初不会提供其他页面的代码。

这可确保即使您添加数百个页面，主页也能快速加载。

仅加载您请求的页面的代码也意味着页面变得孤立。如果某个页面抛出错误，应用程序的其余部分仍然可以工作。

此外，在 Next.js 的生产版本中，每当[`Link`](https://www.nextjs.cn/docs/api-reference/next/link)组件出现在浏览器的视口中时，Next.js 都会在后台自动**预取**链接页面的代码。当您单击链接时，目标页面的代码已在后台加载，页面转换将近乎即时！

### HTML

#### html

`<Head>`使用 代替小写字母`<head>`。`<Head>`是一个内置于 Next.js 的 React 组件。它允许您修改`<head>`页面的名称。

```
import Head from 'next/head'
```

```html
<Head>
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <meta charSet="utf-8" />
        <meta name="Description" content={props.description}></meta>
        <title>{props.title}</title>
</Head>
```

#### img

```
img统一放在public中，引用直接引用img，不需要添加图像路径
src="/head.jpg"
```

#### css

```
<style jsx>{`
  …
`}</style>
```

这是使用一个名为[styled-jsx](https://github.com/vercel/styled-jsx)的库。它是一个“CSS-in-JS”库——它允许你在 React 组件中编写 CSS，并且 CSS 样式将被*限定*（其他组件不会受到影响）。

Next.js 内置了对[styled-jsx 的](https://github.com/vercel/styled-jsx)支持，但您也可以使用其他流行的 CSS-in-JS 库。我用的是materialUI框架中的css-in-js

- 全局样式

  如果你希望**每个页面**都加载一些 CSS，添加pages/_app.js文件

  ```
  import '../styles/global.css'
  export default function App({ Component, pageProps }) {
    return <Component {...pageProps} />
  }
  ```

  创建一个顶级styles目录并global.css在里面创建。将其导入pages/_app.js

## 内置API

某些页面需要获取外部数据以进行预渲染。有两种情况，在每种情况下，您都可以使用 Next.js 提供的特殊功能：

1. 您的页面 **内容** 取决于外部数据：使用 `getStaticProps`。
2. 你的页面 **paths（路径）** 取决于外部数据：使用 `getStaticPaths` （通常还要同时使用 `getStaticProps`）。

**getStaticProps**函数在**构建时**被调用，并允许你在预渲染时将获取的数据作为 `props` 参数传递给页面。**getStaticProps不会在页面组件中生效**

Next.js 允许你创建具有 **动态路由** 的页面。例如，你可以创建一个名为 `pages/posts/[id].js` 的文件用以展示以 `id` 标识的单篇博客文章。当你访问 `posts/1` 路径时将展示 `id: 1` 的博客文章。但是，在构建 `id` 所对应的内容时可能需要从外部获取数据。**getStaticPaths**函数在构建时被调用，并允许你指定要预渲染的路径。

```jsx
// 此函数在构建时被调用
export async function getStaticPaths() {
  // 调用外部 API 获取博文列表
  const res = await fetch('https://.../posts')
  const posts = await res.json()

  // 据博文列表生成所有需要预渲染的路径
  const paths = posts.map((post) => ({
    params: { id: post.id },
  }))

  // We'll pre-render only these paths at build time.
  // { fallback: false } means other routes should 404.
  return { paths, fallback: false }
}
```

为了让页面使用服务端渲染，你需要导出 getServerSideProps 异步函数。这个函数将在**每次请求**时在服务端被调用。例如，假设你的页面需要用最新的数据预渲染（通过外部的 api 获取数据）。你应该写下 getServerSideProps 来获取数据传递给 Page。

getServerSideProps 和 getStaticProps 很像，但是区别的是，getServerSideProps 是每个请求都会调用而不是在构建时。

## mardown解析

### 插件

https://dev.to/imranib/build-a-next-js-markdown-blog-5777

- [react-markdown](https://www.npmjs.com/package/react-markdown)将帮助我们解析和渲染 Markdown 文件

- 代码格式化：`react-syntax-highlighter`包

- gray-matter](https://www.npmjs.com/package/react-markdown) 将解析我们博客的*顶部内容*。（文件顶部的部分`---` ）

  我们需要这样的元数据`title`，`data` 并`description`和`slug`。您可以在此处添加任何您喜欢的内容

| 参数        | 意义         |
| ----------- | ------------ |
| slug        | 导航的参数   |
| title       | 文章名称     |
| data        | 最新时间     |
| updated     | 文章更新日期 |
| tags        | 文章標籤     |
| category    | 文章分類     |
| description | 文章描述     |

- [raw-loader](https://www.npmjs.com/package/raw-loader)将帮助我们导入我们的markdown文件。 

### 流程

https://dev.to/imranib/build-a-next-js-markdown-blog-5777

https://thetombomb.com/posts/adding-code-snippets-to-static-markdown-in-Next%20js

## Tips

- material,classname报错，每次刷新，material失去效果。添加_app.js和__document.js文件



