# Vue基础语法

```html
<div id='root'>
    <h1>
        {{name}}
    </h1>
</div>
```

```js
const x = new Vue({
    el : '#root',	//	element，元素，它的值通常是一个选择器，指定当前实例为哪个容器服务，或者用原生的dom去查找
    data: {
        name: '张天成'
    }		//	需要展示的数据，通常用对象，然后在前端代码中使用插值语法，{{name}}，来进行展示，数据供el所指定的元素使用
});
```

- 一个容器只能被一个vue实例接管

  - ```js
    //	html中有一个id为root的容器
    new Vue({
        el: '#root',
        data: {name: 'ztc'}
    });
    new Vue({
        el: '#root',
        data: {name: 'test'}
    });
    //	由于第一个vue实例已经接管了root容器，虽然第二个vue实例的代码也可以正常运行，但是并不会产生作用，容器与实例之间只能是一对一的关系
    ```

- {{}}插值语法中必须是js表达式，且可以使用data中的所有属性，一旦data发生改变，页面也会随之改变

## 模板语法

- 使用v-bind可以简写成：去绑定这个属性，让其可以得到vue实例中的值

  - ```html
    <div id='root'>
        <a href='url'></a>    <!--这种写法无法获得vue实例中的数据，因为被当成普通的字符串解析了-->
        <a :href='url'></a>   <!--使用v-bind以后，就可以获取到vue实例中的值了-->
        <a v-bind:href='url'></a>	<!--或者使用这种方法-->
    </div>
    ```

  - ```js
    new Vue({
        el: '#root',
        data: {
            url: 'xxxx'
        }
    });
    ```

- 插值语法用于指定标签里面的内容，而模板语法用于指定标签的属性

- vue模板语法两大类

  - 插值语法：用于解析标签体的内容，使用{{xxx}}的方式

  - 指令语法：用于解析标签，包括标签属性，标签体内容，绑定事件，使用v:bind或者其简写：，使用了指令语法以后，就会被当成js表达式执行

    - ```html
      <a href='Date.now()'></a>	<!--不会有任何改变，就是一个普通的字符串-->
      <a :href='Date.now()'></a> 	<!--会被当成js表达式执行-->
      ```

- data中的数据也可以是多级对象结构

## 数据绑定

- v-bind是一个单向的数据绑定，当vue实例中的data数据发生改变时，页面会随之改变，但是页面中的数据改变时，不能影响vue中的data

- v-model是双向数据绑定，即当页面中的数据发生改变时，data中的数据也会发生改变，但是v-model智能应用在表单类型数据上，即输入类元素上，v-model:value可以简写为v-model，因为v-model就是收集的value的值

- ```html
  <input :value='name'>		<!--v-bind的简写-->
  <input v-model='name'>		<!--v-model的简写-->
  ```

## el与data的两种写法

- el的两种写法

  - ```js
    new Vue({
        el: '选择器',
        data: {}
    });		//	第一种，必须在new时，就指定el的值
    ```

  - ```js
    const v = new Vue({
        data: {}
    });
    v.$mount(选择器);	//	第二种，在接收到new返回的vue实例时，通过v.$mount()方法去指定选择器
    ```
  
- data的两种写法

  - ```js
    new Vue({
        data: {}
    })	//	对象式
    ```

  - ```js
    new Vue({
        data: function() {
            return {
                key: value
            }
        }
    	//	一般写成data() {函数体}    
    });		//	函数式，data是一个函数，并且，函数必须有返回值，且为对象类型
    ```

  - 当时用函数式时，data中的函数中的this是Vue实例对象，表明是Vue在底层调用了这个函数，从而获得了这个函数的返回值，注意这里的函数不能是箭头函数，因为箭头函数不存在this，也就无法指向Vue实例对象

- 由Vue管理的函数，不能使用箭头函数，因为使用了箭头函数，那么this就不指向Vue实例了

## MVVM模型

- MVVM
  - M：model模型，对应data中的数据
  - V：view视图，模板
  - VM：视图模型，vue实例
- 将一些数据通过视图模型放在视图上
- data中所有的属性，最后都出现在vm身上
- vm身上所有的属性，以及Vue原型身上的所有属性，都可以在vue模板中直接使用

## 数据代理

- Object.defineProperty(被添加属性的对象, 属性名, 配置对象);

  - 配置对象中的value属性，即是被添加的属性的属性值

  - ```js
    Object.defineProperty(person, age, {
        value: 18	//	18即是age的值
    });
    ```

- 通过这个方法添加的属性，不参与遍历或枚举，如果想让添加的对象是可枚举的，则使用配置对象中的属性enumerable:true，即可枚举

  - 还不能被修改，如果想要被修改，则需要使用配置对象的writable:true，则可以修改
  - 还不能被删除，如果想要删除，则需使用configurable:true
  - 当有人读取该属性名时，则会调用get方法，这个方法返回一个值，这个值就是age的值
  - 当有人修改该属性值时，则会调用set方法，这个方法有一个参数，就是修改的那个值
  - ![image-20230505170737062](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230505170737062.png)

- 数据代理：通过一个对象代理对另一个对象的读和写

  - ![image-20230505171029464](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230505171029464.png)

- vue中的数据代理

  - 通过配置项设置的data值，存在于vm._data这个属性中，通过该属性即可以获得
  - ![image-20230505172257047](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230505172257047.png)
  - 通过vm对象，去代理配置对象中的data对象，可以更加方便的操作数据
  - 通过Object.defineProperty()方法把data中的对象，添加到vm身上
  - 为每一个添加到vm身上的属性，都指定一个setter，getter，在setter的内部去更改_data和获取_data

## 事件处理

- 在vue中使用v-on:事件，进行事件处理，由于vue中只能使用vue配置项中的东西，所以回调函数也需要写在vue配置项中
- 通过methods属性，定义一些函数，既可以在v-on:事件=处理函数，中调用
- ![image-20230505173437818](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230505173437818.png)
-  v-on:click=function，可以简写为@click=function
- 回调函数可以加小括号，但在不需要参数时，一般是省略，如果需要参数，则对应着普通函数的调用，如果在回调函数中，还想使用event对象，则使用$event占位符，那么在回调函数中既可以收到event
- ![image-20230505174212319](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230505174212319.png)
- 在vue中的事件回调也可以在vm身上看到
- ![image-20230505175058013](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230505175058013.png)
- 事件修饰符
  - 当触发事件后，不想再调用该事件的默认事件，@click.prevent即可以阻止该默认事件
  - vue中常用的事件修饰符
    - ![image-20230505175420634](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230505175420634.png)
- 键盘事件
  - ![image-20230505181056676](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230505181056676.png)
  - 通过在事件后加.enter去触发
  - 如果需要自定义的键盘事件，则先通过事件对象e.key获得这个键的名字，然后使用.连接在事件名的后面，并且，如果这个键是组合单词的话，那么就需要变换为小写，然后中间用
    - ![image-20230505181825101](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230505181825101.png)
    - ![image-20230505181521063](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230505181521063.png)
    - 当使用tab键触发事件时，需要使用keydown去触发，由于tab默认会将元素的焦点切换至下一个元素，所以如果用up，在元素触发事件时，焦点已经切换，就无法获得该元素的值
  - 特殊用法
    - ![image-20230505181844721](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230505181844721.png)
    - ![image-20230505182526342](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230505182526342.png)
    - 修饰符可以连用
      - ![image-20230505182821731](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230505182821731.png)
      - ![image-20230505183024678](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230505183024678.png)

## 计算属性

- 只要data中的数据发生改变，那么vue一定会重新解析模板

