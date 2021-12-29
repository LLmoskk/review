## React类组件和函数组件之间有什么区别？

react的函数组件hooks写法是在react16.8后引入的，函数式写法是官方更加推荐的新写法。

常用的hooksAPI有useState与useEffect，useState用来替代类组件中的this.setState,而useEffect则是替代了class组件中的生命周期，例如componentDidMount与componentDidUpdate与componentWillUnmount。

useEffect副作用分为两种情况:需要清除的与不需要清除的。不需要清除的情况例如调接口，操作DOM之类的。需要清除的情况是例如订阅了外部的数据源，不希望内存泄漏，就需要用到useEffect的清除功能，在useEffect中返回一个函数，这是effect的可选清除机制，等同于class组件中的componentWillUnmount。

useEffect 会在每次渲染后都执行，但通常页面中有未发生变化的元素，class组件中我们可以通过在Update生命周期对比是否发生变化，来判断需不需要重新渲染，useEffect中可以很方便的进行控制，就是第二个参数，在方括号内写入要监听变化的值，空的话表示在初始时只触发一次。

## 结合生命周期，谈一谈React类组件的运行机制。

生命周期分为三个周期,两个阶段  挂载时，更新时，卸载时。render阶段与commit阶段。

首先是挂载时，先调用构造函数constructor(),若是不初始化state或不进行方法绑定，则不需要实现构造函数。在constructor内应该先其他语句之前调用super(props),用来传递props。构造函数的作用主要是用来初始化内部state、为事件处理函数绑定实例。

接下来说一下挂载与更新时都有的render(),这是class组件中唯一必须实现的方法，render函数是一个纯函数，纯函数的意思就是不改变入参，返回值永远是不变的。在不修改组件 state 的情况下，每次调用时都返回相同的结果。

一般都是在componentDidMount，DidUpdate，WillUnmount做操作。Mount阶段调接口，Unmount阶段清除外部订阅。Update会在更新后被立即调用，首次渲染不会调用，一般在这里做setState的操作，有一点要注意的是它必须被包裹在一个条件语句里，否则会造成死循环。

## React中组件间通信，你有哪些方案?

**父子组件通信：**react中数据流是单向的，父组件通过props传递给子组件。子组件向父组件通信通过回调函数，父组件将一个函数作为props传递给子组件，子组件中调用该函数。

**跨级组件通信：**

1、通过props层层传递（过于繁琐） 

2、使用Context，为了避免繁琐的props传递，可以使用Context 共享那些对于一个组件树而言是“全局”的数据，例如主题或语言。

**非嵌套组件间通信：** 状态提升，可以通过寻找共同的父组件来传值

**引入状态管理第三方库：**可以使用状态管理工具，例如常见的redux，mobx

## 谈一谈props和state之间的区别。

首先要明确props是只读的,对外的，state是可读可写的,对内的。

我们可以在props上挂东西来传递给子组件。

state是组件管理自身的数据，具有响应式特性，当state被this.setState()修改时，视图会自动更新

## this.setState()有什么特点？

this.setState()是用在class组件中的，分为两种情况 

**旧值与新值有关：**使用this.setState(fn,callback)语法，因为需要读取旧值 例：this.setState(state=>({count:state.count+1}))

**旧值与新值无关：**使用this.setState({},callback)语法，直接修改为新值  例：this.setState({name:'abc'})

this.setState 默认是异步的，因为如果开发者多次调用this.setState()，如果它同步的，这会导致多次render()，这显然是一种性能损耗。所以官方把this.setState设置为异步的可以浅合并的，以节省性能。

this.setState第二个参数callback是用来表示异步工作是否完成的

## 有哪些定义React组件的方式？

定义react组件的方式有两种，类组件与函数式组件。JSX就是对于React.createClass方法的语法糖

```js
// React.createClass方法
function HelloComponent(props) /* context */{ 
  return React.createElement( 
    "div", 
    null, 
    "Hello ", 
    props.name 
  ); 
} 
```

在react hooks出来之前，函数式组件被称为无状态组件，只用来返回视图。在react16.8之前用的基本上是类组件 继承React.componet或React.purecomponent创建。

## 谈一谈你对React受控组件的理解。

