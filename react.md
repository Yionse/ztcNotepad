# react基础

## react基础文件解析

- react.development.js，react核心库，基础
- react-dom.development.js，react-dom库，用于支持react操作dom
- babel.min.js，用于将jsx转换为js
- 这里需要注意一点，如果在没有使用脚手架的情况下，需要按照上述顺序依次进行引入，因为只有先引入了核心库，才能有后面的其他库

## 编写一个react-Demo

- script中的type属性必须写为text/babel，注明是jsx语法

- 创建虚拟DOM时，不要加引号，因为它不是字符串

- 然后通过ReactDOM.render(虚拟DOM，容器)，这里的容器是真正的DOM

- render方法是覆盖的不是追加

- ```jsx
  	<!-- 准备一个容器 -->
    <div id="test">
    
    </div>
    <!-- 引入js核心库，且必须第一个引入，后面的库都依赖它 -->
    <script src="./js/react.development.js"></script>
    <!-- 引入React-dom库，用于React操作dom -->
    <script src="./js/react-dom.development.js"></script>
    <!-- 引入babel，用于将jsx语法，转换为js -->
    <script src="./js/babel.min.js"></script>
    <!-- 编写自己的脚本 -->
    <script type="text/babel">    //  type=“text/babel”必须写，否则默认为js语法，加上了以后就是jsx语法
      const VDOM = <h1>Hello，React！</h1>    //  创建一个虚拟DOM，注意不要加引号，不是字符串
      ReactDOM.render(VDOM, document.querySelector('#test'));
    </script>
  ```

## 为何在React中不用js语法

- 在js中创建虚拟DOM的方法不方便

  - ```js
    const VDOM = React.createElement(标签名,属性,标签体内容)
    ```

  - 如果出现嵌套则非常不方便，

  - 所以一般使用jsx直接编写

- 使用jsx时，可以外加一对括号，方便换行

- 最后jsx通过babel翻译成js时，就是将上述的代码执行

## 虚拟DOM和真实DOM的区别

- 虚拟DOM就是一个Object类型

  - ```js
    console.log(VDOM),	输出Object
    console.log(typeof VDOM)，	输出Object
    console.log(VDOM instaceof Object),		输出TRUE
    ```

- 虚拟DOM比较"轻"，身上的属性和方法少，这是由于虚拟DOM是React内部在用，无需那么多的方法

- 虚拟DOM最终会被React转换为真实DOM渲染到页面上

## jsx语法规则

- 定义虚拟DOM时，不要写引号

- 在虚拟DOM中，想要读取外部变量，需要加{}

  - ```jsx
    const myData = 'Hello，React';
    const VDOM = (
    	<h1>{myData}</h1>
    );
    ```

- 标签中混入js表达式时，需要加{}

- 在jsx中需要添加class，需要写className

  - ```jsx
    const VDOM = (
    	<h1 className="title">{myData}</h1>
    );
    ```

  - 避免与class混淆

- 要添加style内联样式，需要添加两层{}

  - 这是因为外面一层代表着这是一个js表达式，里面的代表着这是一个对象

  - ```jsx
    const VDOM = (
    	<h1 style={{color: 'white', fontSize:'12px'}}>{myData}</h1>
    );
    ```

  - 如果是多个单词组合的话，写成小驼峰

  - {{key:value}}

- jsx中不允许有多个根标签，只能有一个根标签

- 标签必须闭合

- jsx语法中的标签

  - 如果是小写字母，则转换为同名的HTML标签，若没有，则报错
  - 如果是大写字母，则被认为是一个组件，若组件未定义，则会报错

- 在React的虚拟DOM中，如果写一个js数组，可以遍历出来，如果是一个对象，则会报错

- jsx中不允许写语句，只可以写表达式

# React面向组件编程

- React中有两种组件
  - 函数式组件
  - 类式组件

## 函数式组件

- 首字母必须大写

- 适用于简单组价

- ```jsx
  const MyComponent = () => {
        return <h2>这是我的第一个函数式组件</h2>
      }
      ReactDOM.render(<MyComponent />, document.querySelector('#test'));
  ```

- 函数式组件中的this指向是undefined

- 当babel翻译后，会自动开启js严格模式

- 执行了ReactDOM.render之后，React会解析组件标签，找到了MyComponent组件，然后发现该组件是函数式的，随后调用该函数，然后将该函数返回的虚拟DOM，转为真实DOM，渲染到页面上

# 函数式组件

- 当使用函数式组件时，如果返回的标签和return在同一行上，则不需要大括号，如果不在同一行上，则需要加上大括号
- 如果不在同一行上，组需要使用括号括起
- 如果有多个标签，那么可以使用一个空标签包裹起来，然后再返回

# 事件处理

- ​	只有在原生的js中才存在有事件处理，所以需要在原生HTML标签上绑定属性，然后传入对应的回调函数

  - ```tsx
    export default function App () : any {
      function handleClick() :void {
        console.log('这是一个回调');
      }
    
      return (
        <>
          <button onClick={handle}></button>
        </>
      );
    }
    ```

