# NodeJs概念

- NodeJs运行环境包含两块
  - V8引擎
  - 内置API
    - fs
    - path
    - http
    - js内置对象
    - querystring
- 浏览器是js的前端运行环境
- Node是js的后端运行环境
- NodeJs无法调用DOM和BOM、AJax等
- node -v 查看当前node的版本号
- 在Node环境下执行js代码
  - node  文件的所在路劲

# fs文件系统

- require('fs')；引入fs模块即可，然后用const变量接

  - ```js
    const fs = require('fs');
    ```

- 读取文件

  - ```js
    fs.readFile(path, [options], callback);
    path：文件路径	//	必须
    options：以何种文件的编码格式，读取文件		//	可缺省
    callback：回调函数，数据在回调函数中呈现	//	必须
    	callback回调函数中，有两个参数，第一个是err，代表失败的结果，第二个是data，代表成功的结果
    //	如果读取成功，则err的值为null，data的值为读取的数据
    //	如果读取失败，则data的数据为undefined，而err为一个错误对象
    ```

  - ```js
    const fs = require('fs');
    fs.readFile('../normal.css', function(err, data) {		//如果options参数不写，则需要将结果.toString()，进行转换，反之则不需要
        console.log(data);
    });
    ```

  - 判断文件是否读取成功，可以在读取完成以后，判断err的值，如果err的值为null，则读取成功

- 写入文件

  - ```js
    fs.writeFile(path, data, [options], callback);
    data:	要写入的内容
    options:	一般省略，默认是utf8
    ```

  - ```js
    const fs = require('fs');
    fs.writeFile(''):
    ```

  - 如果文件写入成功，那么回调函数中的err为null，当写入失败时，err则为一个错误对象

  - 如果指定的目录下的文件不存在则会新建那个文件

  - 判断err的值是否为null，如果err的值为null则为写入成功
  
- __dirname，是指当前js文件的目录，可以在访问文件的时候，写在文件路径的前面，后面再跟相对路径

# Path路径模块

- path.join();将多个字符串拼接，成为一个完整的字符串

  - ```js
    path.join([...path]);		//	参数是任意个路径，最后返回一个路径
    path.join('a', 'b', 'c');	//	a/b/c
    ```

- path.basename();在路径中，将文件的名字解析出来

  - ```js
    const path = require('path');
    path.basename('a/b/c/a/index.html');		//	输出index.html
    path.basename('a/b/c/a/index.html', '.html')	//	输出index
    //	第一个参数为必选，为文件的路径，第二个参数为后缀名，添加第二个参数则只有后缀名
    ```

- path.extname(path);获取路径中的扩展名

  - ```js
    path.extname('a/b/c/index.html');	//返回.html
    ```


# Http模块

- http模块就是NodeJs提供的用来创建Web服务器的模块，通过http.createServer()方法，就可以把一台电脑变成一个web服务器
- 使用之前使用require('http');导入

## 创建服务器

- 先导入http模块

  - ```js
    const http = require('http');
    ```

- 创建一个web服务器实例

  - ```js
    const server = http.createServer();
    ```

- 为服务器实例绑定request事件，监听客户端

  - ```js
    server.on('request', (request, response) => {
        console.log('访问了你的服务器');
    })
    ```

- 用.listen()方法启动服务器

  - ```js
    server.listen(80, () => {
        console.log('服务器运行中');
    });
    ```

## request和response

- 它俩都是服务器在被访问时，回调函数中的两个参数

- req包含了客户端的一些属性

  - req.url是客户端请求的URL地址
  - req.method是客户端的请求方式

- res包含了服务器相关的数据和属性

  - res.end(data);	发回给客户端的数据

- 在返回给客户端数据时，如果存在中文字符，则会出现乱码现象，需要在响应时，设置请求头

  - ```js
    res.setHeader('Content-type', 'text/html; charset=utf-8');
    ```

## 根据不同的URL响应不同的页面

- 获取请求的url
- 设置默认的相应内容为 404 Not Found
- 判断用户请求的是否为 根路径 / 或首页 /index.html
- 判断用户请求的是否为 /about.html 关于页面
- 设置 Content-type 防止中文乱码出现
- 使用res.end()；将内容响应给客户端

# 模块化

## NodeJs中的模块分类

- 根据模块来源不同，NodeJs中将模块分为三大类
  - 内置模块，fs，path，http
  - 自定义模块，用户自己的每个js文件
  - 第三方模块，其他人开发出来，可供使用，使用之前，需要先下载