- 要使用计算属性，就在配置对象中，增加一项computed

  - ```js
    new Vue({
        el: '',
        data: {
            key, vlaue
        },
        computed: {
            fullname: {
                get(){
                    
                }	//	当fullnamne，被读取时，则它里面的get方法就会被调用
            },	//	多个计算属性之间，以逗号分隔
        }
    });
    ```

  - ![image-20230505201036503](D:\Project\Web\Notepad\image-20230505201036503.png)

  - 在get方法中的this，经过vue的操作，已经变为了那个vue的实例

  - get方法在两个时刻被调用

    - 当fullname被读取时，如果多次读取fullname，但所依赖的数据，并未被修改时，会去缓存中读取
    - 当所依赖的数据发生变化时

  - 还存在set方法，就是当fullname的值被修改时，就会调用setter

- 总结

  - ![image-20230505202750001](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230505202750001.png)

- 简写

  - 当计算属性，不适用setter时，则可以采用简写形式，即省去再使用配置对象，而是直接将一个匿名函数赋值给计算属性的属性值，这个函数就会被当成getter使用
  - ![image-20230505203122370](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230505203122370.png)


## 监视属性

- 在vue的模板中，不可以使用原生的js方法与对象，因为vm身上不存在window和document所以不可以使用

  - ```html
    <div id='root'>
        <button @click='alter(1)'>	<!--错误的写法，因为vm身上，不存在window更不存在alter-->
            
        </button>
    </div>
    ```
    
    - 新添加一个配置项，watch，是一个对象，其中每个key都是要监视的对象，不仅可以监视data中的数据，也可以监视计算属性中的数据
    
  - ```js
    new Vue ({
        watch: {
            key-one: {
           		handler() {
        
    			 } 
        	  },
            key-two: {
             	handler(newValue,oldValue) {		//	内置方法，当监视的对象发生改变时，就调用handler函数，两个参数分别为修改后的值，和修改前的值
        
    			  }，
               immediate: true,	//	初始化时，就让handler调用一次，默认值为false，即不调用
             }
        }
    });
    ```

- 也可以在获得vue实例对象，之后，通过$watch方法添加监视，第一个参数为，要坚实的键名，第二个值为配置对象，同第一种方法中的配置对象

  - ![image-20230506150124535](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506150124535.png)

- 总结

  - ![image-20230506150313459](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506150313459.png)

## 深度监视

- 当出现多级的对象时，通过对象.方式去监视，但需要注意的是，由于key是一个字符串，所以再写时，需要将对象.用引号括起来
  - ![image-20230506150731606](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506150731606.png)
- 如果想要深度监视某个对象，那就需要使用配置项，deep
  - ![image-20230506151147121](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506151147121.png)
- 总结
  - ![image-20230506151231507](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506151231507.png)
- 简写形式，当配置项，中只有handler时，可以采用简写形式，如果有其他配置项，则不可以
  - ![image-20230506151551506](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506151551506.png)
- 使用vm.$watch也可以使用简写形式，也是同理，不需要配置项时，可以采用简写形式
  - ![image-20230506151742341](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506151742341.png)
  - 同样不允许使用箭头函数

## 计算属性和监视器

- 计算属性是靠return去维护数据，但是监视器却不是
- 计算属性不可以开启异步任务，但是监视器可以
- ![image-20230506153845222](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506153845222.png)
  - 由于setTimeout的回调函数不被vue所管理，所以它里面的回调函数，所指向的this是Windows，而如果写成箭头函数时，由于箭头函数没有自己的this，所以就会往外，去找定时器那一层级的this，而定时器那一层级的this，所指向的就是vm

## 绑定样式

- 字符串写法
  - ![image-20230506155218694](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506155218694.png)![image-20230506155242213](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506155242213.png)
- 数组写法：如果需要动态的操作多个样式，则使用数组的写法，在数据中，定义数组，在模板中使用这个数组即可
  - ![image-20230506161254281](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506161254281.png)
  - 随后只需要对数组中的元素进行操作，就可以对样式进行更改了
- 对象写法：如果需要样式之间，进行切换，则使用该写法，在data中定义对象，key值为类名，value值为Boolean值，true代表使用，false代表不使用
  - ![image-20230506161735778](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506161735778.png)
  - ![image-20230506161749793](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506161749793.png)
- style写法：使用的内嵌式css
  - ![image-20230506162426700](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506162426700.png)
  - styleObj就是一个样式对象，以键值对的方式存在，且key必须是css样式，且以驼峰命名
  - styleArr是一个数组，它里面的每个值都是一个样式对象，即styleObj
- 总结
  - ![image-20230506162649848](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506162649848.png)

## 条件渲染 V- if

- v-show条件渲染，它的值是true或false或一个表达式，这个表达式的值为Boolean
  - ![image-20230506163327394](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506163327394.png)
- v-if 用法同v-show
- 但v-show是使用display：none去做的，而v-if是直接删除掉
- v-show节点还存在，v-if啥也没了
- v-else-if一组判断，同js中的if-else用法
  - ![image-20230506163901762](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506163901762.png)
- v-else，同js用法，不用写条件
  - ![image-20230506164013142](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506164013142.png)
- v-if，v-else-if，v-else是一个整体，中间不允许打断
  - ![image-20230506164107789](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506164107789.png)
  - 这种方式是无效的
- template可以搭配这v-if使用，template在解析时，不会被解析到DOM节点中
  - ![image-20230506164525501](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506164525501.png)
- 总结
  - ![image-20230506164546189](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506164546189.png)

## 表单收集

- 在收集没有value值的表单时，可以自己写一个value值即可
  - ![image-20230507185546703](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230507185546703.png)
- 收集checkbox时，也需要自己去配置value值，如果不配置，则默认收集的是checked的值
  - 需要手机checkbox时，需要将收集的变量设置为数组类型
  - ![image-20230507190028823](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230507190028823.png)
- v-model的修饰符
  - .number，规定收集到的信息是数字型，不带引号
  - .lazy，表单元素失去焦点的一瞬间再收集数据
  - .trim，去掉收集到的信息的前后空格
- 总结
  - ![image-20230507191209177](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230507191209177.png)

## 过滤器

- 处理现有数据的一种方法
- 语法如下
  - ![image-20230507192017608](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230507192017608.png)
- 在插值语法中，使用一个管道符，而后面是过滤器的名称，本质上也是一个函数，但不需要加括号，然后在vm的配置对象中，增加一个filters，filters的内容就是不同的过滤器，上图使用的是简写模式，过滤器接受一个参数，这个参数为管道符前的那个值
- 过滤器返回的值，将会替换掉那个使用了插值语法的地方
- 过滤器也是可以使用参数的，而且，第一个参数永远都是管道符前的那个值，就算不写实参，也会默认存在，而后面可以自行去增加参数，在形参中自己定义变量接收使用即可
- 过滤器还可以实现连接
  - ![image-20230507193025082](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230507193025082.png)
  - 可以连用多个管道符，然后写不同的过滤器，每个过滤器的参数，都是前一个过滤器的返回值
- 在vm中配置的过滤器，是局部的，在不同的Vue实例或组件中，无法实现复用
- 全局过滤器
  - 如果想多个vm或组件复用一个过滤器，可以将这个过滤器注册为全局的
  - 即通过Vue.filter()去注册
  - ![image-20230507193510881](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230507193510881.png)
  - 第一个参数为，过滤器的名称，第二个参数为函数，同样也可以收到形参
- 过滤器只支持两种用法
  - v-bind或者:
  - 插值语法{{}}
  - 除此之外，别的地方使用过滤器可能会造成错误
- 总结
  - ![image-20230507194405453](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230507194405453.png)

## 常见内置指令

- v-text

  - 会替换当前标签的整个文本内容

  - ```html
    <div v-text='key'></div>
    ```

  - 实际上不如插值语法方便，因为插值语法只会替换插值部分，而不会替换整个文本

  - 不解析标签，所有的内容都当成文本解析