- 其中事件处理函数，可以时由props传入的，也可以是在组件内部定义的，但通常以handle开头，后面跟上事件名称

  - ```tsx
    function Son({handleClick} : any) : any {
      return (
        <>
          <button onClick={handleClick}></button>
        </>
      );
    }
    
    function Father () : any {
      return <Son handleClick={function() :void {console.log('adas');}} />
    }
    ```

- 有时间逻辑较为简单的时候，可以直接在jsx语法中定义一个箭头函数

  - ```tsx
    function Son({handleClick} : any) : any {
      return (
        <>
          <button onClick={() : void => {console.log('12312')}}></button>
        </>
      );
    }
    ```

- 在React中所有的事件都会向上冒泡

# Hook

- Hook函数必须在组件的顶层使用

## useState

- 用于生成一个state变量，是响应式的，当发生改变时，它可以触发模板的重新解析

- 使用useState函数生成一个变量时，它会返回一个数组，第一个则是返回的那个响应式对象，第二个是一个函数，当需要更改返回的数据时，需要调用此函数进行修改

  - ```tsx
    let [index, setIndex] = useState(0);
    ```

- setIndex会触发React重新渲染组件

- state是完全私有的，它的父组件也不知道是否会有state的存在

- setState可以是一个具体的值，也可以是一个更新函数，这个函数会有一个参数n，它是上一次的结果

- useState也可以接收一个函数，这个函数必须有一个返回值，这个返回值就是state的值

## useEffect

- 为了解决副作用

- 将Ajax请求、操作DOM、localStorage等操作放在useEffect中

- 当通过修改状态，更新组件时，useEffect中的函数就会执行一次，副作用也会不断执行

- ```ts
  import { useEffect } from 'react';
  function App() {
      useEffect( () => {
        //	这里面的语句每次页面更新都会执行  
      } );
  }
  ```

- 通过依赖项控制副作用执行时机

  - 默认状态-组件初始化时，执行一次，然后当页面更新时，会再次执行

  - 添加空数组-只会执行一次，就是首次

  - 添加特定依赖项-在数组中加入依赖的项，每次当依赖项改变时，执行该函数

    - ```ts
      import { useEffect } from 'react';
      function App() {
          useEffect( () => {
          }, [count] );		//	只有当count发生改变时，才会执行里面的函数
      }
      ```

- 当在useEffect中用到了数据状态，就应该出现在数组依赖项中进行声明，否则会出现bug

- 在useEffect中，添加一个return，返回一个函数，在函数体中做清楚副作用

  - ```ts
    function App () {
        useEffect(() => {
    		let timer = setIntervar(() => {
                //	做一些操作
            }, 1000);
            return () => {
                clearInterval(timer)
            }
        });
    }
    ```

  - 如果不做清除的话，那么当组件被销毁时，定时器还是在运行，这样会浪费性能

- 注意使用useEffect发送网络请求时，不要直接卸载useEffect的参数函数上，会导致垃圾清理问题，争取的做法是，在匿名函数内，重新再定义一个具名函数

  - ```tsx
    function App() {
        useEffect(() => {
            async function getList() {
                //	发送请求
            }
        });
    }
    ```

- 

## 渲染

- 当需要频繁的更新顶层组件时，这会带来性能问题，因为这将导致React频繁的递归的重新渲染下面的子组件，但子组件的状态可能并未发生修改

- set函数永远只关心当时的时候数据是怎样的，就算经历了异步操作也是一样的

- ```tsx
  export default function LightSwitch() : any {
  
    const [number, setNumber] = useState(0);
  
    return (
      <>
        <h1>{number}</h1>
        <button onClick={() => {
          setNumber(number + 5);
          setTimeout(() : void => {
            alert(number);
          }, 3000);
        }}>+5</button>
      </>
    )
  }
  ```

- 上述程序会先将h1中的内容更改为5，然后再去执行异步队列中的语句，而将alert语句放入异步队列时，它的值是0，所以最后弹出的就会是0

- **React 会使 state 的值始终”固定“在一次渲染的各个事件处理函数内部。** 你无需担心代码运行时 state 是否发生了变化

## 更新state中的对象

- 在React中不要直接更改useState生成的对象，如果需要更改需要新生成一个对象，可以通过如下方式

  - ```tsx
    const [person, setPerson] = useState({
      name: 'Niki de Saint Phalle',
      artwork: {
        title: 'Blue Nana',
        city: 'Hamburg',
        image: 'https://i.imgur.com/Sd1AgUOm.jpg',
      }
    });
    
    const nextArtwork = { ...person.artwork, city: 'New Delhi' };
    const nextPerson = { ...person, artwork: nextArtwork };
    setPerson(nextPerson);
    
    
    //	如下也是可以的
    setPerson({
      ...person, // 复制其它字段的数据 
      artwork: { // 替换 artwork 字段 
        ...person.artwork, // 复制之前 person.artwork 中的数据
        city: 'New Delhi' // 但是将 city 的值替换为 New Delhi！
      }
    });
    ```

  - 先通过展开运算符将里面的一层进行展开，获取一个新的对象，这个对象必须包含了对象的更新

  - 然后再将外层对象通过展开运算符，复制剩余的属性，再将更新过后的对象进行替换

