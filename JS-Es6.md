# Js的注释 
    单行注释    //
    多行注释    /* */

# Js编写的三种位置
    行内式 <p 事件="函数名"></p>
    嵌入式 <script>  </script>
    链入式 <script src=""> </script>

# Js的输入和输出语句
```js
    alert(msg) ==== 弹出对话框
    console.log(msg) ==== 控制台打印
    console.dir()   ====    打印一个对象具体的值
    prompt(info) ==== 输入框 ==== 取出来的是字符串类型
```

# Js的变量声明
- 单变量声明
        var 变量名 = 值;
- 多变量声明

        var 变量名1 = 值1,
            变量名2 = 值2,
            变量名3 = 值3;
- 特殊情况
        1. 只声明，不赋值
```js
            var age;
            console.log(age);
            //会输出undefined   未定义的
```
        2. 不声明，直接使用
```js
            console.log(未定义的变量);
            //会直接报错
```
        3. 不定义，直接使用
```js
            name = "帅逼";
            conosle.log(name);
            //可以运行，但不推荐这种方法
```
# 变量命名规范
    1. 由数字字母下划线及$组成
    2. 严格区分大小写
    3. 不得以数字开头
    4. 不能是关键字保留字等
    5. 要使用有意义的单词
    6. 驼峰命名法
# 数据类型
    1. Number    数字型         默认值-0
```js   
            var example1 = 012;    //八进制
            var example2 = 0x12;    //十六进制
            alert(Number.MAX_VALUE);    //最大范围
            alert(Number.MIN_VALUE);    //最小范围
            Infinity    //无穷大
            -Infinity   //无穷小
            isNaN(变量) //判断变量是否为数字类型
```
    2. String    字符串类型     默认值-空
```js
            var str = '我是"帅哥"';     //单双交错
            \n  换行
            \\  \
            \'  '
            \t  table键
            \b  空格

            字符串长度
            var str = "12345";
            console.log(str.length);

            字符串拼接
            console.log("我" + "是" + 11);
            字符串拼接时会把所有的类型转换为字符串类型
```
    3. Boolean   布尔值类型     默认值-false
    4. Undefined 未定义         undefined
```js
            var varibale = undefined;
            console.log(variable + 1);  //最后输出的是 NaN 代表非数字 ，undefined与数字相加
```
    5. Null      空             null
```js
            var varibale = null;
            console.log(null + 1);     
            与数字相加时，当做0
```
    6. typeof(变量)     输出变量的数据类型
```js
            var num = 10;
            console.log(typeof num);       //输出 Number
            var variable = null;
            console.log(typeof variable);   //输出的是Object类型 并非 null类型
```
-   null和undefined的区别是，前者是赋值了，但是值为null，后者是没赋值
    -   undefined + 1 为NaN，而null + 1，为1
# 数据类型转换
1. 数字型转成字符串型
```js
    var num = 10;

    1. num.toString();
    2. String(num);
    3. console.log("" + num);
```
2. 转换成数字型
```js
    parseInt(变量);         // 转成整形，不会四舍五入，可以识别以数字开头的字符串
    parseFloat(变量);       //转成浮点型
    Number(变量);           //只能识别纯数字
    利用运算符：
    console.log(变量 - * 0);    //加好会拼接
```
3. 转换成boolean类型
```js
    非空串或非0数字转换后都为     1
    1. undefined
    2. null
    3. NaN
    4. ''
    5. 0
    都转换成为                  0
```
-   隐式转换，除+号外的其他字符，如果第一个变量是数值类型，则会将第二个变量转换为数值类型
    -   ```js
            log(+'123')，会得出数字型
        ```

# Js运算符
    1. 算术运算符
        + - * / %
        当浮点数进行算术运算时时会出现误差
```js
        var num = 0.1 + 0.2;        //不等于0.3
        var result = 0.07 * 100;    //不等于7
```
    1. 递增运算符
        前置递增    ++num     先自加1 再进行运算
        后置递增    num++     先使用 再自加1
    2. 递减运算符
        前置递减    --num     先自减1 再进行运算
        后置递减    num--     先使用 再自减1
    3. 比较运算符
         ==      只判断字面量
```js
        console.log('123' == 123);      //相等
```
        ===     既判断字面量，还判断类型
```js
        console.log('123' == 123);      //不相等
```
    1. 逻辑运算符
       1. 非    !       取逻辑值的相反
```js
        var num = true;
        !true   // false
```
       1. 与    &&      两侧都为真才为真
       2. 或    ||      一侧为真即为真
       3. 短路逻辑运算
            逻辑与
            如果表达式1的值为真，则返回表达式2的值
            如果表达式1的值为假，则返回表达式1的值
```js
        console.log(123 && 456);  //456
        console.log(0 && 123);    //0
```
            逻辑或
            如果表达式1的值为真，则返回表达式1的值
            如果表达式1的值为假，则返回表达式2的值
```js
        conosle.log(123 || 345);        //123
        console.log(0 || 123);          //123
        console.log(0 || 123 || 456);   //123
```
    1. 赋值运算符   
        右边的值 赋值给 左边的变量
        += -= *= /= %=
    2. 运算符优先级
        从高到低分别为
            小括号
            一元运算符  ！
            算术运算符
            关系运算符
            相等运算符
            逻辑运算符   && || 
            赋值运算符
            逗号运算符
# Js流程控制语句
## 分支结构
    1. if语句
```js
        if ( 10 > 5) {
            console.log('分支一');
        }else if (5 > 8) {
            console.log('分支二');
        }else {
            console.log('分支三');
        }
```
    1. switch语句
```js
        switch(表达式) {
            case value ： 语句; break;
            default : 最后的语句; 
        }
        // 如果表达式的值与value的值相匹配则执行后面的语句，表达式与case的值必须全等才可以
```
    1. 三元表达式
```js
        ? :
        console.log(10 > 5 ? true ：false);
```
## 循环结构
    1. for
```js
        var result = 0;
        for (var i = 1; i < 101; i++) {
                result += i;
        }
```
    1. while
```js  
        //先判断后运行
        var num = 0;
        while (num != 100) {
            num++;
        }
```
    1. do while
```js   
        //先执行后判断
        var num = 100;
        do {
            num--;
        }while (num != 0)
```
    1. continue     
        跳出本轮循环，但不结束整个循环体
    2. break
        直接结束整个循环，不再执行
# 数组
-    一组数据的集合，存储单个变量下
-    可放任意数据类型，包括undefined，Null，NaN
-    如果出现索引越界，不会报错，而是会输出undefined
-    数组的长度可以直接修改
     - ```js
            var array = [1, 2, 3];
            array.length = 5;
            for(var i = 0; i < array.length; i++) {
                console.log(array[i]);      //最后两个为undefined
            }
        ```
## 创建数组
```js
    var array = new Array();   //利用new创建         
    var array = [1, 2, 3];     //利用数组字面量创建
```
# 函数
-   函数的声明方法
```js
    function 函数名(形参1，形参2) {
        函数体;
        return 需要返回的结果;
    }
    函数名(实参1，实参2);       //方法一，命名函数
    var fun = function() {
        函数体;
    }
    fun();                     //方法二，匿名函数
    //又称为函数表达式，也可以传递参数
```
-   形参与实参的特殊情况
    -   如果实参比形参多，则不会有其它的误差结果，程序正常运行
    -   如果实参比形参少，则形参中没得到实参的变量则会被赋予undefined，再进行运算的情况下，会得到NaN
    -   形参的默认值为undefined
    -   return 返回的可以使任意类型的
    -   如果没有写return，则会返回undefined
## arguments的使用方法
-   argumengts存储了当前函数的所有实参，本质上是一个内置对象
-   实际上是伪数组
    -   具有数组的字面量[]
    -   具有数组的length属性
    -   可以按照索引的方式获取元素的值
    -   不具备pop和push方法
```js
    function fn() {
        console.log(arguments.length);
    }
    fn(1, 2, 3, 4);             // 4
    fn(1, 2, 3, 4, 5, 6);       // 6
```
# js作用域
-   就是变量在某个范围可以起效果的区域，目的是为了提高程序可靠性
-   js的作用域(ES6)之前
    -   全局作用域
        -   整个script标签，或者是单独的js文件
    -   局部作用域
        -   在函数内部就是局部作用域
