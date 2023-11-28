# React钩子


{{< music auto="https://music.163.com/#/playlist?id=60198" >}}

平常函数式组件用的多一点,React Hooks 是 React 团队在两年前的 16.8 版本推出的一套全新的机制,它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。

## 几个内置Hooks的作用以及使用

### useState ：让函数组件具有维持状态的能力

```js
const[count, setCount]=useState(0);
```

**优点：**

**让函数组件具有维持状态的能力**，即：在一个函数组件的多次渲染之间，这个 state 是**共享**的。便于维护状态。

### **缺点：**

**一旦组件有自己状态，意味着组件如果重新创建，就需要有恢复状态的过程，这通常会让组件变得更复杂。**

**用法：**

1. useState(initialState) 的参数 initialState 是创建 state 的初始值。

> 它可以是任意类型，比如数字、对象、数组等等。
1. useState() 的返回值是一个有着两个元素的数组。第一个数组元素用来读取 state 的值，第二个则是用来设置这个 state 的值。

> 在这里要注意的是，state 的变量（例子中的 count）是只读的，所以我们必须通过第二个数组元素 setCount 来设置它的值。

1. 如果要创建多个  **state** ，那么我们就需要多次调用 useState。

**什么样的值应该保存在 state 中？**

通常来说，我们要遵循的一个原则就是：**state 中不要保存可以通过计算得到的值** 。

- 从 props 传递过来的值。有时候 props 传递过来的值无法直接使用，而是要通过一定的计算后再在 UI 上展示，比如说排序。那么我们要做的就是每次用的时候，都重新排序一下，或者利用某些 cache 机制，而不是将结果直接放到 state 里。
- 从 URL 中读到的值。比如有时需要读取 URL 中的参数，把它作为组件的一部分状态。那么我们可以在每次需要用的时候从 URL 中读取，而不是读出来直接放到 state 里。
- 从 cookie、localStorage 中读取的值。通常来说，也是每次要用的时候直接去读取，而不是读出来后放到 state 里。

useEffect：执行副作用

> useEffect(fn, deps);

useEffect ，顾名思义，用于执行一段副作用。

### 什么是副作用？

通常来说，副作用是指一段和当前执行结果无关的代码。比如说要修改函数外部的某个变量，要发起一个请求，等等。

也就是说，在函数组件的当次执行过程中， **useEffect**  **中代码的执行是不影响渲染出来的**  **UI** 的。

对应到 Class 组件，那么 useEffect 就涵盖了 ComponentDidMount、componentDidUpdate 和 componentWillUnmount 三个生命周期方法。不过如果你习惯了使用 Class 组件，那千万不要按照把 useEffect 对应到某个或者某几个生命周期的方法。你只要记住，useEffect 是每次组件 render 完后判断依赖并执行就可以了。

useEffect 还有两个特殊的用法：没有依赖项，以及依赖项作为空数组。我们来具体分析下。

1. 没有依赖项，则每次 render 后都会重新执行。例如：

```javascript
javascript
复制代码useEffect(() => {
  // 每次 render 完一定执行
  console.log('渲染...........');
});
```

1. 空数组作为依赖项，则只在首次执行时触发，对应到 Class 组件就是 componentDidMount。例如：

```scss
scss
复制代码useEffect(() => {
  // 组件首次渲染时执行，等价于 class 组件中的 componentDidMount
  console.log('did mount........');
}, []);
```

### 小结用法:

总结一下，useEffect 让我们能够在下面四种时机去执行一个回调函数产生副作用：

1. 每次 render 后执行：不提供第二个依赖项参数。

> 比如useEffect(() => {})。

1. 仅第一次 render 后执行：提供一个空数组作为依赖项。

> 比如useEffect(() => {}, [])。

1. 第一次以及依赖项发生变化后执行：提供依赖项数组。

> 比如useEffect(() => {}, [deps])。

1. 组件 unmount 后执行：返回一个回调函数。

> 比如useEffect() => { return () => {} }, [])。

useCallback：缓存回调函数

> useCallback(fn, deps)

### 为什么要使用useCallback?

在 React 函数组件中， **每一次**  **UI**  **的变化，都是通过重新执行整个函数来完成的** ，这和传统的 Class 组件有很大区别：函数组件中并没有一个直接的方式在多次渲染之间维持一个状态。