- v-html

  - 解析标签，会存在安全性问题
  - ![image-20230507200904408](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230507200904408.png)

- v-cloak

  - 当网速过慢，vue还没被解析时，不让原本的vue模板被解析到页面上，然后搭配css将原本包含v-cloak的隐藏，直到vue被请求到，就会自动删除掉v-cloak，使得显示
  - ![image-20230507211924252](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230507211924252.png)
  - ![image-20230507211951085](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230507211951085.png)

- v-once

  - ![image-20230507212335333](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230507212335333.png)

- v-pre

  - ![image-20230507212529385](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230507212529385.png)


## VUE监测数据的原理

- ![image-20230507163303693](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230507163303693.png)
- 在创建vue实例时，data中的数据，在被赋值给_data时，vue内部会帮助加工，对每个数据进行数据代理，然后在修改时，必然会调用setter，此时在setter中，就可以重新解析模板
- Vue.set(target, key, val);
  - vue中进行添加属性的方法，并且可以实现响应式，有getter和setter
  - target为为哪个目标添加一个属性
  - vm.$set()跟上述方法，一样的使用
  - 但是target不能是vue实例，或者vue._data，只能为data中的数据项
- 如果_data中的数据，是数组，那么就不会添加getter 和 setter，只会为对象添加getter和setter
  - 在数组中使用了如下方法，就会被vue解析为，修改了数据，这样就会触发页面模板重新解析
    - push
    - pop
    - shift
    - unshift
    - sort
    - splice
    - reverse
  - 如果使用vue中的这些数组，方法，并不是Array.prototype.push，而是vue自己定义的push，但是在自己定义的push中，调用了array原型对象中的push，然后重新解析模板，这样就可以实现响应式
  - ![image-20230507173203199](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230507173203199.png)
- 总结
  - ![image-20230507174632320](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230507174632320.png)

## 自定义指令

- 自定义指令中，需要自己动手，亲自操纵DOM元素
- 定义时，可以不加-前缀，但是使用时需要添加
- 在Vue实例的配置项中，添加一项，directives，后面跟上自定义指令，不需要添加前缀-，有两种方式
  - 对象式，以对象的方式，有一些vue定义的函数，它们分别在一些时刻调用
    - bind在指令与元素绑定成功时
    - inserted在真实DOM被插入到页面时
    - update在页面重新解析时
    - 且它们也有参数，同函数式的参数
    - bind和update中的操作往往是重复的
    - ![image-20230508080247976](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508080247976.png)
  - 函数式，直接将自定义指令写成一个函数，这个函数接收两个参数，第一个是使用当前指令的那个DOM元素，第二个参数是一个对象，包含了，自定义指令的一些参数，最重要的是第二个参数里面的value值
  - ![image-20230508074236389](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508074236389.png)
- 自定义指令不是靠返回值的
- ![image-20230508074705272](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508074705272.png)
- 多个单词之间使用-分隔
  - ![image-20230508080448671](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508080448671.png)
  - 在配置指令时则需要以如下方式
    - ![image-20230508080545612](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508080545612.png)
- 所有在directive中的函数里面的this，指向的都是window，且它们都是局部指令
- 定义全局指令，同过滤器的用法，定义在Vue.directive中，第一个参数是名称，第二个参数是需要执行的操作，如果是函数式，则把函数给第二个参数，如果是对象式，则把对象给那个参数
- 总结
  - ![image-20230508081312848](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508081312848.png)

## ref属性

- 在vue中唯一标识一个DOM元素，在拿到以后就可以进行操作了
- 在template中使用ref属性，再给值，相似于原生的id属性
  - ![image-20230509202258534](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509202258534.png)
- 由于是在组件中使用的，所以可以通过组件对象，vc获取到所有带ref属性的标签，并且ref属性的值，就是获取时的key
  - ![image-20230509202350607](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509202350607.png)
  - 这样，可以获取到所有设置了ref属性的标签，是一个对象，key是标签上ref的值，value就是那个DOM对象
- 在vm中也可以使用
- 组件也可以有ref属性，获取到的是那个组件的vc对象，如果给组件加上id属性，通过原生获取的话，则获取到的是DOM对象，且，id属性不再包裹在最外层
- ![image-20230509203650531](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509203650531.png)

## v-for

- 使用v-for形成循环遍历，语法如下，另外需要给每个循环的列表，添加一个单独的key
  - ![image-20230506165400815](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506165400815.png)
  - 遍历中，形参可以有两个，也同js语法，第一个形参为item，第二个形参为index索引
    - ![image-20230506165736290](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506165736290.png)
    - ![image-20230506165628688](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506165628688.png)
- v-for中的in可以换成of
- v-for还可以遍历对象
  - ![image-20230506170013457](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506170013457.png)
  - ![image-20230506170104926](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506170104926.png)
  - ![image-20230506170047277](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506170047277.png)
  - 此时的两个参数，第一个为value，第二个为kay
- 还可以遍历字符串，第一个参数为每个字符，第二个参数为索引
- 还可以遍历指定次数值
  - ![image-20230506170327262](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506170327262.png)

## v-for中的key

- key就是给当前节点进行标识
- 所有元素身上的key都是特殊属性，在渲染时，将不渲染到页面中，而是在vue的内部进行使用
- 什么使用index会有问题
  - ![image-20230506172007150](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506172007150.png)
  - 由于在数据没有改变前，会先依据数据去产生虚拟DOM，然后再讲虚拟DOM转换为真实DOM，但是用户的输入时存在于真是DOM之中，而虚拟DOM之中并不存在用户的输入
  - 在增加新数据之后，数据的顺序发生改变了，索引也就随之改变，当新增了数据以后，会再次生成虚拟DOM，与之前旧的虚拟DOM进行对比，当新的虚拟DOM中的数据发生改变时，会重新产生真实DOM，而如果没有改变，注意，这里的虚拟DOM并没有残留用户的输入，所以再对比时，是一致的，当经过对比时一致时，就不再去重新产生真实DOM，而是直接复用了旧的虚拟DOM，这样就会产生上述问题
- 使用id作为key时，就不会产生问题
  - ![image-20230506172554750](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506172554750.png)
  - 初始化步骤同上，在新增数据之后，也会产生新的虚拟DOM，然后跟旧的虚拟DOM进行比较，发现只有新增的那个li，不跟旧的虚拟DOM一样，就会将这个新增的li转为真实DOM，而剩下的那些li，跟旧的虚拟DOM中保持一致，就会直接复用，就不会有问题产生，而且性能更高，因为只新生成了一个真实DOM
- 如果不写key，那么vue会在使用v-for指令时，自动将index值作为key
- 总结
  - ![image-20230506173032796](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506173032796.png)

## 列表筛选

- ![image-20230506183927934](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506183927934.png)

## 列表排序

- ![image-20230506183948330](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230506183948330.png)

# 生命周期

