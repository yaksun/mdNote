## 1.代码实现事件订阅和发布

* 基本结构

```javascript
//引入事件模块
var events = require('events')
//EventEmitter 
var myEvent = new events.EventEmitter
//订阅事件 
myEvent.on('show',function(info){
    console.log(`接收到的数据是${info}`)
})
//发布事件 触发事件
myEvent.emit('show','天下兴亡,匹夫有则')
```
* 小案例
```javascript
var events = require('events')
var util = require('util')

function Person(name){
    this.name = name 
}

// Person继承事件类
util.inherits(Person,events.EventEmitter)

let lili = new Person('lili')
let jiji = new Person('jiji')

let persons = [lili,jiji]

//给每个实例订阅事件
persons.forEach(function(p){
    p.on('say',function(msg){
        console.log(`${p.name}说:${msg}`)
    })
})
//发布事件
 lili.emit('say','你好')
 jiji.emit('say','你妹才好')
```



## 2.数据流和管道

* 写数据流

```javascript
var fs = require('fs')

// 创建一个写入的数据流 流的实例
var myStream = fs.createWriteStream('08_test.txt')
//写入内容
myStream.write('gogog')
//写入结束
myStream.end() 
//写入结束的回调
myStream.on('finish',function(){
    console.log('一切都结束了')
})

```
* 读数据流
```javascript
var fs = require('fs')

var myReadStream = fs.createReadStream('test.txt','utf8')

//单独设置编码格式
//myReadStream.setEncoding('utf8')

var data=''
myReadStream.on('data',function(chunk){
    console.log(chunk)
    data+=chunk
})


myReadStream.on('end',function(){
    console.log('数据收集完毕')
})
```
* 管道的使用
```javascript
var fs = require('fs')

var myReadStream = fs.createReadStream('test.txt')
var myWriteStream = fs.createWriteStream('new.txt')

myReadStream.setEncoding('utf8')

// 通过管道连接两个数据流,把读的流数据导向写的流数据 
myReadStream.pipe(myWriteStream)

```
* 管道在http中的使用
```javascript
var fs = require('fs')
var http = require('http')

//req,res 也是流的实例
var server = http.createServer(function(req,res){

    res.writeHead(200,{'Content-Type':'text/html'})
    //res.writeHead(200,{'Content-Type':'application/json'})

    var myStream = fs.createReadStream('home.html','utf8')

    myStream.pipe(res)

})

server.listen(8080,'127.0.0.1')

```

## 3.常用操作函数

* unlink 删除文件
* mkdir 创建文件夹
* rmdir 删除文件夹 
* readFile 文件读取
* writeFile 文件写入

## 4.接收post和get表单数据

* post第1种方式

```javascript
 const onFunc = function(req,res){
     	//需要用到的模块url 可以解析路径
        var pathname = url.parse( req.url).pathname
        let  data = ''
        req.on('error',function(err){
            console.error(err)
        }).on('data',function(chunk){
                data+=chunk
        }).on('end',function(){
            // 处理post请求
            if(req.method === 'POST'){
                // 需要用到的模块querystring 处理post数据
               let params = querystring.parse(data)
               console.log(params)
                 route(pathname,handler,res,params)
            }else{
                let params = url.parse(req.url,true).query
                route(pathname,handler,res,params)
            }

        })

```
* post第2种方式
```javascript
 const onFunc = function(req,res){
		  //需要用到的模块url 可以解析路径
        var pathname = url.parse( req.url).pathname

        let  data = []
        req.on('error',function(err){
            console.error(err)
        }).on('data',function(chunk){
                data.push(chunk)
        }).on('end',function(){
            // 处理post请求
            if(req.method === 'POST'){
                // 需要用到的模块querystring 处理post数据
				//当请求数据过大时,中断请求  
                if(data.length > 1e6){
                    // 取消请求
                    req.connection.destroy()
                }
                console.log(Buffer.concat(data))
                data = Buffer.concat(data).toString()
               let params = querystring.parse(data)
            
                 route(pathname,handler,res,params)

              
            }else{
                let params = url.parse(req.url,true).query
                route(pathname,handler,res,params)

            }

        })
```

## 5.模块化路由案例
* 包含4个文件,之一handle.js
```javascript
var fs = require('fs')

 const home = function(res){
    res.writeHead(200,{'Content-Type':'text/html'})
    fs.createReadStream('home.html','utf8').pipe(res)
}

 const jsonapi = function(res,params){
    res.writeHead(200,{'Content-Type':'application/json'})
    // fs.createReadStream('myjson.json').pipe(res)

    res.end(JSON.stringify(params))
}

 const news = function(res){
    res.writeHead(200,{'Content-Type':'text/html'})
    fs.createReadStream('new.html','utf8').pipe(res)
}


module.exports={
    home,
    jsonapi,
    news
}
```
* 之一route.js
```javascript
// 根据路由对应不同页面
var fs = require('fs')

function route(pathname,handler,res,params){
    //匹配到路径 
    if( typeof handler[pathname] == 'function'){
        handler[pathname](res,params)
    }else{
        // 找不到路径
        res.writeHead(200,{'Content-Type':'text/html'})
        fs.createReadStream('404.html','utf8').pipe(res)
    }
}

exports.route = route
```



## 6.使用express简化路由案例
* app.js
```javascript
var express = require('express')
var IndexRoute = require('./route/index')
var NewsRoute =  require('./route/news')

var app = express() 

app.get('/',IndexRoute)
app.get('/news',NewsRoute)


app.listen(8080)
```
* index.js(news.js类似)
```javascript
var express = require('express')

var IndexRoute = express.Router()

IndexRoute.get('/',function(req,res){
    res.send('首页')
})

module.exports=IndexRoute
```