```javascript
javascript
复制代码function Counter() {
  const [count, setCount] = useState(0);
  const handleIncrement = () => setCount(count+1);
  return <button onClick={handleIncrement}>+</button>
}
```

思考下这个过程。 **每次组件状态发生变化的时候，函数组件实际上都会重新执行一遍** 。在每次执行的时候，实际上都会创建一个新的事件处理函数  **handleIncrement** 。

这也意味着，即使 count 没有发生变化，但是函数组件因为其它状态发生变化而重新渲染时（函数组件重新被执行），这种写法也会每次创建一个新的函数。创建一个新的事件处理函数，虽然不影响结果的正确性，但其实是没必要的。因为这样做不仅增加了系统的开销，更重要的是： **每次创建新函数的方式会让接收事件处理函数的组件，需要重新渲染** 。

比如这个例子中的 button 组件，接收了 handleIncrement ，并作为一个属性。如果每次都是一个新的，那么这个 React 就会认为这个组件的 props 发生了变化，从而必须重新渲染。因此，我们需要做到的是： **只有当**  **count**  **发生变化时，我们才需要重新定一个回调函数** 。而这正是 useCallback 这个 Hook 的作用。

```javascript
javascript
复制代码import React, { useState, useCallback } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  const handleIncrement = useCallback(
    () => setCount(count + 1),
    [count], // 只有当 count 发生变化时，才会重新创建回调函数
  );
  return <button onClick={handleIncrement}>+</button>
}
```

useMemo：缓存计算的结果

```js
js
复制代码useMemo(fn, deps);
```

> useCallback(fn, deps) 相当于 useMemo(() => fn, deps)。

这里的 fn 是产生所需数据的一个 **计算函数** 。通常来说， **fn**  **会使用**  **deps**  **中声明的一些变量来生成一个结果，用来渲染出最终的**  **UI** 。

这个场景应该很容易理解：如果某个 **数据** 是通过其它数据计算得到的，那么只有当用到的数据，也就是依赖的数据发生变化的时候，才应该需要重新计算。

### 避免重复计算

> 通过  **useMemo**  这个 Hook，可以避免在用到的数据没发生变化时进行的重复计算。虽然例子展示的是一个很简单的场景，但如果是一个复杂的计算，那么对于 **提升性能** 会有很大的帮助。

举个例子：

```css
css
复制代码const calc = (a, b) => {
    // 假设这里做了复杂的计算，暂时用次幂模拟
    return a ** b;
}
const MyComponent = (props) => {
    const {a, b} = props;
    const c = calc(a, b);
    return <div>c: {c}</div>;
}
```

如果 calc 计算耗时 1000ms，那么每次渲染都要等待这么久，怎么优化呢？

> a, b 值不变的情况下，得出的 c 定是相同的。

所以我们可以用 useMemo 把值给缓存起来，避免重复计算相同的结果。

```javascript
javascript
复制代码const calc = (a, b) => {
    // 假设这里做了复杂的计算，暂时用次幂模拟
    return a ** b;
}
const MyComponent = (props) => {
    const {a, b} = props;
    // 缓存
    const c = React.useMemo(() => calc(a, b), [a, b]);
    return <div>c: {c}</div>;
}
```

useCallback 的功能其实是可以用 useMemo 来实现的:

```javascript
javascript
复制代码 const myEventHandler = useMemo(() => {
   // 返回一个函数作为缓存结果
   return () => {
     // 在这里进行事件处理
   }
 }, [dep1, dep2]);
```

### 小结一下：

**感觉到这有这种感觉，其实 hook 就是建立了一个绑定某个结果到依赖数据的关系。只有当依赖变了，这个结果才需要被重新得到。**

useRef：在多次渲染之间共享数据

```ini
ini
复制代码const myRefContainer =useRef(initialValue);
```

我们可以把  **useRef**  看作是在函数组件之外创建的一个容器空间。在这个容器上，我们可以通过唯一的 current 属设置一个值，从而在函数组件的多次渲染之间共享这个值。

### useRef 的重要的功能

**1.**  **存储跨渲染的数据**

