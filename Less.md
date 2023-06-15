## 注释

- 以//	开头的注释不会被编译到css文件中
- 以/**/开头的则会被编译到css中

## 变量

- 在less中，变量以@ 开头，可以代替各种属性值，与数值

  ```less
  @color : skyblue;
  @width : 10px;
  a {
      color: @color;
      width : @width;
  }
  ```

- 还可以一次性声明一整条css语句，但是相似于函数调用，需要加括号

  ```less
  @background: {
      background: white;
      color: red;
      }
      div {
          @background();
          }
  ```

  

- 还可以将属性与选择器也使用变量代替，需要加括号括起来，再使用

  ```less
  @selector : .nav;
  @color : skyblue;
  @width : 10px;
  @{selector} {
      width: @width;
  }
  
  @nat: nav;
  .@{nav} {
      width: @width;
  }
  #@{nav} {
      color: @color;
  }
  
  @widthStyle : width;
  .@{nav} {
      @{widthStyle}: @width;
  }
  ```

- url，定义时需要将路径用引号括起，引用时要放在括号内，同样要加''

  ```less
  @images : "../iamges/";
  body {
      background: url('@{images}xxx.jpg')
  }
  ```

- 相加减，在运算的时候，尽量将运算符两边加上空格

  ```less
  @width: 300px;
  @color: #333;
  div {
      width: @width - 10;		//	不需要加单位
      padding: @width * 20;	
      color: @color - #111;
      color: @color * 2;
  }
  ```

- 变量作用域，类似于js，就近原则

  ```js
  @width: 200px;
  body {
      @width: 300px;
      width: @width;
  }	//	以第二个width为准
  ```

- 用变量定义变量

  ```less
  @width:300px;
  @minWidth: width;
  body {
      width: @@minWidth;
  }
  ```

## 嵌套

- 在html中经常出现嵌套的标签，&代表紧跟上一级，而不再是它的儿子

  ```less
  div {
      width: 300px;
      p{
          height: 200px;
          &:nth-child(10) {
              width: 100px;
          }
      }
      &::after {
          content: '';
      }
  }
  
  //	生成的css
  div {
    width: 300px;
  }
  div p {
    height: 200px;
  }
  div p:nth-child(10) {
    width: 100px;
  }
  div::after {
    content: '';
  }
  ```

## 混合

- 通过类似定义类的方法，将css写在里面，后面的可以直接调用

  ```js
  .w() {			//	这里也可以不加括号，不加括号的话，.w也会有对应的样式，同样也可以是#开头
      width: 1200px;
  }
  body {
      .w();
  }
  ```


