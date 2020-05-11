## 01.ts编译环境

* 全局安装typescript

  `cnpm i typescript -g`

* 初始化项目，生成ts配置文件tsconfig.json

  `tsc --init`

* 修改转换成js的路径配置

  ```json
   // 配置转换成的js保存的路径
     "outDir": "./js", 
  ```
* vs编译器中设置监听
  
  * 终端 --》 运行任务 --》监视tsconfig

## 02.ts数据类型

* 元组类型,多种数据的组合

  ```typescript
  // 一一对应
  let abc:[number,string] = [123,'abc']
  console.log(abc)
  ```

* 枚举类型,类似于对象

  ```typescript
  enum myColor{
      hhe=52,
      gge='aetgerg'
  }
  
  let jiji:myColor = myColor.gge
  
  console.log(jiji)
  ```

* void类型,函数没有返回值的时候使用

  ```typescript
  function show1():void{
      console.log('hhaha')
  }
  
  show1()
  ```

* null和undefined

  ```typescript
  let kiki:undefined 
  // 声明成undefined,就不需要赋值,默认就是undefined
  console.log(kiki)
  
  let cici:null 
  // 声明成null,还要赋值成null,否则报错
  cici=null
  console.log(cici)
  
  // 声明多种类型
  let cici2:undefined | null |  number | string
  cici2=1233
  console.log(cici2)
  ```

* never永远不会出现的类型

  ```typescript
  //不会报错
  let cici3:never=(
      ()=>{
          //报错的语法
          throw new Error('error')
      }
  )()
  
  console.log(cici3)
  ```

* any类型,任意类型

  * 定义数组的第一种语法

    ```typescript
    let cici4:any[]=['agwg',123,{},['fewfew']]
    console.log(cici4)
    
    let cici5:number[]=[52,21]
    ```

  * 定义数组的第二种语法

    ```typescript
    let c:Array<any> = [12,15,'abd']
    console.log(c)
    
    let d:Array<number>=[95,56,45]
    ```
## 03.ts函数

* 定义参数

  ```typescript
  function show1(name:string,age:number):void{
      console.log(`${name}----${age}`)
  }
  ```
  
* 参数可选

  ```typescript
  function show2(name:string,age?:number):void{
      if(age){
          console.log(`${name}---${age}`)
      }else{
          console.log(`${name}--年龄保密`)
      }
  }
  ```
  
* 参数默认值
  
  ```typescript
  function show3(name:string,age:number=18):void{
      console.log(`${name}----${age}`)
  }
  
  show3('cici')
  show3('didi',78)
  ```
  
 * ...操作符 

   ```typescript
   // 展开运算符...
   function get1(...res:number[]):void{
       res.forEach((item)=>{
           console.log(item)
       })
   }
   
   get1(95,95,23,12)
   ```

   ```typescript
   // 剩余运算符...
   function get2(a:number,b:number,...res:Array<number>):void{
     res.map(item=>{
           item=item*2
       })
   
       console.log(res)
   }
   
   get2(1,2,4,5,6)
   ```
   
* ts函数重载

  ```typescript
  // 函数重载,根据参数类型不同执行不同
  function get3(params:any):any{
      if(typeof params == 'string'){
          console.log(`你认识${params}吗?`)
      }else{
          console.log(`你能打${params}个?`)
      }
  }
  
  get3('kiiki')
  get3(20)
  ```
## 04.ts类语法

* 基本结构

  ```typescript
  class Animal{
      name:string
      constructor(name:string){
          this.name = name
      }
  
      run():void{
          console.log(`${this.name}在奔跑`)
      }
  
  }
  
  
  let a1 = new Animal('小猫')
  a1.run()
  
  // 继承Animal
  class Cat extends Animal{
      constructor(name:string){
          super(name)
      }
  
      work(){
          console.log(`${this.name}在抓老鼠`)
      }
  }
  
  let c1 = new Cat('阿花')
  c1.work()
  ```
  
* ts静态属性和方法

  ```typescript
  class Person{
      
      static  age:number=20
      
      // 内部访问静态属性
      static say():number{
         return Person.age
      }
  
     
  }
  
  // 外部访问静态属性
  console.log(Person.age)
  console.log(Person.say())
  ```
  
* ts抽象类
  
  ```typescript
  abstract class Animal{
      //抽象类除了有抽象方法外,还可以定义其它的方法
      abstract eat():void
  }
  
  //子类继承父类,必须实现父类的抽象方法
  class Cat extends Animal{
      eat(){
          console.log(`小猫吃鱼`)
      }
  }
  
  
  let c = new Cat() 
  
  c.eat()
  ```
## 05.ts接口 

