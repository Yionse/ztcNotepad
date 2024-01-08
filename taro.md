## Taro简单介绍

- **Taro** 是一个开放式跨端跨框架解决方案，支持使用 React/Vue/Nerv 等框架来开发 [微信](https://mp.weixin.qq.com/) / [京东](https://mp.jd.com/?entrance=taro) / [百度](https://smartprogram.baidu.com/) / [支付宝](https://mini.open.alipay.com/) / [字节跳动](https://developer.open-douyin.com/) / [QQ](https://q.qq.com/) / [飞书](https://open.feishu.cn/document/uYjL24iN/ucDOzYjL3gzM24yN4MjN) 小程序 / H5 / RN 等应用。
- 综合来说就是一个跨端的解决方案，可以编写一套代码，通过taro可以编译成其他平台的小程序或者RN

# 安装

- Taro 项目基于 node，请确保已具备较新的 node 环境（>=16.20.0），推荐使用 node 版本管理工具 [nvm](https://github.com/creationix/nvm) 来管理 node，这样不仅可以很方便地切换 node 版本，而且全局安装时候也不用加 sudo 了。

- 安装脚手架

  - ```npm
    # 使用 npm 安装 CLI
    $ npm install -g @tarojs/cli
    
    # OR 使用 yarn 安装 CLI
    $ yarn global add @tarojs/cli
    
    # OR 安装了 cnpm，使用 cnpm 安装 CLI
    $ cnpm install -g @tarojs/cli
    ```

- 可以使用 `npm info` 查看 Taro 版本信息，在这里你可以看到当前最新版本

  - ```npm
    npm info @tarojs/cli
    ```

- 初始化项目

  - ```npm
    taro init FinalMarkDownForBLOGMiniApp
    ```

  - npm 5.2+ 也可在不全局安装的情况下使用 npx 创建模板项目：

    - ```npm
      npx @tarojs/cli init myApp
      ```

    - 很奇怪，开头说了node>16，但16版本自带的npm是8版本的，可以无需安装cli，而直接使用npx，由于npx也是默认安装的，所以说也可以不安装cli

# 编译运行

- 使用 Taro 的 `build` 命令可以把 Taro 代码编译成不同端的代码，然后在对应的开发工具中查看效果。
- Taro 编译分为 `dev` 和 `build` 模式：
  - **dev 模式（增加 --watch 参数）** 将会监听文件修改。
  - **build 模式（去掉 --watch 参数）** 将不会监听文件修改，并会对代码进行压缩打包。
  - dev 模式生成的文件较大，设置环境变量 `NODE_ENV` 为 `production` 可以开启压缩，方便预览，但编译速度会下降。
- 