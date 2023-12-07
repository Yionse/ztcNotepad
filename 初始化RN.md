## 初始化

- 使用命令行

- ```node
  npx react-native@0.64.4 init RnForNavAndBasesss --version 0.64.4 
  ```

- 生成指定版本的Native，需要在native和Version后面同时指定，指定一边是无效的(与官网不一致，但经过试验，这种方法可行)

## 引入TS

- 详情参见官网
- https://www.reactnative.cn/docs/typescript

## 引入NativeBase

- 不要先创建Native项目，然后再引入NativeBase，而是直接使用nativeBase中的，可以直接一键生成RN和NativeBase，且附带了Ts
- npx react-native init MyApp --template @native-base/react-native-template-typescript
- 需要注意的是需要在运行时在react-native后面指定一下版本，最好为0.64.4,

## 引入Navigation

- 可根据官网步骤，但需要注意的是，官网默认安装的都是最新的包，可能会导致一些问题，但大多数都能通过降低或提升包的版本进行解决

## 报错解决

- > Task :app:checkDebugAarMetadata FAILED

  - 可能是版本问题，将小版本进行提升，出现这个问题时RN是0.64.0，但提升为0.64.4后，问题解决

- > Task :app:mergeDebugAssets FAILED

  - 可能是版本问题，将版本进行提升，出现时是0.64.1，提升为0.64.4后，问题解决

- Deprecated Gradle features were used in this build, making it incompatible with Gradle 7.0.
  Use '--warning-mode all' to show the individual deprecation warnings.
  See https://docs.gradle.org/6.7/userguide/command_line_interface.html#sec:command_line_warnings
                                                                                                                                                                                                              
  FAILURE: Build completed with 2 failures.                                                                                                                                                                   

  1: Task failed with an exception.

  - 将"react-native-screens": 的版本降低一下即可

- 