使用  **useRef**  保存的数据一般是和 UI 的渲染无关的，因此当  **ref**  的值发生变化时，是不会触发组件的重新渲染的，这也是  **useRef**  区别于  **useState**  的地方。

举例：

```scss
scss
复制代码 const [time, setTime] = useState(0);
 // 定义 timer 这样一个容器用于在跨组件渲染之间保存一个变量 
 const timer = useRef(null);

  const handleStart = useCallback(() => {
    // 使用 current 属性设置 ref 的值
    timer.current = window.setInterval(() => { setTime((time) => time + 1); }, 100);
  }, []);
```

**2.**  **保存某个**  **DOM**  **节点的引用**

> 是在某些场景中，我们必须要获得真实 **DOM** 节点的引用，所以结合 React 的  **ref**  属性和  **useRef**  这个 Hook，我们就可以获得真实的 DOM 节点，并对这个节点进行操作。

React 官方例子：

```javascript
javascript
复制代码function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // current 属性指向了真实的 input 这个 DOM 节点，从而可以调用 focus 方法
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```

**理解：**

可以看到ref 这个属性提供了获得 DOM 节点的能力，并利用 useRef 保存了这个节点的应用。这样的话，一旦 input 节点被渲染到界面上，那我们通过 inputEl.current 就能访问到真实的 DOM 节点的实例了

### useContext：定义全局状态

### 为什么要使用 useContext？

React 组件之间的状态传递只有一种方式，那就是通过 props。缺点： **这种传递关系只能在父子组件之间进行。**

那么问题出现：跨层次，或者同层的组件之间要如何进行数据的共享？这就涉及到一个新的命题： **全局状态管理** 。

react提供的解决方案： **Context**  机制。

### 具体原理：

React 提供了 Context 这样一个机制， **能够让所有在某个组件开始的组件树上创建一个**  **Context** 。这样这个组件树上的所有组件，就都能访问和修改这个 **Context** 了。

那么在函数组件里，我们就可以使用 useContext 这样一个 Hook 来管理 Context。

### 使用：（这儿用了官方例子）

```javascript
javascript
复制代码const themes = {
  light: {
    foreground: "#000000",
    background: "#eeeeee"
  },
  dark: {
    foreground: "#ffffff",
    background: "#222222"
  }
};
// 创建一个 Theme 的 Context

const ThemeContext = React.createContext(themes.light);
function App() {
  // 整个应用使用 ThemeContext.Provider 作为根组件
  return (
    // 使用 themes.dark 作为当前 Context 
    <ThemeContext.Provider value={themes.dark}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

// 在 Toolbar 组件中使用一个会使用 Theme 的 Button
function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

// 在 Theme Button 中使用 useContext 来获取当前的主题
function ThemedButton() {
  const theme = useContext(ThemeContext);
  return (
    <button style={{
      background: theme.background,
      color: theme.foreground
    }}>
      I am styled by theme context!
    </button>
  );
}
```

### 优点：

**Context**  提供了一个方便在多个组件之间共享数据的机制。

### 缺点：

Context 相当于提供了一个定义 React 世界中全局变量的机制，而全局变量则意味着两点：

**1.**  **会让调试变得困难，因为你很难跟踪某个**  **Context**  **的变化究竟是如何产生的。**

**2.**  **让组件的复用变得困难，因为一个组件如果使用了某个**  **Context** ，它就必须确保被用到的地方一定有这个 **Context** 的 **Provider** 在其父组件的路径上。

**实际应用场景**

由于以上缺点，所以在 React 的开发中，除了像 Theme、Language 等一目了然的需要全局设置的变量外），我们很少会使用 Context 来做太多数据的共享。需要再三强调的是，Context 更多的是提供了一个强大的机制，让 React 应用具备定义全局的响应式数据的能力。

> 此外，很多状态管理框架，比如 Redux，正是利用了 Context 的机制来提供一种更加可控的组件之间的状态管理机制。因此，理解 Context 的机制，也可以让我们更好地去理解 Redux 这样的框架实现的原理。

### 最后

其实了解学会了useState 和 useEffect 这两个 核心 Hooks，基本能完成绝大多数 React 功能的开发了。

useCallback、useMemo、useRef 和 useContext。这几个 Hook 都是为了解决函数组件中遇到的特定问题。


