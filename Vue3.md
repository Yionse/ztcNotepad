# Vue3简介

- 与Vue2的区别如下
  - 更好的支持了TypeScript
  - CompositionApi(组合API)
    - setup配置
    - ref与reactive
    - watch与watchEffect
    - provide与inject
  - 新的内置组件
    - Fragment
    - Teleport
    - Suspence
  - 新的生命周期钩子
  - data选项始终被声明为一个函数
  - 移除keyCode支持作为v-on的修饰符
  - Vue3组件中，可以不存在根标签

# 常用CompositionAPI(组合式API)

## setup

- Vue3中的一个新的配置项，值为一个函数
- 在Vue2中写的data、methods、watch、computed、生命周期钩子等等，都写在setup中
- setup有两种返回值的格式
  - 若返回一个对象，则对象中的属性、方法可以直接在模板中使用
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230610220235168.png" alt="image-20230610220235168" style="zoom: 50%;" />
  - 若返回一个渲染函数，则可以自定义渲染函数中的内容
    - ![image-20230610220645173](D:\Project\Web\Notepad\image-20230610220645173.png)
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230610220657878.png" alt="image-20230610220657878" style="zoom:67%;" />
    - 这个时候，就会替换掉template中的所有内容，不用该方法
- 在Vue3中也可以使用Vue2的配置与方法，可以直接在Vue3的模板中访问到，但Vue2中却访问不到Vue3中的数据与方法，且当Vue2和Vue3出现冲突时，以Vue3的值为准，但非常不推荐用这种写法
- setup不能是一个async函数，因为此时的返回值就不再是普通的返回值了，而是一个Promise，数据都包裹在Promise中去了

## ref函数

- 在setup中的函数，更改了setup中的属性，数据是会被更改，但是不会被Vue捕获到，也就不会触发，模板的重新解析，数据就失去了响应式
- Vue3中要实现响应式数据，需要引入在Vue中引入一个ref函数，在赋值时，值需要被ref函数包裹
- 而经过ref函数包裹的变量，就变成了一个对象
  - ![image-20230610222146885](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230610222146885.png)
- ref会返回一个RefImpl的引用对象
- 在更改的时候，需要使用.value属性，从而进行更改，这样就可以引发页面模板重新解析
- 且直接在template中使用时，不需要.value
- ref接收的数据可以是基本的数据类型，也可以是复杂数据类型，当传入的是复杂数据类型时，响应式就不是依靠的Object.defineProperty了，而是封装了一个reactive函数，这个函数实现了对Proxy的封装

## reactive函数

- Vue3中专门用于处理对象类型数据的函数，不能传入一个普通的类型，否则会报错
- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230610225541472.png" alt="image-20230610225541472" style="zoom:67%;" />
- 通过reactive处理的变量，可以直接更改其值，会触发Vue模板的重新解析
- ![image-20230610225617410](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230610225617410.png)
- reactive的响应式是深层次的，内部基于Proxy实现

## Vue2中的响应式原理

- Vue2中的响应式原理是基于Object.defineProperty实现的，在访问时调用setter，修改时调用getter，而在检测数组类型时，需要调用指定的方法，才能触发Vue的模板重新解析
- 且在新增属性、删除属性时，界面不会更新。另外在直接使用数组下标修改数据时，模板也不会重新解析
- 在新增属性时，要使用this.$set方法，进行新增属性，这样新增后的属性就是响应式的。而删除属性时，使用的是this.$delete方法进行删除。在对数组进行修改时，就需要使用到Vue2指定的，会触发模板重新解析的那些函数
- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230610230927066.png" alt="image-20230610230927066" style="zoom: 67%;" />
- 手写实现Vue2响应式
  - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230610231739123.png" alt="image-20230610231739123" style="zoom:67%;" />

## Vue3中的响应式原理

- 在Vue3中直接对对象进行添加属性操作，会触发模板重新解析，而这一切都是reactive的功劳
- 在Vue3中实现响应式，使用到了ES6中新增的Proxy
- 模拟实现
  - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230610234034327.png" alt="image-20230610234034327" style="zoom:50%;" />
  - 同时使用到了代理和反射，Reflect中有get、set、deleteProperty方法，可以用这些方法会对象进行读取、修改、删除操作，且有返回值，记录了当前操作是否成功
- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230610234149801.png" alt="image-20230610234149801" style="zoom: 50%;" />

## reactive和ref

- ![image-20230610234257164](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230610234257164.png)


## setup的两个注意点

- setup的执行，比beforeCreate执行得还要早，且此时的this为undefined
- ![image-20230611135151769](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230611135151769.png)

## computed函数

- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230611141452489.png" alt="image-20230611141452489" style="zoom:67%;" />

## watch函数

- Vue3中的watch也是一个组合式API，写在setup中，但是如果用旧的Vue2的写法，也会生效
- 正确的是写在setup中，以函数的形式
- 同时又分为两种情况，监视ref函数生成的值，和reactive生成的值，有些许区别
  - ref
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230611144452073.png" alt="image-20230611144452073" style="zoom:67%;" />
    - 当需要一次性监视多个值时，则第一个参数写为一个数组，而后面的newValue和oldValue也就变成了数组
    - 第三个参数是配置项，可以配置立即执行，immeiate
  - reactive
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230611144859789.png" alt="image-20230611144859789" style="zoom:67%;" />
    - 坑巨多