- mounted配置项，也是一个函数
- ![image-20230508082924611](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508082924611.png)
- vue配置项的一个子项，同data，el平级
- 生命周期函数，在每个不同的时刻，去调不同的函数，而mounted就是其中的一个
- ![image-20230508084555479](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508084555479.png)
- 周期又被称为钩子
- 钩子函数详解
  - beforeCreate：无法通过vm访问到data中的数据和methods中的方法，因为此时数据代理还没有开始
    - ![image-20230508171406149](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508171406149.png)
  - created：可以通过vm访问到data中的数据和methods中的方法，因为此时数据代理已经执行完毕
    - ![image-20230508171534400](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508171534400.png)
  - 此时开始检查vue的配置项，如果不存在template选项的话，则将el的那个HTML作为模板解析，此时页面中还不存在真实DOM
    - ![image-20230508172001848](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508172001848.png)
  - beforeMount：此时的页面上是未经编译的DOM结构，所有对DOM的操作，都不奏效，而在函数执行完毕之后，真实的DOM就开始被插入页面了
    - ![image-20230508172350736](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508172350736.png)
  - mounted：此时页面中呈现的都是真实DOM，但尽量避免对DOM的操作，此时一般：开启定时器、发送网络请求、订阅信息、绑定自定义事件等初始化操作
    - ![image-20230508173411630](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508173411630.png)
  - beforeUpdate：数据被修改时，但此时数据已经发生改变，不过页面尚未更新
    - ![image-20230508174311175](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508174311175.png)
    - 此时会经过对比，将新的虚拟DOM 和 旧的虚拟DOM进行对比，最终完成页面更新
  - updated：数据和页面都更新完成
  - vm.$destroy()：销毁vue实例，清理与其他实例(组件)的连接，解绑它的全部指令及事件监听器，但只解绑自定义事件，而原生的DOM事件还是会被触发
  - beforeDestroy：vm中所有的data、methods、指令等等现在还处于可用状态，马上就要销毁。一般在此阶段关闭定时器、取消订阅消息、解绑自定义事件，等收尾工作，在这个过程中，修改的数据，都不会触发更新
    - ![image-20230508175502313](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508175502313.png)
  - destroyed：忽略不计
- 总结
  - ![image-20230508180424889](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508180424889.png)
  - ![image-20230508181509092](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508181509092.png)

# 组件化

- 组件就是实现应用中局部功能代码和资源的集合
- 非单文件组件
  - 一个文件中包含n个组件
- 单文件组件
  - 一个文件中只包含1个组件，后缀名为a.vue
- 使用组件的三步
  - 创建组件
  - 注册组件
  - 使用组件

## 新建一个组件

- 使用Vue.extend();进行创建组件，也需要传入一个配置对象，同new Vue的配置方法，但有点区别
  - 由于是组件化，无法直接定义模板，都是通过template去定义模板，然后在模板中，使用data函数返回的数据
  - Vue.extend();中不允许使用el节点，因为组件不需要指定为哪个节点服务
  - data不允许使用对象式，只能使用函数式
  - ![image-20230508195116498](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508195116498.png)

## 注册组件

- 在组件创建完成以后，那么Vue实例中的data配置项就不需要了，因为数据都在组件中了，这时需要使用新的配置项components，在vm中注册的组件时局部组件，其它的vm无法使用
  - ![image-20230508195401777](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508195401777.png)
- components也是一个对象，以key-value形式存在，而value 的值，就是创建的组件，一般为了简写，组件名和key保持一致，也就是简写形式
- 全局注册
  - 先创建好一个组件，然后通过Vue.component()；进行注册全局的，第一个参数为组件名称，对应局部中的key，第二个参数为组件，对应value
  - ![image-20230508200028602](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508200028602.png)
  - 全局组件不常用

## 使用组件

- 在vm的root节点中，使用注册组件时的key值，形成一个标签，被称为vue标签，这样，即可完成组件的使用
  - ​	![image-20230508195343279](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508195343279.png)
- 且在vm中还是可以照常使用data定义数据
- 相同之间组件的数据互不干扰

## 创建-注册-使用组件总结

- ![image-20230508200125016](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508200125016.png)

## 组件注意点

- 组件的命名，如果在命名时是单个单词，且是小写的情况下，在Vue开发者工具视图下，可以看到会显示为首字母大写的形式，也可以直接在注册时就将key写成首字母大写
- 如果是多个单词，首字母小写，然后用短横杠-区分开，即可，同样在Vue开发者工具中，显示的也是首字母大写。
  - 也可以在定义时，直接全部使用首字母大写，但是需要注意的是，这种情况下，需要使用脚手架
- ![image-20230508201237225](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230508201237225.png)
  - 可以在创建vue组件时，指定一个name属性，这样再vue开发者工具中，就会显示这个name的值，而在使用时，还是必须要使用vm中的那个key

## 组件的嵌套

- 在创建完一个子组件以后，将其注册到父组件身上，然后在父组件身上的template模板上使用，然后一级一级嵌套
- 最后所有的组件都是app的后代，vm只管理一个组件，那就是app，剩下的组件都挂载在app身上

## VueComponent

- 组件的本质就是一个构造函数，不是程序员定义的吗，是Vue.extend()生成的
- 只需要在template中使用组件，那么vue就会创建VueComponent实例
- 每次调用vueComponent时，返回的都是一个新的VueComponent，不同的组件就是不同的实例对象
- ![image-20230509143951701](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509143951701.png)
- 分析Vue与VueComponent的关系
  - ![image-20230509160253550](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509160253550.png)
  - ![image-20230509160657741](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509160657741.png)

## 单文件组件

- 单文件组件的文件名为xxx.vue
- 文件中只支持三个标签
  - ![image-20230509161859290](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509161859290.png)
  - 分别为html、交互、css
  - ![image-20230509162514183](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509162514183.png)
    - 这里使用到了暴露对象，方便调用者可以使用， 一般使用默认暴露，即export default options
- 在使用时，先写好部分的.vue文件，然后再新建一个App.vue，便于管理
- 然后在main.js中，创建vm实例，将app.vue引入，然后使用，注册组件
  - ![image-20230509170300206](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509170300206.png)
  - 但此时还不可以执行，因为要在脚手架中才可以正确运行

## props属性

- 如果一个组件里面的数据不想在一定义时就确定，而是想在使用时再确定，那么就需要用到此属性
- 此时data中并没有所需要的数据
  - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509205404382.png" alt="image-20230509205404382" style="zoom: 67%;" />
  - 如果直接运行会报错
  - 需要在调用这个组件时，以属性的方式将值传递过去
    - ![image-20230509205455594](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509205455594.png)
    - 如果想传过去的是Number类型的则在age前加一个：或者v-bind，进行绑定，而此时18就会被当成表达式，这个表达式的值就是数字的18
  - 既然传递了，那么在组件端，就需要接收，如此，就是用到了props属性，它同data同级，接收时，也有三种方法
    - 直接以数组的形式
    - ![image-20230509205632901](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509205632901.png)
    - 以对象形式，且后面跟上数据的类型，如果传过来的数据，不符，那么就会在控制台报错
    - ![image-20230509205749459](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509205749459.png)
    - 以对象的形式，且每一个值又是一个对象，且在内向内部，可以进行一些限定
      - ![image-20230509210035726](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509210035726.png)
      - default是当属性缺失时，存在一个值，可以替代
- props中的数据是不能修改的，可能会生效，但是会出现警告
  - 如果非要修改，可以再在data中定义一个属性去接收那个props中的值，这样就可以实现修改
  - 如果这样实现，那么template中也要进行相应修改
  - ![image-20230509210649116](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509210649116.png)
- props中的数据比data中的数据优先级更高
- ![image-20230509210827132](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509210827132.png)

## nextTick

- 这个方法，接收一个函数参数，作用是，在DOM节点解析完成后，再执行回调函数，一般用于异步操作
- ![image-20230513181339423](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230513181339423.png)

# 使用Vue脚手架

- Vue脚手架是Vue提供的官方开发平台
- 通过npm安装 
  - npm i -g @vue/cli 	安装脚手架
  - 进入到项目的目录下  使用   vue create 项目名称   创建项目
- 运行项目
  - 进入到项目目录下
  - npm run serve
  - 点击链接即可进入
- 项目文件解析
  - ![image-20230509201133238](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509201133238.png)
  - ![image-20230509201152700](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509201152700.png)
  - ![image-20230509201202291](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509201202291.png)

## render函数

