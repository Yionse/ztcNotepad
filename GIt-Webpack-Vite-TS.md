# Git

- git pull将远程仓库中的文件，与本地仓库进行同步
- git -v 查看当前git的版本
- git clone git地址，将远程仓库克隆到本地
- git init 将当前文件夹，初始化为一个git仓库，成功后，多了一个.git文件夹
- git config [--global] user.name 进行配置文件设置，添加了global以后是全局的
- git status， 暂存区的状态
- git add 文件名， 将文件添加到暂存区
- git rm --cached 文件名，将暂存区的文件删除掉
- git commit -m'描述'，将文件提交
- git log [--online，一行显示]查看日志
- git restore 文件名，将在本地删除的文件，进行恢复
- git reset --hard 版本号，这样即使是上一个方法无法恢复，通过该方法也可以进行恢复，但是需要通过 git log查看提交的版本号
- git revert 版本号，还原到这个版本号之前，包括提价记录也都还原，reset重置后，提交记录不可见了
- git branch 创建分支，但必须存在commit之后
- git branch -v，查看当前所有的分支
- git checkout 分支名，切换分支
- git checkout -b 分支名，创建一个分支，且切换为当前分支
- git branch -d 分支名，删除分支
- git merge 分支名，将分支与master进行合并，但需要当前处在master分支下

# Webpack

- 浏览器并不是别es6的模块化，所以如果使用了import，然后直接在浏览器环境下打开，是无法正常得到结果的
- npm init -y，初始化一个包描述文件，package.json
- 5大核心概念
  - 入口(entrt)：提示Webpack从哪个文件开始打包
  - 输出(output)：指示Webpack打包完的文件输出到哪里去
  - 加载器(loader)：Webpack本身只能处理js，json资源，需要通过加载器处理其他资源
  - 插件(plugins)：扩展Webpack功能
  - 模式(node)：Webpack有两种模式，development(开发模式)、production(生产模式)
- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230610145949074.png" alt="image-20230610145949074" style="zoom:67%;" />

## 使用步骤

- 初始化项目
  - npm init -y
  - 安装依赖，webpack、webpack-cli，要使用-D，因为这是开发依赖，项目不依赖于此
  - 在项目中创建src目录，为源代码目录
  - 使用npm run build对项目进行打包
  - 要使用Build命令，需要进行配置

## webpack.config.js

- Webpack的配置文件，配置决定了Webpack如何进行工作
- 配置项
  - mode，设置打包的模式
    - production生产模式
    - development开发模式
  -  entry，设置打包时的入口文件，默认是src下的index.js
    - 一般不建议修改
    - entry也是可以传入一个数组
    - 也可以是对象，当是对象的时候，则是分别打包，对象的属性就是一会打包出来的文件名，值就是需要打包的文件的路径
    - 一般都是单文件
  - ![image-20230613195423675](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230613195423675.png)
  - output，输出打包后的地址，是一个对象
    - filename：打包后的文件名，当打包的是多个文件时，值应为一个数组
      - ![image-20230613195932962](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230613195932962.png)
    - clean：是否自动清空dis目录，确保dist文件每次打包之前，都已经清空了
    - path：指定打包的目录，可以自定义，但必须要绝对路径
  - loader
    - 当需要打包css文件时，可以直接在入口文件上面，使用import引入即可
    - 需要安装css-loader、style-loader，且在填写loader时，要将css-loader写在后面，因为是从后开始执行得
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230613201841500.png" alt="image-20230613201841500" style="zoom:67%;" />
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230613202847512.png" alt="image-20230613202847512" style="zoom: 50%;" />
    - 引入图片时，不需要loader，需要再rules中额外创建一条规则
      - ![image-20230613203519057](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230613203519057.png)
  - babel
    - 是一个js编译器，对js进行语法转换，将高版本的js转换为低版本的js，以提高代码的兼容性
    - 安装  babel-loader、@babel/core、@babel/preset-env
    - babel其实是一个新的规则，所以是配置在rules数组中的
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230613205918593.png" alt="image-20230613205918593" style="zoom: 50%;" />
    - exclude表示不处理的文件夹，而use中的对象中则是，更加详细的配置loader
    - 在package.json中配置，需要兼容哪些版本的浏览器
      - ![image-20230613210358749](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230613210358749.png)
  - plugins
    - 插件，用来帮Webpack扩展功能，值是一个数组
    - html-webpack-plugin，可以在打包后，自动生成html文件
    - 先安装依赖，然后进行配置
    - 由于是插件，所以需要再main.js中进行引入
    - ![image-20230613211207461](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230613211207461.png)
    - 可以传入一个参数的配置对象
    - title：生成的html的标题，而template则是已经存在的一个html文件，设置了该项，则以该html文件为模板