- 加载模块，使用require();方法，可加载上述的三大类
- 加载自定义模块时，其会被执行
- 在使用require()；方法加载自定义模块时，不写后缀名也是不影响的
- 模块作用域是指，在自定义模块中定义的变量与函数，只能在当前模块被访问，这种限制级别叫做模块级作用域
- module对象
  - 每个自定义模块都有的对象，它存储了当前模块的一些信息
  - module.exports，中存储了自定义对象中，想供其他模块共享使用的成员，当其他模块使用require()；进行导入时，得到的就是module.exports所指向的那个对象
  - exports和module.exports指向的同一个对象
- commonJs规范
  - 摸个模块内部，module对象表示当前模块
  - module.exports对象是对外的接口
  - 加载某个模块时，得到的返回值是那个对象的exports属性

## npm与包

- NodeJs中的第三方模块又被称为 包
- 包是基于内置模块封装出来的 
- 包的安装
  - npm install  包名称
  - 简写为 npm i 包名称
- 为了协同工作，一般需要将项目上传，但项目上传时，一般会删掉node_modules文件夹，这里面存放了项目所有的依赖包
  - 这样其他使用者在使用时，就会出现错误
  - 使用npm i 或者 npm install 命令，一键安装项目所需的所有包即可
- 卸载包
  - npm uninstall 包名
- devDependcies节点，如果某些包只在开发阶段用到，则可以将其放在devDependcies节点下
  - npm i 包名 -D
  - npm install 包名 --save--dev
  - 包名和-D的顺序不重要
- 如果开发和上线都要用，则添加到dependcies
- 切换为npm镜像服务器下载地址，为了解决包下载速度慢的问题
  - npm config get registry 	查看当前的npm下载地址
  - npm config set registry=https://regstry.npm.taobao.org/      设置npm包下载服务器地址为国内淘宝
- 包的分类
  - 项目包
    - 在node_module中的包为项目包
  - 全局包
    - 在安装时 后面加了一个-g，则是全局包
    - npm i 包名 -g
    - 卸载全局包 npm uninstall 包名 -g
    - 只有工具性质的包，才有全局安装的必要，它们提供了 好用的终端命令

## 模块的加载机制

- 模块在第一次执行以后，后续再加载时，会优先在缓存中加载，优化了加载的效率
- 内置模块的加载优先级是最高的
- 当加载内置模块时，需要以./或../开头，否则NodeJs将会当成内置或第三方模块加载，这样会出现错误
- 如果加载内置模块时，缺省了扩展名，则NodeJs则会依次补全.js、.json、.node这三个，如果还没有，则会报错

# Express

## Express基础

- express是基于内置的http模块封装的更简单、强大的web框架

- 创建基本的web服务器

  - ```js
    const express = require('express');
    const app = express();
    app.listen(80, () => {
        console.log('服务器开启');
    });
    ```

- 相应get和post请求

  - get和post方法中间的参数

    - 参数1：url，客户端请求的url地址，当方式相同，且url相同，则会触发回调
    - 参数2：回调函数，其中有两个参数，第一个为req，客户端相关信息，第二个为res为服务端响应
    - 通过res.send();方法向客户端发回数据

  - ```js
    const express = require('express');
    const app = express();
    
    app.get('/test', (req, res) => {
        res.send('测试成功！--get');
    });
    
    app.post('/test', (req, res) => {
        res.send();
    });
    
    app.listen(80, () => {
        console.log('服务器成功开启！');
    });
    ```

- 获取客户端发来的数据

  - 当客户端使用？name=zs这种格式的参数时，可以使用req.query拿到发来的数据
  - 默认情况下，req.query是一个空对象

- 获取客户端发来的动态参数

  - 当使用:数据这种格式的参数时，使用req.params拿到这个数据
  - 默认是一个空对象

- 创建静态资源服务器，让别人可以访问

  - ```js
    const express = require('express');
    const app = express();
    app.use(express.static('GoBang'));
    app.listen(80, () => {
        console.log('服务器已启动！');
    });
    ```

  - 使用app.use(express.static('文件的路径'));

  - 需要注意的是，指定的文件夹路径将不会出现在地址中

  - 如果需要托管多个文件夹，则多次调用即可

- 挂载路径前缀

  - 推荐的做法

  - 在app.use()；方法中，添加一个参数，就是路径前缀，而这个路径前缀一般与文件夹相同

  - ```js
    const express = require('express');
    const app = express();
    app.use('/GoBang', express.static('./GoBang'));
    app.listen(80, () => {
        console.log('服务器已启动！');
    });
    ```

## Express路由

