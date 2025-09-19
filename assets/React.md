# React

React是一个用于 **构建Web和原生交互界面的库**

官方中文网站: https://zh-hans.react.dev/learn/installation

------

[TOC]

------



## 一、构建工具

使用**Vite**构建工具创建 React项目

```bash
# my-app 是项目名称,可自行修改
npm create vite@latest my-app -- --template react
cd my-app
npm install
npm run dev
```

使用 `create-react-app` 脚手架工具快速创建React项目

```bash
npx create-react-app my-app
cd my-app
npm start
```



## 二、UI组件库

### Ant Design

官方网站: https://ant.design/index-cn

Ant Design 是一款由蚂蚁集团推出的企业级 React UI 组件库，它基于一套精心设计的设计体系，旨在帮助开发者高效构建高质量的中后台产品。

#### 使用方法

1. 在Vite中使用

   ```bash
   #使用之前需要先创建一个React项目
   npm install antd --save
   
   #--save 的作用: npm会自动把 antd 及其安装的版本号写入 dependencies，这样其他人克隆你的项目后，只需运行 npm install，就能自动安装包括 antd 在内的所有依赖
   #在 npm 5.0 及以上版本中，--save 已经是默认行为了,可省略
   ```

2. 在Next.js中使用

3. 在Umi中使用

4. 在 Rsbuild中使用

5. 在Farm中使用



## 三、JSX基础

### (一) JSX是什么

**JSX（JavaScript XML）** 是一种 JavaScript 的语法扩展，它允许你在 JavaScript 代码中编写类似 HTML 的结构。它并不是字符串，也不是 HTML，而是一种描述 UI 应该长什么样的语法格式。

简单来说，JSX 就是让你在 JavaScript 里写 HTML。

### (二) JSX 的本质

JSX 只是一种**语法糖**。浏览器本身并不认识 JSX，它需要通过构建工具（如 Babel）被编译成标准的 JavaScript。

Babel 会把 JSX 编译成 `React.createElement()` 函数调用。

### (三) JSX 的基本用法

#### 1. 看起来像 HTML

JSX 看起来和 HTML 几乎一模一样。

```jsx
const element = <h1 className="greeting">Hello, World!</h1>;
```

**注意**：因为 `class` 是 JavaScript 中的关键字，所以在 JSX 中要用 `className` 来替代 HTML 中的 `class` 属性。

#### 2. 嵌入 JavaScript 表达式

这是 JSX 最强大的特性。你可以在 JSX 中使用大括号 `{}` 嵌入任何有效的 JavaScript 表达式（如变量、函数调用、计算等）。

```jsx
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>; // 输出: Hello, Josh Perez

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const element = (
  <h1>
    Hello, {formatName(user)}! 
  </h1>
); // 输出: Hello, Harper Perez!
```

#### 3. 指定属性

你可以使用引号来将属性值指定为字符串字面量，也可以用大括号 `{}` 来在属性值中插入一个 JavaScript 表达式。

```jsx
// 字符串字面量
const element = <div tabIndex="0"></div>;

// JavaScript 表达式
const element = <img src={user.avatarUrl}></img>;
```

#### 4. 只有一个根元素

JSX 表达式必须有一个根元素。通常用 `<div>` 或 `<>`（Fragment，片段）来包裹。

```jsx
// 错误！两个同级元素
const element = (
  <h1>Hello</h1>
  <h2>World</h2>
);

// 正确！用一个 div 包裹
const element = (
  <div>
    <h1>Hello</h1>
    <h2>World</h2>
  </div>
);

// 正确！使用空标签（Fragment），它不会在最终HTML中创建真实DOM节点
const element = (
  <>
    <h1>Hello</h1>
    <h2>World</h2>
  </>
);
```

#### JSX中使用JavaScript表达式

**注意**:if 语句、switch 语句、变量声明属于语句，不是表达式，不能出现在 {} 中