- render函数是在使用了阉割版的vue时，不带有模板解析器，必须要调用的函数，此时就不需要再用template配置项了，而render函数必须要有返回值

  - 如果使用的是完整版的vue，就可以使用模板解析器了 

- render是有参数的，它的第一个参数就是一个函数，用来创建元素

  - 但默认情况下，是一个箭头函数的写法，直接将组件当成实参，传递给render函数即可

  - ```js
    render: h => h(组件);
    ```

  - ![image-20230509182140237](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509182140237.png)

- 为什么要使用render
  
  - 因为只在开发阶段需要使用到模板解析器，在开发结束，进行打包时，需要将模板解析器剔除掉，于是在引入阶段，就不引入模板解析器了，通过render函数去进行解析
- 总结
  
  - ![image-20230509183052497](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230509183052497.png)

## 修改配置文件

- 由于vue当前的配置文件值默认隐藏，所以需要通过vue inspect > out.js命令，去输入这些配置项，然后，就可以查看了
- 在vue.config.js中进行配置即可

## mixin混入

- 两个不同的组件，公用相同的方法
- 通俗说，就是复用配置，而经常需要将mixin抽离出去，封装为一个单独的js文件，然后通过import进行引入
- 示例
  - 抽离出去的js，那么在使用到这些方法的组件时，就不需要再写相同的配置了
  - ![image-20230510141523078](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230510141523078.png)
  - 使用
    - 先引入，引入以后，通过mixins的配置项去进行接收，注意mixins配置项是一个数组，数组的元素是每一个minxin.js文件，然后在组件中就可以使用mixin的方法与数据了
    - ![image-20230510141628676](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230510141628676.png)
    - 如果出现冲突时，以组件本身为主
      - 但例外是，当混合种出现生命周期钩子时，两个都会执行，并且，mixin中的钩子先执行，而组件本身的后执行
- 以上配置的都是局部的，只能在单个.vue文件中使用
- 全局混合
  - 在mian.js文件中进行混合，通过Vue.mixin(XXX);，进行注册
- ![image-20230510142238997](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230510142238997.png)

## Vue插件

- 本质上也是一个js文件，这个文件里面是一个对象，通过export暴露出去，对象里面必须要有一个install方法，方法接收一个参数，就是Vue的构造函数
- 通过参数，可以讲很多的过滤器，计算属性等等，挂载在Vue身上，那么其他的vc和vm都可以使用了
- ![image-20230510143225459](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230510143225459.png)

## scoped样式

- vue中，每个组件的样式最后都会汇总到一起，所以也就不可避免的出现了冲突的情况
- 如果两个组件定了相同的类名，且样式不一样，那么按照引入的顺序，后引入的会覆盖前引入的
- ![image-20230510143653700](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230510143653700.png)
- 则School中的如果存在相同类名的样式会覆盖student中的
- 解决这一冲突可以在写样式时，奖赏scoped关键字，这样，就使得这个样式是局部的，只服务于当前组件
  - ![image-20230510143849980](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230510143849980.png)
- 实现原理就是，在底层实现时，另外新增一个属性，另外通过标签属性选择器去进行选择

# TODOList案例

## 个人总结

- props中的数据不要直接修改
- 计算属性可以修改，但是要写完整形式，通过getter和setter去更改
- 数据在哪个组件就在哪个组件配置方法进行操作数据，不要跨组件操作数据
- 现阶段，组件间要通信，只能是通过props中的传递函数去实现

## B站总结

- ![image-20230511212050817](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230511212050817.png)

# 组件自定义事件

## 绑定

- 顾名思义，给组件用的事件
- 给谁添加自定义事件写在谁身上
  - ![image-20230513154339901](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230513154339901.png)
- 在子组件中，使用this.$emit(自定义事件名称, [参数1, 参数2]);，这样即可以触发该自定义事件
  - ![image-20230513154356733](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230513154356733.png)
- 还可以使用简写形式
  - v-on:ztc="callback" 或者 @ztc="callback"
- 还有第二种写法，不直接在template中写，而是通过给子组件添加ref属性，然后在mounted钩子中，获取到那个vc实例，在它身上写该方法
  - ![image-20230513155402861](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230513155402861.png)
- this指向
  - 在使用第二种写法时，需要注意一件事情，如果自定义事件的回调，不是methods中配置好的，而是自己写的，此时这个函数中的this指向的是那个组件，而不再是当前组件了
  - ![image-20230513161455369](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230513161455369.png)
  - 也可以巧妙的避免，因为可以讲回调，写成箭头函数，由于箭头函数没有自己的this，会往外找，而它的外层肯定是APP组件，所以也就巧妙的解决了this指向
- 组件调用原生的事件
  - 由于在给组件定义事件时，vue会默认的都认为是自定义事件，但由于原生的和自定义事件的写法一致，就会导致歧义
  - 在绑定原生事件时，使用@click.native去绑定，这样就可以执行了

## 解绑

- 如果在某个时刻，不想用该事件了，可以通过this.$off();解绑自定义事件
  - ![image-20230513155928891](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230513155928891.png)
- 如果一个组件的实例对象被销毁了，那么这个组件身上的所有自定义事件都不奏效了

## 总结

- ![image-20230513162059316](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230513162059316.png)

# 组件通信

## 全局事件总线

- ![image-20230513163614449](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230513163614449.png)

- 本质上就是vm对象，在创建vue实例时，往Vue的原型对象身上添加一个对象，一般叫做$bus，让它等于即将创建的vm实例，这里必须要使用到钩子，就是在vue实例还没生成的时候，就将vm挂载到Vue的原型上，等Vue创建完成，那么就可以了

  - ```js
    new Vue({
        beforeCreate() {
            Vue.protoyep.$bus = this;	//	因为此时的this指向的就刚好是vm实例
        }
    });
    ```

- 经过上一步之后，那么所有的vc和vm都可以访问到$bus了，这是因为所有的vc和vm都可以访问到Vue.prototype

- 这样在组件间需要通信时，只需要给$bus绑定自定义事件，然后在另一个组件中触发该自定义函数，并且传递一些数据，即可，那么在回调函数中，即可拿到

  - 定义
    
    - ![image-20230513171031945](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230513171031945.png)
    
  - 触发
    
    - ![image-20230513171039831](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230513171039831.png)
    
  - 关闭
  
    - ```
      this.$bus.$off('demo');
      ```
  
    - 在事件销毁之前，记得取消全局事件

## 消息订阅与发布

- 使用前，需要先引用，第三方库，pubsub-js

- 引入完成后，得到的是一个对象，消息的订阅与发布，就是通过这个对象身上的一些方法去完成的

  - 消息订阅

    - ```js
      pubsub.subscribe('消息名', 回调);
      ```

    - 当有人发布了消息时，且消息明相同，则会触发回调，需要注意的是，回调会接收两个参数，第一个参数是消息名，第二个参数开始才是接受到的数据

    - ![image-20230513174045076](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230513174045076.png)

    - 如果在该组件即将被销毁时，需要取消订阅，由于取消订阅并不是简单的将消息名，传入，在订阅时，会返回一个id，类似于定时器，所以需要将其挂到this身上，然后再通过this去取消定于

  - 消息发布

    - ```js
      pubsub.publish(消息名, 数据);
      ```

    - ![image-20230513174229860](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230513174229860.png)

- 回调中的this指向

  - 由于使用消息订阅与发布时，使用到的是第三方库，所以不再保证this还是指向当前的vc对象，所以一般不使用传统的函数，而是通过箭头函数，这样就可以使this指向当前的vc独享
  - 另外一种方法是，将回调定义在methods中，然后在订阅消息时，只传入方法名就可以了

- 总结

  - ![image-20230513174552634](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230513174552634.png)

# Vue动画与过渡

## 动画