* 参数接口,把接口当做参数传递

  ```typescript
  // 参数要匹配接口的字段
  interface Params{
      username:string,
      age?:number
  }
  
  function show(params:Params):void{
      if(params.age){
          console.log(`${params.username}---年龄:${params.age}`)
      }else{
          console.log(`${params.username}---年龄保密`)
      }
  }
  
  show({username:'yakusn'})
  show({username:'kkik',age:50})
  ```

  ```typescript
  // 定义一个约束ajax参数的接口
      interface Config{
      type:string
      url:string
      data:string
      dataType:string
      }
  
  function ajax(config:Config):any{
      var xhr = new XMLHttpRequest()
  
      xhr.open(config.type,config.url,true)
      
      xhr.send(config.data)
      
      xhr.onreadystatechange = function(){
          if(xhr.readyState== 4 && xhr.status==200 ){
              console.log('success')
          }
      }
  }
  
  ajax({
      type:'post',
      url:'http://www.baidu.com',
      data:'hello,everyone',
      dataType:'json'
      
  })
  
  ```
  
* 函数接口

  ```typescript
  interface Params{
      (username:string,age:number):void
  }
  
  //约束函数 包括参数和返回值
  const md5:Params = function(username,age){
      console.log(`${username}---${age}`)
  }
  
  md5('goog',18)
  let showT:Params = function(){
      console.log('什么都没有传入')
  }
  
  showT('HEHHE',123)
  ```
  
* 索引接口

  ```typescript
  //匹配数组的接口
  interface NumberIndes{
      [index:number]:number
  }
  
  let arr:NumberIndes=[51,85,5522]
  console.log(arr)
  
  //匹配对象的接口
  interface ObjIndex{
      [index:string]:string
  }
  
  let obj:ObjIndex={"name":"yaksun","age":"20"}
  
  console.log(obj)
  ```
  
* 把类作为接口
  
  ```typescript
  interface Animal{
      name:string
      work(name:string):void
  }
  
  // 实现接口必须实现接口的方法
  class Person implements Animal{
      name:string 
      constructor(name:string){
          this.name = name 
      }
  
      work(){
          console.log(`${this.name}今年18岁了`)
      }
  }
  
  let p1 = new Person('小明')
  p1.work()
  ```
  
  * 类接口可以被继承
  * 类实现接口,必须实现接口所有的方法,包括继承过来的

## 06.ts泛型

* 泛型函数

  ```typescript
  //返回类型和参数类型一致
  function run<T>(name:T):T{
      return name
  }
  
  console.log(run<string>('小明'))
  console.log(run<number>(18))
  ```
  
* 泛型类
  
  ```typescript
  class Count<T>{
       list:T[]=[]
  
       add(value:T){
          this.list.push(value)
       }
  
      //  找出数组中的最小值
       min(){
           let l = this.list.length 
           if(l>0){
              //  假设最小的值是第一个
               let minRes=this.list[0]
              for(let i=0;i<l;i++){
                  if(this.list[i]<minRes){
                      minRes = this.list[i]
                  }
              }
  
              return minRes
           }else{
               alert('没有可比的内容')
           }
       }
  }
  
  //简化代码,提高代码复用性
  //只需要在实例化的时候定义数据类型就可以了
  let c = new Count<number>()
  c.add(98)
  c.add(13)
  c.add(200)
  console.log(c.min())
  
  let s = new Count<string>()
  s.add('f')
  s.add('b')
  s.add('c')
  console.log(s.min())
  ```
  
* 泛型接口

  ```typescript
  // 泛型函数接口 
  
  // 第1种写法
  interface Person{
      <T>(name:T):T
  }
  
  
  let run:Person = function<T>(name:T):T{
      return name
  }
  
  console.log(run<number>(18))
  console.log(run<string>('小黑'))
  
  
  // 第2种写法
  
  interface Animal<T>{
      (name:T):T
  }
  
  function show<T>(name:T):T{
      return name 
  }
  
  let show1:Animal<number>=show
  console.log(show1(12))
  
  let show2:Animal<string>=show 
  console.log(show2('小明'))
  ```
## 07.泛型进阶用法