```jsx
function App() {
  const number=10
  const getNumber=()=>{
    return 10
  }
  return (
    <>
      <h1>Hello Vite + React!</h1>
      {/* {使用引号传递字符串} */}
      {'hello world'}
      {/* 识别js变量 */}
      {number}
      {/* 函数调用 */}
      {getNumber()}
      {/* 方法调用 */}
      {new Date().toLocaleDateString()}
      {/* 使用js对象 */}
      <div style={{color: 'blue'}}>Hello</div>
    </>
  )
}

export default App
```

#### 渲染列表

在 React 中使用 [`filter()`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) 筛选需要渲染的组件和使用 [`map()`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array/map) 把数组转换成组件数组。

```jsx
function App() {
  const people = [{
    id: 0,
    name: '凯瑟琳·约翰逊',
    profession: '数学家',
  }, {
    id: 1,
    name: '马里奥·莫利纳',
    profession: '化学家',
  }, {
    id: 2,
    name: '穆罕默德·阿卜杜勒·萨拉姆',
    profession: '物理学家',
  }, {
    id: 3,
    name: '珀西·莱温·朱利亚',
    profession: '化学家',
  }];

  const chemists = people.filter(person =>
    person.profession === '化学家'
  );
  const listItems = chemists.map(person =>
  <li key={person.id}>
     <p>
       <b>{person.name}:</b>
       {' ' + person.profession + ' '}
       因{person.accomplishment}而闻名世界
     </p>
  </li>
  )
  return <ul>{listItems}</ul>;
}

export default App
```

#### 条件渲染

在 React 中，你可以通过使用 JavaScript 的 `if` 语句、`&&` 和 `? :` 运算符来选择性地渲染 JSX。

```jsx
//使用if 语句
function Item({ name, isPacked }) {
  if (isPacked) {
    return null;
  }
  return <li className="item">{name}</li>;
}

//使用 && 运算符
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {name} {isPacked && '✅'}
    </li>
  );
}

//使用? : 运算符
function Item({ name, isPacked }) {
    return (
      <li className="item">
        {isPacked ? name + ' ✅' : name}
      </li>
    );
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride 的行李清单</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="宇航服" 
        />
        <Item 
          isPacked={true} 
          name="带金箔的头盔" 
        />
        <Item 
          isPacked={false} 
          name="Tam 的照片" 
        />
      </ul>
    </section>
  );
}

```

## 四、React基础

### (一) React中的事件绑定

#### 1.基础事件绑定

on + 事件名称 = { 事件处理函数 } ，遵循驼峰命名法

```jsx
function App() {
  const handleClick=()=>{
    console.log("点击");
  }
  return (
    <div className="App">
      <button onClick={ handleClick }>React</button>
    </div>
  )
}

export default App;
```

#### 2.获取事件对象参数`e`

```jsx
function App() {
    //***
  const handleClick=(e)=>{
    console.log("点击",e);
  }
  return (
    <div className="App">
      <button onClick={ handleClick }>React</button>
    </div>
  )
}

export default App;
```

#### 3.传递自定义参数

```jsx
function App() {
  const handleClick=(id)=>{
    console.log("点击",id);
  }
  return (
    <div className="App">
          {/* 重点: ()=>handleClick(2) */}
      <button onClick={ ()=>handleClick(2) }>React</button>
    </div>
  )
}

export default App;
```

#### 4.同时传递事件对象和自定义参数

**注意:** 参数顺序要对应,可交换顺序

```jsx
function App() {
  const handleClick=(e,id)=>{
    console.log("点击",e,id);
  }
  return (
    <div className="App">
          {/* 重点: (e)=>handleClick(e,2) */}
      <button onClick={(e)=>handleClick(e,2)}>React</button>
    </div>
  )
}

export default App;

```

### (二) 表单绑定

#### 快速上手

```jsx
import { useState } from 'react';
function App() {
  const [value, setValue] = useState('')
  return (
    <div className={'app'}>
      <input value={value} onChange={(e) => setValue(e.target.value)} /> 
    </div>
  )
}

export default App;
```



### (三) React组件使用

在React中，一个**首字母大写的函数**就是一个组件,内部存放了组件的逻辑和视图UI，渲染组件只需要把组件当成标签书写即可。

**注意**: 组件名称必须**首字母大写**

#### 快速上手

