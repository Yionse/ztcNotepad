# JQuery常用API

## 简单语法

- 加载文档完成以后再执行代码
  - $(document).ready(function(){})——等文档中主要DOM加载完毕以后，再执行函数
  - $(function(){})，效果同上，但是更简洁
  - 相当于原生中的DOMContentLoaded
- $(this)，相当于原生中的this
- $(this).index()，得到当前元素的索引号

## JQuery的顶级对象$

- $相当于Js中的window对象

## DOM对象和JQuery对象

- 用原生js获取的就是DOM对象
- 通过JQuery获取的就是JQuery对象
  - $('选择器')
- JQuery对象智能使用JQuery方法，DOM对象只能使用原生的Js方法和属性
- DOM对象转换为JQuery对象
  - $(DOM对象);
- JQuery对象转换为DOM对象
  - $('div')[index]
  - $('div').get(index)
  - 这样获取到的就是DOM对象了

## JQuery选择器

- id选择器			$('#one');
- 全选选择器        $('*');
- 类选择器           $('.div');
- 标签选择器        $('span');
- 并集选择器        $('div, ul, li');
- 交集选择器        $('li.current');
- 层级选择器
  - 后代选择器	$('ul li');
  - 子代选择器    $('ul>li');
- JQuery筛选选择器，只有JQuery才有，原生的JavaScript没有
  - $('li:first');		选择第一个li
  - $('li:last');		选择最后一个li
  - $('li:eq(n)');	  选到的li中，选择第n个元素，下标从0开始
  - $('li:odd');		选择第奇数个li
  - $('li:even');		选择第偶数个li
- JQuery筛选方法
  - $('li').parent();	             li的父节点
  - $('li').parents('ul');           返回指定的祖先元素
  - $('ul').children('');            查找最近一级的指定元素，参数缺省的话，会选出所有的
  - $('ul').find('li');                 相当于后代选择器，参数不可以缺省
  - $('p').siblings();                除了自身之外，当前元素的所有兄弟元素
  - $('p').nextAll();                 当前元素后面所有的同辈元素
  - $('p').prveAll();                 当前元素之前所有的同辈元素 
  - $('.exapmle').hasClass('example');    检查当前元素是否含有特定的类，不加点

## 隐式迭代

- 给匹配到所有的元素进行循环遍历，执行相应的方法，而不用再手动进行循环

# JQuery样式操作

## 通过CSS样式操作

- $('div').css(属性，属性值);		设置属性
  - 多组样式之间采用对象的形式，以逗号分隔
  - $('div').css({属性:属性值，属性:属性值});      且属性和属性值都不需要加引号
- $('div').css(属性);                   如果不设置属性值，则是获取当前元素的属性值，带单位
- 如果是数字的话，可以不带引号和单位，也可以自动识别
- 组合属性的话，用驼峰命名

## 通过类操作

- $('div').addClass(类名);	添加类名，不需要加点
- $('div').removeClass(类名);	删除类名，不需要加点
- $('div').toggleClass(类名);	切换类名，不需要加点，存在的话，就删除，不存在就加上
- 不会影响原先的类名

# JQuery效果

## 动画效果

- 如果短时间内多次的触发动画，则每个动画都会执行，就会造成排队现象
- 停止排队使用stop()，且必须写在动画函数的前面
- JQuery里面所有的动画都存在排队问题

## 显示隐藏

- show([speed: slow, fast, time]，[easing: swing, linear], [fn()])	速度，切换效果，回调函数
- hide([speed: slow, fast, time]，[easing: swing, linear], [fn()])
- toggle([speed: slow, fast, time]，[easing: swing, linear], [fn()])，切换效果

## 滑动

- slideDown([speed: slow, fast, time]，[easing: swing, linear], [fn()])
- slideUp[speed: slow, fast, time]，[easing: swing, linear], [fn()])
- slideToggle([speed: slow, fast, time]，[easing: swing, linear], [fn()])

## 切换

- hover([over()],[out()])	，切换效果，进入触发over，离开触发out
- 如果只有一个参数，则经过，离开都会触发

## 淡入淡出

- fadeIn([speed: slow, fast, time]，[easing: swing, linear], [fn()])
- fadeOut([speed: slow, fast, time]，[easing: swing, linear], [fn()])
- fadeToggle([speed: slow, fast, time]，[easing: swing, linear], [fn()])
- fadeTo(speed, opcity, [easing: swing, linear], [fn()])，速度和透明度参数必须写

## 自定义动画

- animate(params,[speed: slow, fast, time]，[easing: swing, linear], [fn()])，第一个参数是需要变化的样式，必须以对象的形式传递，且以驼峰命名，属性值可以不带单位

