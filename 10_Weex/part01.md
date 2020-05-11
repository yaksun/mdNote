## 01.开发环境配置

* 安装调试工具(脚手架) 

  ```
  cnpm i weex-toolkit -g
  // 测试是否安装成功
  weex -v
  ```
  
* 全局安装`webpack`

  ```
  npm i webpack -g
  ```

* 创建项目

  ```
  weex create XXX
  ```

* 根目录下安装依赖

  ```
  npm install
  ```

* 生成android包

  ```
  weex platform add android
  //不使用android studio开发时使用(使用夜神模拟器)
  weex run android
  ```

* 安装android studio(中文社区安装包)

   记录下`sdk`路径`C:\Users\Administrator\AppData\Local\Android\Sdk`  
* 配置android studio下载`23.0.2SDK`

   `android studio -> SDK manager->SDK Tools->Show Package Detail`
* 配置环境变量

  * 新建ANDROID_HOME: `C:\Users\Administrator\AppData\Local\Android\Sdk`
  
  * 系统path中新增(分昊隔开)
  
    ```
    C:\Users\Administrator\AppData\Local\Android\Sdk\platform-tools
    C:\Users\Administrator\AppData\Local\Android\Sdk\tools
    ```
* 使用android studio打开项目为新生成的android包





