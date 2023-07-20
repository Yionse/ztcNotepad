# TS类型

## 字符串类型

- ```ts
  const a : string = 'aaa';
  ```

## 数字类型

- ```tsx
  const a : number = 'bbb';
  ```

## 布尔类型

- ```tsx
  const booleanA : boolean = true;
  ```

- 当使用new Boolean()时，生成的并不是boolean字面量，而是一个Boolean对象，可以通过Boolean函数获得一个Boolean值

  - ```tsx
    const booleanB : boolean = Boolean(1);
    ```

## 空值类型

- Js中没有空值类型，但是TS中有，使用Void

- 在TS中使用void表示没有返回值的函数

- ```tsx
  function test() :void {
    
  }
  ```

## NULL和undefined类型

- ```tsx
  let lll : null = null;
  let kaj : undefined = undefined;
  ```

- void与undefined和null的区别是

  - undefined和null时所有类型的子类型，就是所有undefined和null类型的变量，都可以被赋值给其它变量

  - ```tsx
    let lll : null = null;
      let kaj : undefined = undefined;
      let str : string = lll;
      const aaa : void = undefined;
      const bbb : void = lll;
    ```

- 当配置了严格模式时，两种变量之间不可以相互赋值

## Any类型

- 随便是哪种类型，不需要进行类型检测，随时可以赋值其它类型

- ```tsx
  let anyany : any = 1231;
    anyany = '12312';
    anyany = 123123;
  ```

- 声明变量时，没有指定类型则默认为any类型

- 当使用any类型时，TS就失去了意义

- any类型可以调用对象中的属性和方法，且调用的是不存在属性时，也不会报错

- ```tsx
  let anyany : any = { a: 1};
  console.log(anyany.b);		//	不会报错，会输出undefined
  ```

## UnKnown类型

- TS3.0中引入的，它与any的作用相似，但它更安全

- 与any一样，它也可以赋值给其他的所有类型

- ```tsx
  let test : unknown;
    test = '123';
    test = 123;
    test = true;
    test = [];
    test = {};
  ```

- 但UnKnown类型不可以作为子类型，只能作为父类型，但any可以作为子类型，也可以作为父类型

- ```tsx
  //	这是可以的
  let anyanyL : any;
    let sss : number = anyanyL;
    let adas : unknown;
  //	这是不被允许的
  let aaa : string = adas;
  ```

- 但UnKnown类型可以被赋值给any类型

- UnKnown类型不可以调用属性和方法

## 接口和对象

- 在TS中，定义对象时，要使用interface(接口)，用interface做一种约束

  - ```tsx
    interface goods {
      id : string,
      price : number,
    }
    ```

- 使用时需要定义所有在接口中定义过的属性，不能多一个也不能少一个

  - ```tsx
    const pen : goods = {
        id : '001',
        price : 234.5
      }
    ```

- 还可以为该接口追加一个属性

  - ```tsx
    interface goods {
      type : string
    }
    ```

  - 但在后续中需要加进去，否则就会出现错误

  - ```tsx
    const pen : goods = {
        id : '001',
        price : 234.5,
        //	不加type的话会报错
        type : '笔'
      }
    ```

- 追加属性还可以通过继承的方式

  - ```tsx
    interface goods {
      id : string,
      price : number,
    }
    
    interface foods extends goods {
      from : string
    }
    
    const rice : foods = {
        id : '001',
        price : 123,
        from : 'china'
      };
    ```

- 可选属性，表明该属性可以有，也可以没有

  - ```tsx
    interface foods extends goods {
      from : string,
      color ?: string
    }
    
    const rice : foods = {
        id : '001',
        price : 123,
        from : 'china'
    	//	color属性可以有，也可以没有  
    };
    ```

- 任意属性，它可以时任意类型，且属性名也是任意的，但需要注意的是，当定义了任意类型后，其它确定属性和可选属性都必须是任意属性的类型的子类

  - ```tsx
    interface goods {
      id : string,
      price : number,
      [propName : string] : string		//	这种情况是不行的，因为string并不是其它类型的子类
    }
    
    interface goods {
      id : string,
      price : number,
      [propName : string] : any			//	这种是可以的，因为其它类型都是any的子类
    }
    ```

- 只读属性，它是不允许被修改的

  - ```tsx
    interface goods {
        readonly id : string
    }
    const rice : goods = {id : '001'};
    goods.id = '002';		//	报错
    ```

- 添加函数

  - ```tsx
    interface goods {
      readonly id : string
      //	这两种定义函数的方法都可以  
      a() :void,
      b : () => void
    }
    
    const rice : goods = {id : '001', a() {
        
      },b() {
        
      },};
    ```

- 数组类型

  - ```tsx
    let arr : number[] = [1, 2, 3, '2'];
    //	报错，不允许添加字符类型的
    ```

- 数组泛型

  - ```tsx
    let arr : Array<number> = [1, 2 ,3];
    ```

- 接口 描述数组

  - ```tsx
    interface StringArray {
      [index : number] : boolean
    }
    
    let arr2 : StringArray = [true, false];
    
    ```

- 多维数组

  - ```tsx
    let data : number[][] = [[1, 2], [3, 4]];
    ```

- arguments参数

  - 当在函数中使用到了arguments参数时，形参的类型必须为any，否则会报错

    - ```tsx
      function test2 (...args : any) : void {}
      function test2 (...args : string) : void {}	//	会报错
      ```

  - 当需要将arguments参数赋值给其它数组，那么需要将接收的变量设置为IArgument

    - ```tsx
      function test2 (...args : any) : void {
          let arr : IArguments = arguments;
      }
      ```

# 函数

- 函数的参数必须固定个数与类型，不能多传，也不可以少传，且类型必须相等

- ```tsx
  function test2 (name: string, age : number, type ?: unknown) : void {}
  test2('ztc', 22 );
  test2('ztc', 22, '22' );
  //	以上两种调用的方式都可以
  ```

- 还可以设置参数的默认值，这个就同JS语法了，只不过要做出类型限制

- 函数重载，方法名相同，但是参数和返回的值不同

  - ```tsx
    
    ```

- 接口定义函数

  - ```tsx
    interface add {
        (a:number, b: number):number
    }
    
    const fn : add = (a:number, b:number) : number => {
        return a + b
    }	//	使用接口时函数定义为箭头函数
    ```