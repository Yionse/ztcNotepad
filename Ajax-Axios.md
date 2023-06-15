# Http协议

## 请求报文

- 请求行
  - 包含请求方式和请求地址
- 请求头
  - 包含一些浏览器信息
- 请求主题
  - 发送给服务器的数据内容

## 响应报文

- 状态行
  - 请求是否成功，请求的状态
- 响应头
  - 服务器的信息，服务器返回给浏览器的一些信息
- 相应主体
  - 用户要看到的内容

# XMLHTTPRequest

- 默认不存在，需要通过new创建

- ```javascript
  const xhr = new XMLHttpRequest();
  xhr.open(方式[get|post], url);			//	请求行
  xhr.setRequestHeader('key','value');	//	请求头，get请求可以省略请求头，以键值对
  xhr.send();                      //	请求主体
  ```

- xhr.onload = function() {};     回调函数，当请求得到回应时，会触发这个函数

- 服务器的相应存放在xhr对象中

## 两种方式

- get
- post
  - 发送数据时，需要设置请求头
    - xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
  - 发送数据时，写在send函数中，以键值对的方式进行传递
  - send('key=value&key1=value1');

# Ajax的同步和异步

## 异步

- 在open时，最后一个参数默认不写就是异步，即可以同时做很多事情

## 同步

- 在open时，最后一个参数为false则为同步，即只有在请求完成后，才可以做后面的事情

## open

- xhr.open(方式, url, 是否使用异步);	默认是true，选择了false则是同步

## 回调函数

- onload或者是load在请求完成后，触发
- onreadystatecheng   当请求状态更改时触发
  - 配合xhr.state使用，当xhr.state = 200时，则请求成功，其它的则为失败
  - xhr.readystate，代表当前xhr的状态

# Ajax返回数据的两种格式

## XML

- 示例

  - ```xml
    <root>
    	<name>张天成</name>
        <age>21</age>
        <gender>男</gender>
    </root>
    ```

  - 

- 在后端环境中使用file_get_content(xml文件路径);返回一个字符串

  - 如果在前端页面中使用responseText接收则收到的是字符串

  - 需要在后端中写一个状态行语句

    - ```php
      header('content-type:text/xml;charset=utf-8');
      ```

  - 此时在前端页面中用

    - ```php
      xhr.responseXML，即可得到完整的xml对象，然后解析即可
      ```

    - ```js
      xhr.responseXML.querySelector(标签名).innerHtml;
      ```

- 解析方式

  - ```js
    xhr.responseXML.querySelector('name').innerHtml;     //	张天成xhr.responseXML.querySelector('age').innerHtml;     //  21	xhr.responseXML.querySelector('gender').innerHtml;    //	男
    ```

## JSON

- JSON实际上就是一种字符串

- 基本上所有的编程语言都提供了对应的方法进行解析

- 属性名和属性值必须使用双引号包裹，数值除外

- 在.json文件中定义JSON字符串

  - ```json
    [
        {
            "name": "ztc",
            "age" : 21
        },
        {
            "name": "ztcccc",
            "age" : 40
        }
    ]
    ```

  - ```js
    let str =  JSON.stringifly(JSON字符串);
    let arr =  JSON.parse(str);         //	即可返回对应JSON字符串的数组
    ```
  
- 返回时有一个语句，可以加可以不加

  - ```js
    header('content-type:application/json;charset=utf-8');
    ```


## JQuery封装Ajax

- data允许传入对象

- 当success触发不成功的时候，则会触发error

- complete当请求完成时，触发，不管请求成功与否

  - ```js
    $.ajax({
        complete : function() {
            回调
        }
    });
    ```

- 自己指定返回的数据类型

  - ```js
    $.ajax({
        dataType: 'json'
    });
    ```

- ```js
  $.ajax({
      type: 'get'|'post',
      url : 'xxx.php',
      data: 'key=value&key2=value2';
      success : (data) => {
     		请求成功后的回调函数 
  	},
      error : (xhr, 错误信息， 错误具体行数) => {
      }
  });
  ```

- ```js
  $.ajax({
      type: 'get'|'post',
      url : 'xxx.php',
      data: 'key=value&key2=value2';
      success : (data) => {
     		请求成功后的回调函数 
  	}
  });
  ```

- ```js
  $.get({
      url : 'xxx.php',
      data: 'key=value&key2=value2';
      success : (data) => {
     		请求成功后的回调函数 
  	}
  });
  ```