```jsx
//写法一:
//const Button = () => {
//  return <button>按钮</button>
//}

//写法二:
function Button() {
  return <button>按钮</button>
}

function App() {
  return (
    <div className="App">
      <Button/>
    </div>
  )
}

export default App;
```



### (四) React中获取DOM

使用`useRef`钩子函数

#### 快速上手

```jsx
import { useRef } from 'react';

function App() {
  const inputRef = useRef(null);
  return (
    <div className={'app'}>
      <input ref={inputRef} />
      <button onClick={() => {
        if (inputRef.current) {
          console.dir(inputRef.current);
          inputRef.current.value = 'Hello, World!';
        }
      }
      }>
        按钮
      </button>
    </div>
  )
}

export default App;
```

**注意:**

虽然`useRef(null)`和`useRef()`在大多数情况下可以互换使用，但最佳实践是：

- 对于DOM引用，使用`useRef(null)`
- 对于存储任意可变值，可以使用`useRef()`或提供适当的初始值

### (五) ReactHooks函数

###### Hook 使用规则

(1)只在最顶层使用 Hook：不要在循环、条件或嵌套函数中调用 Hook。

(2)只在 React 函数中调用 Hook：在 React 的函数组件中调用 Hook，或者在自定义 Hook 中调用其他 Hook。

#### useState

useState是`React Hooks`中最基本和常用的Hook(钩子)之一，它允许在函数组件中添加和管理状态。

##### 1.关键概念解释

- **状态声明**: `useState(initialValue)` 声明一个状态变量
- **返回值**: 返回一个数组，包含当前状态值和更新状态的函数
- **状态更新**: 调用set函数会触发组件重新渲染
- **函数式更新**: 可以传递函数给setter，接收前一个状态值

##### 2.useState的核心作用

1. **状态管理**: 让函数组件拥有内部状态
2. **触发重新渲染**: 状态更新时组件会重新渲染
3. **保持状态**: 在组件重新渲染期间保持状态值
4. **替代类组件**: 使函数组件能够实现类组件的状态功能

##### 3.快速上手

```jsx
import { useState } from 'react';
function App() {
  const [ count, setCount ] = useState(0);
  const handleClick = () => {
    setCount(count + 1);
  };

  return (
    <div className="App">
      <button onClick={handleClick}>{count}</button>
    </div>
  )
}

export default App;
```

##### 4.多种状态

```jsx
  // 基本类型状态
  const [count, setCount] = useState(0);
  
  // 对象类型状态
  const [user, setUser] = useState({ name: '张三', age: 25 });
  
  // 数组类型状态
  const [items, setItems] = useState(['项目1', '项目2']);
  
  // 基于props的初始状态
  const [inputValue, setInputValue] = useState('');
```

##### 5.对象类型状态演示

```jsx
import { useState } from 'react';
function App() {
  const [ user, setUser ] = useState({id:1,name:'Yu',age:18});
  const handleClick = () => {
      //使用 展开运算符...
    setUser({
        ...user,
        name: 'Tom' ,
        age:user.age+1
    })
  };

  return (
    <div className="App">
      <button onClick={handleClick}>{user.id}--{user.name}--{user.age}</button>
    </div>
  )
}

export default App;
```



#### useEffect

useEffect是**React Hooks**中最重要的Hook之一，它允许你在函数组件中执行副作用操作。

通常在异步请求数据时使用

##### 1.核心作用

处理副作用: 在函数组件中执行数据获取、订阅、手动DOM操作等副作用

替代生命周期: 模拟类组件中的componentDidMount、componentDidUpdate和componentWillUnmount

分离关注点: 将相关逻辑组织在一起，而不是分散在不同的生命周期方法中

##### 2.使用模式

###### (1)只在初始渲染时执行

```jsx
useEffect(() => {
  // 这里的代码只在组件挂载时执行一次
}, []);
```

###### (2)在挂载和依赖项变化时执行

```jsx
useEffect(() => {
  // 这里的代码在挂载时和count变化时执行
}, [count]);
```

###### (3)每次渲染后都执行

```jsx
useEffect(() => {
  // 这里的代码在每次渲染后都会执行
});
```

###### (4)清理副作用

