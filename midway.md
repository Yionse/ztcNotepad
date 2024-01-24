# Midway

- Midway 基于 TypeScript 开发，结合了`面向对象（OOP + Class + IoC）`与`函数式（FP + Function + Hooks）`两种编程范式，并在此之上支持了 Web / 全栈 / 微服务 / RPC / Socket / Serverless 等多种场景，致力于为用户提供简单、易用、可靠的 Node.js 服务端研发体验。

# 基础

- 快速初始化项目
  - ````npm
    npm init midway@latest -y
    //  快速初始化项目```
    ````
  - 文件目录如下
    - src 整个 Midway 项目的源码目录，你之后所有的开发源码都将存放于此
    - test 项目的测试目录，之后所有的代码测试文件都在这里
    - package.json Node.js 项目基础的包管理配置文件
    - tsconfig.json TypeScript 编译配置文件
  - 其它常见的目录
    - controller Web Controller 目录
    - middleware 中间件目录
    - filter 过滤器目录
    - aspect 拦截器
    - service 服务逻辑目录
    - entity 或 model 数据库实体目录
    - config 业务的配置目录
    - util 工具类存放的目录
    - decorator 自定义装饰器目录
    - interface.ts 业务的 ts 定义文件

# 路由和控制器

## 路由

- 控制器文件一般来说在 src/controller 目录中，我们可以在其中创建控制器文件。
- Midway 使用 @Controller() 装饰器标注控制器，其中装饰器有一个可选参数，用于进行路由前缀（分组），这样这个控制器下面的所有路由都会带上这个前缀。
- 同时，Midway 提供了方法装饰器用于标注请求的类型。
- 示例：创建了一个首页的控制器，并且需要通过 GET 方式访问

```ts
// src/controller/home.ts
import { Controller, Get } from "@midwayjs/core";
@Controller("/")
export class HomeController {
  @Get("/")
  async home() {
    return "Hello Midwayjs!";
  }
}
```

- @Controller 装饰器告诉框架，这是一个 Web 控制器类型的类，而 @Get 装饰器告诉框架，被修饰的 home 方法，将被暴露为 / 这个路由，可以由 GET 请求来访问。
- 另外还有常见的 POST 装饰器，用于相应 POST 请求
- 此外还有一些特殊的，例如 DEL、PUT、ALL 等，`ALL`可以相应所有类型的请求
- 可以将多个路由绑定到一个方法上

```ts
@Get('/')
@Get('/main')
async home() {
  return 'Hello Midwayjs!';
}
```

- 所有的路由方法都是 async

## 装饰器参数约定

- Midway 添加了常见的动态取值的装饰器，我们以 @Query 装饰器举例， @Query 装饰器会获取到 URL 中的 Query 参数部分，并将它赋值给函数入参。下面的示例，id 会从路由的 Query 参数上拿，如果 URL 为 /?id=1 ，则 id 的值为 1，同时，这个路由将会返回 User 类型的对象。

```ts
// src/controller/user.ts
import { Controller, Get, Query } from "@midwayjs/core";
@Controller("/api/user")
export class UserController {
  @Get("/")
  async getUser(@Query("id") id: string): Promise<User> {
    // xxxx
  }
  //  当存在多个Query参数时@Query() queyData: { test1: string; test2: number }
}
```

- 还存在一些其他的装饰器参数，例如 Session、Param、Body、Headers 等
- 还可以使用 Inject 装饰器，定义上下文，然后在方法中，通过 this.ctx.query 去获取 Query 参数

```ts
// src/controller/user.ts
import { Controller, Get, Inject } from "@midwayjs/core";
import { Context } from "@midwayjs/koa";

@Controller("/user")
export class UserController {
  @Inject()
  ctx: Context;

  @Get("/")
  async getUser(): Promise<User> {
    const query = this.ctx.query;
    // {
    //   uid: '1',
    //   sex: 'male',
    // }
  }
}
```

- Body 参数就是 POST、PUT、DEL 等请求类型时的参数，取值也同上，可以通过装饰器@Body 取出来，也可以通过@reject 装饰器，通过上下文的方式取出来