- ```js
  $.post({
      url : 'xxx.php',
      data: 'key=value&key2=value2';
      success : (data) => {
     		请求成功后的回调函数 
  	}
  });
  ```

## 跨域请求

- 同源

  - 当协议，地址，端口号都一样时，则被称之为同源，被解析为在同一台服务器下

- 不同源

  - 以上三个中，有一个不一样就称之为不同源

- 跨域

  - 不同源之间的页面互相发送请求就被称为跨域
  - 浏览器默认限制跨域访问

- 解决方案

  - cross

    - 在服务端设置的代码，H5才支持

    - ```js
      header('Access-Control-Allow-Origin:*');
      ```

  - JSONP

    - DOM元素的src属性是支持跨域访问的
    - JSONP就是利用了script标签src属性
    - 在script标签的后面写上需要请求的页面，发送了一个方法名到服务器，服务器接收到方法名之后拼接一个方法的调用，在参数中传入了需要给浏览器的数据
    - 返回给浏览器，浏览器把它当做js解析
    - 没有跨域问题，从服务器获取到了数据

# Axios

## 基础概念

- axios是一个基于Promise的Ajax库

## Axios请求类型

- get

  - 获取

  - ```js
    axios({
        method : 'GET',		
        url :	'http://localhost'	//	请求的url
    }).then(response => {	//	axios返回的是一个promise对象
        console.log(promise对象);
    });
    ```

- post

  - 添加

  - ```js
    axios({
        method : 'POST',		//	POST, PUT, DELETE
        url :	'http://localhost',	//	请求的url
    	 data: {
             key: value			//	需要发送给服务端的数据，以对象呈现，写在data里面
         }
    }).then(response => {	
        console.log(promise对象);
    });
    ```

- put

  - 更新

  - ```js
    axios({
        method : 'PUT',		//	POST, PUT, DELETE
        url :	'http://localhost/需要修改的那个数据的id'	//	请求的url
    	 data : {
       		key: value 
    	}
    }).then(response => {	//	axios返回的是一个promise对象
        console.log(promise对象);
    });
    ```

- delete

  - 删除

  - ```js
    axios({
        method : 'DELETE',		//	POST, PUT, DELETE
        url :	'http://localhost/删除的id'	//	请求的url
    }).then(response => {	//	axios返回的是一个promise对象
        console.log(promise对象);
    });
    ```

- 还可以使用axios.request()方法，发送请求，用法与axios()完全一致

- axios.post()；相当于axios(method: 'post')，但参数不一样

  - ```js
    axios.post(url, [data, config]);
    ```

- 

## 使用方式

- 后端页面通过 npm 去安装那个包

  - ```js
    npm i axios
    ```

- 前端可以通过CDN，或者是本地引入

  - <script></script>

## Axios相应的Promise对象

- 返回的整个Promise对象如下

  - ![image-20230423133406410](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230423133406410.png)

- 其中config为请求对象，包含了请求的类型，url，请求体

  - ![image-20230423133524219](D:\Project\Web\Notepad\image-20230423133524219.png)

- data为响应体的结果，就是服务器发回的数据，且自动的做了JSON解析

  - ![image-20230423133641828](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230423133641828.png)

- headers为响应头保存了服务器相关的信息

  - ![image-20230423133750450](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230423133750450.png)

    ​	

- request为原生的Ajax请求对象，在axios发起请求时，创建的XMLHttpRequest对象

  - ![image-20230423133842669](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230423133842669.png)

## 配置Axios默认的选项

- 如果需要发送get请求，需要每次都在对象里面写method: get；不方便，可以在开头就进行定义

  - axios.default.method = 'GET'；
  - 其它的选项同理

- baseUrl，url的前缀不用每次发送请求的时候都写了

  - 配置之前

    - ```js
      axios({
          url: 'http://127.0.0.1/api'
      });
      ```

  - 配置之后

    - ```js
      axios.default.baseUrl = 'http://127.0.0.1';
      axios({
          url: '/api'
      });
      ```

# axios的使用

## axios创建实例对象

- ```js
  const axios = axios.create({
      一些配置项
      value: key,
      baseUrl: 'http://127.0.0.1'
  });
  
  //	也可以使用
  axios({
      url: '/api'
  }).then(response => {
      console.log(response);
  });
  
  axios.get('/api').then(response => {
      console.log(response);
  });
  ```

