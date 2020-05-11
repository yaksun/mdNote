## 01.参考网站

```
https://docs.nestjs.cn
https://docs.nestjs.com
```

## 02.初始化项目

```
//安装脚手架
$ npm i -g @nestjs/cli

$ nest new project-name
//查看快捷命令
nest -h 
//新建posts控制器
nest g co posts
新建posts模块
nest g mo posts
```

## 03.自动生成swagger

```
// 安装插件
npm install --save @nestjs/swagger swagger-ui-express
```

```js
//添加配置 可参考文档https://docs.nestjs.com/recipes/swagger
import { NestFactory } from '@nestjs/core';
import { SwaggerModule, DocumentBuilder } from '@nestjs/swagger';
import { AppModule } from './app.module';

async function bootstrap() {
  
  const app = await NestFactory.create(AppModule);

  const options = new DocumentBuilder()
  .setTitle('我的博客')
  .setDescription('这是一个描述')
  .setVersion('1.0')
  .build();
const document = SwaggerModule.createDocument(app, options);
SwaggerModule.setup('api', app, document);


  await app.listen(3000);
}
bootstrap();
```

## 04.`swagger`的配置

* 分组

  ```js
  //在要分组的控制器前添加ApiTags
  @Controller('posts')
  @ApiTags('博客')
  export class PostsController {
  ```

* 接口描述

  ```js
    // 列表 添加描述ApiOperation
      @Get()
      @ApiOperation({summary:'博客列表'})
      index(){
          return '博客列表'
      }
  
  ```

* 添加参数和参数描述

  ```js
   // 新建
      @Post( )
      @ApiOperation({summary:'添加博客'})
      async create(@Body() addPost : createPostDto){ 
          // 从body中获取数据，还可以@Query和@Param
          return addPost
      }
  ```
```
  
  ```js
  
  class createPostDto{
      // 参数描述和示例
  	@ApiProperty({description:'标题',example:'帖子标题1'})
      title:string
      // 参数描述和示例
      @ApiProperty({description:'内容',example:'内容标题2'})
      content:string
  }
```

* 获取指定参数

  ```js
  //修改
      @Post(':id')
      @ApiOperation({summary:'修改博客'})
      update(@Body() updPost : CreatePostDto,@Param('id') id:string){
          return {
              success:true
          }
      }
  ```

## 05.连接`mongodb`

* 安装插件,参考文档`https://docs.nestjs.com/techniques/mongodb`

  ```
  cnpm install --save mongoose @types/mongoose
  cnpm i @typegoose/typegoose --save
  ```

* `post.modle.ts`

  ```js
  import { prop, getModelForClass } from '@typegoose/typegoose';
  import * as mongoose from 'mongoose';
   
  export class Post {
    @prop()
    title:string 
    
    @prop()
    content:string
  }
   
  export const PostModel = getModelForClass(Post);
  ```

* `main.ts`中连接`mongodb`

  ```js
  async function bootstrap() {
  
    mongoose.connect('mongodb://localhost:27017/yaksun',{
      useNewUrlParser:true,
      useFindAndModify:false,
      useCreateIndex:true
    })
  
  ```

* 获取数据

  ```js
  import {PostModel} from './post.modle'
  //........
  
  @Controller('posts')
  @ApiTags('博客')  // 分组
  export class PostsController {
  
      // 列表
      @Get()
      @ApiOperation({summary:'博客列表'})
     async show(){
         //获取数据 异步
         return await PostModel.find()
      }
  
  ```

* 添加数据

  ```js
      @Post( )
      @ApiOperation({summary:'添加博客'})
      async create(@Body() addPost : CreatePostDto){
          await PostModel.create(addPost)
  
          return {
              success:true 
          }
      }
  
  ```

* 数据详情

  ```js
    //详情
      @Get(':id')
      @ApiOperation({summary:'博客详情'})
      async detail(@Param('id') id:string){
          return await  PostModel.findById(id) 
      }
  
  ```

* 修改数据

  ```js
   @Post(':id')
      @ApiOperation({summary:'修改博客'})
     async update(@Body() updPost : CreatePostDto,@Param('id') id:string){
      await PostModel.findByIdAndUpdate(id,updPost)
  
          return {
              success:true
          }
      }
  ```