- 路由值的是客户端的请求和服务器处理函数之间的映射关系
- 路由由三部分组成，请求类型，请求路径，回调函数

## Express路由模块封装

- 路由模块

  - 利用express获得router对象，然后将路由挂载router对象身上，然后再利用exports将对象暴露出去

  - ```js
    const express = require('express');
    const router = express.Router();
    router.get('/a', (req, res) => {
        res.send('测试成功');
    });
    router.post('/a', (req, res) => {
        res.send('测试成功');
    });
    module.exports = router;
    ```

- 使用

  - 使用require将自定义的路由模块引入进来，得到的是已经挂载了的router对象，然后用app.use()；方法即可

  - ```js
    const express = require('express');
    const app = express();
    const router = require('./04-router');
    app.use(router);
    app.listen(80, () => {
        console.log('服务器已经启动');
    });
    ```

- 为路由模块添加前缀的方法，同创建静态服务器时，所挂载的方法一样，写在app.use方法的第一个参数中，则后续客户端访问是，都需要加上这个前缀

  - ```js
    app.use( '/api' ,router);
    ```

- 

## app.use()的使用

- 注册全局中间件的

## Express中间件

- 特指业务流程的中间处理环节

- 如果一个函数没有包含next()函数，则这个函数是一个路由处理函数，如果包含了next()函数则是中间件函数，路由处理函数只有req和res，且next函数必须在最下面

- next函数是多个中间件流转的关键，它表示将流转关系转给下一个中间件

- 客户端发起的任何请求，都会触发的中间件，叫做全局中间件，通过调用app.use(中间件函数)即可定义一个全局中间件

  - ```js
    const mw = (req, res, next) => {
        console.log('这是一个简单的中间件函数');
        next();		//	需要流转给下一个中间件或者是路由
    }
    app.use(mw);		//	需要在app对象中使用这个全局中间件
    ```

  - 全局中间件的简单模式

    - ```js
      app.use((req, res, next) {
              console.log('这是简化的中间件函数');
      		next();
      })
      ```

- 中间件的作用

  - 多个中间件之间共享一份req，res，可以在上游定义一些属性和方法，那么下游的中间件和路由就可以使用了

- 定义多个全局中间件时，会依据顺序依次执行这些全局中间件

- 局部中间件，不是用app.use()定义的中间件就叫做局部中间件，只有当它挂载的那个路由被请求时，才会被触发

  - ```js
    app.get('/', (req. res. next) {
            console.log('这是中间件处理函数');
            next()
            },
        (req, res) {
        console.log('这是路由处理函数');
    });
    ```

  - 还可以定义多个局部中间件，第一个参数永远是url，最后一个参数一定是路由处理函数，中间的可以放任意多个中间件函数

    - ```js
      app.get('/', mw1. mw2, mw3, (req, res) {})
      app.get('/', [mw1. mw2, mw3], (req, res) {})
      ```

- 中间件的注意点

  - 一定要在路由之前定义中间件
  - 客户端发来的请求，可以调用多个中间件进行处理
  - 中间件处理完之后，要调用next()函数流转给下面的中间件或者是路由
  - 防止代码逻辑混乱，在调用完next()之后，不要写其它的代码
  - 连续调用多个中间件时，它们是共享req和res的

## 中间件的分类

- 应用级

  - 通过app.use()、app.get()、app.post()绑定到app实例上的中间件都是应用级中间件

- 路由级

  - 绑定到router身上的中间件就称为路由级中间件，与应用级中间件的用法完全一致，只是它们俩的绑定对象不一样

- 错误级

  - 专门用来绑定整个项目中出现的异常错误，避免整个项目崩溃，错误级的中间件函数有四个参数，依次是err、req、res、next

    - ```js
      app.get('/', (req, res) => {
          thorw 'error';
          res.send('测试成功');
      });
      app.use((err, req, res, next) => {
          console.log('发送了错误', err.message);
          res.send('服务端相应出错！');
      });
      ```

  - 错误级别的中间件必须注册在所有路由之后

- express内置的

  - express.static()，快速托管静态资源

  - express.json()，解析JSON格式的请求体数据，有兼容性，在4.16.0+的版本可用

    - ```js
      app.use(express.json());
      app.get('/', (req, res) => {
          console.log(req.body);
      });		//	所有从客户端发来的请求体数据都存在req,body属性中，所以再使用之前，需要先配置中间件，否则得到的就是undefined
      ```

  - express.urlencoded()，解析URL-encoded的请求体数据，有兼容性，在4.16.0+的版本可用

    - ```js
      app.use(express.urlencoded({extended : false}));
      app.get('/', (req, res) => {
          console.log(req.body);
          	//	需要先配置中间件，否则得到的req.body就是一个空对象
      });
      ```