受控组件例如一个input输入框，我们给他的vlaue绑定上state内的值，这时候我们在input试着输入时会发现无法输入，也就是被受控了，想要解除受控就给input一个onchange事件输入时触发相对应的函数。大部分情况推荐使用受控组件来实现表单。

## 什么是组合？组合与继承在实现组件复用上的差别。

## 谈一谈你对JSX的理解。

jsx是JavaScript的语法扩展。一种把视图与逻辑集合在一个文件内。在 JSX 语法中，可以在大括号内放置任何有效的 JavaScript 表达式。JSX可以防XSS攻击，因为所有内容在渲染前都会经过转义。

## 描述 Diff运算过程，如何比较两个虚拟DOM的差异？

当对比两棵树时，React首先比较两棵树的根节点。

**对比不同类型的元素：** 当根节点为不同类型的元素时，React会拆卸原有的树并建立新的树。例如a标签变化为img标签，button变成了div都会触发完整的从建流程。

**对比同一类型的元素：**同一类型的元素指的就是标签不变，class或style发生改变。React会保留DOM节点，仅仅更新有所变更的属性。



## 谈一谈 React Context 上下文的实践应用

## 高阶组件有哪些应用场景？

## 常用 React Hooks API 及其作用、特点。

- **useState**

  `setState` 函数用于更新 state。它接收一个新的 state 值并将组件的一次重新渲染加入队列。

  在后续的重新渲染中，`useState` 返回的第一个值将始终是更新后最新的 state。

  如果你的更新函数返回值与当前 state 完全相同，则随后的重渲染会被完全跳过。

  与 class 组件中的 `setState` 方法不同，`useState` 不会自动合并更新对象。你可以用函数式的 `setState` 结合展开运算符来达到合并更新对象的效果。