```jsx
useEffect(() => {
  // 设置订阅、事件监听器等
  
  return () => {
    // 清理订阅、事件监听器等
  };
}, []);
```

##### 3.快速上手

```jsx
import { useEffect,useState } from 'react';

function Home() {
  useEffect(() => {
    const timer = setInterval(() => {
      console.log('定时器执行');
    },1000);
    return () => {
      clearInterval(timer);
      console.log('组件卸载，定时器被清除');
    }
  }, []);

  return <div>Home</div>
}
function App() {
  const [isShow, setIsShow] = useState(true);
  return (
    <div className={'app'}>
      {isShow && <Home />}
      <button onClick={()=>setIsShow(!isShow)}>按钮</button>
    </div>
  )
}

export default App;
```



#### 自定义Hook函数

自定义 Hook 是一个函数，其名称以 “**use**” **开头**，函数内部可以调用其他的 Hook。通过自定义 Hook，可以将组件逻辑提取到可重用的函数中。

##### 1.快速开始

```jsx
import { useState } from 'react';

function useToggle() {
  const [value, setValue] = useState(true);
  //函数式更新 setValue(v => !v)
  const toggle = () => setValue(v => !v);
  return {
    value,
    toggle
  }
}
function App() {
  const {value,toggle}=useToggle();
  return (
    <div className={'app'}>
      {value && <div>Div</div>}
      <button onClick={toggle}>按钮</button>
    </div>
  )
}

export default App;
```

##### 2.语法细节

###### 1. 函数式更新 `setValue(v => !v)`

- **工作原理**：接收一个函数，该函数以当前状态为参数并返回新状态
- **优势**：总是基于最新的状态值，避免闭包问题
- **适用场景**：连续多次更新、异步操作、更新依赖于前一个状态

```jsx
const toggle = () => setValue(v => !v);
```

###### 2. 直接值更新 `setValue(!value)`

- **工作原理**：直接传递新值
- **优势**：代码更简洁直观
- **潜在问题**：在连续多次更新时可能使用过期的闭包值

```jsx
const toggle = () => setValue(!value);
```



### (六) 组件通信

##### 1.父传子

###### (1)props

props可传递任意数据， **数字、字符串、布尔值、数组、对象、函数、JSX**

###### (2)数据读取要求

子组件只能读取props对象中的数据,**不能修改**,父组件的数据只能由父组件修改

###### (3)快速上手

```jsx
function Son(props) {
  console.log(props);
  return <div>子组件---{props.data}---{props.list}---{ props.children}</div>
}

function App() {
  const value = 'hello world'
  return (
    <div className={'app'}>
      <Son
        data={value}
        list={[1, 2, 3]}
        children={<div>标签</div>}
      />
    </div>
  )
}

export default App;

```

###### (4)特殊写法-children

把内容嵌套在子组件标签中时， 子组件会自动在名为`children`的`props`属性中接受该数据

```jsx
function Son(props) {
  console.log(props);
  return <div>子组件 {props.children }</div>
}

function App() {
  return (
    <div className={'app'}>
      <Son>
        <div>这是一个子节点</div>
      </Son>
    </div>
  )
}

export default App;

```

##### 2.子传父

```jsx
import { useState } from "react";

function Son({onGetSonMsg}) {
  const sonMsg='我是子组件传递的消息';
  return (
    <div>
      <button onClick={() => onGetSonMsg(sonMsg)}>发送</button>
    </div>
  )
}
function App() {
  const [msg, setMsg] = useState('');
  const getMsg = (msg) => {
    console.log(msg);
    setMsg(msg);
   }
  return (
    <div className={'app'}>
      <Son onGetSonMsg={getMsg} />
      {msg && <h3>来自子组件的消息：{msg}</h3>}
    </div>
  )
}

export default App;

```

##### 3.使用`状态提升`实现兄弟组件通信



##### 4.使用`Context`机制实现跨层级组件通信

###### (1)实现步骤

- 使用`createContext`方法创建一个上下文对象`Ctx`
- 在顶层组件中通过`Ctx.Provider`组件传递数据
- 在底层组件中通过`useContext`钩子函数获取数据