- 第三方

  - 使用npm安装第三方中间件，然后注册中间件即可

  - ```js
    npm i body-parser
    const body-parser = require('body-parser');
    app.use(body-parser.urlencoded({entended : false}));
    //	在后续路由中使用req.body即可得到客户端的请求体数据
    ```

- 自定义中间件

  - req.on('data', (chunk) => {})，每次客户端发数据到服务端就会触发这个事件

  - req.on('end', () => {})，客户端数据发送完毕后，触发该事件

  - NodeJs内置了queryString模块，专门用来处理查询字符串。通过这个模块的parse函数，可以解析字符串

    - ```js
      const qs = require('querystring');
      const body = qs.parse(str);
      ```

## Express接口

- get请求

  - 可以通过req.query获得get请求发来的数据

  - ```js
    res.send({
        status : 0,		//	表示处理的状态，0表示成功，1表示失败
        message: 'Get请求成功',		//	状态的描述
        data : {}			//	发回给客户端的数据
    });
    ```

- post请求

  - 需要注意的是，需要在相应之前，设置相应的中间件解析请求体数据
  - 然后再通过解析出来的请求体数据，返回给客户端所需要的数据即可

- JSONP

  - 不属于Ajax请求，因为没有使用到XMLHttpRequest对象
  - 但是只支持get请求
  - 步骤
    - 获取客户端发过来的数据，解析以后，准备好发回客户端的数据
    - 获得客户端发来的函数名，
    - 根据前两步，拼接楚一个字符串，返回给服务端处理就可以了

## CORS跨域问题

- CORS，主流解决方案

  - 使用CORS中间件解决跨域

    - ```js
      npm i cors 
      const cors = require('cors');
      app.use(cors());
      ```

    - 使用中间件即可解决跨域问题

- JSONP，只能解决GET方式，不能解决POST方式

# Express操纵MySQL

- 步骤

  - 安装第三方模块mysql
  - 通过mysql连接到MySQL数据数
  - 通过mysql执行SQL语句

- 安装

  - npm i mysql

- 配置mysql模块

  - ```js
    const mysql = require('mysql');
    const db = mysql.createPool({
        host: '127.0.0.1',
        user: 'root',
        password: '1234',
        database: 'example'
    });
    ```

  - ```js
    db.query('sql语句', (err, result) {
             执行操作
             });
    ```

## 增删改查

- 执行select语句的话，得到的是一个数组，不管有几条记录

- 执行insert语句，返回的是一个对象，包含了affectedRows属性，表示影响的行数

  - ```js
    const user = {username: 'ztc', password: 1114};
    const sqlStr = 'insert into user (username, password) values (? , ?)';	//	可以使用占位符
    da.query(sql.str, [user.username, user.password], (err, result) => {	//	在执行时将数据补上即可
        if (err) return console.log('出错');
        if (affectedRows === 1) {		//		影响的行数为1行的情况下，表示成功
            //	执行成功
        }
    });
    ```

  - 如果需要插入的数据刚好是整个数据表都有的字段，可以使用简便的方式插入

    - ```js
      const user = {username: 'ztc', password: 1114};
      const sqlStr = 'insert into user set ?';	//	这里改为 set ？ 表示占位符
      da.query(sqlStr, user, (err, result) => {	//	直接将整个对象放进去
          if (err) return console.log('出错');
          if (affectedRows === 1) {		//		影响的行数为1行的情况下，表示成功
              //	执行成功
          }
      });
      ```

- 执行update语句时，也同样可以使用占位符的方式，返回的也是一个对象，包含了affectedRows属性，表示影响的行数，如果为1，则修改成功

  - 也可以使用便捷方式，如果字段与需要修改的信息一一对应，则可以使用便捷方式，同insert的使用

- 执行delete语句时，也可以使用占位符，同insert使用，返回的是一个对象，包含affectedRows对象

  - 使用删除语句时不安全的，这样会导致数据库中不存在该条记录，因此，改为使用标记删除的方式，在表中新增字段，status，标记该数据是否被删除

# Web开发模式

- 服务端渲染：由服务器端直接发送给客户端页面，不需要客户端发送Ajax请求
- 前后端分离：依赖于Ajax技术的广泛应用，后端负责设计Ajax接口，前端负责调用

## 身份认证

- 通过一定的手段，完成用户身份的确认
- 对于不同的web开发模式，推荐使用不同的身份认证
  - 对于服务端渲染的开发模式，推荐使用session认证
  - 对于前后端分离的开发模式，推荐使用JWT认证

