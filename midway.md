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

## 设置 HTTP 响应

- 在 Midway 中你可以简单的使用 `return` 来返回数据。

```ts
import { Controller, Get, HttpCode } from "@midwayjs/core";

@Controller("/")
export class HomeController {
  @Inject()
  ctx: Context;

  @Get("/")
  async home() {
    // 返回字符串
    return "Hello Midwayjs!";

    // 返回 json
    return {
      a: 1,
      b: 2,
    };

    // 返回 html
    return "<html><h1>Hello</h1></html>";

    // 返回 stream
    return fs.createReadStream("./good.png");
  }
}
```

### 设置状态码

- 可以通过@HttpCode 设置返回的状态码

### 设置响应头

- Midway 提供 @SetHeader 装饰器或者通过 API 来简单的设置自定义响应头

### 设置路由重定向

- 如果需要简单的将某个路由重定向到另一个路由，可以使用 @Redirect 装饰器。 @Redirect 装饰器的参数为一个跳转的 URL，以及一个可选的状态码，默认跳转的状态码为 302

```ts
import { Controller, Get, Redirect } from "@midwayjs/core";

@Controller("/")
export class LoginController {
  @Get("/login_check")
  async check() {
    // TODO
  }

  @Get("/login")
  @Redirect("/login_check")
  async login() {
    // TODO
  }

  @Get("/login_another")
  @Redirect("/login_check", 302)
  async loginAnother() {
    // TODO
  }
}
```

## 全局路由前缀

- Controller 级别忽略

```ts
// 该 Controller 下所有路由都将忽略全局前缀
@Controller("/api", { ignoreGlobalPrefix: true })
export class HomeController {
  // ...
}
```

- 路由级别忽略

```ts
@Controller("/")
export class HomeController {
  // 该路由不会忽略
  @Get("/", {})
  async homeSet() {}

  // 该路由会忽略全局前缀
  @Get("/bbc", { ignoreGlobalPrefix: true })
  async homeSet2() {}
}
```

# Web 中间件

- Web 中间件是在控制器调用 之前 和 之后（部分）调用的函数。 中间件函数可以访问请求和响应对象。

## 编写中间件

- 创建一个 `src/middleware/report.middleware.ts` 。我们在这个 Web 中间件中打印了控制器（Controller）执行的时间。
- Midway 使用`@Middleware`装饰器标识中间件，完整的中间件示例代码如下。

```ts
import { Middleware, IMiddleware } from "@midwayjs/core";
import { NextFunction, Context } from "@midwayjs/koa";

@Middleware()
export class ReportMiddleware implements IMiddleware<Context, NextFunction> {
  resolve() {
    return async (ctx: Context, next: NextFunction) => {
      // 控制器前执行的逻辑
      const startTime = Date.now();
      // 执行下一个 Web 中间件，最后执行到控制器
      // 这里可以拿到下一个中间件或者控制器的返回值
      const result = await next();
      // 控制器之后执行的逻辑
      console.log(Date.now() - startTime);
      // 返回给上一个中间件的结果
      return result;
    };
  }

  static getName(): string {
    return "report";
  }
}
```

## 使用中间件

- 中间件分为全局中间件和路由中间件，全局中间件是所有的路由都会执行，而路由中间件是制定路由执行
- midway 为@Controller 和@GET、@POST 等提供了第二个参数，它是一个对象，而对象中存在 middleWare 属性，它是一个属性，指定了要执行的中间件

```ts
import { Controller, Get } from "@midwayjs/core";
import { ReportMiddleware } from "../middleware/report.middlweare";

@Controller("/")
export class HomeController {
  @Get("/", { middleware: [ReportMiddleware] })
  async home() {}
}
```

- 为全局所有路由添加全局中间件，在`src/configuration.ts`中 onReady 方法中添加中间件

## 特定路由匹配与忽略

- 可以在中间件中使用 ignore 和 match 方法，对中间件进行匹配

```ts
import { Middleware, IMiddleware } from "@midwayjs/core";
import { NextFunction, Context } from "@midwayjs/koa";

@Middleware()
export class ReportMiddleware implements IMiddleware<Context, NextFunction> {
  resolve() {
    return async (ctx: Context, next: NextFunction) => {
      // ...
    };
  }

  ignore(ctx: Context): boolean {
    // 下面的路由将忽略此中间件
    return (
      ctx.path === "/" || ctx.path === "/api/auth" || ctx.path === "/api/login"
    );
  }

  static getName(): string {
    return "report";
  }
}
// match同理
```

- 除此之外，match 和 ignore 还可以是普通字符串或者正则，以及他们的数组形式。

## 使用社区中间件

- 以 koa-static 为例
- 全局
  ```ts
    async onReady() {
    // add middleware
    this.app.useMiddleware(require('koa-static')(root, opts));
  }
  ```
- 也可以放在装饰器中
  ```ts
  const staticMiddleware = require("koa-static")(root, opts);
  // ...
  class HomeController {
    @Get("/controller", { middleware: [staticMiddleware] })
    async getMethod() {
      // ...
    }
  }
  ```

## 切换中间件顺序

- ```ts
  // src/configuration.ts
  import { App, Configuration } from "@midwayjs/core";
  import * as koa from "@midwayjs/koa";
  import { ReportMiddleware } from "./middleware/user.middleware";

  @Configuration({
    imports: [koa],
    // ...
  })
  export class MainConfiguration {
    @App()
    app: koa.Application;

    async onReady() {
      // 把中间件添加到最前面
      this.app.getMiddleware().insertFirst(ReportMiddleware);
      // 把中间件添加到最后面，等价于 useMiddleware
      this.app.getMiddleware().insertLast(ReportMiddleware);

      // 把中间件添加到名为 session 的中间件之后
      this.app.getMiddleware().insertAfter(ReportMiddleware, "session");
      // 把中间件添加到名为 session 的中间件之前
      this.app.getMiddleware().insertBefore(ReportMiddleware, "session");
    }
  }
  ```

```

```