```js
    <script>
        var number = 10;        //全局作用域，全局变量
        function() {
            var number;         //局部作用域，局部变量
            example = 20;       //在函数内部没有声明，直接赋值的变量，也属于全局变量
        }
    </script>
```
##  全局变量和局部变量
-   全局变量作用于整个程序运行期间，直到浏览器关闭，占用的空间才会被释放
-   局部变量作用于某个函数期间，函数执行完毕，则空间被释放
-   块级作用域是在ES6新增的
## 作用域链
-   内部函数访问外部函数的变量，采取的是链式查找的方法来决定取哪个值，这种结构称为作用域链
# js预解析
-   js代码运行时分为两步，预解析和代码执行
## 预解析
-   js引擎会把js里面的所有var,function提升到作用域的最前面，然后按照顺序，从上往下顺序执行
-   分为变量预解析和函数预解析
    -   变量预解析，就是把所有的变量声明，提升到当前作用域最前面，但不提供赋值
    -   函数预解析(提升)，将所有的函数声明提升到当前作用域最前面
```js
    fn();
    console.log(c);
    console.log(b);
    console.log(a);
    fn() {
        var a = b = c = 9;
        console.log(a);
        console.log(b);
        console.log(c);
    }
    //相当于如下代码
    fn() {
        var a = b = c = 9;  
        //相当于 var a = 9; b = 9; c = 9;
        //并不是多变量声明
        //所以b，c实际上是全局变量
        
        console.log(a);
        console.log(b);
        console.log(c);
    }   //进行函数提升
    fn();
    console.log(c);
    console.log(b);
    console.log(a);
```
# js对象 
## 对象
-   js中对象是一组无序的相关属性和方法的集合，所有事物都是对象，例如字符串，数值，数组，函数
-   属性是事物的特征，方法是事物的行为
## 调用对象
-   调用属性
    -   对象名.属性名;
    -   对象名.['属性名'];
-   调用方法，对象名.方法名();
## 创建对象的三种方式
-   使用字面量创建对象，采取键值对的方式，多个属性和方法之间用逗号隔开，方法后面跟的是匿名函数
```js
    var person = {};    //空对象
    var person = {
        uname : '张天成',
        age : 21,   //属性
        say : function() {
            console.log('My name is' + this.uname);
        }   //方法
    };
```
-   new关键字
```js
    var person = new Object();  //创建一个空对象
    person.uname = '张天成';
    person.age = 21;
    person.say = function() {
        console.log('My name is'+ this.uname);
    };
```
-   利用构造函数创建对象
    -   前面两种方式，一次只能创建一个对象
    -   构造函数就是把创建对象时要使用的重复代码封装在函数里面
    -   构造函数不需要return
    -   new关键字执行过程
        -   当出现new时，队在内存中生成一个空对象
        -   this指向了这个对象
        -   执行构造函数里面的方法，给对象添加属性和方法
        -   返回对象，所以不需要return
```js
    function 构造函数名() {
        this.属性 = 值;
        this.方法 = function() {};
    }       //基本格式规范
========================================    
    function Person(uname, age) {
        this.uname = uname;
        this.age = age;
        this.say = function() {
            console.log('My age is ' + this.age);
        }
    }
    var my = new Person('张天成', 21);
```
## 遍历对象属性
-   for in 用于对数组和对象进行遍历
```js
    for(var k in person) {      //k即是属性名
        console.log(person.k);
    }
```
# Js内置对象
## Js对象分为三种
-   自定义对象
-   内置对象，属于EcmaScript，Js自带的，可以直接使用
-   浏览器对象，WebAPI里面的
-   所有的内置对象都不是构造器，不需要new可以直接使用，类似于java中的静态方法，直接通过对象名.属性调用即可 
## Math对象
-   Math.PI
-   Math.floor();   //向下取整
-   Math.ceil();    //向上取整
-   Math.round();    //四舍五入
-   Math.abs();     //绝对值，如果实参是字符串，会式转换
-   Math.random();  //随机数方法
    -   得到两个数之间的随机数，且是闭区间
```js
        var num = Math.floor(Math.random() * (max - min + 1) + min);
```
## Date对象
-   需要通过构造函数来创建，使用new
-   当传入三个数字型实参时，十二月份是从0开始，即0代一月，11代表十二月
```js
    var date = new Date();  //没有参数返回系统当前日期
    //可以有三个实参，也就是数字日期，也可以是一个实参则是整个字符串
    // var date1 = new Date(2023, 1, 1);
    // var date1 = new Date('2023-1-1');(2023/1/1);
    date.getFullYear();     //返回当前年
    date.getMonth();        //返回当前月，从0开始
    date.getDate();         //返回当前日
    date.getDaty();         //返回周几，从0开始，0是周日，6是周日
    date.getHours();         //返回当前小时
    date.getMinutes();      //返回当前分钟  
    date.getSeconds();      //返回当前秒
    date.valueOf();         //返回1970年至今的毫秒数
    date.getTime();         //返回1970年至今的毫秒数
    var date1 = +new Date();    //返回1970年至今的毫秒数
    Date.now();             //返回1970年至今的毫秒数,H5新增的，兼容性问题
```
## Array对象
```js
    var array = new Array();
    //当实参是一个数字时，表示的是数组长度length，且都为空元素 
    //实参数量大于2时，则是数组元素
```
-   检测是否为数组
    -   instanceof 运算符，可以检测是否为数组，对象等
    -   Array.isArray()，H5新增的
-   常用方法
    -   push()  在数组的末尾，添加一个或多个元素,push有返回值，为添加数组元素后，数组的长度
    -   unshift()   在数组的开头添加一个或多个元素，unshift有返回值，为添加数组元素后，数组的长度
    -   pop()   删除数组末尾的元素，一次只允许删除一个元素，返回值是删除的那个元素
    -   shift() 删除数组头部的元素，一次只允许删除一个元素，返回值是删除的那个元素
    -   indeOf(指定元素,[起始位置，可缺省])    返回指定元素第一次出现的索引
    -   lastIndexOf()   返回指定元素最后一次出现的索引
    -   toString()  数组转换为字符串，且用逗号分隔
    -   join()      转换为字符串，且可以指定分隔符，缺省的话则是默认逗号
    -   concat()    连接两个数组，不影响原数组，返回的是一个新的数组
    -   slice(begin,end)     数组截取，返回截取到的新数组
    -   splice(begin, length)   数组删除，从哪里开始，删除几个，返回被删除的元素，如果元素个数为1，也是Array类型
    -   reverse()   数组逆序
    -   Math.max(...arr);   可以实现将数组中的最大值打印出来
        -   还可以使用...实现数组合并
        -   ... 展开运算符
    -   sort()      实现个位数排序，如果要向实现排序，需要如下
        -   ```js
                array.sort(function(a, b) {
                    return a - b;   //升序排序
                    return b - a;   //降序排序
                });
            ```
    -   forEach()   调用数组中的每个元素，给回调函数，与map的区别是，forEach不返回，适用于对象
        -   ```js
                array.forEach((value, index) => {可以得到value})
            ```
    -   map()       会返回一个新数组，新数组的内容是原数组中的每个值依次调用callback函数以后所形成的，它不会筛选，适用于修改
        -   ```js
                const array = [1, 2, 3];
                const newArray = array.map((value, index) => value+index);
            ```
    -   filter()    创建一个新的数组，新数组中的元素是符合条件的所有元素，并返回，return后的条件不能写算术运算符，仅能写比较运算符，因为叫筛选函数
        -   ```js
                const arr = [10, 20, 30];
                arr.filter(item => return item >= 20);
                //  return 筛选条件
            ```
## String类型
-   基本包装类型
    -   ```js
           var str = 'ztc';
           console.log(str.length);
            //简单数据类型没有属性
            
            //相当于如下步骤
            var temp = new String('ztc');
            str = temp;
            temp = null;
        ```
    -   字符串的值不会被改变，只是表面上更改了，实际上是重新开辟了另外一块空间，然后连到新的空间，但原来的空间实际上还是一直存在
    -   字符串的所有方法都不会修改字符串，都是重新开辟了一个字符串