## Session

- 通过cookie突破http无状态的限制

  - cookie是存储在用户浏览器中一段不超过4kb的字符串，由名称name和值value组成还有其他可选属性
  - 不同域名下的cookie各自独立
  - cookie的特性
    - 自动发送，不需开发者手动发送
    - 域名独立，每个域名之间的cookie相互独立
    - 过期时限，会自动发送未过期的cookie到服务端
    - 4KB限制

- 在项目中使用session认证

  - 安装express-session

    - npm i express-session

  - 配置session中间件

    - ```js
      const session = require('express-session');
      app.use(session({
          secret : '自定义字符串',
          resave : false,		//	 固定写法
          saveUninitalizied : true	//	固定写法
      }));
      ```

  - 当session中间件配置完成之后，就可以使用req.session存储用户信息，在配置之间是没有req.session这个属性的

  - 清空session

    - 使用req.session.destroy()可以清空服务器保存的session信息

## JWT

- JWT是跨域认证解决方案

- 服务器在客户端成功登录后，将用户的信息对象经过加密之后，生成一个token字符串，返回给客户端，客户端存储在localStorage或者是sessionStorage中。当客户端再次发起请求时，通过请求头的Authorization字段，将token发送给服务端，服务器再将得到的token字符串解析为用户信息生成特定的相应内容发回客户端

- JWT由三个部分组成，分别是Header(头部)，Payload(有效载荷)，Signature(签名)，三者之间使用逗号进行分隔

  - Header：安全性相关的部分，为了保证token的安全性
  - Payload：是真正的用户信息，是经过加密之后生成的字符串
  - Signature：安全性相关的部分，为了保证token的安全性

- 使用方式

  - 客户端每次访问服务端的时候，都要带上这个JWT字符串，从而进行身份认证推荐的做法是把JWT放在HTTP请求头的Authorization字段中
    - Authorization： Bearer <token>

- 在Express中使用JWT

  - 需要安装两个包  	jsonwebtoken   express-jwt

    - jsonwebtoken 用于生成JWT字符串
    - express-jwt     用于将JWT字符串还原成成JSON对象

  - 分别引入到项目中，得到需要使用的对象

  - 定义secret密钥

    - 为了保证JWT字符串的安全，防止在传输过程中被破解，需要专门定义一个用于加密和解密的secret秘钥
    - 当生成JWT字符串时，需要使用secret字符串对用户信息进行加密
    - 当需要把JWT字符串解密成JSON格式时，需要使用secret字符串进行解密
    - const secretKey = '';   可以使任意字符串，越复杂越好

  - 通过调用jsonwebtoken包的sign()方法，将用户的信息加密成JWT字符串，响应给用户

    - ```js
      jwt.sign(
      	{},			//		需要加密的用户信息，以对象的方式展现
         	secretKey,	//		加密的secret字符串，用于加密用户信息
          {expiresIn: '30s'} 	//	配置对象，决定了这个token的有效期，可以是h代表小时，m代表分钟
      );
      ```

  - 将JWT还原成JSON对象，使用express-jwt中间件进行解密

    - ```js
      app.use(expressJWT({
          secret : secretKey			//	一开始定义的secret字符串，这里用于解密
      }.unless(
      {
          path: [正则]				//	定义哪些路径下的接口不需要访问权限
      }))
      ```
    
    - 使用新版本的express-jwt中间件时，要更换用法

      - ```js
      app.use(expressJWT.expressjwt({
            secret: config.secret, algorithms: ["HS256"]
      }).unless({
            path: [/^\/api\//],
      }));
        ```
  
  
  - 只要配置了express-jwt中间件，就可以把解析出来的用户数据，挂载到req.user这个属性身上
  
  - 不要把密码加密到token字符串中
  
  - 捕获解析JWT字符串失败后产生的错误
  
    - 如果token字符串失效或者是不合法的token字符串时，需要在所有的路由后面注册一个错误中间件，捕获这个错误并做出相应处理
  
    - ```js
      app.use((err, req, res, next) => {
          	if (err.name === 'UnahthorizedError')	return res.send('无效的token');	// 这次错误是由token解析失败导致的
          res.send('其它的未知错误');
      });
      ```

## 密码的加密

- 在NodeJS环境中导入bcryptjs模块，用于将数据加密

  - ```js
    npm i bcryptjs
    ```

  - ```js
    const bcrypt = require('bcryptjs');
    const passwordStr = bcrypt.hasbSync(代加密的字符串, 10);		//	第二个参数表示加密的长度
    ```