* 基本结构,类作为参数传递

  ```typescript
  class User{
      name:string | undefined 
      age:number | undefined
  //以构造函数模拟数据
      constructor(params:{
          name:string | undefined ,
          age:number | undefined
      }){
          this.name = params.name 
          this.age = params.age 
      }
  }
  //普通方式模拟数据
  class Goods{
      name:string | undefined 
      price:number | undefined
  }
  
  //把传递的类定义成泛型
  class Mysql<T>{
      //模拟查询
      get(id: number): any[] {
          return [
              {id:1,name:'小明',password:'123456'},
              {id:2,name:'小花',password:'456789'}
          ]
      }   
  
      // 模拟添加
      add(info:T):boolean{
          console.log(info)
          return true
      }
      // 模拟修改
      update(info:T,id:number):boolean{
          console.log(info,id)
          return true 
      }
      //模拟删除
        delete(id: number): boolean {  
          console.log(id)
          return true 
      }
  
  }
  //以构造函数模拟数据
  let u1 = new User({name:'yaksun',age:18})
  
  //普通方式模拟数据
  let g1 = new Goods()
  g1.name = '张三'
  g1.age = 22
  
  let m1 = new Mysql<User>()
  
  m1.add(u1)
  m1.update(u1,18)
  
  
  let m2 = new Mysql<Goods>()
  m2.add(g1)
  m2.update(g1,12)
  ```
  
* 模块化项目

  ```typescript
  interface DBI<T>{
      // 查询功能 参数id 结果 任意类型数据组成的数组
      get(id:number):any[]
  
      // 添加功能 参数 任意类型数据 结果 布尔值
      add(info:T):boolean
  
      // 更新操作 参数 任意类型数据和id  结果 布尔值
      update(info:T,id:number):boolean
  
      // 删除操作 参数 id 返回 布尔值
      delete(id:number):boolean
  
  }
  
  export class MySql<T> implements DBI<T>{
      // 代码按提示生成
      get(id: number): any[] {
          throw new Error("Method not implemented.");
      }   
       add(info: T): boolean {
          throw new Error("Method not implemented.");
      }
      update(info: T, id: number): boolean {
          throw new Error("Method not implemented.");
      }
      delete(id: number): boolean {
          throw new Error("Method not implemented.");
      }
  
  
  }
  
  export class MoongDb<T> implements DBI<T>{
        // 代码按提示生成
      get(id: number): any[] {
          throw new Error("Method not implemented.");
      }    add(info: T): boolean {
          throw new Error("Method not implemented.");
      }
      update(info: T, id: number): boolean {
          throw new Error("Method not implemented.");
      }
      delete(id: number): boolean {
          throw new Error("Method not implemented.");
      }
  
  
  }
  ```

  ```typescript
  
          import {User} from '../modle/user'
          import {MysqlDb} from '../modules/db2'
  
          let u1 = new User({
              name:'小茶',
              password:'789562'
          })
  
  
          let m1 = new MysqlDb<User>()
  
          m1.add(u1)
  ```
## 08.命名空间(项目模块化)

* namespace.ts

  ```typescript
  /*
  
       一个命名空间就是一个单独的模块
       可以避免类和函数重名带来的错误
       使用命名空间的内容先需要导出
  
  */ 
  
  // 命名空间需要导出 ，里面的每个对象也要导出
  export namespace A{
     export let name:string='小黑'
     export function run(){
          console.log('我跑全班第一')
      }
  
      export class Person{
          show(){
              console.log('is my showtime')
          }
      }
  }
  
  export namespace B{
      export let name='小兰'
      export  function run (){
          console.log('我跑全班第一')
      }
  
      export  class Person{
          show(){
              console.log('is my showtime')
          }
      }
  }
  ```
  
* index.ts

  ```typescript
  import {A,B} from '../modules/namespace'
  
  console.log(A.name)
  
  A.run()
  
  let a1 = new A.Person()
  a1.show()
  
  
  console.log(B.name)
  
  B.run()
  
  let b1 = new A.Person()
  b1.show()
  ```
## 09.装饰器

* l类装饰器基本结构

  ```typescript
  // 装饰器 本质是函数
  // 无参数装饰器
  function mylog(target:any){
      // target 就是要装饰的Person类
      console.log(target)
  }
  
  // 执行装饰器函数,不需要实例化所修饰的类
  @mylog
  class Person{
      run(){
          console.log('嘻嘻')
      }
  }
  
  let p1 = new Person()
  p1.run()
  
  
  // 传递参数装饰器
  function yourlog(params:any){
      return function(target:any){
          console.log(params,target)
      }
  }
  
  // 执行装饰器函数,不需要实例化所修饰的类
  @yourlog('我是传递的数据')
  class Animal{
      work(){
          console.log('每个人都要工作')
      }
  }
  
  ```
  
* 类装饰器继承类
  
  ```typescript
  //logClass继承Person
  function logClass(target:any){
      target.prototype.name = '小明'
      //重写父类方法
      target.prototype.run = function(){
          console.log(this.name)
      }
  }
  
  @logClass
  class Person{
  
      run(){
          console.log('替朕更衣')
      }
  }
  
  
  let p1= new Person()
  p1.run()
  ```
  
