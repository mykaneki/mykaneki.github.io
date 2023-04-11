---
title: "API 项目"
date: 2023-02-25
tags: ["项目"]
showDate: true
draft: true
---



## 前端初始化 - ANT DESIGN PRO

1. 下载node、mysql

2. 阅读[官方文档](https://pro.ant.design/zh-CN/docs/getting-started)，初始化ANT DESIGN PRO

   1. 选择包管理器`tyarn`

      更新npm `npm install -g npm@9.6.0`

      安装yarn、tyarn `npm install yarn tyarn -g`

      安装是否成功测试 `tyarn -v ` 1.22.19 

   2. 初始化脚手架。

      ```bash
      npm i @ant-design/pro-cli -g
      cd D:\ideaprj
      pro create apiback01
      ```

      选择 umi@4

   3. 安装依赖：

      `cd apiback01 && tyarn` 

3. 运行项目

   1. 安装umi-ui `yarn add @umijs/preset-ui -D` 

   2. 找到`package.json` 启动项目

      `yarn run start`

   3. 打开浏览器 http://localhost:8000/ 

   

4. 了解目录结构并项目瘦身（注意：删除页面需要检查路由）

   1. 移除国际化：运行脚本`i18n-remove` 、删除目录`locales`，重新启动测试
   2. `locales` 国际化相关 删
   3. `/src/e2e` 测试目录 删 
   4. `jest.config.js` 测试相关 删
   5. `playwright.config.ts` 测试相关 删



## 后端初始化和基础功能开发

1. 准备MySQL数据库和表
2. 使用springboot-init初始化后端（根据需要修改配置文件）
3. 使用mybatisX插件，生成接口信息对应的增删查改代码
4. 将`/controller/PostController.java` 复制为`/controller/InterfaceInfoController.java`

   1. 将post全局替换为interfaceInfo
   2. 将Post全局替换为InterfaceInfo
   3. 将InterfaceInfoMapping全局替换为关键词PostMapping
5. InterfaceInfoController.java预留了校验请求参数的接口，需要自己编写用户需要传递的参数的对象

   1. 将`src/main/java/com/yupi/springbootinit/model/dto/post`复制为`src/main/java/com/yupi/springbootinit/model/dto/interfaceinfo` 
   2. 将文件名中的Post统统替换为InterfaceInfo
   3. 将`entity/InterfaceInfo`的内容复制粘贴进`InterfaceInfoAddRequest.java`
   4. 修改内容，不需要用户提供的就删掉
   5. 其他的参数接口重复步骤3.4


6. 打开网址 http://localhost:9101/api/doc.html 测试



### 数据库表设计

借助sqlfather项目生成脚本

#### 接口信息表 

```sql
-- 接口信息表
create table if not exists api_db.`interface_info`
(
`id` bigint not null auto_increment comment '主键' primary key,
`name` varchar(256) not null comment '名称',
`description` varchar(256) null comment '描述',
`url` varchar(512) not null comment '接口地址',
`requestHeader` text null comment '请求头',
`responseHeader` text null comment '响应头',
`status` int default 0 not null comment '接口状态 0-关闭 1-开启',
`method` varchar(256) not null comment '请求类型',
`userId` bigint not null comment '创建人',
`createTime` datetime default CURRENT_TIMESTAMP not null comment '创建时间',
`updateTime` datetime default CURRENT_TIMESTAMP not null on update CURRENT_TIMESTAMP comment '更新时间',
`isDeleted` tinyint default 0 not null comment '是否删除(0-未删, 1-已删)'
) comment '接口信息表';
```

​	

### 前端接口调用

#### oneapi插件 自动生成

1. 配置接口规范 openapi

   1. 规范在哪？

      打开网址 [localhost:9101/api/v2/api-docs](http://localhost:9101/api/v2/api-docs) 

      ![image-20230403152352169](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304031523466.png) 

   2. 配置

      ![image-20230403152752151](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304031527394.png)

      ```ts
        openAPI: [
          // {
          //   requestLibPath: "import { request } from '@umijs/max'",
          //   // 或者使用在线的版本
          //   // schemaPath: "https://gw.alipayobjects.com/os/antfincdn/M%24jrzTTYJN/oneapi.json"
          //   schemaPath: join(__dirname, 'oneapi.json'),
          //   mock: false,
          // },
          {
            requestLibPath: "import { request } from '@umijs/max'",
              // schemaPath 可以直接复制所有json代码，也可以设置为网址
            schemaPath: 'https://gw.alipayobjects.com/os/antfincdn/CA1dOm%2631B/openapi.json',
              // 后端项目名称
            projectName: 'apiback01',
          },
        ],
      ```

   3. 测试

      脚本 `"openapi": "max openapi",`

      成功 [openAPI]: ✅ 成功生成 service 文件

      ```cmd
          目录: D:\ideaprj\apifor01\src\services\apiback01
      
      
      Mode                 LastWriteTime         Length Name
      -a----          2023/4/3     15:31           1234 postFavourController.ts
      -a----          2023/4/3     15:31            417 postThumbController.ts
      -a----          2023/4/3     15:31           5770 typings.d.ts
      -a----          2023/4/3     15:31           4627 userController.ts
      -a----          2023/4/3     15:31            815 wxMpController.ts
      
      ```

   获取接口列表

   src/pages/TableList/index.tsx 默认生成的管理的代码

   此时不要使用 start 启动，得用start:dev 

   （可选）src/requestErrorConfig.ts 改为 src/requestConfig.ts