-   基本方法
    -   ```js
            var str = 'abcdefg';
            str.indexOf('a',[起始位置，可缺省]);   //0
            str.indexOf('a',1);     //-1,查找指定元素的下标
            str.charAt(1);         //b,获取指定下标的元素
            str.charCodeAt(1);      //98,获取指定下标的元素的ASCLL码值
            str[1];                 //a,同charAt,但是str是H5新增的，存在兼容性问题
        ```
    -   concat(字符串1，字符串2)
        -   ```js
                concat(str1, str2, str3);
                // 连接三个字符串
            ```
    -   substr(截取的起始位置，长度)
        -   ```js
                var str = 'abc';
                str.substr(1, 2);   //bc
                // 截取字符串
            ```
    -   replace('被替换的字符','新字符')    //被替换的字符也可以是多个
        -   ```js
                var str = 'abccd';
                str.replace('c','f');   //abcfd  
                // 只会替换第一个字符
            ```
    -   split('分隔符')
        -   ```js
                var str1 = 'red,red,red,red';
                var array = str.split(str1);    //返回数组
                // 字符串转换为数组
            ```
##  基本包装类型
-   把简单数据类型包装为引用数据类型
# 简单数据类型与复杂数据类型
-   简单数据类型null，使用typeof返回的是Object，类型，当一个变量将要存放对象类型时，就可以先赋值为null类型
-   复杂数据类型即是使用new关键字新建的
-   简单数据类型都存放在栈区，而复杂数据类型则放在堆区
-   复杂类型是在堆区开辟空间，然后将它的地址存放在栈区，最后定义一个变量指向刚才的栈区的内容，即可得到复杂数据类型的内容
-   简单数据类型被函数调用时，是直接传递的变量的值，不影响实参
-   复杂数据类型时，调用的是指针，如果在函数中被修改，则会影响实参所指向的地址的内容
#   语言基础
##  变量
-   严格模式，‘use strict’，对语法的要求更为严格，例如在ES5中语句可以不以分号结尾，但在使用了严格模式以后，不写分号就会报错。另外在严格模式下，var关键字不能缺省，否则会报错，另外‘use strict’必须出现出现js的第一行
-   在Es6中新增了两个定义变量的关键字，let，const
    -   let 是块级作用域，同var的函数作用域不同
        -   ```js
                if (true) {
                    var uname = '1';
                    console.log(uname);
                }
                console.log(uname);         // 输出1
                // 由于var是函数作用域，所以在if内定义的变量，还是同等于在函数下，所以if的外面也可以访问到uname的值
                if (true) {
                    let uname = '1';
                    console.log(uname);
                }
                console.log(uname);         // 报错
                // 由于let是块级作用域，作用于{}内，所以在{}外就访问不到
            ```
        -   且变量不可被重复定义，而var关键字中的变量则可以被重复定义
        -   var声明的变量会被提升，但是let声明的变量并不会被提升
        -   var声明的变量会成为windows对象的属性，而let则不会
            -   ```js
                    var uName = '12';
                    console.log(windows.uName);     //正常输出
                    let uNName = '12';
                    console.log(windows.uNName);     //undefined
                ```
    -   const 跟let基本相同，但区别是const必须在声明时，同时赋值，且值不能被修改
        -   ```js
                const a;    //报错
                const a = '1';  //正确
                a = '2';        //报错，值不能被修改
            ```
        -   const定义对象时，变量所指向的那个对象不可以被更改，但是这个对象中的属性可以被更改
            -   ```js
                    const bmw = new Car();
                    bmw.price = 33.6;
                    bmw.price = 45.5;   //可行的，只是修改了对象中属性的值
                    bmw = new Car();    //不可行的，因为指向不可以更改
                ```
-   在程序中，尽量不要使用var关键字，最好使用const关键字，let次之
##  模板字符串
-   ```js
        let example = 22;
        console.log(`我今年${example}岁了`);
    ```

# WebAPI

-   是浏览器提供的一套操作浏览器和页面元素的接口(Bom、Dom)

# DOM

##  Dom基础

-   console.dir()，打印返回的元素对象，更好的查看里面的属性和方法
-   获取元素
    -   根据ID获取，getElementByID(id)，若找不到的话，就返回null，参数是字符串，需要带引号，返回的是一个对象
    -   根据标签名获取，getElementsByTagName(标签名)，返回的是对象的集合，以伪数组存储。如果页面中查找到的元素只有一个，依然以伪数组存储。如果没有元素，则返回一个空的伪数组，它能找到所有的元素
    -   根据类名获取，H5新增
        -   getElementsByClassName(类名)，根据类名获得元素，返回的也是一个伪数组
        -   querySelector(选择器)，返回指定选择器的第一个元素，返回的是一个元素对象
        -   querySectorAll(选择器)，返回指定选择器的所有元素，返回的是一个伪数组
    -   获取body元素，document.body;    这是属性，返回的是元素对象
    -   获取html元素，document.documentElement;     返回元素对象   

##  事件基础

-   事件有三部分组成
    -   事件源——事件被触发的对象
    -   事件类型——如何触发，例，鼠标点击触发，经过触发
    -   事件处理程序——事假触发后，做些什么事情
-   执行事件的步骤
    -   获取事件源
    -   绑定事件，注册事件
    -   添加事件处理程序
-   常用鼠标事件
    -   onclick     鼠标点击
    -   onmouseover 鼠标经过(经过子盒子也会触发)
    -   onmouseenter    鼠标经过(自经过自身时触发)
        -   原因是因为mouseover会冒泡
    -   onmouseout  鼠标离开
    -   mouseleave  鼠标离开
        -   同over和enter的区别
    -   onfocus     鼠标获得焦点
    -   onblur      失去鼠标焦点
    -   onmousemove 鼠标移动
    -   onmouseup   鼠标弹起
    -   onmousedown 鼠标按下
    -   contextmene 右键菜单
    -   selectstart 禁止选中文字
    -   pageshow    页面重新加载触发
    -   input       表单输入时触发
    -   change      值发生变化的时候

##  操作元素

-   改变元素
    -   innerText   将元素的内容从头到尾替换，但会去掉空格与换行，且不识别html标签
    -   innerHTML   可以识别空格与换行，且可以识别html标签
    -   以上两个属性都是可读写的
-   src     资源地址
-   href    超链接地址
-   title   标题

##  表单元素的属性

-   type    表单的类型
-   value   改变表单的值应该使用这个 
-   checked 选择
-   disable 是否启用
-   placeholder     提示信息

##  元素样式属性操作

-   通过style进行操作，必须要使用驼峰命名法，js写的样式是属于行内样式，权重很高
-   通过className来更换类名进行操作，如果样式较多且较为复杂就可以使用该方法更换className

##  获得元素属性

-   element.属性名，主要获取内置属性，本身自带的
-   element.getAtrrubute('属性')，主要获取自定义属性，由开发者自己设置
-   element.setAtrrubute('属性','值');，设置自定义属性及其值
-   element.removeAtrrubute('属性')，删除自定义属性

#   H5自定义属性

-   h5规定，所有的自定义属性，都以data-开头，用来区别自定义属性和内置属性
    -   element.setAttribute('data-自定义名称','10');
    -   获取时，可以用 element.getAttribute('data--自定义名称');，或者element.dataset.自定义名称，和element.dataset['自定义名称']，但是具有兼容性问题，ie11以后才支持
    -   实际上element.dataset，是一个集合，存放了所有以data-开头的自定义属性
    -   如果是data-name-set，则在获取的时候，需要将后两个自定义名称，以驼峰命名，即element.dataset.nameSet，才可以获取到

#   节点操作

## 节点的获取

- 利用节点层次关系获取元素，主要体现在父，子，兄，逻辑性强，但是兼容性稍差

- 节点：页面中所有的元素都是一个节点，且均可以通过JavaScript获取

- 一般的节点至少拥有nodeType节点类型，nodeName节点名称，nodeValue节点值这三个基本属性

  -   nodeType：
      -   元素节点为1
      -   属性节点为2
      -   文本节点为3

- 父节点：parentNode可以找到当前元素的父级节点，得到的是离元素最近的父亲，如果找不到父节点就为null

- node.childNodes，可以得到当前element的所有子节点，包括文本节点，但是主要操作的是元素节点，所以就需要自行操作，通过判断nodeType来选择操作元素节点

- 子节点：node.children，该方法可以直接得到所有的子节点，且是所有的子元素节点，不会获得文本节点