## 7.使用express接收get数据

```javascript
var express = require('express')
var app = express()
// http://127.0.0.1:9527/home/yaksun
app.get('/home/:name',function(req,res){
    console.log(req.params.name) //yaksun
    send可以直接显示json数据
    res.send('xiixi')
})

app.listen(9527)
```

```javascript
var express = require('express')
var app = express() 

// http://127.0.0.1:8000/home?find=name
app.get('/home',function(req,res){
    console.log(req.query.find) //name
    res.end('hello')
})

app.listen(8000)
```

## 8.express中接收post数据

* 使用postman测试,用到的插件body-parser

```javascript
var express = require('express')
var bodyParser = require('body-parser')
// body-parser参照文档使用https://www.npmjs.com/package/body-parser

var app = express()

// create application/json parser
var jsonParser = bodyParser.json()
 
// create application/x-www-form-urlencoded parser
var urlencodedParser = bodyParser.urlencoded({ extended: false })

app.get('/getData',function(req,res){
    res.send('okkl')
})

// 接收客户端传递的普通表单数据(不含上传文件)
app.post('/getData',urlencodedParser,function(req,res){
    console.log(req.body)

    res.send('wiwiw')
})

// 接收客户端传递的json数据,postman选择json数据格式
app.post('/json',jsonParser,function(req,res){
   console.log(req.body)

    res.send('goog')
})


app.listen(8000)
```

## 9.express中上传文件

* 主要用到的插件multer

```javascript
var express = require('express')
var multer = require('multer')

var app = express()

var storage = multer.diskStorage({
    destination: function (req, file, cb) {
      cb(null, 'uploads/') //上传文件存放的文件夹
    },
    //文件名
    filename: function (req, file, cb) {
        console.log(file)
        //拼装文件名
      cb(null,  Date.now()+'_'+file.originalname)
    }
  })
   
  var upload = multer({ storage: storage })


  app.post('/profile', upload.single('avatar'), function (req, res, next) {
    // req.file is the `avatar` file
    // req.body will hold the text fields, if there were any

    res.send({code:0})
  })


  app.listen(3000)
```

## 10.ejs模板引擎

* app.js入口

```javascript
var express = require('express')

var app = express() 

// 改变默认模板文件夹为test
app.set('views','test')

//设置引擎类型
app.set("view engine","ejs")


app.get('/home/:name',function(req,res){
    let name = req.params.name
    console.log(name)
    //传递数据
    res.render('home',{person:name})

})

app.listen(3000)
```
* test下的模板文件home.ejs
```html
 <!-- 引入公共部分语法  -->
 <%- include('./header.ejs') -%>
    <h3>首页</h3>
   <h5><%= person %></h5>
```
* 循环语法的使用
```html
  <!-- 引入公共部分语法  -->
    <%- include('./header.ejs') -%>
    <h2>新闻页面</h2>
    <span> <%= data.name %></span>
    <ul>
      <% data.desc.forEach(function(item){ %>
      <li><%= item %></li>
      <% })  %>
   </ul>
```

## 11.express静态伺服语法

```javascript

app.use(express.static('public'))
//给每个路径添加公共路径home
app.use('home',express.static('public'))
```

## 12.中间件的使用

```javascript
var express = require('express')

var app = express()

//像水流一样,next()决定水流是否向下流动 
app.use('/',function(req,res,next){
    // res.end('yyy')
    next()
})

app.use('/home',function(req,res,next){
    res.send('xiix')
})


app.listen(3000)
```

## 13.moongse初次接触
* 核心文件todoController.js
```javascript
var bodyParser = require('body-parser')

var urlencodedParser = bodyParser.urlencoded({ extended: false });

var mongoose = require('mongoose');

// 连接moondb
mongoose.connect('mongodb://yaksun:123456@localhost:27017/node_todo');

// 监听开启
mongoose.connection.once('open',function(){

})

// 监听关闭
mongoose.connection.once('close',function(){

})

// 规定表中的字段
var todoSchema = new mongoose.Schema({
  item: String
});

// 得到表对应的模型 Todoxx任意
var Todo = mongoose.model('Todoxx', todoSchema);


//var data=[{item:'吃饭'},{item:'睡觉'}]


module.exports = function(app){

    app.get('/todo',function(req, res) {
      // 查询操作
        Todo.find({}, function(err, data) {
          if (err) throw err;
          console.log(data)
          res.render('todo', { todos: data });
        });
      })

    app.post('/todo',urlencodedParser, function(req, res) {
      //添加操作
        var itemOne = Todo(req.body).save(function(err, data) {
          if (err) throw err;
          res.json(data);
        });
      })


    app.delete('/todo/:item',function(req, res) {
        // data = data.filter(function(todo) {
        //   return todo.item.replace(/ /g, "-") !== req.params.item;
        // });
        Todo.find({item: req.params.item.replace(/-/g, " ")}).remove(function(err, data) {
          if (err) throw err;
          res.json(data);
        });
      
      })

}
```
* server.js
```javascript
var express = require('express') 
var app = express() 

// 控制路由
var todoController = require('./controllers/todoController')


app.set('view engine','ejs')

app.use(express.static('./public'))

todoController(app)


app.listen(3000,'127.0.0.1')


console.log('server is starting on port 3000')

```