- 精确侦听
  - 具体侦听某个属性，不开启deep的情况下
  - ![image-20230614091141510](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230614091141510.png)
  - 将第一个参数写为函数形式
- ![image-20230611144954696](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230611144954696.png)

## watchEffect函数

- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230611152453834.png" alt="image-20230611152453834" style="zoom:67%;" />

## Vue3生命周期

- Vue3中的生命周期钩子也可以继续使用Vue2中的，但有两个做了小修改
  - ![image-20230611153441020](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230611153441020.png)
- setup就相当于Vue2中的beforeCreate和create钩子了，不提供组合式API的写法，除非写在setup的外面，但是setup的执行优先
- 在组合式API中前面加一个on即可
- ![image-20230611154131754](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230611154131754.png)

## hook函数

- 本质是一个函数，把setup中使用到的组合式API进行封装
- hook函数一般以use开头
- 组合式API在hook中体现
- ![image-20230611172516780](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230611172516780.png)

## toRef

- ![image-20230611173938796](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230611173938796.png)
- 将一个不是ref类型的对象，转为一个ref对象
- ![image-20230611174331832](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230611174331832.png)

## ref组合ref函数获取DOM节点

- 首先调用ref函数，获得一个ref对象
- 通过ref标识，绑定ref对象
- ![image-20230614093644837](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230614093644837.png)
- ![image-20230614093651084](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230614093651084.png)
- 在挂在完毕后，才能获取
- ![image-20230614093854338](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230614093854338.png)
- 默认将组件内部的方法和属性隐藏，通过宏函数对外暴露

# 其他CompositionAPI

## shallowRef与shallowReactive

- shallowReactive只考虑对象的第一层响应式，剩下的不考虑，也就是浅层次的响应式
- shallowRef不处理对象类型的数据，它只能处理普通的数据
- ![image-20230611180940696](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230611180940696.png)

## readonly和shallowReadonly

- ![image-20230611181808966](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230611181808966.png)
- readonly所有的数据都只读，shallowReadonly只让数据的第一层只读

## toRow与markRow

- toRow将一个响应式的数据，变为一个普通的数据
- toRow只能处理reactive类型的响应式数据
- markRow标记一个数据，那么这个数据永远都不能变为响应式了

- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230611184144969.png" alt="image-20230611184144969" style="zoom: 67%;" />

  ## customRef

  - 自定义ref
  - 创建一个自定义的ref，并对其依赖项跟踪和更新触发，进行显示控制
  - ![image-20230611191540217](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230611191540217.png)

## provide与inject

- ![image-20230611202518035](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230611202518035.png)

- 也可用于传递方法
  - ![image-20230614094711664](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230614094711664.png)
  - ![image-20230614094731239](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230614094731239.png)

## 响应式数据的判断

- ![image-20230611202611765](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230611202611765.png)

# 其他

## CompositionAPI的优势

- 在传统的API中需要分别在配置项中进行修改
- 可以更加优雅的组织代码，让数据、函数更有序的在一起

## 新的组件

- Fragment，Vue3可以没有根标签，是因为内部将其它的包裹在Fragment中了，减少层级嵌套，减少内存占用
- Teleport，可以将该组件，移动到任何一个标签上，这样就可以更好的利于调整css
- Suspense，当组件还未加载成功时，页面可能会出现长时间空白的情况，此时需要一个东西来代替该组件
  - ![image-20230611205910849](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230611205910849.png)
  - 注意此时组件必须是动态引入的，因为静态引入的组件是必须等所有组件全部加载完成之后，再一起显示到页面上，所以静态组件对该方法是无效的
  - 动态引入组件的方法
    - ![image-20230611210010678](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230611210010678.png)

# 组件间通信

- 在setup语法糖下，子组件无需注册，可以直接使用
- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230614092108678.png" alt="image-20230614092108678" style="zoom:67%;" />

## 父传子

- 在子组件中使用defineProps函数，接收
- ![image-20230614092300475](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230614092300475.png)
- 如果想在setup中使用，则定义变量接收即可

## 子传父

- 父组件定义好自定义方法，在子组件中通过defineEmits函数，接收所有的自定义方法，defineEmits函数，返回一个函数，一般称为emit
- 通过emit调用
- ![image-20230614093104607](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230614093104607.png)

# Pinia

- Vuex的替代品
- 提供了更加简单的API，去掉了mutation
- 提供符合组合式API的新风格，与Vue3风格统一
- 去掉了modules的概念，每一个store都是一个独立的模块
- 搭配TypeScript一起使用提供可靠的类型推断
- 使用npm i pina安装，在main.js中，引入一个createPinia方法，得到实例，通过app.vue()进行注册
- ![image-20230615102521826](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230615102521826.png)

## getters与异步action

- getter与在setup中定义一样，调用computer函数
- ![image-20230615102734971](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230615102734971.png)
- 异步写法，也是同步写法，需要使用return，返回出去
- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230615103048791.png" alt="image-20230615103048791" style="zoom:50%;" />

## storeToRefs

- 在模板中，使用store中的属性和方法时，可能需要频繁的.获取，如果直接使用普通的解构，可能导致响应式丢失，此时需要使用此方法
- ![image-20230615103413383](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230615103413383.png)
- pinia中的方法
- store中的方法，可以解构赋值，无需调用该方法