# 1.图片map映射

```
<!-- usemap->map.name-> area-> 设置 -->
<img src="yingshe.jpg" alt="" usemap="#face">
<map name="face">   <!-- shape 形状-->   <!-- coords 关键点坐标-->   
<area shape="rect" coords="43,140,140,242" href="#" alt="" onclick="show()">
</map>`
```



# 2.线包字效果

```
<fieldset style="width: 200px">  

 <legend><span style="color: blue; ">受审状态</span></legend>  

 <input type="radio" name="status" value="yishen">已审核   

<input type="radio" name="status" value="weishen">未审核

</fieldset>
```

# 3.iframe语法

```
<a href="http://www.taobao.com" target="myIframe">跳转到淘宝</a> <br>
	<!--src 域名要完整地址 -->
	<iframe src="http://www.baidu.com" frameborder="0" name="myIframe" width="500px" 		height="400px">
</iframe>
```