* 类的属性装饰器  
  
  ```typescript
  // 无参数的属性装饰器
  function logAttr(target:any,attr:any){
      target[attr] = 'hehe'
  }
  
  class Person{
      // 定义了Person上__proto__的name
      // 放在谁的上面就修饰谁
      @logAttr
      name:string =  '小明'
      age:number=18
  }
  
  
  let p1 = new Person() 
  console.log(p1)
  
  
  
  // 传递参数的属性装饰器
  function logAttr2(params:any){
      return function(target:any,attr:any){
          console.log(params)
          target[attr]=params
      }
  }
  
  class Animal{
      // 定义了Animal上__proto__的name
      @logAttr2('这是一个美丽的传说')
      name:string = '小花'
  }
  
  let a1 = new Animal() 
  console.log(a1)
  ```
  
* 类的方法装饰器
  
  ```typescript
  // 无参数方法装饰器
  function logFn(target:any,methods:any,desc:any){
      console.log(target)  //对应方法和构造函数
      console.log(methods) //方法名,字符串
      console.log(desc)  //四大美女:value: ƒ, writable: true, enumerable: true, configurable: true
  }
  
  class Person{
  
      @logFn
      run(){
          console.log('向前跑就是了')
      }
  }
  
  // 传递参数方法装饰器
  function logFn2(params:any){
      return function(target:any,methods:any,desc:any){
          let oMethod = desc.value
          console.log(oMethod)
          // 重写父类的方法
          // 把数字转成字符串
          desc.value = function(...args:any[]){
              args = args.map(i=>{
               // 把数字转成字符串
                 return String(i)
              })
              console.log(args)
  
              oMethod.apply(this,args)
          }
  
      }
  }
  
  class Animal{
  
      @logFn2('sheji')
      show(...args:number[]){
          console.log(args)
      }
  }
  
  let a1 = new Animal()
  a1.show(2,25,97,100)
  ```
  
* 类中方法的属性装饰器
  
  ```typescript
  // 类中方法的参数装饰器
  
  // 无参数传递
  function logFnParams1(){
      return function(target:any,methods:any,index:any){
          console.log(target)  //修饰属性对应的方法和构造函数
          console.log(methods) //方法名
          console.log(index) //装饰器在所有参数中的排行下标
      }
  }
  
  //有参数传递
  function logFnParams2(parmas:any){
      return function(target:any,methods:any,index:any){
          console.log(`${parmas},你装饰了我,还一笑而过`)
      }
  }
  
  class Person{
  
      run(@logFnParams1() val1:string, @logFnParams2("我是传递的参数") val2:string):void{
          console.log(val1,val2)
      }
  
  }
  
  let p1 = new Person()
  p1.run("hehe","xiix")
  ```
  
* 装饰器执行顺序
  
  ```typescript
  // 从下往上,从右往左
  // 执行顺序
  // 属性装饰器-》类中方法的属性装饰器-》类的方法装饰器-》类装饰器
  // 类装饰器
  function logClass1(params:any){
      return function(target:any){
          console.log('我是1号类装饰器')
          console.log(params)
      }
  }
  
  function logClass2(params:any){
      return function(target:any){
          console.log('我是2号类装饰器')
          console.log(params)
      }
  }
  
  // 类的属性装饰器
  function logAttr1(params:any){
      return function(target:any,attr:any){
          console.log('我是1号类的属性装饰器')
          console.log(params)
      }
  }
  
  function logAttr2(params:any){
      return function(target:any,attr:any){
          console.log('我是2号类的属性装饰器')
          console.log(params)
      }
  }
  
  // 类的方法装饰器
  function logFn1(params:any){
      return function(target:any,methods:any,desc:any){
          console.log('我是1号类的方法装饰器')
          console.log(params)
      }
  }
  
  
  function logFn2(params:any){
      return function(target:any,methods:any,desc:any){
          console.log('我是2号类的方法装饰器')
          console.log(params)
      }
  }
  
  // 类中方法的属性装饰器
  function logFnAttr1(params:any){
      return function(targe:any,methods:any,index:any){
          console.log('我是1号类中方法的属性装饰器')
          console.log(params)
      }
  }
  
  function logFnAttr2(params:any){
      return function(target:any,methods:any,index:any){
          console.log('我是2号类中方法的属性装饰器')
          console.log(params)
      }
  }
  
  
  @logClass1('11')
  @logClass2('12')
  class Person{
  
      @logAttr1('15')
      @logAttr2('16')
      name:string='yaksun'
  
      @logFn1('13')
      @logFn2('14')
      run(@logFnAttr1('17') val1:any,@logFnAttr2('18') val2:any){
          console.log('showdemade')
      }
  }
  
  
  ```
  
  






