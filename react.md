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

# 