- 第一个子节点和最后一个子节点：node.firstChild，node.lastChild，获取的是第一个子节点，包括文本节点

- 第一个子节点和最后一个子节点：node.firstElementChild，node.lastElementChild，这个不包含文本节点，仅仅包含元素节点，但是在IE9以上才支持

- 实际开发的写法，会直接通过node.children然后通过索引的下标去获得

- 下一个兄弟节点：nextSibling，得到的节点包括文本节点，找不到返回null 

- 上一个兄弟节点：previousSibling，同上

- nextElementSibling，上一个元素兄弟节点，仅仅得到元素节点

- previousElementSibling，下一个兄弟元素节点，IE9以上才支持

  - 自己封装的下一个元素节点函数

    - ```js
          function getPreviousElementSibling(element) {
              let el = element;
              while(el = element.nextSibling) {
                  if(el.nodeType === 1) {
                      return el;
                  }
              }
              return null;
          }
          //上一个元素节点同上
      ```

## 创建节点

-   document.createElement('tagname');  创建一个元素节点    
-   node.appendChile(child);     将元素添加到它的父亲身上，即可完成连接，并且是在父元素的后面添加，相当于数组的push
    -   node.insertBefore(child,指定元素)，可以将一个节点插入到指定元素的前面

##  删除节点

-   node.removeChild(node)，删除父节点中的某个子节点

##  节点克隆

-   node.cloneNode(false,true)，赋值一个node节点，返回一个节点，如果形参为true则是深拷贝，复制所有的内容，如果是false的话，则只会复制节点，不复制子节点，是为浅拷贝

##  三种动态创建元素的区别

-   document.write(需要写入的标签);括号里面要写全，但是在文档流执行完毕后，再写入标签时，会导致页面重绘，就是点击后，可能之前的页面都不存在了，不推荐使用
-   innerHTML，在创建多个元素时，会比较耗时，如果使用数组的形式会更加省时
-   document.createElement(nodeName)，推荐使用这种方法

#   重点核心

##  给元素注册事件的方式

-   传统，element.onclick，只能添加一个方法，后面的方法会覆盖前一个
-   element.addEventListener(type, function(), useCapture[true|false]);，同一个元素可以添加多个，以顺序执行
    -   type:   不用加on，click，mouseover，必定是字符串
    -   function:   事件函数
    -   capture:    后面给出
-   element.attachEvent(type, function)，type必须带on，IE独有的

##  删除事件，解绑事件

-   传统的element.onclick = null，即可解绑
-   element.removeEventListener(type, function);，必须删除的是命名函数，如果是匿名函数的话，无法删除，参数不用带括号，直接写函数名即可
-   element.detachEvent(type, function);，type是要带on，且直接写函数名即可

## DOM事件流

-   事件流分为三个步骤
    -   捕获阶段
    -   当前目标阶段
    -   冒泡阶段
-   element.addEventListener(type, function(), true|false);，第三个参数是true则处于捕获阶段，则从父节点依次往下执行。如果第三个参数是false或者缺省，则处于冒泡阶段，执行顺序则更好相反

##  事件对象

-   在给一个元素添加了事件以后，就会有一个事件对象，如果是鼠标事件，则会有鼠标事件，如果是键盘事件，则会得到一个键盘事件
-   在使用的时候，是在事件监听函数的形参中进行命名，它存在兼容性问题，在IE中要是用Window.event，才可以得到事件对象。且不需要传递实参，是有系统直接赋予的
-   常见属性
    -   e.target，返回触发事件的对象或元素，e.target返回的是触发事件的对象，this返回的是绑定事件的对象。e.target是点击了哪个元素就返回哪个元素，而this是谁绑定了就返回谁。在IE中使用的是e.srcElement
    -   e.type返回事件类型，不带on
    -   e.preventDefault()阻止默认事件，不让链接跳转，不让按钮提交。针对IE678则使用e.returnValue，或者可以使用return false，也可以阻止默认行为，但是return 后的语句就不执行了，且仅适用于传统的.click方法
    -   e.stopPropagation()，添加了此方法以后，则不再继续往上冒泡，存在兼容性，IE678中，使用e.cancelBubble = true，停止冒泡
-   事件委托(代理，委托)
    -   原理：不是给每个子节点单独设置监听器，而是将监听器设置在其父节点身上，然后利用冒泡影响其每个子节点
    -   通过事件对象e.target获得当前触发事件的子元素，然后再进行操作
    -   e.tagtName获得当前触发元素的标签名，但标签名都是大写的

##  鼠标事件对象

-   e.clientX，返回鼠标相当于浏览器可视化窗口的 X坐标
-   e.clientY，返回鼠标相当于浏览器可视化窗口的 Y坐标，总是相对于浏览器可视窗口为主，就算文档长度发生变化，依然是以浏览器窗口可视化为基准
-   e.pageX，返回鼠标相当于文档页面的 X 坐标
-   e.pageY，返回鼠标相当于文档页面的 Y 坐标，IE9以上支持
-   e.screenX，返回鼠标相当于电脑屏幕的 X 坐标
-   e.screenY，返回鼠标相当于电脑屏幕的 Y 坐标

##  键盘事件对象

-   onkeyup，某个键弹起时触发
-   onkeydown，某个键按下时触发
-   onkeypress，某个键按下时触发，但不识别功能键，Ctrl，shift，箭头等，识别大小写
-   当onkeydown和onkeypress同时触发时，先执行onkeydown
-   e.keyCode，可以得到按下的键的ASCLL码值
-   keydown和keyup不识别大小写
-   keydown和keypress在文本框中的特点是，在事件被触发的时候，文字还没有被输入到文本框之中
-   e.key，直接等于该键的值，推荐使用

#   BOM操作

##  BOM概述

-   BOM即是浏览器模型，跟页面相关的，其核心对象是bom，其缺乏标准
-   window是一个顶级对象，所有的全局属性和函数都是window的属性

##  window常见事件

-   window.onload是页面加载函数，在文档全部执行完毕后，才会执行这个事件
-   document.addEventListener('DOMContentLoaded', function());，仅在DOM支持以后就可以执行这个事件，不包含css，图片，flash，IE9以上才支持
-   window.onresize，当浏览器窗口发生改变的时候触发
    -   window.innerWidth，浏览器的宽度
-   两种定时器
    -   setTimeout(调用函数, [延迟的毫秒数])，仅写函数名就可以，也可以为匿名函数，或者使用字符串括起来setTimeout('function()', 1000)如果毫秒数缺省就是默认为0。页面中可能有很多的定时器，经常可以给定时器起一个标识符
    -   setInterval(调用函数, [间隔的毫秒数])，大都同上，区别是反复调用，间隔设置的毫秒数就会不停的调用函数
    -   window.clearTimeout(定时器的标识符);

##  执行队列

-   JavaScript是单线程语言
-   Js中将任务分为两类，同步任务放在执行栈中，异步任务回调函数放在消息队列中。一旦执行栈中的所有的任务执行完成，则将消息队列中的异步任务放入执行栈中进行执行
-   在执行栈中的异步任务会先被调到异步处理程序，在被触发时，将需要执行的语句放入到消息队列中，等执行栈中的语句执行完毕以后，就可以将消息队列中的语句执行

##  location对象

-   location常见属性
    -   location.href，获取或设置整个URL
    -   location.host，获取当前主机
    -   location.part，获取端口号
    -   location.pathname，返回路径
    -   location.search，返回参数
    -   location.hash，返回片段，常见于#后面的内容，用于链接和锚点 
-   常见方法
    -   location.assign()，跟href一样，可以跳转页面，也称为重定向页面，会记录，可以后退页面
    -   location.replace()，替换当前页面，但不记录，所以无法后退页面
    -   location.reload()，重新加载页面，相当于刷新，如果参数写true，则相当于Ctrl+F5，强制刷新

##  navigator对象

-   包含浏览器相关的信息
-   navigator.useAgent

##  history对象

-   常见方法
    -   back()，回退到上一个页面
    -   forward()，前进功能
    -   go(参数)，如果参数是1，则前进一个页面，如果参数是-1，则后退一个页面

#   网页元素偏移

##  offset

