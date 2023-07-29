# umi3

- 首先新建一个项目文件夹，在这个文件夹下，通过命令

  - ```git
    yarn create @umijs/umi-app
    ```

  - 即可创建一个项目，然后通过yarn 安装所需依赖，然后通过yarn start启动项目

## config配置

- 一般会在根目录下，新建一个config文件件，里面用于放置配置文件
- routes--路由相关
- proxy--代理相关
- theme--主题
- 当同时存在.umi文件和config文件夹时，会以.umi文件为准，所以一般会删除掉

## antd引入

- 内置了两个版本，PC版本和Mobile版本
- 也可以手动安装版本

## public文件夹

- 存放公共的临时文件
- 在assets中的文件通常是直接写在文件中

## 使用less

- 自带了less解析
- 使用scss需要另外安装
- 全局变量 可以定义在global.less文件中，那么可以直接使用，且不需要导入和模块化，可以定义一些全局使用的css

## 默认配置

- 默认的脚手架内置了 @umijs/preset-react，包含布局、权限、国际化、dva、简易数据流等常用功能。比如想要 ant-design-pro 的布局，编辑 `.umirc.ts` 配置 `layout: {}`，并且需要安装 `@ant-design/pro-layout`。

- ```ts
  import { defineConfig } from 'umi';
  
  export default defineConfig({
  + layout: {},
    routes: [
      { path: '/', component: '@/pages/index' },
    ],
  });
  ```

## 部署发布

- yarn build
- 构建以后，默认会生成一个dist目录，所有的文件放在dist目录下
- public目录下所有的文件在Build时，都会被copy到dist目录下

## 路由配置

- 一般会将配置文件从.umi中抽离出来，单独创建一个config文件夹，然后在此文件夹下，新建两个文件config.js和route.js，然后在config.js中进行配置

- routes项是一个数组，可以抽离出去，然后进行封装

  - ```js
    export default [{
        path : //	路径
        component : //	对应的组件
        exact : //	是否严格匹配
        routes: //	配置二级路由
        redirect: //	路由重定向，当访问该path时，指向另外一个路径，常用于根目录/，指向首页
        wrappers : //	指向一个组件，在这个组件中进行路由验证
        title : //	配置当前页面的标题
    }];
```
    
  
- 约定式路由无需在routes中配置，只需要按照约定，将.tsx文件放在指定位置就可以直接通过地址访问

- 重定向组件，redirect组件，可以直接重定向到某个页面

- 可以在pages中可以配置一个404.tsx，当路由不存在时，则会显示该界面

- 配置二级路由时，新建一个文件夹，文件夹中新建一个文件_layout.tsx在这里书写当前路由的结构

- 下面的子结构则会变成_layout组件的一个prop，然后在_layout中显示即可

## 路由验证

- 在route中配置一个选项wrappers，它指向一个组件
- 当给一个路由配置了wrappers时，再此访问这个路由时，则会跳转到wrappers所指向的路由，然后这里面可以进行操作，是否展示原本的组件
- 这样就形成了路由验证

## 页面跳转

- 可以将umi中的History解构出来使用，也可以使用useHistory来使用
- 可以使用push和goBack方法进行路由跳转
- Link组件，可以有一个to属性，进行路由跳转，但只能跳转内部路由，外部链接使用a
- 