- 用axios.create方法创建出来的对象，跟axios的直接用法，几乎没有差别

- 作用，当需要给两个不同的域名发请求时，可以用到

  - ```js
    const A = axios.create({
        baseUrl: 'http://127.0.0.1';
    });
    A.get();
    const B = axios.create({
        baseUrl: 'http://129.0.0.1';
    });
    B.get();
    //	如果这里使用默认配置的话，则需要配置两次
    ```

## axios拦截器

- 拦截器其实就是一些函数

- 两大类

  - 请求拦截器
    - 发送请求之前，借助回调，对请求的参数和内容进行处理或检测，再决定是否发送请求
  - 响应拦截器
    - 当服务器传回数据以后，可以先对结果进行预处理 ，选择要不要返回给客户端

- 用法

  - ```js
    axios.interceptors.request.use(function(config) {
        console.log('请求拦截器，成功！');
        return config;		//	这里是需要有一个返回值的，因为还要到下一步响应拦截器
    }， function(error) {
        console.log('请求拦截器，失败');
        return new Promise.reject(error);	//	返回一个失败的Promise对象
    });
    
    axios.interceptors.reponse.use(function(response) {
        console.log('响应拦截器， 成功');
        return respone;
    }, functin(error) => {
        console.log('相应拦截器， 失败');
        return new Promise.reject(error);		//	返回一个失败的Promise
    });
    ```

- 需要注意的是，如果有多个请求拦截器，则后定义的请求拦截器，会先执行，而相应拦截器，则按照顺序执行

  - ```js
    axios.interceptors.request.use(function(config) {
        console.log('请求拦截器，成功！');
        return config;		
    }， function(error) {
        console.log('请求拦截器，失败');
        return new Promise.reject(error);	
    });
    
    //	这个拦截器会先执行， 也就是会先输出  front
    axios.interceptors.request.use(function(config) {
        console.log('请求拦截器，成功！    front');
        return config;		
    }， function(error) {
        console.log('请求拦截器，失败      front');
        return new Promise.reject(error);	
    });
    ```

- 请求拦截器中的  参数  config，实际上就是配置对象，可以进行参数的设置

  - ```js
    config.params = {id : 100};
    ```

- 响应拦截器中的  参数  response，是axios默认生成的对象，也是可以进行修改的

  - ![image-20230423175642316](C:\Users\ZhangTiancheng\AppData\Roaming\Typora\typora-user-images\image-20230423175642316.png)

  - ```js
    //	后续的操作，不需要那么多冗余的数据，就可以使用
    axios.interceptors.reponse.use(function(response) {
        console.log('响应拦截器， 成功');
        return respone.data;		//	只需要response里面的data部分
    }, functin(error) => {
        console.log('相应拦截器， 失败');
        return new Promise.reject(error);		//	返回一个失败的Promise
    });
    ```

## 取消axios请求

- 利用了配置对象，如果一个请求有被取消的可能性，则需要使用cancelToken这个属性，它写在配置对象中

  - ```js
    //	定义全局变量
    const cancle = null;
    
    axios({
        url:,
        method:,
        cancelToken : new Axios.CancelToken(function(e) {
        	//	将这个形参e，传递给cancel即可
        	cancle = e;		//	e本质上是一个函数
    	})
    });
    
    //如果需要取消的话，直接调用cancle()即可以取消
    ```

- 多次点击发送请求时，如果前一次的请求还没有结束，则需要取消请求，还是定义一个变量，在每次发送请求时，进行判断，如果请求还没有结束，则取消上一次请求，如果直到请求结束，还没有再次被点击，那么在结束后，再讲cancel的值复原即可

  - ```js
    const cancle = null;
    button.onclick = () => {
        if (cancle) {		//	如果cancel的值不为空，则直接取消上一次的请求
            cancle();
        }
        axios({
            url:,
            method:,
            cancelToken : new axios.CancelToken(function(e) {
                    //	将这个形参e，传递给cancel即可
                    cancle = e;		//	e本质上是一个函数
                })
        }).then(response => {
            //	当请求顺利完成后，需要修改cancel的值，让它变成null
            cancel = null; 
        });
    }
    ```

# axios源码

- 核心代码都在lib目录中
- axios与Axios的区别与联系
  - axios.js是整个包的核心，入口函数