* 删除数据

  ```js
   @Post('del/:id')
      @ApiOperation({summary:'删除博客'})
     async remove(@Param('id') id:string){
          await PostModel.findByIdAndDelete(id) 
  
          return {
              success:true
          }
      }
  ```
## 06.使用验证插件

* 安装 `$ npm i --save class-validator class-transformer`

* `main.ts`中全局引入验证

  ```js
    const app = await NestFactory.create(AppModule);
  	//全局验证管道
    app.useGlobalPipes(new ValidationPipe())
  ```

* 控制器中给模型添加验证

  ```js
  import {IsNotEmpty} from 'class-validator'
  
  class CreatePostDto{
  
      @ApiProperty({description:'标题',example:'帖子标题1'})
      @IsNotEmpty({message:'标题不能为空'})  // 当提交缺少title时，返回message
      title:string
  
      @ApiProperty({description:'内容',example:'内容标题2'})
      content:string
  }
  ```
  
```js
  //验证返回内容
  { 
    "statusCode": 400,
    "message": [
      "标题不能为空"
    ],
    "error": "Bad Request"
  }
```
## 07.使用依赖注入模式

* 参考`https://www.npmjs.com/package/nestjs-typegoose`

* 安装`cnpm i nestjs-typegoose --save`

* main.ts中取消引入mongdb

* `app.module.ts`

  ```js
  import { Module } from '@nestjs/common';
  import { AppController } from './app.controller';
  import { AppService } from './app.service';
  // 只引入module,不引入控制器
  import { PostsModule } from './posts/posts.module';
  import { TypegooseModule } from "nestjs-typegoose";
  
  @Module({
    imports: [
      TypegooseModule.forRoot("mongodb://localhost:27017/yaksun", {
        useNewUrlParser: true
      }),
      PostsModule
    ],
    controllers: [AppController],
    providers: [AppService],
  })
  export class AppModule {}
  
  ```

* `post.modle.ts`

  ```js
  import { prop } from '@typegoose/typegoose';
   
  export class Post {
    @prop()
    title:string 
    
    @prop()
    content:string
  }
   
  ```

* `posts.module.ts`

  ```js
  import { Module } from '@nestjs/common';
  import { TypegooseModule } from "nestjs-typegoose";
  import {Post} from './post.modle'
  import {PostsController} from './posts.controller'
  
  @Module({
      imports: [TypegooseModule.forFeature([Post])],
    controllers: [PostsController],
  })
  export class PostsModule {}
  
  ```

* `posts.controller.ts`

  ```js
  import { InjectModel } from "nestjs-typegoose";
  import {Post as PostSchame} from './post.modle'
  import { ModelType } from '@typegoose/typegoose/lib/types';
  
  // ...
  
  export class PostsController {
  	//添加依赖注入
      constructor(
       @InjectModel(PostSchame) private readonly postSchame:ModelType<PostSchame>
        ) {}
  
        // 列表
      @Get()
      @ApiOperation({summary:'博客列表'})
     async show(){
         // this.postSchame 引用实例
         return await this.postSchame.find()
      }
  
      
  ```

## 08.`crud`快捷操作

* 安装`cnpm i nestjs-mongoose-crud --save`

* `news.controller.ts`

  ```js
  import {Crud} from 'nestjs-mongoose-crud'
  // 这个文件class中只需要下面的构造方法
  @Crud({
      model:NewsSchame
  })
  @Controller('news')
  export class NewsController {
      constructor(
          @InjectModel(NewsSchame) private readonly model:ModelType<NewsSchame>
        ) {}
  
  }
  
  ```
  
* `new.modle.ts`

  ```js
  // 既做模型，也做DTO
  import { prop } from '@typegoose/typegoose';
  import {ApiTags,ApiOperation,ApiProperty} from '@nestjs/swagger'
   
  export class New {
    @ApiProperty({description:'标题',example:'帖子标题1'})
    @prop()
    title:string 
    @ApiProperty({description:'内容',example:'内容标题2'})
    @prop()
    content:string
  }
   
  ```
* 配置已更新，无法复现