- 前提是，需要已经定义好了动画，并且添加两个类名，必须按照Vue的规范去命名，且需要将要添加动画的标签，用transition标签包裹起来，这样才会生效，transition标签可以添加两个属性，name属性是指该动画的名称，appear表示，该动画一进入页面就执行一次
  - ![image-20230513183133989](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230513183133989.png)
- 如果给transition定义了属性，则定义类名时，必须以name的值开头
  - ![image-20230513183203627](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230513183203627.png)
- 类名严格按照规范，一个进入动画，一个褪去动画

## 总结

- ![image-20230513190441523](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230513190441523.png)
- ![image-20230513190457934](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230513190457934.png)

# 配置代理

- 解决CORS的三种办法
  - 后端配置响应头
  - jsonp
  - 代理服务器
- Vue中一般都使用第三种

## 如何开启代理服务器

- 在从config.js中进行配置

  - ```js
    devServer: {
        proxy: 'http://localhost:4000'
      }
    ```

  - 这里只需要配置到端口号即可

  - 配置的是要转发的地址，而不是代理服务器的地址

  - 只能配置一个代理

  - 不能灵活控制使不使用代理服务器

    - 因为如果代理服务器上有要请求的资源，就不会再转发了

## 第二种方法，推荐使用

- 还是在config.js中进行配置
- ![image-20230514084004473](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514084004473.png)
- 在proxy中，可以配置多个代理，以对象的形式存在
  - target是要转发的服务器，只需要写到端口号即可
  - pathReWrite路径重写，由于在配置代理服务器之后，在请求时，要加上前缀，但是这里需要把前缀去掉，所以使用到该配置项
    - pathRewrite是一个对象，以key：value存在，key就是要被替换的路径，而value写为空，key一般是一个正则
  - ws就是指websocket，值支持，默认是true
  - changeOrigin是指，是否要伪装为目标服务器同源，默认是true
- 配置多个代理服务器
  - ![image-20230514084617771](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514084617771.png)
- 实例
  - 请求
  - 需要在地址中加上去前缀
    - ![image-20230514092746641](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514092746641.png)
  - 配置
    - ![image-20230514092759567](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514092759567.png)

- 总结
  - ![image-20230514084721265](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514084721265.png)
  - ![image-20230514084803692](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514084803692.png)

# slot插槽

- 在引用一个组件时，如果想自己定义一些组件里面没有的部分，就可以使用插槽技术
- ![image-20230514144036201](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514144036201.png)
- ![image-20230514144048829](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514144048829.png)
- 当时用插槽以后，所有的结构都是使用自定义组件的地方解析完成以后，再放入到slot标签中的

## 默认插槽

- 将组件中间的所有内容，全部放在用<slot>标签代替，如果使用者在使用组件时，写了自定义的标签，则会被slot标签代替，如果没写，则会显示slot的默认内容
- ![image-20230514144114153](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514144114153.png)

## 具名插槽

- 也可以使用多个插槽，但是在组件中需要定义一个name属性，然后在使用时，将所有带了slot属性的结构，且slot的值等于name的值，放在一个位置
- ![image-20230514144620212](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514144620212.png)
- ![image-20230514145239436](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514145239436.png)
- 由于可能存在多个结构，需要插槽，可以使用template标签，由于template标签，解析时会忽略，所以这是最好选择
  - 如果使用了template标签，那么slot属性可以换一种写法
  - ![image-20230514145138581](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514145138581.png)

## 作用域插槽

- 插槽的使用者，将结构放入插槽中，但是数据确是在插槽的那个组件中
  - 数据在插槽中
    - ![image-20230514150154007](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514150154007.png)
  - 使用者
    - 如果要使用插槽传过来的数据，要在最外层，包裹一个template标签，且设置一个scope属性，这个属性的值，就是接收到的数据
    - ![image-20230514150340070](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514150340070.png)
    - ![image-20230514150353838](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514150353838.png)
    - 插槽也可以传入多个数据，在接收时，接收到时一个对象，这个对象的key就是传入的自定义名称
- 在template中也可以使用新的slot-scope，同scope

## 总结

- ![image-20230514150838056](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514150838056.png)
- ![image-20230514150902228](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514150902228.png)
- ![image-20230514150949027](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514150949027.png)

# Vuex

- 专门在Vue中实现集中式状态(数据)管理的一个Vue插件，对Vue应用中多个组建共享的状态，进行集中式的管理，也是一种组件间的通信方式，而且适用于任意间组建通信
- 如果一个数据多个组件都要共享，那么最好放在Vuex身上
- 使用场景
  - 多个组件依赖于同一个状态
  - 来自不同组件的行为，需要变更同一状态

## 工作原理

- Vuex中包括这些部分
  - Actions，行为，组件将需要进行的行为告诉Actions，然后由Actions调用$store.commit()通知到下一个步骤，但有时候，不是必须的。另外，如果组件所需要进行的操作，涉及到数据，但数据不在组件中，需要通过Ajax进行获得，那么Ajax请求只能在Actions中发生，如果数据也给了，行为也给了，那么此步骤可以跳过，直接到mutations中
  - Mutations：操作，上一步骤，commit之后，就来到了这里，这里才是真正操作数据的地方，一般Mutations中的函数都是全大写的，为了与Actions区分
  - State，数据都存在这里
  - getters，类似于计算属性，里面的内容为键值对，但值都是函数，所以可以直接写成一个函数，也是依靠于返回值，并且需要将其注册
    - 可以在开发工具中看到getters
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514174538132.png" alt="image-20230514174538132" style="zoom:50%;" />
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514174629504.png" alt="image-20230514174629504" style="zoom:50%;" />

## 安装环境

- 使用import语句时，会自动的将所有的import语句，先执行，类似于变量提升
- Vue2中，只能使用Vuex3版本
- Vue3中，只能使用Vuex4版本
- 否则在安装时，会出现错误

- npm i vuex
  - ![image-20230514162109132](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514162109132.png)

- 使用vuex，通过，Vue.use()方法，使用，同理，这里是在main.js中的操作，因为别的地方，没有引入Vue
  - ![image-20230514162145729](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514162145729.png)

- 获得store
  - 在引入之前，在vm中传入store配置项，是无效的，但是在引入Vuex之后，就可以了
  - 在引入之后，那么所有的vc和vm都可以访问到store
  - 在项目的根目录下，新建一个文件夹，命名为store，然后新建一个index.js文件
  - ![image-20230514162716566](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514162716566.png)
  - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514163118692.png" alt="image-20230514163118692" style="zoom:50%;" />
- 创建完成之后，就在main.js中进行引入
  - ![image-20230514163223142](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514163223142.png)
  - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514163228429.png" alt="image-20230514163228429" style="zoom:50%;" />
- 由于import语句会被提升，所以这里会出现出错，正确的引用方式是，将Vue和Vuex都在index.js中引入
  - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514163837660.png" alt="image-20230514163837660" style="zoom:50%;" />
  - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514163853640.png" alt="image-20230514163853640" style="zoom:50%;" />

- 让所有的组件，都可以得到store
- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514164257143.png" alt="image-20230514164257143" style="zoom:67%;" />

## 使用流程

