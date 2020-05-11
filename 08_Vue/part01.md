## 1.样式class和style语法

* style语法

  ```html
   <div id="div1" :style="aClass"></div>
      <div id="div2" :style={width:a,height:b,background:c}></div>
      <script>
          new Vue({
              el:'#div1',
              data:{
                  aClass:{
                      width:'200px',
                      height:'200px',
                      backgroundColor:'red'
                  }
              }
          })
  
          new Vue({
              el:'#div2',
              data:{
                  a:'200px',
                  b:'200px',
                  c:'blue'
              }
          })
      </script>
  ```

  

* class语法

  ```html
   <div id="div1" :class={aClass:isShow,bClass:isToggle}></div>
      <div id="div2" :class="isShow"></div>
      <div id="div3" :class="[a,b]"></div>
      <div id="div4" :class="hehe">
          <button @click="handleClick">切换</button>
      </div>
      <script>
          new Vue({
              el:'#div1',
              data:{
                  isShow:true,
                  isToggle:true
              }
          })
          new Vue({
              el:'#div2',
              data:{
                 isShow:{
                     aClass: true
                 }
              }
          })
          new Vue({
              el:'#div3',
              data:{
                  a:'aClass',
                  b:'bClass'
              }
          })
          new Vue({
              el:'#div4',
              data:{
                  isShow:true
              },
              methods:{
                  handleClick(){
                     this.isShow = !this.isShow
                  }
              },
             // 利用计算属性定义样式
              computed:{
                  hehe:function () {
                      if(this.isShow){
                          return {
                              aClass:true
                          }
                      }else{
                          return {
                              aClass:false
                          }
                      }
                  }
              }
          })
      </script>
  ```

  

## 2.列表搜索和排序

练习

## 3.表单提交

练习

## 4.动画的使用

隐藏和显示切换中的动画

```html
 <div id="div1" >
        <button @click="handleClick">你个死变态</button>
        <br>
        <div @mouseenter="handleEnter">
            <transition name="XXX" >
                <input type="text" v-show="ok" value="我会打死你的哟">
            </transition>
        </div>
    </div>
    <script>
        new Vue({
            el:'#div1',
            data:{
                ok:true
            },
            methods:{
                handleClick(){
                    this.ok = !this.ok
                },
                handleEnter(){
                    console.log(9999);
                }
            }
        })
    </script>
```

```css
<style>
        .XXX-enter-active , .XXX-leave-active{
            transition:all 2s;
            font-size:15px;
        }

        .XXX-enter , .XXX-leave-to{
           font-size: 25px;
        }
    </style>
```



## 5.自定义过滤器

```html
 <div id="div1">
        {{time}} <br>
     //当数据获取到的时候再显示
        <p v-cloak> {{ time | formateTime }}</p>

    </div>
    <div id="div2">
        {{time | formateTime2('HH:MM:ss')}}
    </div>
    <script>
        Vue.filter('formateTime2',function (val,template='YYYY MM DD:HH MM ss') {
            return moment(val).format(template)
        })
        new Vue({
            el:'#div1',
            data:{
                time:new Date(),
                timer:null
            },
            mounted(){
                let _this = this
               this.timer = setInterval(function () {
                    _this.time = new Date()
                },1000)
            },
            destroyed(){
                clearInterval(this.timer)
            },
            filters:{
                formateTime(value,template='YYYY'){
                    return moment(value).format(template)
                }
            }
        })

        new Vue({
            el:'#div2',
            data:{
                time:new Date()
            }
        })
    </script>
```

```css
   [v-cloak]{
            display: none;
        }
```



## 6.自定义指令