- **useEffect**

  在函数组件主体内（这里指在 React 渲染阶段）改变 DOM、添加订阅、设置定时器、记录日志以及执行其他包含副作用的操作都是不被允许的，因为这可能会产生莫名其妙的 bug 并破坏 UI 的一致性。

  使用 `useEffect` 完成副作用操作。赋值给 `useEffect` 的函数会在组件渲染到屏幕之后执行。你可以把 effect 看作从 React 的纯函数式世界通往命令式世界的逃生通道。

  通常，组件卸载时需要清除 effect 创建的诸如订阅或计时器 ID 等资源。要实现这一点，`useEffect` 函数需返回一个清除函数。

  如果组件多次渲染，则**在执行下一个 effect 之前，上一个 effect 就已被清除**。

  `useEffect` 的函数会在浏览器完成布局与绘制**之后**，在一个延迟事件中被调用。防止阻塞浏览器对屏幕的更新。若一个对用户可见的 DOM 变更就必须在浏览器执行下一次绘制前被同步执行，可以使用 [`useLayoutEffect`](https://zh-hans.reactjs.org/docs/hooks-reference.html#uselayouteffect) Hook 来处理这类 effect。

- **useContext**

  接收一个 context 对象（`React.createContext` 的返回值）并返回该 context 的当前值。当前的 context 值由上层组件中距离当前组件最近的 `<MyContext.Provider>` 的 `value` prop 决定。

- **useReducer**

  [`useState`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usestate) 的替代方案。它接收一个形如 `(state, action) => newState` 的 reducer，并返回当前的 state 以及与其配套的 `dispatch` 方法。类似redux

- **useMemo**

  作为性能优化的手段，但不要把它当成语义上的保证

## 封装自定义Hooks，有哪些注意事项。react-use（自定义hooks库）

## React技术栈中，有哪些代码复用的技巧？

## React中的refs作用是什么？

## 从React的角度，说一说有哪些性能优化策略。

- **避免使用内联函数**

  我们应该在组件内部创建一个函数，并将事件绑定到该函数本身。这样每次调用 `render` 时就不会创建单独的函数实例

- **组件懒加载**

   ```js
  const johanComponent = React.lazy(() => import(/* webpackChunkName: "johanComponent"));
   
  export const johanAsyncComponent = props => (
    <React.Suspense fallback={<Spinner />}>
      <johanComponent {...props} />
    </React.Suspense>
  );
  ```

- **服务端渲染**

- **用碎片语法（<></>）代替div**

## 谈一谈你对React服务端渲染的理解

## react-router常用组件有哪些，分别有什么用？

## withRouter有什么用？如何自定义封装withRouter？

## 什么是动态路由？如何使用路由传参？什么是嵌套路由？什么场景下会用到嵌套路由？

## mobx中有哪些核心概念？常用的修饰器方法有哪些？

## 说一下redux的工作流程，谈一谈你对redux数据流的理解。

## 谈一谈mobx和redux的区别、使用场景。

## react常用生命周期，及其特点、使用场景。

挂载阶段：constructor =>  render => componentDidMount => 结束
更新阶段： render => componentDidUpdate => 结束
卸载阶段：componentWillUnmount => 结束

- **constructor**
  构造函数用于以下两种情况：

  ​		1.通过给 this.state 赋值对象来初始化内部 state。this.state = {prop: val, ...}
  ​	     2.为事件处理函数绑定实例 this.handler = this.handler.bind(this);
  如果不初始化 state 或不进行方法绑定，则不需要为 React 组件实现构造函数。
  super(props)应写在该方法的最前面;否则，this.props在构造函数中可能会出现未定义的bug

- **render**
  
  render()方法是class组件中唯一必须实现的方法。
  返回以下类型之一：React元素, 数组或fragments, Portals, 字符串或数值类型, 布尔类型或null
render()函数应该为纯函数
  
- **componentDidMount()**

  会在组件挂载后(插入DOM树中)立即调用

  进行依赖于DOM节点的初始化，如需通过网络请求获取数据，此处是实例化请求的好地方。
  添加订阅的地方。如果添加了订阅。不要忘记在 componentWillUnmount() 里取消订阅
  可以在 componentDidMount()里可以直接调用setState()。将触发额外渲染，会导致性能问题

- **componentDidUpdate()**
  componentDidUpdate(prevProps, prevState, snapshot) 有三个默认参数
  componentDidUpdate() 会在更新后会被立即调用。首次渲染不会执行此方法。
  可以在此处对 DOM 进行操作。可以选择在此处进行网络请求。
  可以在 componentDidUpdate()中直接调用 setState()，但请注意它必须被包裹在一个条件语件里，否则会导致死循环。导致额外的重新渲染，会影响组件性能。
  如果组件实现了getSnapshotBeforeUpdate(), 则它的返回值将作为 componentDidUpdate() 的第三个参数 “snapshot” 参数传递。否则此参数将为 undefined。

- **componentWillUnmount()**
  componentWillUnmount() 会在组件卸载及销毁之前直接调用。在此方法中执行必要的清理操作，例如，清除 timer，取消网络请求或清除在 componentDidMount() 中创建的订阅等。
  componentWillUnmount() 中不应调用 setState()，因为该组件将永远不会重新渲染。组件实例卸载后，将永远不会再挂载它。

## react严格模式下有哪些限制

注：严格模式检查仅在开发模式下运行，它们不会影响生产构建。

- **识别不安全的生命周期**

  对于过时的生命周期方法报出警告

- **关于使用过时字符串 ref API 的警告**

  使用新增加的 `createRef` API，避免使用回调 ref ，虽然它依旧适用。

- **检测意外的副作用**

  严格模式不能自动检测到你的副作用，但它可以帮助你发现它们，使它们更具确定性。通过故意重复调用以下函数来实现的该操作：

  - class 组件的 `constructor`，`render` 以及 `shouldComponentUpdate` 方法
  - class 组件的生命周期方法 `getDerivedStateFromProps`
  - 函数组件体
  - 状态更新函数 (即 `setState` 的第一个参数）
  - 函数组件通过使用 `useState`，`useMemo` 或者 `useReducer`

- **检测过时的 context API**

- **关于使用废弃的 findDOMNode 方法的警告**

## 什么Fiber？谈一谈你对React Fiber架构的理解。

Fiber 是 React 16 中新的协调引擎。它的主要目的是使虚拟DOM可以进行增量式渲染。

就是一种能让React视图更新过程变得更加流畅顺滑的处理手法。

https://juejin.cn/post/7006612306809323533

## 什么是redux中间件？常用的有哪些？谈一谈redux-saga、redux-thunk的理解。

## 什么是React合成事件？为什么存在合成事件？有什么特点？

## 以你的做过一个react项目为例，说一说如何优雅地地实践组件化和数据流管理？