- 将数据配置到state中
- 当组件需要操作state中的数据时，调用dispatch方法，且要在一开始的index.js中去相应的配置该动作的函数
- 步骤
  - 调用dispatch
    
    - ```js
      this.$store.dispath('jia', value);
      ```
    
    - 
  - 配置该动作的函数
    - 简写为
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514165355824.png" alt="image-20230514165355824" style="zoom:67%;" />
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514165329634.png" alt="image-20230514165329634" style="zoom:67%;" />
    - 可以接收到两个参数，分别是第一个值是ministore，一个迷你版的store，但是没有store全，官方定义第一个参数为上下文，第二个参数是传入的需要进行操作的值
      - 这里还进行了一步操作，调用了commit函数，将此时的步骤转给mutations
      - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514165732869.png" alt="image-20230514165732869" style="zoom:67%;" />
  - 在mutations中配置与上一步对应的动作
    - 为了与actions中的动作区别，通常mutations中的动作要用大写
    - 同样的在mutations中进行函数的配置，也可以进行简写形式
    - 可以接到两个参数，第二个参数为接收到的需要进行操作的数据，第一个参数就是那个state，数据都在state，也就是在这里进行数据操作时，要用到第一个参数，注意，state是一个对象
      - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514170206033.png" alt="image-20230514170206033" style="zoom: 67%;" />
  - 所有的流程下来，就完成了Vuex的工作
- 总结
  - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514173754780.png" alt="image-20230514173754780"  />
  - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514173910387.png" alt="image-20230514173910387" style="zoom:67%;" />

# Vuex中的一些方法

## mapState对象写法

- 能够快速的获得Vuex中的数据，而不用一遍遍的再去写this.$store.state.sum

- 使用步骤，由于这个方法存在于Vuex中，需要先引入，且用的是分别引入

  - ```js
    import {mapState} from 'vuex';
    ```

  - 然后通过调用该方法，可以得到一个<span style="color:red">对象</span>，里面是三个函数，是自己传进去的所需要的state中的返回数据的函数，调用此函数就可以得到state中的数据，且mapState中的参数，是一个对象，以键值对的形式存在

    - <span style="color:red">key就是一会返回的函数名，而value是想要得到的state中的数据名</span>
    - ![image-20230514180033899](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514180033899.png)

  - ![image-20230514175954890](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514175954890.png)

  - 通常mapState不写在mounted中，而是写在computed中，而且以对象展开的形式，这样，就直接将三个函数，暴露出来，可以直接在模板中使用

    - ![image-20230514180714704](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514180714704.png)

## mapState数组写法

- 直接在mapState的参数中传一个数组，数组的内容就是要获取的state属性
- 当函数名和state中的数据同名时，就可以使用数组写法
- ![image-20230514181013123](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514181013123.png)

## mapGetters

- 配置项getter中的数据，也可以通过同上的方法取出来
- ![image-20230514181212348](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514181212348.png)
- 生成函数以后，在模板中使用
- ![image-20230514190938776](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514190938776.png)

## mapMutations

- 总的方法类似于上述的两个方法，但是区别是，这里是对commit的简写
- ![image-20230514190607021](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514190607021.png)
- 返回两个函数，但是由于commit是需要参数的，所以在模板中使用时，需要亲自去调用，并且将参数传过去
- 也可以用数组的写法，同上
- 使用同上

## mapActions

- 类似上面
- ![image-20230514190957372](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514190957372.png)
- 使用同上
- ![image-20230514190938776](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514190938776.png)
- 数组方法同样的操作

## 总结

- ![image-20230514181319308](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514181319308.png)
- ![image-20230514191140746](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514191140746.png)

# Vuex模块化

- 如果一个Vuex中，有个组件的数据，且它们之间，毫无关系，这时候需要考虑将其进行模块化
- 按照功能进行拆分，用对象的形式
- ![image-20230514193713775](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514193713775.png)
- 这里的每个Options其实就是一个小的store，里面也会有actions，mutations，state，getters
- 然后将其再汇总在真正的store身上
- 这里需要使用一个新的配置项modules
  - ![image-20230514193838692](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514193838692.png)
- 在使用时还是可以通过map方法簇去获得，但是有两个前提条件
  - 在配置的时候，需要多加一个namespaced属性，且值设置为true
  - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514194347587.png" alt="image-20230514194347587" style="zoom:50%;" />
  - 在使用map方法簇时，在前面再添加一个参数，这个参数就是前面定义的那个按照功能拆分的对象的分类的名字，而后面则依然是对象或者数组的写法，但是这时候，是可以直接写state中的值的kye了，无需再重新写state.了
  - ![image-20230514194512663](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514194512663.png)
  - ![image-20230514194639867](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514194639867.png)
- 关键就是需要在配置的时候，打开namespaced，然后在使用时指定，这样即可恢复到之前的写法
- 通常将不同功能的配置分别写在不同的js文件，然后通过模块化进行引入
  - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514200241681.png" alt="image-20230514200241681" style="zoom:67%;" />

## 正常写法，不使用map方法簇

- 如果没有使用这些简写的方式，那么写法会有一点点小修改
  - 需要在操作数据时，加上分类名
  - ![image-20230514195259400](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514195259400.png)
- getters中的使用方法，有所区别
  - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514195809369.png" alt="image-20230514195809369" style="zoom:67%;" />
  - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514195833363.png" alt="image-20230514195833363" style="zoom:67%;" />

## 总结

- ![image-20230514200606048](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514200606048.png)

- ![image-20230514200650925](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230514200650925.png)


# Vue-route

- 路由是一组key-value的对应关系
- 多个路由需要经过路由器的管理
- ![image-20230515081119698](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515081119698.png)
- vue-router本质上是一个vue的插件库
- 安装
  - vue-router也更新了4版本，且只能在vue3中使用
  - vue-router3才可以在vue2中使用
  - npm i vue-router@3
  - Vue.ues();
- 使用
  - 在安装以后，可以在main.js中引入一个全新的配置项，router
  - 新建一个文件夹，命名为router，创建一个文件index.js
  - ![image-20230515082900200](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515082900200.png)
  - 然后通过new 暴露该配置对象
    - ![image-20230515084629341](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515084629341.png)
    - 配置对象是一个数组，里面是一组组的key-value，key则是路由，而value是对应的组件
  - 使用
    - 首先引入刚刚写好的main.js文件，然后通过Vue.use()
    - 最后，将新的配置项router加在vm身上 
    - ![image-20230515084850086](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515084850086.png)
- 总结
  - ![image-20230515084913037](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515084913037.png)

## 注意点

- 将组件分类，分为一般组件和路由组件，将路由组建单独放在一个文件夹，一般叫pages
- 当切换路由时，就是将当前被切走的路由，进行销毁，可以通过生命周期钩子进行验证
-  每个组件身上都有一个route，但是他们身上的router都是同一个，因为路由器只能有一个，用于管理所有的route，但是route却都是单独的
- ![image-20230515093744340](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515093744340.png)

## 嵌套(多级)路由

- 路由中还嵌套着子路由
- 增加一个配置项
- 所有routes中的配置，都是一级路由
- 而routes中的路由还可以继续配置路由，就是它的子路由
  - 增加一个children配置项，也是一个配置项，数组的形式，里面的值是对象，也是一组组key-value
  - 需要注意，子路由的path是不需要加/的
  - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515094549039.png" alt="image-20230515094549039" style="zoom:50%;" />
- 在使用route-link时，需要在to的前面加上它的父路由名
- 总结
  - ![image-20230515094955904](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515094955904.png)

## 路由传参

- 路由可以携带参数
- query参数，类似于地址中的?&携带的参数
  - 通过query携带的参数，保存在当前组件的route中的query对象身上，字符串写法 
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515102708073.png" alt="image-20230515102708073" style="zoom:50%;" />
    - 在组件中读取时，要在$route的身上读取，
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515103547289.png" alt="image-20230515103547289" style="zoom:50%;" />
    - 不适用于太多参数
  - 对象写法，将to写成一个对象，里面有两个配置项，一个path，路由地址，另外一个query，要携带的数据
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515103808098.png" alt="image-20230515103808098" style="zoom:50%;" />
    - 取得时候，还是在query中取
  - 总结<img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515103923089.png" alt="image-20230515103923089" style="zoom: 50%;" />