###### (2)快速上手

```jsx
import { createContext,useContext } from "react";

const MsgContext = createContext();

function A() {
  return (
    <div>
      A-
      <B />
    </div>
  )

}
function B() {
  const msg = useContext(MsgContext)
  return <div>B+ 传递的数据: { msg}</div>
}
function App() {
  const msg="hello context"

  return (
    <div className={'app'}>
      <MsgContext.Provider value={msg}>
        <div>App*</div>
        <A />
      </MsgContext.Provider>
    </div>
  )
}

export default App;

```



### (七) 基础样式优化

##### 1.`classnames`工具优化类名控制

网址: https://github.com/JedWatson/classnames

classnames是一个简单的JS库，可以直观的通过条件动态控制class类名的显示。

(1)安装

```bash
npm install classnames
```

(2)快速上手

```jsx
//使用后
import classNames from 'classnames';

<div className={classNames('app',{dark:type === 'dark'})}>

</div>
```

```jsx
//使用前
<div className={`app ${type === 'dark' ? 'dark' : 'white'}`}>
</div>
```



### (八) Redux

#### 1.Redux是什么

Redux 是React最常用的集中状态管理工具,类似于Vue中的Pinia(Vuex)，可以独立于框架运行。

#### 2.Redux核心概念与工作流程（三大原则）

Redux 的核心可以用三个基本概念来理解，它们也构成了 Redux 的工作流程：

##### 		(1)Store (仓库)

- **是什么**：一个包含整个应用状态（State）的 JavaScript 对象。一个应用有且仅有一个 Store。
- **作用**：它是状态的“唯一数据源”（Single Source of Truth）。

##### 		(2)Action (动作)

- **是什么**：一个普通的 JavaScript 对象，用于**描述发生了什么事件**（比如“添加一个待办事项”、“用户登录成功”）。它是改变 State 的“意图”或“指令”。
- **要求**：Action 对象必须有一个 `type` 字段来表示动作类型，还可以包含其他需要的数据（如新的待办事项文本）。

##### 		(3) Reducer (纯函数)

- **是什么**：一个**纯函数**，它负责根据当前的 State 和接收到的 Action 来**计算出新的 State**。
- **如何工作**：它接收 `(currentState, action)` 作为参数，并返回一个新的 State。**它绝不能修改旧的 State，而必须返回一个全新的对象**。

#### 3.工作流程（数据流）

Redux 的数据流是**严格的单向数据流**，非常清晰：

1. **触发**：用户在界面上进行操作（如点击按钮），**dispatch (分发)** 一个 Action。
2. **计算**：Store 自动调用 Reducer 函数，传入当前的 State 和收到的 Action。
3. **更新**：Reducer 处理 Action，并返回一个**全新的 State** 作为 Store 的新状态。
4. **通知**：Store 保存新的 State，并**通知**所有订阅了 Store 的组件。
5. **渲染**：组件收到通知后，从 Store 中获取最新的 State，并重新渲染 UI。

#### 4.Redux使用--安装插件

在React中使用Redux，需要安装两个官方插件 **Redux Toolkit**  和  **react-redux**

##### 	(1)Redux Toolkit (RTK) 

​	 官方推荐编写Redux逻辑的方式，是一套工具的集合,**简化书写方式**。

##### 	(2)react-redux

​	 用来连接Redux和React组件的**中间件**。

##### 	(3)安装命令

```bash
# 使用 npm
npm install @reduxjs/toolkit react-redux

# 或使用 Yarn
yarn add @reduxjs/toolkit react-redux
```

#### 5.Redux快速上手

##### 准备工作

```jsx
import store from './store'
import { Provider } from 'react-redux'
//使用<Provider>组件
createRoot(document.getElementById('root')).render(
  <StrictMode>
    <Provider store={store}>  
      <App />
    </Provider>
  </StrictMode>,
)
```



##### (1)同步请求数据

