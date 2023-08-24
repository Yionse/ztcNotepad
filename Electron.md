# Eletron创建项目流程

- 新建一个文件夹，将该文件夹初始化

  - ```js
    npm init
    ```

- 这样文件夹就有了package.json文件

- 然后安装一下依赖，可使用yarn和npm都可以

- 然后在当前项目下，安装依赖，首先安装默认需要的包

  - ```git
    yarn 或 npm i
    ```

- 然后安装electron，这里可以安装位开发依赖

  - ```git
    yarn add electron --dev	或	npm i -d electron
    ```

- 然后在package.json中的script中添加一行

  - ```json
    "start": "electron ."
    ```

- 然后运行

  - ```git
    yarn start
    ```

# Mainjs

- 引入electron，导入两个 模块

  - ```js
    const { app, createWindow } = require('electron');
    ```

  - 由于electron对es语法的import支持还不完善，这里就是用commonjs

- 将可复用函数写入实例化窗口

  - ```js
    const createWindow = () => {
      const win = new BrowserWindow({
        width: 800,
        height: 600
      })
    
      win.loadFile('index.html')
    }
    ```

- 在应用准备就绪时，调用函数

  - ```js
    app.whenReady().then(() => {
      createWindow()
    })
    ```

- 此时运行start命令即可打开一个包含网页的窗口

