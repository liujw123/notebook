# cordova & vue-cli 开发单页应用

**请确保你当前已经配置好cordova开发环境和vue开发环境**

### 1. 安装 `vue-cli`,cmd 命令行运行 `vue --version` 查看是否安装成功

```

    npm install -g @vue/cli

    vue --version
```

### 2. 到指定目录创建cordova项目

1. 创建
    ```
        $ cordova create 项目名称
    ```
2. 切入项目目录
    ```
        $ cd 项目名称
    ```
3. 添加平台
    ```
        $ cordova platform add 平台名(android) --save
    ```
4. 检查你当前平台设置状况:
    ```
        $ cordova platform ls
    ```
5. 检测你是否满足构建平台的要求:
    ```
        $ cordova requirements

        // 下面为示例
        Requirements check results for android:
        Java JDK: installed .
        Android SDK: installed
        Android target: installed android-19,android-21,android-22,android-23,Google Inc.:Google APIs:19,Google Inc.:Google APIs (x86 System Image):19,Google Inc.:Google APIs:23
        Gradle: installed

        Requirements check results for ios:
        Apple OS X: not installed
        Cordova tooling for iOS requires Apple OS X
        Error: Some of requirements check failed
    ```
6. 运行调试(先保证cordova项目没问题，再进行增加vue-cli)
    ```
        $ cordova run 平台名(android)
    ```