-   获得元素距离带定位父元素的位置
-   获得自身元素的大小
-   都不带单位
-   如果父亲没有单位则继续往外翻，直到body
-   常用属性
    -   element.offsetTop，获得元素距离带定位父元素的顶部距离
    -   element.getBoungdingClientRect()，获得元素到视口区域的距离，返回的是一个对象，包含了X和Y属性，不包含单位
    -   element.offsetLeft，获得元素距离带定位父元素的左部距离
    -   element.offsetWidth，获得元素宽度，包括padding，边框，内容
    -   element.offsetHeight，获得元素高度，包括padding，边框，内容，且都不带单位
    -   element.offsetParent，获得带定位的父元素，如果没有，则返回body
-   offset与style的区别
    -   style只能得到行内样式表，得不到内嵌与链入式的样式，而offset可以得到任意样式表中的值
    -   offset得到的是不带单位的，style带单位
    -   offset获取到的宽度和高度包含了padding和border，而style则不包含
    -   offsetWidth，和offsetHeight只能读，但是不能赋值，而style可以赋值也可以获取
    -   如果只需要读取，使用offset更为合适，如果需要更改则使用style

## client系列

-   通过client获取元素的相关可视区的相关信息
-   常用属性
    -   element.clientTop，返回元素的上边框宽度
    -   element.clientLeft，返回元素的做边框宽度
    -   element.clientWidth，返回自身包括padding、内容的宽度，不含边框
    -   element.clientHeight，返回自身宝库padding、内容的高度，不含边框，返回的是数值，不带单位

##  scroll系列

-   scroll可以得到实际的大小，当内容超过盒子的大小时，scroll将会真正显示盒子的大小
-   scrollTop，是指被卷去的头部，如果文字过多，需要使用到滚动条时，就会出现此类情况
-   onscroll，当滚动条发生变化时，会被触发
-   window.pageYOffset，整个document被卷去的头部

##  三大系列总结

-   offset包含边框，而client和scroll不包含
-   scroll可以获得超出盒子部分的大小，也就是包含溢出部分，而client不包含溢出部分

#   立即执行函数

- 无需调用，立马能够自己执行的函数

- 写法如下

  - ```js
        (function FNname(){})(参数)
        (function(){}(参数))
    ```

  - 函数名也可以写上

- 作用是，在函数内部声明了很多局部变量，不会有命名冲突的情况出现

#   动画函数封装

- 核心原理就是让盒子不断地移动setInterval

- 缓动动画公式(目标值-当前值)/10，需要判断步长是否为负数，如果为负数则需要向下取整，如果是正数向上取整

- 如果把函数当实参，传递给另外一个函数当形参，然后在函数结束时，就执行回调函数

- ```js
      function animat(obj, target, callback) {
          clearInterval(obj.timer);
          obj.timer = setInterval(function() {
              let step = (target - obj.offsetLeft) / 10;
              step = step > 0 ? Math.ceil(step) : Math.floor(step);
              if(obj.offsetLeft != target) {
                  obj.style.left = obj.offsetLeft + step + 'px';
              }else {
                  if(callback) {
                      callback();
                  }
              }
          }, 15)
      }
  ```

#   节流阀

- 当上一个动画函数执行完毕以后，再去执行下一个动画函数，让事件无法连续触发

- 核心思路，利用回调函数，添加一个变量控制

- ```js
      let flag = true;
      if(flag) {
          flag = false;
          //  具体的操作
      }
  ```

#   页面滚动动画

-   window.scroll(x, y);
-   通常只设置y，并且x，y不设置单位

#   本地存储

- 数据存储在用户浏览器中

- 设置读取方便，刷新也不丢失数据

- sessionStorage约5M，localStorage约20M

- 只能存储字符串，可以使用JSON.stringify(复杂数据类型)，编码后存储

  - ```js
        const obj = {
            name : 'z',
            age : 18
        };
        localStorage.setItem('obj', JSON.stringify(obj));
        localStorage.getItem('obj');    //此时获取到的是被JSON转换后的JSON字符串
        JSON.parse(localStorage.getItem('obj'));    //即可重新取回刚才存储的对象
    ```

- sessionStorage

  -   生命周期为浏览器窗口
  -   在同一个页面可以共享
  -   以键值对进行存储
  -   存储数据，sessionStorage.setItem(key, value);
  -   获取数据，sessionStorage.getItem(key);
  -   删除数据，sessionStorage.removeItem(key);
  -   删除所有数据，sessionStorage.clear();

- localStorage

  -   生存周期是永久的，除非手动删除
  -   多窗口共享，就是多个页面中可以读取数据
  -   方法同sessionStorage

#   正则表达式

- 正则表达式是用于匹配字符串中字符组合的模式。在js中正则表达式也是对象

- 常用方法

  -   reg.test(字符串);   检测字符串中是否含有规则中定义的字符串，返回布尔值
  -   reg.exec(字符串);   查找匹配的字符串，返回的是数组，包含了首次出现的位置，

- 定义正则表达式的语法

  - ```js
        const 变量名 = /表达式/     //  斜杠中间就是字面量
        const str = '我我前端前我前端端我前我前我前端端我前端我前端端我前端';
        const reg = /前端/;
        reg.test(str);  //  返回boolean值   true
        reg.exec(str);  //返回一个数组，其中包含了下标，2
    ```

- 元字符，特殊的字符，具备强大的功能，比如输入26个英文字母，可以表示为[a-z]

  - 边界符，表示位置，必须以什么开头和结尾

    - ^   表示匹配行首的文本，以谁开始

    - $   表示匹配行末的文本，以谁结束

      - ```js
            /^a/.test('a'); //true
            /^a/.test('aa'); //true
            /^a/.test('ba'); //false
        
            /^a$/.test('ba'); //false
            /^a$/.test('a'); //true
            /^a$/.test('aa'); //false
            //  如果^和$同时出现，则只有在字符串等于^$中间的字符时才匹配正确
        ```

  - 量词，表示重复次数

    - 设定某个模式出现的次数

    - *重复零次或多次

    - +重复一次或多次

    - ？重复零次或一次    

    - ```js
          /^a*$/  //  * 代表 >= 0
          /^a+$/  //  * 代表 >= 1
          /^a?$/  //  * 代表 0 || 1
      ```

    - {n}，重复n次

    - {n，}，重复 >=n 次

    - {n,m}，重复n到m次，包含m， 逗号左右不能有空格

    - ```js
          /^a{3}$/  //  代表只能是3个 a
          /^a{3,}$/  //  >=3 个a
          /^a{3,6}$/  //  [3,6]个a，逗号不要带空格
      ```

  - 字符类，\d表示0-9

    - []，匹配字符集的集合，只要包含中括号里面的任何一个就可以

    - ^   当出现在中括号内的时候，则是取反，[^a-z]，除了小写字母外的任何字符

    - .   除了换行外的任何一个字符

    - \D，匹配除0-9外所有的字符，相当于[^0-9]

    - \w，匹配任意数字字母下划线，相当于[a-zA-z0-9_]

    - \W，匹配除数字字母下划线的所有字符，相当于[^a-zA-z0-9_]

    - \s，匹配空格，包括换行制表空格等，相当于[\n\t\s\v\f]

    - \S，除空格外所有字符，相当于[^\n\t\s\v\f]

    - |   或 [a|b]，a或b

    - ```js
          /^[abc]$/.test('atbc'); //false
          /[abc]/.test('atbc');   //true
          /[a-z]/.test('atbc');   //true
          /[a-zA-z0-9]/.test('atbc123');   //true
      ```

- 修饰符，写在斜杠后面

  - i表示不区分大小写

  - g是全局查找

  - ```js
        /^java$/.test('java'); //true
        /^java$/.test('JAVA'); //false
        /^java$/i.test('JAVA'); //true
    ```

- 替换文本

  - 字符串.replace(/正则表达式/, '替换的文本')

  - ```js
        const str = 'abcdefghijkl';
        str.replace(/abcd/i, '000');    //将abcd替换为000
        str.replace(/abcd/ig, '000');    //将abcd替换为000，全局替换
    ```

#   作用域 

- 作用域可以分为全局作用域和局部作用域，作用域规定了变量可以被访问的范围

  -   局部作用域又分为函数作用域和块级作用域
      -   函数作用域中的变量在函数结束后，会被清除掉
      -   块级作用域是指被{}包裹的变量
  -   全局作用域是指在script>标签和.js文件的最外层定义的变量，其它的变量都可以使用
  -   作用域链的本质是从下往上的查找机制，会优先在当前函数作用域中查找变量，如果当前函数作用域没有则会找到父级作用域直至全局作用域

