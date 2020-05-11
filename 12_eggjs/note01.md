## 01.初始化项目`https://eggjs.org/zh-cn/intro/quickstart.html`

* 安装脚手架 `cnpm i egg-init -g`

* 创建项目 `egg-init xx --type=simple`

* 安装插件`eggjs`,可以快速创建controller和service等

* 配置路由` router.get('/', controller.home.index);`,指向home控制器下的index方法(`MVC`模式)

* 引用创建的服务

  ```js
    // 获取服务也要使用异步函数
      let user =await this.service.user.getUserInfo() 
  // 服务之间可以相互引用,但不常用，主要是控制器中引用
  ```
  
* 控制器中可以直接访问配置

  ```js
  let url =  this.config.url 
  ```


## 02.模板配置和渲染

* 主要参考`npm`中的 `egg-view-ejs`的配置

  ```js
  //使用 egg-view-ejs 的配置
    config.view = {
      mapping: {
        '.html': 'ejs',
      },
    };
  ```

  ```js
    ejs : {
      enable: true,
      package: 'egg-view-ejs',
    }
  
  ```
  
* 模板渲染

  ```html
    <% for(var i=0;i<data.length;i++){ %>
          <li>
              <%= data[i]%>
          </li>
         <%}%>
  ```

  ```html
    <h2><%= user['name'] %></h2> --- <h2><%= user.age%></h2>
  ```
## 03.动态传参

* 接收query参数
  ```js
  async index() {
      this.ctx.body='管理员'
  
      let query = this.ctx.query 
      console.log(query)
    }
  ```
  
* 接收`params`参数

  ```js
  // 动态路由传参配置
   router.get('/admin/:name', controller.admin.test);
  ```
  
  ```js
    async test(){
          this.ctx.body = '动态路由'
      let params = this.ctx.params 
  
        console.log(params)
    }
  ```
  


## 04.扩展功能的使用

* 在`src`下新建extend文件夹,添加扩展文件,文件名固定只有5种`application 、 content 、helper 、request 、response `

* `helper`扩展定义好后,可以在模板中直接处理数据

  ` <h3><%= helper.foo(1588595569532) %></h3>`

  ```js
  // helper.js
  module.exports = {
      foo(param) {
        // this 是 helper 对象，在其中可以调用其他 helper 方法
        // this.ctx => context 对象
        // this.app => application 对象
  
        return new Date(param)
  
      },
    };
  
  //其它文件中调用
  async helperFn(){
        let temp =  this.ctx.helper.foo(1588595569532) 
        console.log(temp)
  
        this.ctx.body = '工具扩展'
  
    }
  ```
  

## 05.中间件的使用

* 配置`csrf`验证中间件(egg自带`csrf`)

  ```js
  // middleware下的auth.js
  module.exports = (options,app)=>{
      // 函数名不做要求
      return async function auth(ctx,next) {
         // 存入state中,模板可以直接使用
          ctx.state.csrf = ctx.csrf
  
          await next()
      }
  }
  // 配置中添加中间件
  
    // add your middleware config here
    config.middleware = ['printdate','forbideip','auth'];
  ```
  ```html
     <!-- 模板中直接使用配置好的csrf -->
  <form action="/add" method="post">
        <input type="hidden" name="_csrf" value="<%= csrf%>">
        姓名: <input type="text" name="username"><br>
        年龄: <input type="text" name="age"><br>
        <input type="submit" value="提交">
    </form>
  ```

* 配置禁用指定`ip`

  ```js
  // middleware 下的forbideip.js
  module.exports = options =>{
  
      return async function forbide(ctx,next) {
  
          // 获取当前客户端访问的ip
          let clientIp = ctx.request.ip 
  		// 只要有一个满足,使用some
          // options.ips接收传递的数据
         let temp =   options.ips.some( val=> {
             
              if(val == clientIp ){
                  return true
              }
          })
  
        
          if(temp){
              ctx.status = 304
              ctx.body = '你访问的ip被禁止'
  
          }else{
  
              await next()
          }
          
  
      } 
  
  }
  ```

  ```js
   // 配置文件中给中间件传递数据
    // 传入要禁止的ip
    config.forbideip = {
      ips:[
        '127.0.0.1',
        '192.168.0.102'
      ]
    }
  
  ```

* 配置访问后自动打印日期

  ```js
  //middleware下的 printdate.js
  module.exports= options =>{
  
      // 这里的函数名随便写
    return async function show(ctx,next) {
  
      console.log(new Date())
   
          await next()
      }
  
  }
  ```
* 配置文件中声明中间件就可以使用`  config.middleware = ['printdate','forbideip','auth'];`

## 06.`cookie`的使用

* 写入cookie

  ```js
  'use strict';
  
  const Controller = require('egg').Controller;
  
  class CookieController extends Controller {
    async setCookie() {
      
      this.ctx.cookies.set('username',"张三",{
          encrypt:true , // 是否加密，可以保存中文
          maxAge:1000*3600*24,
          signed:true // 防止前台修改cookie
      })
      this.ctx.body='cookie '
    }
  }
  
  module.exports = CookieController;
  
  ```

* 读取cookie

  ```js
    async getCookie(){
      this.ctx.body = 'cookie:'+this.ctx.cookies.get('username',{
  
        encrypt:true  // 保存的时候加密，获取的时候也要设置加密,加密可以获取中文
      })
    }
  
  ```

* 清空cookie

  直接赋值为空或者把过期时间清零

## 07.`session`的使用

* 写入session

  ```js
  'use strict';
  
  const Controller = require('egg').Controller;
  
  class SessionController extends Controller {
    async index() {
      this.ctx.session.userInfo = {
          name:'yaksun',
          age:20
      }
  
      this.ctx.body = 'session设置成功'
    }
  }
  
  module.exports = SessionController;
  
  ```

* 读取session

  ```js
   async getSession(){
      console.log(this.ctx.session.userInfo)
      this.ctx.body = '获取session'
    }
  ```
  
* cookie和session的关系

  * session保存在服务端，cookie保存在客服端
  * session的使用依赖于cookie,但cookie可以单独存在
  * 当服务端设置了session的时候,只要客户端发起访问,服务端就会给客户端返回cookie信息

  * 客户端就会写入关于session信息的cookie,当再次访问的时候,就会把保存的cookie传递到服务端
  * 服务端再根据接收到的cookie验证，是否开放客户端session的访问权

  

  

  