- 不要直接修改一个State对象，要为它创建一个新的对象

- 当嵌套很多层时，可以使用immer

  - ```git
    npm i -G use-immer
    ```

## 更新State中的数组

- 数组同对象类似，也不要直接更改由State生成的数组
- 要避免使用会更改数组内部元素的方法
  - push、shift、pop、unshift、splice、reverse、sort这些将会改变数组内部的元素
  - 也不要直接使用索引修改数组中的元素arr[index]  =  number
- 推荐使用不改变原数组的方法
  - slice(start, end);	将返回一个原数组的拷贝，浅拷贝，start是开始的下表，end是结束的下标，不包含结束位置。如果只有一个参数，则就是需要拷贝的个数，而此时的下标是从0开始的。另外参数也可以时负数，代表着数组最后的n个数。
  - concat、filter、map、以及...[arr]等

## 使用immer更改数组中的对象

- 当更改对象时，setter函数可以传入一个函数，这个函数可以接收到一个参数，它是对原数据的代理，然后再对它进行修改即可

- ```tsx
  setProducts(draft => {
        const myObj : obj = draft.find(item => item.id === productId) as obj;
        myObj.count += 1;
      })
  ```

- 这就类似于是直接在操作state数据，但其实并不是的

- 目前开发中没有使用

# 状态管理

- **对 React 来说重要的是组件在 UI 树中的位置,而不是在 JSX 中的位置！**

## 构建state规则

- **合并关联的 state**。如果你总是同时更新两个或更多的 state 变量，请考虑将它们合并为一个单独的 state 变量。
- **避免互相矛盾的 state**。当 state 结构中存在多个相互矛盾或“不一致”的 state 时，你就可能为此会留下隐患。应尽量避免这种情况。
- **避免冗余的 state**。如果你能在渲染期间从组件的 props 或其现有的 state 变量中计算出一些信息，则不应将这些信息放入该组件的 state 中。
- **避免重复的 state**。当同一数据在多个 state 变量之间或在多个嵌套对象中重复时，这会很难保持它们同步。应尽可能减少重复。
- **避免深度嵌套的 state**。深度分层的 state 更新起来不是很方便。如果可能的话，最好以扁平化方式构建 state。
- **注意不要将props再使用useState**，如果父组件重新传入一个porps，则组件中的state将不会重新更新
- **如果你想在重新渲染时保留 state，几次渲染中的树形结构就应该相互“匹配”**
- 指定一个 `key` 能够让 React 将 `key` 本身而非它们在父组件中的顺序作为位置的一部分。这就是为什么尽管你用 JSX 将组件渲染在相同位置，但在 React 看来它们是两个不同的计数器。因此它们永远都不会共享 state。每当一个计数器出现在屏幕上时，它的 state 会被创建出来。每当它被移除时，它的 state 就会被销毁。在它们之间切换会一次又一次地使它们的 state 重置

# Context 深层传递参数

- Context 让父组件可以为它下面的整个组件树提供数据

## 步骤

- 创建context

  - ```tsx
    import { createContext } from 'react';
    const LevelContext = createContext(1);
    ```

  - 可以传递任何类型的值（甚至可以传入一个对象）

- 使用context

  - ```tsx
    import { useContext } from 'react';
    const level = useContext(LevelContext);
    ```

  - 这样即可得到
  
- Context甚至可以在顶层的APP中往下传递

# useRef

- 可以生成一个不会被React更新的数据，通过对象的形式返回

  - ```tsx
    { 
      current: 0 // 你向 useRef 传入的值
    }
    ```

  - 会返回一个上述对象

- 与state的区别，它被更改时，不会重新渲染页面

- ref 是一个应急方案，用于保留不用于渲染的值。 你不会经常需要它们。

- ref 是一个普通的 JavaScript 对象，具有一个名为 `current` 的属性，你可以对其进行读取或设置。

- 你可以通过调用 `useRef` Hook 来让 React 给你一个 ref。

- 与 state 一样，ref 允许你在组件的重新渲染之间保留信息。

- 与 state 不同，设置 ref 的 `current` 值不会触发重新渲染。

- 不要在渲染过程中读取或写入 `ref.current`。这使你的组件难以预测。

## 使用ref获取页面上的DOM节点

- 生成一个ref对象
- 然后将这个ref对象，绑定到对应的DOM结构的ref 属性上

## forwardRef

- 将访问另一个组件的DOM节点

- ```tsx
  const MyInput = forwardRef((props, ref) => {
    return <input {...props} ref={ref as any} />;
  });
  ```


# Prop类型校验

- 安装PropTypes包

- 引入PropTypes

- 使用组件名.PropTYpes定义所需类型

- ```tsx
  import PropTypes from 'prop-types'
  Component.propTypes = {
      属性：类型.isRequired
      //	isRequired则代表着必填
  }
  ```

- 还可以添加规则，比如必填项等