- 垃圾回收机制

  -   js中的分配的内存一般有如下生命周期
      -   内存分配：当声明变量、函数、对象时，内存会自动为它们分配内存
      -   内存使用：即在使用变量、对象时，和在调用函数时，也就是读写内存
      -   内存回收：使用完毕以后，由垃圾回收机制自动回收不再使用的内存
  -   全局变量的值一般不会被回收，但在页面关闭后就可以被回收掉
  -   内存泄漏：程序中分配的内存由于某些原因未释放或无法被释放就称为内存泄漏
  -   垃圾回收机制算法说明：
      -   堆与栈的分配区别
          -   程序中所定义变量、基本数据类型由操作系统自动分配，都放在栈区中，且自动回收
          -   堆区中存放的都是复杂数据类型由程序员分配，如果程序员不回收则由垃圾回收机制回收
      -   引用算法说明，采用引用技术算法，看该对象是否还在被引用，如果还存在引用则+1，如果没有引用则-1，如果引用次数为0时，则回收
      -   标记算法说明，如果可以从根部扫描所有的对象，如果是能到达就被定义为需要使用的，如果从根部无法到达的对象，则会被回收

- 闭包

  - 闭包 = 内存函数 + 外层函数的变量

  - 闭包的作用就是外部也可以访问内部函数的变量

    - ```js
          function outer() {
              let a = 10;
              function inner() {
                  console.log(a);
              }
              return fn;
          }
          const a = outer();
          a();    //  即可得到内部的值
      
          //  闭包应用，可以使用数据的私有
          //  统计函数被调用的次数
          function count() {
              let i = 0;
              return function() {
                  console.log(++i);
              }
          }
          const fun = count();
          fun();
      ```

- 变量提升，在代码执行之前，会将所有的var声明的变量，提升到最前面，先进行声明，只提升声明，不提升赋值

#   函数进阶

##  函数提升

-   函数提升与变量提升相似，会在代码执行之前，将所有的函数声明提升到当前作用域的最前面，值提升函数声明，不提升函数调用

##  函数参数

- 动态参数

  -   arguments是函数中的动态参数，是是一个伪数组形式，可以获取到所有的实参。每个函数都存在arguments

- 剩余参数

  - 允许将不定量的参数表示为一个数组

  - ```js
        function getSum(...arr) {
            let sum = 0;
            for (let i = 0; i < arr.length; i++) {
                sum += arr[i];
            }
            return sum;
        }
    ```

- 剩余函数和动态参数的区别

  -   动态参数是不需要写形参，所有的形参都在arguments中
  -   剩余参数则可以定义形参，如果有多余的话，则剩下的都放在剩余的数组中，并且剩余数组是真数组，可以使用pop和push方法
  -   多使用剩余参数

##  箭头函数

- 箭头函数是更简短的函数写法，且不绑定this，用来替换需要使用匿名函数的地方

- 基本语法

  - ```js
        const fn = (参数) => {
            函数体;
        }   //一个简单的箭头函数声明
    
        const fn1 = 参数 => {
            函数体;
        }   //如果只有一个参数，则可以省略括号
    
        const fn2 = (参数) =>   console.log();
            //如果只有一行语句，可以省略花括号
    
        const fn3 = 参数 => 语句
            //箭头函数可以省略return
    
        const fn4 = (参数) => ({key: value})
            //可以直接返回一个对象，但需要用括号括起来
    ```

- 箭头函数属于函数表达式，不会出现变量提升

- 箭头函数的参数

  -   箭头函数没有arguments，但是有...arr剩余参数

- 箭头函数的this指向

  - 箭头函数不会自己创建this，它只会从自己的作用域链上一层沿用this

    - ```js
          const fn = () => console.log(this);
              //  输出window，并不是因为是window调用的，而是它的上一层作用域就是window
      
          const obj = {
              fn: () => console.log(this);
          }
          obj.fn();   //输出的是window，因为fn()函数本身没有this，虽然它是通过obj调用的，所这里的this并不指向obj，而是指向它的上一层，obj的上一层是window
      
          const obj = {
              fn : function() {
                  const uname = 'ztc';
                  const fn2 = () => console.log(this)
                  fn2();
              }
          }
          obj.fn();
          //  这里输出的是obj，同理，由于箭头函数本身没有this，只会往上一层作用域去找，上一层是正经的匿名函数，是有this的，this指向调用者，于是这里的this便指向obj
      
          element.addEventListener('clic', () => console.log(this));
          //  由于这里的箭头函数是element的回调函数，所以也不存在this，只会去找element的调用者，于是就找到了window
      ```

  - DOM事件回调函数通常不推荐使用箭头函数

#   解构赋值

##  数组解构

- 数组解构是将数组的值快速批量的给一系列变量赋值的语法

  - ```js
        const arr = [1, 2, 3, 4];
        const [a, b, c] = arr;
        console.log(a, b, c);   // 1, 2, 3
    
        //  快速交换两个变量的值
        let a = 1;
        let b = 2;
        [b, a] = [a, b];
    
        let a = 1
        let b = 2
        [b, a] = [a, b]
        //  解构之前如果不加分号会报错
        //  js两种必须加分号的情况，其一是使用立即执行函数时，必须要加分号，其二就是如果语句是以数组开头的话，则数组前面必须要加分号
        
        //  如果变量少的情况下，可以利用剩余参数来解决
        const [a, ...arr] = [1, 2, 3] 
        //  arr就等同于函数的剩余参数
    
        //  还可以使用默认参数
        const [a, b, c = 4] = [1, 2, 3]     // c = 3
        const [a, b, c = 4] = [1, 2]     // c = 4
    
        //  可以按需传递，不需要使用的可以直接忽略，但是要加分号
        const [a, , c] = [1, 2, 3]     // a=1， c=2
    
        //  可以接多维数组
        const [arr1, arr2] = [[1,2], [3,4]]
    
    ```

- 

##  对象解构

- 将对象的值快速批量的赋值给一些变量

- 语法与数组类似

  - ```js
        const {uname, age} = {uname: 'ztc', age: 20};
        //  变量名与属性名完全一致
        const {uname, age, fn} = {uname: 'ztc', age: 20, fn: () => console.log('这是箭头函数')};
    
        //  可以出现改名的情况
        const {uname:username, age} = {uname: 'ztc', age: 20};
        //  在变量名后，加一个分号，起新的变量名
    ```

##  数组对象解构

- ```js
      const arr = [
          {
              uname : 'ztc',
              age : 21
          },{
              one : '666',
              two : 22
          }
      ]
      const [{uname, age},{one, two}] = arr;
      console.log(uname, age, one, two);
  
      //  数组内有多个对象，且对象属性相同时，需要重新定义变量名
      const arr = [
          {
              uname: 'ztc',
              age: 21
          }, {
              uname: '666',
              age: 22
          }
      ]
      const [{ uname, age }, { uname: myName, age: myAge }] = arr;
      console.log(uname, age, uname, myName, myAge);
  ```

##  多级对象数组解构

- ```js
      //  多级对象解构
      const arr =
      {
          uname: 'ztc',
          family: {
              one: 1,
              two: 2
          },
          age: 21
      }
      const {uname, family:{ one, two }, age} = arr;
      console.log(uname, one, two, age);
  ```

##  对象数组混合解构

- ```js
      //  混合解构
      const arr =
          [
              {
                  uname: 'ztc',
                  family: {
                      one: 1,
                      two: 2
                  },
                  age: 21
              }
          ]
      const [{ uname, family: { one, two }, age }] = arr;
      console.log(uname, one, two, age);
  ```

#   构造函数

##  深入对象

- 创建对象的三种方式

  - 字面量创建  {}

  - new Object

  - 利用构造函数，可以快速地构建多个类似的对象

    -   约定一：以大写字母开头
    -   约定二：使用new操作符去进行使用
    -   构造函数里面没有return

  - ```js
        const obj = {};         //  字面量最常用
        const obj = new Object();
        
        //  利用自定义构造函数创建
        function NewPerson(name, height, gender) {
            this.name = name;
            this.height = height;
            this.gender = gender;
        }
        const ztc = new NewPerson('ztc',177,'男');
    ```