- params参数
  - 参数可以传可以不传，如果地址中配置了该参数，但是却没有传过去，那么此时路径会出现验证错误，此时就需要在配置时，在参数后加一个？。另外，如果传的是一个空串，也会导致同样的问题，路径出错，这个问题的解决办法是通过逻辑运算符，后面或上一个undefined
  - 将数据直接写在地址中用/分隔，然后在配置项的path中，加占位符，然后直接在path中加/分隔数据
    - ![image-20230515104838003](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515104838003.png)
    - ![image-20230515104955052](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515104955052.png)
  - 取得时候在params中取，类似于query参数
  - 也可以使用对象的写法，但是要将数据写在params中
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515105109422.png" alt="image-20230515105109422" style="zoom:50%;" />
  - <span style="color:red">要注意的是，如果使用了对象写法，那么路由只能写name形式的，不可以再写path</span>
  - 总结
    - ![](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515105323056.png)
    - ![image-20230515105328940](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515105328940.png)
    - ![image-20230515105346300](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515105346300.png)

## 命名路由

- 给哪个路由起名字，就增加一个配置项，name，name的值就是这个路由的名字
- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515104114531.png" alt="image-20230515104114531" style="zoom:50%;" />
- 在使用时，则不再需要写路由的前缀，直接写路由名即可
  - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515104212279.png" alt="image-20230515104212279" style="zoom:50%;" />
- 如果是只有一个配置项，不需要传参数，那么就需要在to的里面，写成对象的形式
  - ![image-20230515104339549](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515104339549.png)
- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515104420543.png" alt="image-20230515104420543" style="zoom:67%;" />
- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515104424941.png" alt="image-20230515104424941" style="zoom: 67%;" />

## 路由中的props

- 简化组件收到出来的数据时，需要写很多的this和$route
- 需要在配置项中多定义一个props，一共有三种写法
  - 对象写法
    - ![image-20230515105851086](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515105851086.png)
    - 在使用时，就去配置了这个props的组件中中，声明props去接收，然后就可以直接使用了
  - 值为布尔值
    - ![image-20230515105947789](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515105947789.png)
    - ![image-20230515110016523](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515110016523.png)
    - 存在局限性，只能接受params参数
  - 值为函数
    - 函数的返回值必须是一个对象，且必须有返回值
    - 是一个回调函数，VueRouter会在适当的时候进行调用
    - 可以收到一个参数，并且，这个参数就是当前组建的$route
    - 可以直接在参数中进行解构赋值
      - ![image-20230515110619933](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515110619933.png)
  - ![image-20230515110634579](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515110634579.png)
- 总结
  - ![image-20230515110649041](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515110649041.png)

## route-link的两种模式

- ![image-20230515160431941](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515160431941.png)

## 编程式路由导航

- 使用$route身上的push和replace方法，即可实现跳转，而方法里面的参数，是和route-link的参数是一样的，也是一个配置对象
- ![image-20230515172952842](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515172952842.png)

## 缓存路由组件

- 如果想一个组件，在切换时，不被销毁，而是被缓存下来，那么就使用一个标签，将router-view包裹起来
- 且该标签可以带一个属性include，如果不写，则默认将所有的组件都缓存，不销毁，而在include中定义的组件，则会被缓存下来
- ![image-20230515173635692](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515173635692.png)
  - ![image-20230515173817394](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515173817394.png)

- 注意，这里的include中的属性，写的是组件的bane属性

![image-20230515173718586](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515173718586.png)

## Router中独有的生命周期钩子

- activated和deactivated，一个指激活时，一个值失活时
- activated从没出现到出现时，就会调用该函数，而deactivated正好相反
- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515174559073.png" alt="image-20230515174559073" style="zoom:50%;" />

## Router的两种工作模式

- 存在history和hash两种工作模式，当时用hash时，会在地址栏中显示一个井号，而history则不会
- 可以在路由配置中，借助mode配置项进行修改
- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515184037933.png" alt="image-20230515184037933" style="zoom:50%;" />
- 区别
  - hash兼容性更好，history兼容性略差
  - history在打包完成后，部署在服务器时，在刷新的时候，会出现路径问题，反之hash模式则不会有
  - 因为当处于hash模式下，#后面的会被当成资源去服务器访问，所以会出现Not 404出现，在后端中可以引入一个中间件去解决这个问题
    - 引入connect-history-api-fallback
    - 然后应用该中间件，引入中间件时，返回的是一个函数，而应用时，参数就写入这个函数即可
    - ![image-20230515200049769](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515200049769.png)
- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515200134929.png" alt="image-20230515200134929" style="zoom:67%;" />

## <span style="color:red">解决push和replace方法跳转时的异常</span>

- 由于push和replace方法需要接收两个参数，由于它是基于promise的，所以需要传入两个回调，一个失败的回调，一个成功的回调![image-20230516152938814](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230516152938814.png)
- 这种写法，每次使用时，都需要重新配置，一般不采用
- 

# 路由守卫

- 保护路由的安全，有点权限的意思
- 路由守卫是在路由的配置中，设置，如果要配置的话，就不可以再使用简写形式暴露对象，而是需要定义变量，接收New VueRouter后的返回值，设置路由守卫的方法，在实例身上

## 全局守卫

- 全局守卫，所有的路由都会存在这个守卫

- 通过beforeEach方法去设置，这个方法的参数是一个函数，这里可以写成箭头函数
- 这个函数在每次路由<span style="color:red">切换之前调用以及初始化时调用</span>
- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515175617951.png" alt="image-20230515175617951" style="zoom:50%;" />
- beforeEach可以接受三个参数，分别是to,from,next，beforeEach又被称为前置路由守卫
  - to表示即将要切换的路由，注意，这个方法是还没切换时输出的
    - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515175849174.png" alt="image-20230515175849174" style="zoom:50%;" />
  - from当前是要跳转至哪个路由
  - next是一个函数，即将走到下一个路由，必须调用next，否则无法转接到下一个路由
- $router.meta中可以存放自己的独有的数据，可以加一个是否需要权限的配置项
  - ![image-20230515180652113](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515180652113.png)
  - 这样可以在进行判断路由时，直接读取该配置项
- afterEach是指后置路由守卫，类似于前置路由守卫，初始化的时候，会被调用一次，然路由切换之后会被调用一次，注意是，后置路由守卫，没有next方法，因为已经切换了，不再需要这个方法了
  - 一般不常用，是指，在切换完成后，做一下修改title的事情
- 总结
  - <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515181954721.png" alt="image-20230515181954721" style="zoom: 50%;" />

## 独享路由守卫

- 如果想给一个路由，单独设置一个守卫，那么就需要使用该技术，直接在配置项中，加一个beforeEnter即可，参数跟beforeEach一模一样，且使用方法也一样
- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515182212099.png" alt="image-20230515182212099" style="zoom:50%;" />
- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515182333330.png" alt="image-20230515182333330" style="zoom: 50%;" />
- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515182411976.png" alt="image-20230515182411976" style="zoom:50%;" />

## 组件内路由守卫

- 不是写在配置项中，而是写在组件内
- 需要在vc中配置一个，beforeRouteEnter函数，意思是当路由进入之前调用，同理存在一个beforeRouteLeave函数，路由离开之前
- <img src="C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515182625149.png" alt="image-20230515182625149" style="zoom:50%;" />
- 必须是通过路由规则时，进入组件时，才会被触发，如果是一开始就初始化在页面上，是不会触发该函数的
- ![image-20230515183509936](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515183509936.png)

# Vue UI

- 移动端
  - Vant
  - Cube UI
  - Mint UI
- PC端
  - Element UI
  - IView UI
- 按需引入
  - ![image-20230515205201265](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230515205201265.png)
  - 