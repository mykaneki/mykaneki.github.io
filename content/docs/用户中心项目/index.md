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
      pro create user-center
      ```

      选择 umi@3 、Simple

   3. 安装依赖：

      `cd user-center && tyarn` 
      
      遇到问题：继续运行命令`npx update-browserslist-db@latest` 
      
      ```powershell
      $ umi g tmp
      Browserslist: caniuse-lite is outdated. Please run:
        npx update-browserslist-db@latest
        Why you should do it regularly: https://github.com/browserslist/update-db#readme
      $ husky install
      fatal: not a git repository (or any of the parent directories): .git
      ```

3. 运行项目

   1. 安装umi-ui `yarn add @umijs/preset-ui -D` 

   2. 找到`package.json` 启动项目

      `yarn run start`

   3. 打开浏览器 http://localhost:8000/ 

   ![image-20230304184344125](D:\Hugo\Sites\mykameki\assets\img\index\image-20230304184344125.png)

4. 了解目录结构并项目瘦身（注意：删除页面需要检查路由）

   1. 移除国际化：运行脚本`i18n-remove` 、删除目录`locales`，重新启动测试
   2. `locales` 国际化相关 删
   3. `/src/e2e` 测试目录 删 
   4. `jest.config.js` 测试相关 删
   5. `playwright.config.ts` 测试相关 删



## 后端初始化

1. 准备MySQL数据库5.7

2. 使用IDEA初始化Spring项目 

   ![image-20230304191637748](D:\Hugo\Sites\mykameki\assets\img\index\image-20230304191637748.png)

   ![image-20230304192542581](D:\Hugo\Sites\mykameki\assets\img\index\image-20230304192542581.png)

3. 根据官方文档，导入Mybatis-Plus的依赖，并配置

   1. 编辑配置文件`src/main/resources/application.yml` 把properties改为yml

      ```yaml
      # application.yml
      spring:
        application:
          name: user-center-back
        datasource:
          driver-class-name: com.mysql.cj.jdbc.Driver
          url: jdbc:mysql://localhost:3306/user-center-back-01
          username: root
          password: 123456
      server:
        port: 8080
      ```

   2. 在 Spring Boot 启动类中添加 `@MapperScan` 注解，扫描 Mapper 文件夹（新建文件夹 `com.name.usercenter.mapper`  ）。

      ```java
      @SpringBootApplication
      @MapperScan("com.name.usercenter.mapper")
      public class UserCenterBackApplication {
          public static void main(String[] args) {
              SpringApplication.run(UserCenterBackApplication.class, args);
          }
      }
      ```

      

   3. 新建model文件夹，存放和数据库对应的Java对象。

      

   

   

​	