- 实例成员和静态成员

  - 在对象中的实例成员就是静态成员

  - 构造函数中的属性和方法被称为静态属性和静态方法

  - 静态成员和静态方法只能通过构造函数来访问

  - ```js
        function Person() {
            Person.eyes = 2;
            Person.say = () => console.log('我在说话！');
        }
    ```

##  内置构造函数

- Object

  - 三个常用的静态方法

    - Object.keys(对象)，返回一个数组，这个数组包含所有的键

    - Object.values(对象)，返回一个数组，这个数组包含所有的值

    - Object.assign(新对象， 原对象)，返回一个新对象，对旧对象进行拷贝，常用于追加，不覆盖原对象里面的属性

      - ```js
            const obj = {uname:'ztc'};
            Object.assgin(obj, {age:21});
            //  如果有多个属性则可以体现出简便
        ```

- Array

  - 实例方法

    - forEach()，遍历数组，不返回新数组

    - map()，返回处理后的数组

    - filter()，返回筛选以后的数组

    - reduce()，迭代器，经常用语求和，会返回一个值

      - ```js
            const arr = [1, 2, 3, 4];
            arr.reduce(匿名函数，[起始值]);
            arr.reduce(function(上一次的返回值，当前的值){}，[起始值]);
            //  如果有起始值就会添加到数组的累计和里面
        
            //  有初始值
            arr.reduce((pre, current) => pre+current, 10);  //返回20
            //  无初始值
            arr.reduce((pre, current) => pre+current);  //返回10
        
            const arr = [
                {name:'one', salary:1},
                {name:'two', salary:20},
                {name:'three', salary:30}
            ]
            arr.reduce((pre, current) => pre + current.salary,0);
            //  当有起始值时，pre则是起始值，如果没有起始值，则pre的值时数组的第一个值，这里会出现问题，因为数组的第一个值是对象，此时的pre会被赋值为一个对象
        ```

    - find()，查找函数，查找符合测试函数条件的第一个数组元素

      - ```js
            const arr = [
                {name:'one', salary:1},
                {name:'two', salary:20},
                {name:'three', salary:30}
            ]
            arr.find(item => item.salary === 30)
            //  返回符合条件的第一个元素，注意是元素，这里的第一个元素是一个对象，然后箭头函数可以省略return
        ```

    - every()，测试一个数组内的元素是否都能通过测试函数，返回的是一个Boolean值，注意是全体元素都必须通过

      - ```js
            const array = [1, 2, 3];
            array.every(item => item >= 2);     //false
            array.every(item => item >= 1);     //true
        ```

    - some()，测试一个数组内任意以个元素是否能通过测试函数，返回的是一个Boolean值，注意是任意元素通过即可返回true，与every相反

      - ```js
            const array = [1, 2, 3];
            array.some(item => item >= 4);     //false
            array.some(item => item >= 3);     //true
            array.some(item => item >= 2);     //true
            array.some(item => item >= 1);     //true
        ```

  - 静态方法

    -   Array.from(伪数组)，伪数组转换成真数组，返回一个真数组

- String

  - 字符串的本质也是一个对象，被js底层进行包装

  - 常用实例方法

    - split(分隔符)，将字符串分隔为一个数组，以分隔符分隔

    - substring(开始位置,[结束位置]),从一个字符串中截取一个字符串，结束为止可以缺省，如果缺省则默认截到字符串末尾 ，结束位置不被包含，是开区间

    - stratsWith(子字符串,[位置])，从位置开始在字符串中检索，是否出现了字符串，如果字符串出现则返回true，否则false，位置可以缺省，如果缺省，则从0开始

    - includes(子串,[位置])，判断某个子串是否包含在字符串中，返回一个Boolean值

    - ```js
          const str = '0-1-2-3-4';
          str.split('-');         //返回[0,1,2,3,4]
          str.substring(2,5);      //返回1-2
          str.startsWith('0-1');  //true
          str.startsWith('0-1',1);    //false
          str.startsWith('2-3',4);    //true
          str.includes('2-3');    //true
          str.includes('-4');    //true
          str.includes('2-3',5);    //false
      ```

- Number

  -   实例方法：toFixed([位数])，设置保留的小数位，位数可以省略，如果省略则是0，会有四舍五入

#  原型对象

## 构造函数

- 构造函数体现了面向对象的封装性
- Js要想实现封装必须要使用构造函数来进行实现，但是会出现浪费内存的情况出现

## 原型对象

- 构造函数通过原型分配的函数是所有对象共享的
- Js规定每个构造函数都有一个prototype属性，指向另一个对象，也称为原型对象，这个对象可以挂载函数，但实例化不会创建多次，节省空间
- 原型就是一个对象
- 可以把不变的方法和属性定义在prototype身上，这样再进行实例化时，就只需要用一次就可以了
- 原型对象里面的函数的this指向实例对象

## constrcutor

- 每个原型对象都有一个这个属性，作用是指向该原型对象的构造函数，构造函数里面的prototype对象里的constructor指向prototype的构造函数

- 在使用prototype进行批量复制的时候一定要将constructor属性指回构造函数

- ```js
  function Person() {
      
  }
  Person.prototype = sin : function() {}		//	进行赋值，将公共的方法挂载到prototype身上
  Person.prototype = sin : function() {}		//	进行赋值，将公共的方法挂载到prototype身上
  Person.prototype = sin : function() {}		//	进行赋值，将公共的方法挂载到prototype身上
  Person.prototype = sin : function() {}		//	进行赋值，将公共的方法挂载到prototype身上
  Person.prototype = sin : function() {}		//	进行赋值，将公共的方法挂载到prototype身上
  
  //	如果有多个方法需要挂载，就显得繁琐于是可以这样
  Person.prototype = {
      sin : function(){}
      sin	: function(){}
  }//	执行这样的操作以后，这个{}对象就将原来的prototype对象覆盖掉的，而原来那个对象存在的constructor就不见了
  
  Person.prototype = {
      constructor : Person,
      sin : function(){},
      sin	: function(){}
  }	//于是可以这样，在整体赋值之前，将constructor指回它的构造函数即可
  ```

  

## 对象原型

- 对象都会有一个属性

- ```js
  __proto__，指向构造函数的prototype原型对象，之所以实例化对象可以使用构造函数的prototype原型对象的方法和属性就是因为这个对象有__proto__属性进行相连接
  需要注意的是 __proto__是非标准的js属性，有些浏览器会显示[[prototype]]，他们俩意义相同
  它是一个只读对象
  ```

- ```js
  function Star(){}
  const one = new Star();
  console.log(Star.prototype.constructor);
  ```

## 区别于相关联

- 原型对象

  - 每个构造函数都有prototype属性，它挂载了一些公共属性和方法，这样使得被这个构造函数所创建出来的实例化对象都可以访问那些公共属性和方法。也节省了资源。同时这个prototype也被称为原型对象，本质上是一个对象，这个对象有一个属性叫constructor，这个属性指向了它的构造函数。这个就是原型对象。

- 对象原型

  - 对象原型是每个实例化对象才有的，它必须通过实例化对象进行访问与使用

  - ```js
    Star.prototype === obj.__proto__
    ```

  - 它们俩相等的情况下，又由于prototype的constructor可以访问到构造函数，但是他们原本就是同一个东西，所以都可以访问到他们的构造函数 

## 原型继承

- 在Js中如果要实现继承，就要是用到原型对象，故也称为原型继承，它的思路是，由于每个原型对象prototype本质上也是一个对象，name就可以更换这个原型对象。经过这个操作以后，后面被实例化出来的对象就可以访问刚才新继承的prototype的那个新对象里面的方法和属性，需要注意的是，因为prototype里面又constructor属性，它指向构造函数，但是公共的对象并不会有这个属性，所以需要在更改后，再添加上它的constructor属性，使其指向构造函数

- ```js
  const Person = {
      head : 1,
      eyes : 2,
      say : () => console.log('说话');
  }	//	这个就是需要被继承的属性和方法，可以用这个对象替代prototype属性
  function Man() {}	//	需要使用继承的对象
  Man.prototype = Person;		//	这样就将原来的prototype变成了person
  Man.prototype.constructor = Man;	//	再定义它的constructor属性即可
  ```