```jsx
//src\App.jsx
import { useDispatch, useSelector } from "react-redux";

import { increment, decrement, addToNum } from "./store/modules/counterStore";
function App() {
  const {count} =useSelector(state => state.counter)
  const dispatch = useDispatch()
  return (
    <div className="App">
      <button onClick={() => dispatch(decrement())}>-</button>
      {count}
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(addToNum(10))}>+10</button>
      <button onClick={() => dispatch(addToNum(-10))}>-10</button>
    </div>
  )
}

export default App;

```

```jsx
//src\store\index.jsx
import { configureStore } from "@reduxjs/toolkit";

import counterReducer from "./modules/counterStore";


const store = configureStore({
    reducer: {
        counter: counterReducer
    },
})

export default store;

```

```jsx
//src\store\modules\counterStore.jsx
import { createSlice } from "@reduxjs/toolkit";

const counterStore = createSlice({
    name: "counter",
    //初始化state
    initialState: {
        count: 0
    },
    //修改状态的方法  同步方法  支持直接修改
    reducers: {

        //action
        increment(state) {
            state.count++;
        },
        decrement(state) {
            state.count--;
        }, 
        addToNum(state, action) {
            state.count += action.payload;
        }
    }
})

//解构出来actionCreater函数
const { increment, decrement, addToNum } = counterStore.actions

//获取reducer
const reducer = counterStore.reducer

//按需导出actionCreater函数
export { increment, decrement, addToNum }

// 默认导出reducer
export default reducer
```

##### (2)异步请求数据

```jsx
//src\App.jsx
import { useSelector, useDispatch } from "react-redux";
import { getData } from "./store/modules/dataStore";
import { useEffect } from "react";

function App() {
  const {dataList}=useSelector(state=>state.data)
  const dispatch = useDispatch()

  useEffect(() => {
    dispatch(getData())
    console.log('useEffect执行了')
  }, [dispatch])

  return (
    <div className="App">
      <ol>
        {dataList.map(item => <li key={item.id}>{item.title}</li>)}
      </ol>
    </div>
  )
}

export default App;

```

```jsx
//src\store\index.jsx
import { configureStore } from "@reduxjs/toolkit";
import counterReducer from "./modules/counterStore";
import dataReducer from "./modules/dataStore";

const store = configureStore({
    reducer: {
        counter: counterReducer,
        data: dataReducer
    },
})

export default store;

```

```jsx
//src\store\modules\dataStore.jsx
import { createSlice } from "@reduxjs/toolkit"
import axios from "axios"

const dataStore = createSlice({
    name: "data",
    initialState: {
        dataList: []
    },
    reducers: {
        setData: (state, action) => {
            state.dataList = action.payload
        }

    }
})

const { setData } = dataStore.actions

// 异步action
const getData = () => {
    return async (dispatch) => {
        const res = await axios.get("https://jsonplaceholder.typicode.com/posts")
        dispatch(setData(res.data))
    }
    
}

export { getData }

const reducer=dataStore.reducer
export default reducer
```

### (九) ReactRouter

路由

#### 1.安装ReactRouter包

```bash
npm install react-router-dom
```

#### 2.快速开始

```jsx
//src\router\index.jsx
import { createBrowserRouter } from "react-router-dom";
import LayoutPage from "../pages/layout";
import AboutPage from "../pages/about";

const router = createBrowserRouter([
    {
        path: "/",
        element: <LayoutPage />,
    },
    {
        path: "/about",
        element: <AboutPage />
    }
])

export default router;
```

```jsx
//src\App.jsx
import { RouterProvider } from "react-router-dom";
import router from "./router";

function App() {
  return (
    <div className="App">
      <RouterProvider router={router} />
    </div>
  )
}

export default App;
```

#### 3.路由导航跳转

##### (1)声明式导航

使用`<Link>`组件实现跳转

```jsx
import { Link } from 'react-router-dom'
```

```jsx
<Link to="/about">跳转</Link>
```

##### (2)编程式导航

使用`useNavigate`钩子函数，这种方式更加灵活。

```jsx
import { useNavigate } from 'react-router-dom';

function App() {
  const navigate = useNavigate()
  return (
    <>
      <button onClick={() => navigate('/about')}>跳转</button>
    </>
  )
}

export default App
```

#### 4.导航跳转传参

##### (1)searchParams传参

**传递参数**

