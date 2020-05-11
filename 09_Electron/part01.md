## 01.基础结构

* 使用`html`构建桌面应用

```js
var electron = require('electron')

var app = electron.app   //应用

var BrowserWindow = electron.BrowserWindow  //浏览器窗口

var mainWindow = null 

app.on('ready',()=>{
    mainWindow = new BrowserWindow({width:500,height:400})

    mainWindow.loadFile('./index.html')

    mainWindow.on('closed',()=>{
        mainWindow = null
    })

})
```

## 02.`require`配置

```js
    mainWindow = new BrowserWindow({
        width:800,
        height:600,
       // 除入口程序外使用require常用配置
        webPreferences:{nodeIntegration:true}
    })
```

## 03.渲染进程使用remote

* 在渲染进程中使用,主进程不需要

```js
var fs = require('fs') 
const btn = this.document.querySelector('#btn')
// 使用remote过渡
const BrowserWindow = require('electron').remote.BrowserWindow

window.onload = function(){
   
    btn.onclick = function(){
        newWin = new BrowserWindow({
            width:500,
            height:400
        })

        newWin.loadFile('./yellow.html') 

        newWin.on('closed',()=>{
            newWin = null 
        })
    }
}
```

## 04.自定义顶部菜单

```js
const {Menu,BrowserWindow} = require('electron') 

//菜单模板
var template =[
    {
        label:'新时代',
        submenu:[
            {
                label:'斗地主',
                //添加快捷键
                accelerator:'ctrl+n',
                // 定义菜单单击事件
                click:()=>{
                    // 主进程中不需要remote
                    newWin = new BrowserWindow({
                        width:500,
                        height:400,
                    })
                    

                    newWin.loadFile('./yellow.html')

                    newWin.on('closed',function(){
                        newWin = null
                    })
                }
            
            },
            {
                label:'仙桃癞子'
            }

        ]
    },
    {
        label:'大乐透',
        submenu:[
            {
                label:'幸运大转盘'
            },
            {
                label:'王者归来'
            }
        ]
    }
]
//创建自定义模板
var t= Menu.buildFromTemplate(template) 
//设置使用模板
Menu.setApplicationMenu(t)
```

```js
const {app,BrowserWindow} = require('electron') 

var mainWindow = null 

app.on('ready',()=>{
    mainWindow = new BrowserWindow({
        width:800,
        height:600,
    })
    // 自动打开调试面板
    mainWindow.webContents.openDevTools()
    
    //主程序中引入自定义菜单
    require('./menu/index')

    mainWindow.loadFile('./index.html') 

    mainWindow.on('closed',()=>{
        mainWindow = null
    })
})
```

## 05.自定义右键菜单

```js
const {Menu,remote} = require('electron') 

var template = [
    {
        label:'复制',
        accelerator:'ctrl+c',

    },
    {
        label:'粘贴',
        accelerator:'ctrl+v',

    }
]

// 在主页面中引入使用，属于渲染程序,要使用remote
var m = remote.Menu.buildFromTemplate(template) 

window.addEventListener('contextmenu',(e)=>{
    // 阻止默认事件
       e.preventDefault() 
		//固定语法
       m.popup({window:remote.getCurrentWindow()})

    })
```

```html
// 主页面中引入 
<script src="./render/rightMenu.js"></script>
```

## 06.通过浏览器打开链接

* 渲染进程

```js
    const {shell} = require('electron')

    var oA = this.document.querySelector('#myHref')
	//默认在应用中打开
    oA.onclick =function(e){
           //阻止默认跳转
    e.preventDefault()

    var newHref = oA.getAttribute('href') 
	// 在chrome中打开链接
    shell.openExternal(newHref)
    }

```

## 07.界面中嵌套网页

```js
const {app,BrowserWindow,BrowserView} = require('electron') 


app.on('ready',()=>{
    mainWindow = new BrowserWindow({
        width:800,
        height:600,
        webPreferences:{nodeIntegration:true}
    })

    mainWindow.loadFile('./index.html') 
    
	//嵌套页面核心模块BrowserView
    var view = new BrowserView() 
    mainWindow.setBrowserView(view)
    //设置坐标和宽高
    view.setBounds({x:0,y:100,width:600,height:500})
    // 加载界面
    view.webContents.loadURL('http://jspang.com')
    
    mainWindow.on('closed',()=>{
        mainWindow = null
    })
})
```

## 08.应用中打开子窗口

```js
// 原生js
window.onload = function(){
    var oBtn = this.document.querySelector('#btn')
    oBtn.onclick = function(e){
        e.preventDefault() 
        window.open('http://jspang.com')
    }
}
```

## 09.选择文件对话框