- 安装webpack-dev-serve，安装一个开发服务器
  - 当代码修改时，则会自动进行更新
  - npm run serve --open在构建完成后，自动打开
  - 当使用该命令时，并不会马上构建，也就看不到dist目录，而是打包在微服务器上
- dev-tool，表示的是开发工具
  - ![image-20230613212316974](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230613212316974.png)
  - 就可以在运行时，打断点

# Vite

- 下一代的前端构建工具，同webpack
- vite没有根目录的说法，可以直接将文件写在项目目录中
- 安装vite，npm i -D vite
- 使用 npm vite启动项目
- npm vite preview，预览打包后的文件
- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230613213959525.png" alt="image-20230613213959525" style="zoom:50%;" />
- 快捷创建项目
  - ![image-20230613214222254](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230613214222254.png)

## Vite的配置

- 文件名为`vite.config.js`
- 使用es6的模块化进行向外暴露

# TypeScript

- Ts不能直接被浏览器运行，TS是JS的超集
- TypeScript增加了什么
  - 类型
- 即使可能ts出现错误，编译时，可能还是会编译成功，将ts编译为js

## 类型

- TypeScript可以强制一个变量的类型为某种
- 通过如下方式，设置一个变量的类型为number类型
  - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230612163050304.png" alt="image-20230612163050304" style="zoom:67%;" />
  - ![image-20230612163158525](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230612163158525.png)
- number-string-boolean
- 就算不确定类型，那就就以第一次赋值的类型为准，后续的类型只能与第一次相同
  - ![image-20230612163908355](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230612163908355.png)
  - ts可以自动进行类型判定
- 类型限制，也可以用在函数身上，进行参数类型限制
- ![image-20230612165403881](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230612165403881.png)
- 也可以使用字面量限制值，也可以使用联合类型
  - ![image-20230612165807046](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230612165807046.png)
- any表示什么类型都可以，相当于对该变量关闭了类型检测，如果声明变量，不赋值，不指定类型，则会自动判断类型为any，就为隐式any。另外any赋值给别的变量时，就算做了类型限制，也不会报错
- unknown表示未知类型，跟any的区别是unknown类型的不能赋值给其他类型，就是一个类型安全的any，不能直接赋值给其他变量
  - 可以使用typeof进行类型判断，之后再赋值给其它的变量
  - ![image-20230612173452689](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230612173452689.png)
  - 类型断言
- void和never经常用在函数的返回值中
- object类型
  - object类型，由于对象，数组，函数，在js中都是object类型，所以意义不大，一般采用{}这种限制写法
  - ![image-20230612174629734](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230612174629734.png)
  - 如果想属性是可选的，那么可以使用[propName:string]:any
    - ![image-20230612174709522](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230612174709522.png)
    - 表示任意类型的属性值
- 定义函数结构
  - ![image-20230612175041394](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230612175041394.png)
  - 如果参数个数与类型或者是返回值不一致的话，就会出现错误
- array数组
  - ![image-20230612175301426](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230612175301426.png)
  - 在类型后面加上[]，即是数组
  - 另外一种写法
    - ![image-20230612175325640](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230612175325640.png)
- tuple，元祖，就是固定长度的数组，存储效率会更高
  - ![image-20230612175614626](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230612175614626.png)
  - 元素严格按照指定的类型写，否则会出错
- enum，枚举
  - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230612180009665.png" alt="image-20230612180009665" style="zoom:50%;" />
  - 要通过enum提前设置好
- 类型的别名
  - 为了方便自定义的类型
  - ![image-20230612180759733](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230612180759733.png)

## 配置选项