```jsx
<Link to="/about?id=10&name=与">跳转</Link>
```

```jsx
<button onClick={()=>navigate('/about?id=10&name=与')}>跳转</button>
```

**接受参数**------使用`useSearchParams` 函数

```jsx
import { useSearchParams } from "react-router-dom"
const About = () => {
    const [params] = useSearchParams()
    const id = params.get('id')
    const name = params.get('name')
    return (
        <div>About-{id}-{name}</div>
    )
}

export default About
```

##### (2)params传参

**注意**:不要忘记**修改路由**

**传递参数**

```jsx
<Link to="/about/10/与">跳转</Link>
```

```jsx
<button onClick={()=>navigate('/about/10/与')}>跳转</button>
```

**接受参数**------使用`useParams`函数

```jsx
import { useParams } from "react-router-dom"

const About = () => {
    const params = useParams()
    const { id, name } = params
    //const id=params.id 作用同上
    return (
        <div>About-{id}-{name}</div>
    )
}

export default About
```

**修改路由**------注意**参数顺序对应关系**

```jsx
const router = createBrowserRouter([
    {
        path: "/about/:id/:name",
        element: <AboutPage />
    }
])
```

#### 5.嵌套路由

##### 实现步骤

(1)使用`children`属性配置路由嵌套关系

```jsx
const router = createBrowserRouter([
    {
        path: "/",
        element: <LayoutPage />,
        children: [
            {
                path: "home",
                element: <Home />
            },
            {
                path: "about",
                element: <AboutPage />
            }
        ]
            
    }
])
```

(2)使用`<Outlet/>`组件配置二级路由渲染位置

在合适的位置放置`<Outlet/>`组件

```jsx
import { Outlet } from 'react-router-dom'

function App() {
  return (
    <div className="App">
      <Outlet/>
    </div>
  )
}

export default App;
```

(3)配置默认二级路由

```jsx
const router = createBrowserRouter([
    {
        path: "/",
        element: <LayoutPage />,
        children: [
            {
                index: true,    //重定向
                element: <Home />
            }
        ]
    }
]}
```

#### 6.404路由

```jsx
const router = createBrowserRouter([
    {
        path: "*",
        element: <div>404</div>
    }
])
```

#### 7.获取路由信息

使用useLocation函数



### (十)MarkDown文档渲染

要在前端页面展示 Markdown 格式的文章，核心是将 Markdown 文本**转换为 HTML**，再通过 HTML 渲染到页面上。整个过程可以借助专门的工具库简化。

在 React 项目中实现 Markdown 文档转换，推荐使用 **`react-markdown`** 库，它是专门为 React 设计的 Markdown 解析器，遵循 React 组件化思想，安全性高（默认过滤危险 HTML），且支持插件扩展（如代码高亮、表格等）。

#### 1.常用工具库

推荐几个成熟的 JavaScript 库，简化转换和渲染流程：

- **marked**：最流行的 Markdown 转 HTML 库，轻量且功能全面；
- **github-markdown-css**：提供类似 GitHub 的 Markdown 样式，美化排版；
- **highlight.js**：处理代码块的语法高亮。

#### 2.框架集成

若使用 React/Vue 等框架，可使用对应库：

- React：`react-markdown`（替代 marked）；
- Vue：`vue-markdown` 或 `marked` + 自定义指令。

#### 3.安装依赖

```bash
# 核心库：Markdown 转 React 组件
npm install react-markdown

# 可选：支持 GitHub 风格的 Markdown（表格、任务列表等）
npm install remark-gfm

# 可选：代码块语法高亮
npm install rehype-highlight

# 可选：代码高亮样式（多种主题可选）
npm install highlight.js

# 可选：样式库（美化外观） 提供类似 GitHub 的 Markdown 排版样式
npm install github-markdown-css
```

(1)从本地 `.md` 文件加载内容

先安装 `raw-loader`：

```bash
npm install raw-loader --save-dev
```

然后在组件中导入：

```jsx
// 导入本地 .md 文件
import demoMarkdown from './docs/demo.md';

// 使用：<MarkdownRenderer markdownContent={demoMarkdown} />
```
