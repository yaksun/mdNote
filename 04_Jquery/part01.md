## 01.入口函数

```javascript
//四种写法 
$(document).ready(function () {

                })
jQuery(document).ready(function () {

                })
jQuery(function () {

                })
$(function(){
                   //编写代码
               })
```

## 02.交出控制权

```javascript
 // ya替代$
  var ya = $.noConflict()
        ya(function () {
            console.log(999);
   })

//ya替换jQuery
  var ya = jQuery.noConflict()
        ya(function () {
            console.log(999);
   })
```

## 03.方法 map和each
* each方法

  ```javascript
   // jquery中的each方法更强大,还可以遍历伪数组
          $(function () {
              // 原生forEach值在前，下标在后
              arr.forEach(function (val,index) {
                  console.log(val,index);
  
              })
  
              $.each(obj,function (index,val) {
                  // 下标在前，值在后
                  console.log(index,val)
              })
          })
  ```
  
* map方法

  ```javascript
    $(function () {
              // 原生map的回调函数中可以接收三个参数
              arr.map(function (val,index,array) {
                  console.log(val,index,array);
              })
  
                  // jquery的静态方法map照样可以循环伪数组
              $.map(obj,function (val,index) {
                  console.log(val,index);
              })
         })
  ```
  
* map和each的区别
  
  ```javascript
   $(function () {
              var res1 = $.each(obj, function (index, val) {
                  //回调函数中不能对原数组做处理
              })
              // 返回原伪数组
              console.log(res1);
  
              var res2 = $.map(obj, function (val, index) {
                  console.log(index);
                  return index+val
  
              });
              console.log(res2);
  
  
          })
  ```
## 04.其它常用静态方法  

```javascript
	 var str = '  4556 1233    '  	
	// 1.去除两端的空格，中间的不能去除
        var res2= $.trim(str)
        console.log(res2);

        var arr = [1,3,5,7,9]

        //2. 判断对象是否是一个数组
        var res1 =   $.isArray(arr)
        console.log(res1);

        var fn = function () {

        }
		//3.判断是否是一个函数
        var res3 = $.isFunction(fn)
```

## 05.关闭和开启ready

```javascript
 		//关闭ready,$或jQuery后面的函数不再执行
        $.holdReady(true)
        $(function () {
            alert('我是ready')
        })
        
        
        var oBtn = document.getElementsByTagName('button')[0]
    	oBtn.onclick = function () {
        // alert('heh')
        // 开启ready()
        	$.holdReady(false)
   		 }
 
```

## 06.内容选择器

```javascript
   $(function(){
            // 选中内容为空的元素
            var oDiv = $('div:empty')
            console.log(oDiv)

            // 选取内容不为空的元素
            var oDiv2 = $('div:parent')
            console.log(oDiv2)

                // 选取含有某段文本的元素
            var oDiv3 = $('div:contains("我是div")')
            console.log(oDiv3)

            // 选取含有某个标签的元素
            var oDiv4 = $('div:has("span")')
            console.log(oDiv4)
       })
```

## 07.获取位置和尺寸

```javascript
 $(function(){
                 var oBtns = document.getElementsByTagName('button')

                   oBtns[0].onclick = function () {
                       console.log($('#father').width());
                       // 相对于body
                       console.log($('#son').offset().left);
                        // 相对于定位的父级元素
                       console.log($('#son').position().left);
                   }

                   oBtns[1].onclick = function () {
                        // $('#father').width('500px')
                       $('#son').offset({
                           left:10
                       })
                   }
  })
```

## 08.绑定和解绑事件

```javascript
$('button').on('click',test2)

 $('button').mouseenter(test3)

$('button').mouseleave(test4)

// $('button').off()

$('button').off('mouseenter')

// 解除事件类型中的具体函数
$('button').off('click',test2)
```

## 09.阻止冒泡和默认事件

```javascript
$('.son').click(function (event) {
alert('我是son')
 //  // 阻止冒泡第一种方式
 // return false
// 第二种方式
event.stopPropagation()
 })

$('a').click(function (event) {
 alert('弹出框')
// 阻止默认行为的第一种方式
return false
// 第二种方式
event.preventDefault()
})
```

## 10.触发事件

* 自定义触发事件

  ```javascript
    $(function(){
                     // 自定义事件必须满足两个条件
                     // 1.事件只能用on语法
                     //2.必须用trigger触发
  
                     $('html').on('myClick',function () {
                         alert('触发自定义事件')
                     })
  
                     $('html').trigger('myClick')
   })
  ```
  
* 阻止触发事件默认行为和冒泡

  ```javascript
    		 // 页面加载后自动触发点击事件
             $('.father').trigger('click')
  
              // $('.son').trigger('click')
              // triggerHandler 阻止默认行为和冒泡
              $('.son').triggerHandler('click')
  ```
 ## 11.给触发事件加上命名空间 

```javascript
  $(function(){

            $('.son').on('click',function () {
                alert('触发自定义事件yiwen')
            })

            $('.son').on('click.yaksun',function () {
                alert('儿子触发自定义事件yaksun')
            })

            $('.father').on('click.yaksun',function () {
                alert('父亲触发自定义事件yaksun')
            })
            $('.father').on('click',function () {
                alert('父亲触发自定义事件1111')
            })

            // 当带上命名空间的时候 由于冒泡 还只触发父元素中带上命名空间的事件
            $('.son').trigger('click.yaksun')

            // 当不带命名空间的时候 ，
            // 子元素和父元素不论是否带有命名空间，
            // 所有同类型事件都会触发
            // $('.son').trigger('click')
        })
```

## 12.事件委托
* 基本结构

  ```javascript
  $(function(){
                     $('button').click(function () {
                         $('ul').append(' <li>我是新增的li</li>')
                     })
  
                     // 这种情况下,新增加的动态li就不会被绑定click事件
                     // $('ul>li').click(function () {
                     //     console.log($(this).html());
                     // })
  
                     // 使用事件委托
                     // 前提先找一个固定的元素
                     // 使用固定元素来代理动态元素的事件
                     $('ul').delegate('li','click',function () {
                         console.log($(this).html());
                     })
  
   })
  ```
  
* 小案例
  
  ```javascript
     $(function () {
              $('button').on('click',function () {
                  //$mask 接收对象
                  $mask=$('<div class="login">ewew<span>X</span></div>')
                  ('body').append($mask)
              })
  
              $('body').delegate('.login>span','click',function () {
                  $mask.remove()
              })
          })
  ```
  
  ## 13.选项卡jQuery
  
  ```javascript
   $(function(){
                     //编写代码
                     $('.small>li').on('mouseenter',function () {
                         // 1.给当前的标题添加样式
                         $(this).addClass('title')
  
                         // 2.把其他兄弟元素的样式删除
                         $(this).siblings().removeClass('title')
  
                         // 3.获取当前标题的下标
                         var oIndex = $(this).index()
                         console.log(oIndex);
  
                            // 4.给内容区域添加样式
                         $('.big>li').eq(oIndex).addClass('detail')
  
                         //给非当前的元素删除样式
                         $('.big>li').eq(oIndex).siblings().removeClass('detail')
                     })
        })
  ```
  
  ```javascript
   $('.father').hover(function () {
            console.log('鼠标移入或者移出');
   })
  
   // $('.father').hover(function () {
                     //     console.log('鼠标移入了');
                     // }, function () {
                     //     console.log('鼠标移出了');
   // })
  ```
  
  