# 属性操作

## 获取元素固有的属性

- prop(属性名);

## 设置元素属性

- prop(属性名,属性值);

## 元素自定义属性

- attr(属性名);                  获取
- attr(属性名，属性值)       设置

## 数据缓存

- 将一个数据存在DOM元素身上，但不会显示
  - $('span').data(key, vlaue);        存的时候是两个属性
  - $('span').data(key);                  取得时候输入key就可以
- 还可以获取到H5新增的自定义属性
  - $('span').data('id');                  不需要加data
  - $('span').attr('data-id');            需要加data

## 文本内容

- html();            形参缺省的话，就是获取
- html(内容);      有形参则是设置
- text();              只获取文本内容，不带标签

## 表单的值

- val();				      获取表单的值
- val(内容);				设置表单的值

# 元素操作

## 遍历

- each(function(index, domEle));           可以遍历元素，但得到的是DOM对象
- $.each(object, function(index, ele));     可以遍历任何元素，包括数组和对象

## 创建

- $('<a></a>');	只需要将标签写在括号里即可完成创建

## 添加

- 内部添加，即添加完成后，目标元素是被添加元素的儿子
  - element.append(li);			添加到最后面
  - element.prvepend(li);        添加到最前面
- 外部添加，即添加完成后，目标元素是被添加元素的兄弟
  - element.after(li);               添加到这个元素之后
  - element.before(li);            添加到这个元素之前

## 删除

- remove();                             即可直接删除，整个元素都不存在
- empty();                               清空所有的子节点
- html('');                                同empty();

# JQuery事件

## 事件注册

- 跟Js原生一样.click这种形式

- on()，可以一次性处理多个事件，以触发方式和函数以对象的形式放在形参中

  - ```js
    $('div').on(
    	click: () => {
       		//	代码 
    	},
       mouseenter : () => {
           //	代码
       },
       mouseleave : () => {
           //	代码
       }
    );
    ```

- 如果两个触发事件对象的执行函数相同则可以简便写法

  - ```js
    $('div').on('click mouseenter mouseleave', () => {
        //	方法体，当三件事触发时，他们会执行相同的步骤
    });
    ```

- 单个处理事件则还是用相似于Js原生的写法

## 事件处理

- 事件委托，即把原本需要挂载在子元素身上的事件，挂载到父元素身上，然后实现事件委派，提高了程序的性能

  - ```js
    $('ul').on('click', 'li', () => {
        //	代码
    });
    ```

- on可以给未来创建的元素绑定事件，如果采用之前原生的Js那种方法，后来创建的是无法添加事件的 

- off()，解绑事件，如果参数为空，则解绑所有的事件，同时解绑两个事件用空格隔开

- one()，语法同on，但是用one绑定的事件，只会触发一次，后续则不再触发

- 自动触发事件

  - element.click()；										  类似于原生的写法
  - element.trigger(事件类型);
  - element.triggerHandler(事件类型);             不会触发元素的默认行为

## 事件对象

- 同原生中e的用法

# JQuery其他常用方法

## 拷贝对象

- $.extend([deep], newObj, oldObj,[oldObj2])；deep的值为布尔值，为true的话则是深拷贝，否则是浅拷贝，默认是浅拷贝。后面的被拷贝数组可以选择多个，如果存在相同key的，则以被拷贝对象中的key为准
- 浅拷贝在复制中会直接把复杂对象的地址复制过来
- 深拷贝如果key冲突了，而key所指向的是对象的话，不冲突的话则会追加给那个对象

## 多库共存

- 如果怕$与其他的库冲突了，可以改用jQuery.()
- 甚至可以自定义
  - let jjQuery = $.noConflict();
  - jjQuery('div');          等价于  $('div')

## JQuery插件

- 图片懒加载，当页面滚到到图片区域再进行显示

# JQuery尺寸操作

## JQuery尺寸

- width();                     获取宽度，只包含width，如果有参数则是设置
- innerWidth();             加上padding以后的宽度
- outerWidth();             加上border以后的宽度
  - outerWidth(true);             加上margin以后的宽度
- 不带单位，返回数字型
- 如果不给参数，则是获取，写了参数数字型，则是设置

## JQuery位置

- offset()
  - 获取元素的偏移，跟父元素没关系，相对于整个文档
  - 返回的是一个对象{left: 100,       top:100}
- position()
  - 得到相对带有定位的父亲的偏移，如果没有定位则相对于body，返回同上
  - 智能获取，不能设置
- scrollTop()/ScrollLeft()
  - 获取被卷去的头部
  - 写参数则是设置