```html
 <div id="div1">
        <p v-upper="info"></p>
    </div>
    <div id="div2">
        <p v-lower="info"></p>
    </div>
    <script>
        //全局指令
        Vue.directive('lower', function(el,binding){
            //固定参数和语法
                el.innerHTML = binding.value.toLowerCase()

        })
        new Vue({
            el:'#div1',
            data:{
                info:'i am you father'
            },
            //局部指令
            directives:{
                upper:function (el,binding) {
                      //固定参数和语法
                    el.innerHTML = binding.value.toUpperCase()
                }
            }
        })

        new Vue({
            el:'#div2',
            data:{
                info:'I AM YOU FATHER'
            }
        })
    </script>
```



## 7.自定义插件

* 插件文件

  ```js
  (
      function () {
          const MyPlugin={}
  
          //1.安装插件
          MyPlugin.install = function(Vue,options){
              Vue.myGlobalMethodhh=function () {
                  console.log('全局方法',options)
              }
          }
  
          //2.插件中自定义指令
          Vue.directive('hehe',function (el,binding) {
              el.innerHTML = binding.value.toUpperCase()
          })
  
          // 3.注入组件
          Vue.mixin({
              created:function () {
                  console.log('混入注册')
              }
          })
  
  
          //4.实例方法
          Vue.prototype.$xixi=function(options){
              console.log('实例方法',options)
          }
  
  
          window.MyPlugin = MyPlugin
      }
  )()
  
  ```

  

* 使用文件

  ```html
  <div id="div1">
      <p v-hehe="info"></p>
  </div>
  <script>
      Vue.use(MyPlugin)
      Vue.myGlobalMethodhh()
      let em =  new Vue({
          el:'#div1',
          data:{
              info:'i love u'
          }
      })
  
      em.$xixi()
  </script>
  ```

  

## 8.伪数组成真

```js
   var aLi = document.getElementsByTagName('li')
    console.log(aLi.length);
    console.log(aLi instanceof Array)
    console.log(aLi.forEach);
	
// var newArr = Array.prototype.slice.call(aLi)
//关键字
    var newArr =  [].slice.call(aLi)
    console.log(newArr.length);
    console.log(newArr instanceof Array)
    console.log(newArr.forEach)

```



##  9.节点属性

```html
 <div id="div1" name="username">
        好好学习,天天向上
        <ul>
            <li>3333</li>
        </ul>
    </div>
    <script>
        var oDiv  = document.getElementById('div1')
        console.log(oDiv.nodeType);

        var attrObj = oDiv.getAttributeNode('name')
        console.log(attrObj,attrObj.nodeType)

        var txtObj = oDiv.firstChild
        console.log(txtObj,txtObj.nodeType)
    </script>
```



## 10.定义属性

```js
 var obj={
        firstname:'A',
        lastName:'B'
    }

    Object.defineProperty(obj,'fullname',{
        //关键字
        get(){
            return obj.firstname+'-'+obj.lastName
        },
        set(val){
          var  res =  val.split('-')
            obj.firstname = res[0]
            obj.lastName = res[1]
        }
    })

    console.log(obj.fullname);
    obj.fullname='E-F'
    console.log(obj.firstname,obj.lastName);

```

```js
var obj={
        firstName:'A',
        lastName:'B'
    }

    Object.defineProperty(obj,'fullName',{
        //关键字
        value:'E-D',
        writable:true, //是否可编辑
        enumerable:false,  //可枚举的
        configurable:false //可重新定义的
    })

    console.log(obj.fullName);

    obj.fullName='G-J'
    console.log(obj.fullName);

    // 默认可枚举
    console.log(Object.keys(obj));

    console.log(Object.hasOwnProperty('haha'));


    Object.defineProperty(obj,'fullName',{
       enumerable:true
    })

    console.log(Object.keys(obj));
```



##	11.文档碎片

```js
var oUl = document.getElementsByTagName('ul')[0]
//关键字
    var oFrag = document.createDocumentFragment()

    while(oUl.children[0]){
        oFrag.appendChild(oUl.children[0])
    }

    [].slice.call(oFrag.children).forEach(function (item) {
        item.innerHTML='苦海无边,回头是岸'
    })

    oUl.appendChild(oFrag)
```