- tsc xxx.ts -w，当加上-w时，会自动检测该ts文件，当该ts文件，被修改时，会被自动编译
- 当需要监视一个文件夹下所有的ts文件时，需要在当前文件夹下，加一个配置文件，配置文件名为tsconfig.json，然后此时使用tsc命令，就可以将当前文件夹下所有的ts文件，全部编译成js文件
  - 而使用tsc -w则可以自动监视当前目录下所有的ts文件，当ts文件发生修改时，可以自动编译
- include
  - ![image-20230612184155449](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230612184155449.png)
- exclude
  - 一般情况下不使用
  - ![image-20230612184332207](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230612184332207.png)
- extends
  - 继承配置文件
  - ![image-20230612184400793](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230612184400793.png)
- files
  - 包涵需要被编译的文件，要设置单独的文件，但是比较复杂
- compilerOptions
  - 编译器选项
  - 决定了编译器如何工作
  - target
    - 决定编译后的js版本，默认是es3
  - module
    - 指定要使用的模板快规范
    - 可以是es6模块化
    - 也可以是commonjs
  - lib
    - 所需要依赖的库，一般在浏览器的环境下运行，不需要修改
  - outDir
    - 用来指定，编译后的js文件，存放文件夹
  - outFIle
    - 将代码合并为一个js文件
  - allowjs
    - 值为Boolean类型
    - 是否对js文件进行编译，默认是false
  - checkjs
    - 值为Boolean类型，默认是false
    - 是否对js语法进行检查
  - removeComments
    - 是否删除注释，默认为false
  - noEmit
    - 是否不生成编译后的文件，默认为false，为true则不生成了
  - noEmitOnError
    - 当有错误时，编译后是否生成文件，默认为false，为true则不生成了
  - alwaysStrict
    - 编译后的js 文件，是否为严格模式，默认为false
  - noImplicitAny
    - 默认值为false，是否不允许隐式的any类型，默认值为false
  - noImplicitThis
    - 不允许不明确类型的this，默认值为false
  - strictNullChecks
    - 是否严格检查空值，默认值为false
  - strict
    - 所有严格检查的总开关，如果为true，以上的种种严格检查，都会打开了
    - 开发建议设置为true

## 使用Webpack打包Ts文件

- 需要安装四个包，Webpack、Webpack-cli、TypeScript、ts-loader
- 一共需要修改三个配置文件
  - webpack.config.js
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230613090129714.png" alt="image-20230613090129714" style="zoom:50%;" />
  - tsconfig.json
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230613090210258.png" alt="image-20230613090210258" style="zoom:50%;" />
  - package.json
    - 添加一个build命令，进行打包
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230613090246760.png" alt="image-20230613090246760" style="zoom:50%;" />

## 面向对象

- 对象中主要包含两部分，属性、方法
- 在属性前加static，则属性变为静态属性
- readonly修饰的变量，为只读，不可修改，顺序在static之后
- 定义方法，直接写方法名，不用写function，方法加static，则为静态方法，通过类名调用
- 继承
  - 如果在子类中写了constructor函数，那么要在constructor函数中，调用父类的构造函数，即直接调用super即可，并且传入相应的参数
- 抽象类
  - 以abstract开头的类为抽象类，不允许直接new
  - 必须被继承以后，才可以被new
  - 定义抽象方法，abstract fn():void;，子类必须对抽象方法进行实现
- 接口
  - 接口可以进行组合
  - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230613111242313.png" alt="image-20230613111242313" style="zoom: 67%;" />
  - 接口只有ts才有
- 属性的封装
  - 可以在属性前，添加属性修饰符
  - public(公共的，默认值)、private(私有的，只能在类内部进行访问修改)、proteced(受保护的，只能在当前类及其子类中访问)
  - Ts中设置getter和setter的方法
    - get name() {return this.name}
    - set name(value:number) {this.name = value}
  - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230613123707269.png" alt="image-20230613123707269" style="zoom:50%;" />
  - 可以直接将属性定义在构造函数的形参中，需要带属性修饰符，这样就无需再通过this进行赋值
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230613124617090.png" alt="image-20230613124617090" style="zoom:50%;" />
- 泛型
  - 在定义函数或类时，如果遇到类型不明确的就可以使用泛型
  - 泛型就是一个不确定的类型
  - ![image-20230613125203131](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230613125203131.png)
  - 接口也可以作为泛型
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230613125516555.png" alt="image-20230613125516555" style="zoom:67%;" />
    - 类型必须是inter的子类
- 