```js
const {dialog} = require('electron').remote 

var oBtn = document.querySelector('#btn')

oBtn.onclick = function(){
    dialog.showOpenDialog({
        // 改变标题
        title:'请选择你喜欢的小姐姐',
        //默认选择的文件路径
        defaultPath:'3.jpg',
         //过滤，只显示符合条件的文件   
        filters:[{name:'image',extensions:['jpg']}],
        // 修改打开按钮文本
        buttonLabel:'我要打十个'
    }).then(result=>{
        console.log(result)
		//存放图片的容器
        var oImage = document.querySelector('#firstImg')
        oImage.setAttribute('src',result.filePaths[0])

    }).catch(err=>{
        console.log(err)
    })
}

```

## 10.保存文件对话框

```js
var oSave = document.querySelector('#saveBtn') 

oSave.onclick = function(){
    dialog.showSaveDialog({
        title:'保存文件',
        buttonLabel:'保存'
    }).then(result=>{
        console.log(result)
        fs.writeFileSync(result.filePath,'我是被写入的内容')

    }).catch(err=>{
        console.log(err)
    })
}
```

## 11.消息对话框

```js
var oMessageBox = document.querySelector('#messageBox')

oMessageBox.onclick = function(){
    dialog.showMessageBox({
        //type 可以为 "none", "info", "error", "question" 或者 "warning"
        type:'warning',
        //标题
        title:'你今天去哪玩?',
        //按钮组
        buttons:['看樱花','看你妹']
    }).then(result=>{
        console.log(result)
        // 返回信息中包含按钮数组的对应下标

    }).catch(err=>{
        console.log(err)
    })
}
```

## 12.断网联网监控

```js
   //只适用于h5页面 测试没通过
   window.addEventListener('online',()=>{
    alert('我来了,尽情的蹂躏我吧')
	})

	window.addEventListener('offline',()=>{
    alert('今天咱俩不适合')
	})
```

## 13.通知提醒

```js
  var oBtn = document.querySelector('#notice')
        var option = {
            title:'我是通知提醒的标题',
            body:'我是通知提醒的内容'
        }
       // 底部消息通知框
     oBtn.onclick = function(){
         // 固定传参
            new window.Notification(option.title,option)
       }
```

## 14.注册全局快捷键

```js
const {app,BrowserWindow,globalShortcut} = require('electron')

app.on('ready',()=>{
    mainWindow = new BrowserWindow({
        width:800,
        height:600
    })

    // 注册快捷键  --》 快捷键组合按键和回调函数
    globalShortcut.register('ctrl+e',()=>{
        //按下快捷键执行的函数
        mainWindow.loadURL('http://jspang.com')
    })

    // 判断是否注册成功
    let isRegister = globalShortcut.isRegistered('ctrl+e') ? 'success' :'fail'
    console.log(isRegister)


    mainWindow.loadFile('./index.html')

    mainWindow.on('closed',()=>{
        mainWindow = null
    })
})

// 应用退出前的回调
app.on('will-quit',()=>{
    console.log('unRegister')
    // 注销指定的快捷键
    globalShortcut.unregister('ctrl+e')
    // 注销所有
    globalShortcut.unregisterAll()
})
```

## 15.剪贴板的功能和使用

```html
    <span>激活码:</span><span id="code">fwgwrgregergregre</span>
	<button id="copyBtn">复制激活码</button>
    <script>
        // 这个api在渲染进程中不需要remote,可以直接使用
        const {clipboard} = require('electron')
        const oCode = document.querySelector('#code')
        const oCopyBtn = document.querySelector('#copyBtn')

        oCopyBtn.onclick = function(){
           // 读取到剪贴板
            clipboard.writeText(oCode.innerHTML)
            alert('复制成功')
        }

    </script>
```

## 16.脚手架创建项目

* 还可以手动创建和从git创建

```
npm install electron-forge -g

electron-forge init my-new-app

cd my-new-app

npm start
```

## 17.拖放文件在指定容器打开

* 使用`h5`的拖放`api`

```js
const fs = require('fs')
window.onload = function(){
    const oDiv = document.querySelector('#content')
    // 阻止默认事件
    oDiv.ondragenter = oDiv.ondragover = oDiv.ondragleave = function(){
        return false
    }

    oDiv.ondrop = function(e){
        console.log(e.dataTransfer.files[0])

        let path = e.dataTransfer.files[0].path
        fs.readFile(path,(err,data)=>{
            oDiv.innerHTML = data
        })
    }
}
```

## 18.使用`loadURL`加载文件

```js
 // mainWindow.loadFile('./index.html')
    // 使用loadURL
   mainWindow.loadURL(path.join('file:',__dirname,'index.html'))
//   mainWindow.loadURL(`file://${__dirname}/index.html`);
```

## 19.`vue`集成`electron`创建项目

```
electron-vue跨平台桌面应用开发实战教程（一）——Hello World
https://www.toutiao.com/a6794615001446351373/
```