- 这里会出现一个问题，如果有两个子类，但是只有一个对象，两个子类的prototype指向的是同一个对象，这样更改了一个就会影响另外一个。于是这里使用构造函数来解决，由于这些对象的方法和属性都是公共的，但是又需要使用不同的对象赋值给他们的prototype所以只需要在继承的时候，new一个新对象赋值给prototype就可以。

  ```js
  function Person() {
      this.eyes = 2;
      this.head = 1;
      this.say = () => console.log('ztc');
  }
  function Man() {}
  function Woman() {}
  Man.prototype = new Person();
  Man.prototype.constructor = Man;
  Woman.prototype = new Person();
  Woman.prototype.constructor = Woman;
  //	这样做以后这两个子类的prototype分别指向两个不同的对象，就不会相互影响，也就可以添加各自独特的方法和属性了
  ```

## 原型链

- 基于原型对象的继承使得不同构造函数的原型对象关联在一起，并且这种关联对象是一种链式结构，也就称为原型链

- 只要是对象就有--proto--，只要是原型对象就有prototype

- 原型链就是一个查找规则，当访问一个对象的属性和方法时，首先在自身查找，如果没有的话，则查找该对象的--proto--也就是它的原型对象身上有没有，如果没有的话，再找上一层原型对象，直至找到顶层对象，为止，底层对象的prototype.--proto--实际上指向的是null

- instanceof用于判断一个对象是否属于一个构造函数，检查这个对象是否存在于原型链上

  - ```js
    consle.log(Array instanceof Object);	//	true
    consle.log([0,1,2] instanceof Object);	//	true
    ```

# 高阶技巧

## 深浅拷贝

- 拷贝都只针对于引用类型，因为简单类型不需要拷贝

- 浅拷贝

  - Object.assgin() ，利用object的静态方法

  - {...obj}，利用展开运算符

  - 如果对象里面嵌套了对象，则里层的对象只会拷贝地址，而又出现了，两个对象指向同一块区域

  - ```js
    const obj = {
        uname : 'ztc',
        age : 21,
        family : {
            father : 'z',
            mather : 'p'
        }
    };
    const o = {};
    Object.assgin(o, obj);		//	这样的话，只拷贝了obj的外层属性，而family属性本质上还是只拷贝了地址，因此对o对象更改属性，obj属性会跟着变
    ```

  - 只能拷贝单层对象

- 深拷贝

  - 拷贝的是对象，而不再是地址了

  - 深拷贝有三种形式

    - 通过函数递归来实现

      - ```js
        const obj = {
            uname : 'ztc',
            age : 21,
            family : {
                father : 'z',
                mather : 'p'
            }
        };
        function deepClone(new, old) {
            for (let k in old) {
                if (old[k] instanceof Array) {
                    new[k] = [];
                    deepClone(new[k], old[k]);
                }else if (old[k] instanceof Object) {
                    new[k] = {};
                    deepClone(new[k], old[k]);
                }else {
                    new[k] = old[k];
                }
            }
        }
        //	这样通过递归的方法即可实现递归深拷贝，即当碰到数组嵌套或者对象嵌套时，再调用函数进行一次拷贝即可
        ```

    - lodash和clonedeep

      - 通过第三方库进行深克隆_.cloneDeep(被克隆的对象)，返回新对象

    - 通过JSON.stringily()实现

      - ```js
        const obj = {
            uname : 'ztc',
            age : 21,
            family : {
                father : 'z',
                mather : 'p'
            }
        };
        const o = JSON.parse(JSON.stringify(obj));
        ```

## 异常处理

- throw 抛异常
  - throw new Error(报错信息);
  - 抛出后，程序中断，后续就不执行了
- try/catch 捕获异常
  - 可能发生错误的代码写在try块中
  - catch(err)，可以输出err.message，输出错误信息，如果产生错误的情况下
  - 报错以后，后面的代码还是会执行，不会中断程序，某些情况下如果需要中断，可以直接抛出一个throw异常即可、
  - finally中的代码块，一定会被执行
- debugger
  - 可以直接在浏览器中刷洗，会直接跳到这一行代码

## 处理this

- this指向

  - 普通函数的this指向调用者
  - 普通函数严格模式下，this指向undefined
  - 箭头函数的this指向它的上一层的this，直至this有意义
  - 箭头函数时用于要使用上层this的情况，在构造函数，原型函数，dom事件函数中不推荐使用箭头函数

- 改变this，在js中允许指定this的指向，有三个方法可以动态的指定普通函数中的this

  - call(新的this对象，参数)，可以改变this的指向

    - ```js
      function fn() {}		//	原本this是指向window的
      const obj = {};
      fn.call(obj, 参数1， 参数2);		//现在fn里的this已经指向obj了
      ```

  - apply(新的this，[参数])，新的this可以设置为null，但是参数智能以数组方式来传递

    - ```js
      function fn(x, y, z) {}		//	原本this是指向window的
      const obj = {};
      fn.apply(obj, [1,2,3]);
      ```

    - ```js
      利用这个方法可以求数组的最大/小值
      const array = [1, 2, 3, 4, 5];
      Math.max.apply(Math, array);
      Math.min.apply(Math, array);
      ```

  - bind()，语法和call()相似，但是不会马上调用函数，会返回 

## 性能优化

- 防抖，是指指单位时间内，频繁触发事件，只执行最后一次

  - lodash库提供的防抖处理

    - 使用    _.debounce(fnu, time)

    - ```js
      function fn() {
          console.log('111');
      }
      box.addEventListener('click', _.debounce(fn, 500));	//每隔500毫秒去调用结果
      ```

  - 手写防抖函数

    - 核心思路

      1. 声明一个定时器变量
      2. 当事件触发时先判断定时器是否存在，如果存在就先清除之前的定时器
      3. 如果没有定时器就开启定时器，然后存在变量里，方便下次清除
      4. 在定时器里调用要执行的函数

    - ```js
      function debounce(fn, time) {
         let timer = null;
         return () => {
            timer && clearTimeout(timer);
            timer = setTimeout(() => {
                    fn()
             }, time);
         }
      }
      document.addEventListener('mousemove', debounce(() => console.log(111), 500));
      ```

- 节流

  - 单位时间内，频繁触发事件，只执行一次，当那一次执行完毕以后，再开始后面的事件

  - lodash节流函数

    - ```js
      function fn() {
          console.log('111');
      }
      box.addEventListener('click', _.throttle(fn, 500));	//500毫秒内只执行一次
      ```

  - 手写节流函数

    - 核心思想类似于防抖，只不过在触发的时候，先检查是否存在定时器，如果存在的话，就不进行操作

    - ```js
      function throttle(fn, time) {
         let timer = null;
         return () => {
             if(!timer) {
                 timer = setTimeout(()=>{
                     fn();
                     timer = null;
                 }, time)
             }
         }
      }
      document.addEventListener('mousemove', throttle(() => console.log(111), 500));
      ```

## 节流小案例

- ontimeupdate，当视频时间出现变化时，就触发该事件
- onloadeddata，当视频的下一帧还没被加载到时，触发该事件

# 类

## 语法

- ES6中的类没有提升，必须在实例化之前定义

- 类中所有的函数都不需要加function

- ```js
  class className {
      constructor() {
          
      }	//	constructor方法是类的构造函数，默认方法，用于传递参数，返回实例对象，通过new生成对象实例，自动调用该方法。如果没有显示定义，系统内部将自动定义
      say() {
          
      }		//	方法可以直接写名字，不需要function
  }
  ```

## 类的继承

- 同java，使用extends

- 但区别是，子类在继承以后，想要使用父类的方法，如果父类的方法访问到了变量，则变量必须要通过父类的构造函数获得

- ```js
  class Father {
      constructor(x) {
          this.x = x;
      }
      show() {
          console.log(this.x);
      }
  }
  class Son extneds Father {
      constructor(x) {
          this.x = x;			//		应该改为super(x);，将x传给父类，因为要使用父类的方法
      }
  }
  const son = new Son(10);
  son.show();		//	报错
  ```

- super关键字，用于调用父类的构造函数和普通函数，必须在子类之前调用

- 如果使用一个方法，如果子类中没有，则去父类中去找

## 注意点

- 类里面的公共属性和方法要使用this
- constructor里面的this指向新建的实例，方法里的this指向调用者

