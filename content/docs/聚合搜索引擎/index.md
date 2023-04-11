---
title: "聚合搜索引擎开发笔记"
date: 2023-04-07
draft: true
tags: ["Java"]

---

## 前端初始化

根据[Ant Design Vue (antdv.com)](https://2x.antdv.com/docs/vue/getting-started-cn)文档初始化项目

```cmd
npm install -g @vue/cli
vue create antd-demo
```

![配置项目](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304062112614.png)

初始化成功

![image-20230406211536616](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304062115691.png)

使用组件

```cmd
npm i --save ant-design-vue@next
```

在 src/main.ts 引入组件

```ts
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import Antd from 'ant-design-vue';
import 'ant-design-vue/dist/antd.css';

createApp(App).use(Antd).use(router).mount('#app')
```

需要配置 prettier 代码美化，添加对 vue 文件的支持

![image-20230406212438756](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304062124815.png)

启动运行

> Bug 1
>
>
> Module not found: Error: Can't resolve 'ant-design-vue/dist/antd.css' in 'D:\project\so-frontend\src'
>
> 解决思路：找到源文件，发现该文件已经修改为` reset.css` 
>
> 故修改import语句：`import "ant-design-vue/dist/reset.css";`

重新启动，成功运行

## 后端初始化

1. 使用初始化脚手架
   1. 修改包名 `Ctrl+Shift+R` 
   2. 修改 `application.yml` （项目名称、数据库、项目启动端口）
   3. 启动项目，打开网址 [接口文档](http://localhost:8101/api/doc.html#/home)
2. 测试基础 api

## 前端项目清理

1. 删除多余的`views`，只留一个`HomeView.vue` （因为聚合搜索引擎只需要一个页面），路由也需要修改

   ![image-20230406231333409](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304062313522.png)

2. 删除 App.vue 中多余的部分

   ```vue
   <template>
     <router-view/>
   </template>
   
   <style>
   ……未做修改
   </style>
   ```

3. 开启vue3语法

   ```vue
   <script setup lang="ts"></script>
   ```

3. 删除 HomeView.vue 中多余的部分，并添加根标签，方便隔离样式

   ```vue
   <template>
     <div class="home-page">
       
     </div>
   </template>
   
   <script setup lang="ts">
   
   </script>
   ```

## 页面设计

1. 需求分析

   1. 一个搜索框
   2. 搜索框下面有很多的导航栏（视频、图片、文章等）
   3. 根据用户点击的导航栏菜单，展示相应的数据

2. 从 ant-design-vue 找组件实现一个搜索框

   ```vue
   <template>
     <div class="home-page">
       <a-input-search
         v-model:value="searchText"
         placeholder="input search text"
         enter-button="Search"
         size="large"
         @search="onSearch"
       />
     </div>
   </template>
   
   <script setup lang="ts">
   import {ref} from "vue";
   // 用户输入绑定的变量
   const searchText = ref("");
   // 搜索方法 value：搜索的内容
   const onSearch=(value:string) => {
       alert(value);
   }
   </script>
   ```

​		![image-20230406233617268](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304062336328.png)

![image-20230406233705021](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304062337091.png)

2. 导航栏

   组件出错（在官方的在线demo都运行不出来），换element-ui-plus代替

   > 根据官方文档 [快速开始 | Element Plus (gitee.io)](https://element-plus.gitee.io/zh-CN/guide/quickstart.html)
   >
   > 安装 element-ui-plus 
   >
   > ```cmd
   > npm install element-plus --save
   > ```
   >
   > 引入组件
   >
   > ```ts
   > import ElementPlus from 'element-plus'
   > import 'element-plus/dist/index.css'
   > 
   > createApp(App).use(ElementPlus).use(Antd).use(router).mount("#app");
   > ```
   
   根据[文档](https://element-plus.gitee.io/zh-CN/component/tabs.html#%E5%9F%BA%E7%A1%80%E7%94%A8%E6%B3%95)将 tab 组件引入 
   
3. 预留列表组件

   1. 文章列表
   2. 用户列表
   3. 图片列表

   ```vue
   <el-tabs v-model="activeName" class="demo-tabs" @tab-click="handleClick">
       <el-tab-pane label="文章" name="1">
           <PostList></PostList>
       </el-tab-pane>
       <el-tab-pane label="用户" name="2">
           <UserList></UserList>
       </el-tab-pane>
       <el-tab-pane label="图片" name="3">
           <PictureList></PictureList>
       </el-tab-pane>
   </el-tabs>
   ```

   ```vue
   <script setup lang="ts">
       // 三个预留组件
   import PostList from "@/components/PostList.vue";
   import UserList from "@/components/UserList.vue";
   import PictureList from "@/components/PictureList.vue";
   </script>
   ```

4. 美化页面

   1. 定义一个分割组件，用于将各个组件的距离隔开

      ```vue
      <template>
        <div class="my-divider"></div>
      </template>
      
      <script></script>
      
      <style scoped>
      .my-divider {
        margin-bottom: 16px;
      }
      </style>
      ```

      使用

      ![image-20230407093817483](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304070938577.png)

      效果

      ![image-20230407093908909](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304070939967.png)

       >也可以使用 Space 间距组件
       >
       >```vue
       ><a-space direction="vertical">
       >   组件1
       >   组件2
       ></a-space>
       >```
       >
       >![image-20230407095001584](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304070950945.png)

   2. 不要挤到边边

      padding: 33px;

      ![image-20230407095526486](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304070955547.png)
   
   3. 调整宽度
   
      max-width: 60%;
   
   4. 将搜索框放在中间
      margin: 0 auto;
   
      ![image-20230407095829517](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304070958631.png)

## 完善用户需求 - 记录搜索状态

目标：用 url 记录页面搜索状态，当用户刷新页面时，能够从 url 还原之前的搜索状态

需要双向同步：url <=> 页面状态

 核心小技巧：把同步状态改成单向，即只允许 url 来改变页面状态，不允许反向

1. 让用户在操作的时候，改变 url 地址（点击搜索框，搜索内容填充到 url 上？切换 tab 时，也要填充）
2. 当 url 改变的时候，去改变页面状态（监听 url 的改变）



如何实现？

1. url <= 页面状态

   1. 动态路由 `path: '/:category'` 

   2. 用户点击 tabs ，搜索参数改变，同时 q 会改变

    绑定事件，监听标签变化 `@tab-change="onTabChange"` 

    http://localhost:8080/#/posts?q=kaneki 

    ```vue
    const onTabChange = (name: TabPaneName) => {
      router.push({
      // bug 没有前面的name http://localhost:8080/#/?pageSize=10&pageNum=1
          //params:{
           //   category:name
         // },
        //使用这个
        path: `/${name}`,
        query: searchParams.value,
      });
    };
    ```

   3. 用户搜索的文字需要体现在 URL 参数上

   4. 更换标签的时候也需要同步搜索文本和参数

        定义公共变量 

        ```vue
        const searchParams = ref({
        	q: undefined,
        });
        
        const onSearch = (value: string) => {
          router.push({
              query: searchParams.value,
          });
        };
        q:null http://localhost:8080/#/posts?q
        q:undefined http://localhost:8080/#/posts?
        q:"" http://localhost:8080/#/posts?q=
        ```
        > Bug 2
        >
        > searchParams 一直为空，无法传递
        
        >原因：没有正确绑定 v-model
        >
        >错误写法：v-model="searchParams.q"
        >
        >正确写法：v-model:value="searchParams.q"

2. url => 页面状态
   1. 方便调试，在页面添加代码`{{route.params.category+JSON.stringify(searchParams)}}`
   
      ![image-20230408091135874](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304080911998.png) 
   
   2. 监听 URL ，将 URL 的信息回传给 searchParams 
   
      ```ts
      watchEffect(() => {
        searchParams.value = {
          q: route.query.q as any,
        };
      });
      ```
   
      > Bug 3
      >
      > 没有正确定义 route router
      >
      > 自动导入的 `import router from "@/router";`
      >
      > 正确写法 
      >
      > ```vue
      > import { useRouter, useRoute } from "vue-router";
      > const route = useRoute();
      > const router = useRouter();
      > ```
   
   2. activeName 也需要从 URL 获取
   
      `const activeName = route.params.category;`
   
   3. 添加其他查询参数
   
      ```vue
      const searchParams = ref({
        	q: undefined,
          pageSize: 10,
          pageNum: 1,
      });
      ```
   
   4. 设置兜底
   
      ```vue
      const activeName = route.params.category;
      const initSearchParams = {
        q: "",
        pageSize: 10,
        pageNum: 1,
      };
      
      // 用户输入绑定的变量
      const searchParams = ref(initSearchParams);
      
      watchEffect(() => {
        searchParams.value = {
          ...initSearchParams,
          q: route.query.q,
        } as any;
      });
      ```
   
      > Bug 3
      >
      > 刷新后虽然能够获取到posts，但是列表必须点击才能显示（未解决）
      >
      > ![image-20230408093628122](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304080936257.png)‘



## 前后端联调

1. 导入axios `npm install axios`

2. 新建文件夹 plugins 和文件 myAxios.tx，将官网的实例代码粘贴进去

3. 导出实例 `export default instance;`

4. 修改 baseURL `baseURL: 'https://localhost:8101/api/'` 

5. 测试 myAxios 

   ```vue
   myAxios.get("/post/get/vo?id="+"1643991085236432897").then((res)=>{
       console.log(res.data);
   })
   ```

   完整的请求地址前面有 /api ，因为我们封装了 axios 所以这里不需要写前缀

   > Bug 4
   >
   > 出现 `net::ERR_SSL_PROTOCOL_ERROR` 错误
   >
   > 原因：向 https 发送了请求，但是并没有申请ssl证书
   >
   > 解决：在 myAxios 的实例中，修改 baseURL 为 http

   测试成功

   ![image-20230408103704772](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304081037876.png)

6. 优化 `console.log(res.data);` 每个请求都这样写太麻烦了，应该在 myAxios 中使用**响应**拦截器，具体写法参考文档

   ```vue
   // 添加响应拦截器
   axios.interceptors.response.use(function (response) {
       // 2xx 范围内的状态码都会触发该函数。
       // 对响应数据做点什么
       const data = response.data;
       if(data.code === 0){
           return data.data;
       }
       console.error("request error",data)
       return response.data;
   }, function (error) {
       // 超出 2xx 范围的状态码都会触发该函数。
       // 对响应错误做点什么
       return Promise.reject(error);
   });
   ```

   > Bug 5
   >
   > 并没有单独取出 data 
   >
   > 原因：使用的并不是自建的实例，而是 axios （复制粘贴）
   >
   > 解决：改为 `instance.interceptors.response.use(function (response) ` 

   此时测试程序发现是 undefined ，只需要把 `console.log(res.data);`  改为`console.log(res);` 

   ![image-20230408105954947](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304081059003.png)

7. 测试获取列表

   ```vue
   myAxios.post("/post/list/page/vo").then((res)=>{
       console.log(res);
   });
   // 以上出错：没有传请求体
   myAxios.post("/post/list/page/vo",{}).then((res)=>{
       console.log(res);
   });
   ```

   ![image-20230408124336175](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304081243260.png)

8. 将查询出的列表显示在页面

   ```vue
   <el-tab-pane label="文章" name="posts">
       {{postList}}
       <PostList></PostList>
   </el-tab-pane>
   
   const postList = ref([]);
   
   myAxios.post("/post/list/page/vo",{}).then((res:any)=>{
       postList.value = res.records;
   });
   ```

   

   ![image-20230408124645412](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304081246492.png)



小插曲：在找 list 组件的时候，发现自己下载的 ant-design-vue 版本和文档不同，原来是 ant-design-vue@4.0.0-alpha.3 ，卸载，重新安装了 npm i ant-design-vue@3.2.17 再试一下 ant-design-vue 的 tab 组件，能不能运行。（见Bug1）

测试结果：可以运行，并且解决了 element-ui 组件刷新后列表必须点击才能显示的问题（Bug3）

于是 `HomeView.vue` 代码修改为

```vue
<template>
  <div class="home-page">
    <a-input-search
      v-model:value="searchParams.q"
      placeholder="input search text"
      enter-button="Search"
      size="large"
      @search="onSearch"
    />
    <my-divider />
    {{ route.params.category + JSON.stringify(searchParams) }}

    <a-tabs v-model:activeKey="activeName" @change="onChange">
      <a-tab-pane key="posts" tab="文章">
        <PostList />
      </a-tab-pane>
      <a-tab-pane key="users" tab="用户">
        <UserList />
      </a-tab-pane>
      <a-tab-pane key="pictures" tab="图片">
        <PictureList />
      </a-tab-pane>
    </a-tabs>
  </div>
</template>

<script setup lang="ts">
import { ref, watchEffect } from "vue";
import PostList from "@/components/PostList.vue";
import UserList from "@/components/UserList.vue";
import PictureList from "@/components/PictureList.vue";
import MyDivider from "@/components/MyDivider.vue";
import { useRouter, useRoute } from "vue-router";
import myAxios from "@/plugins/myAxios";

const postList = ref([]);

myAxios.post("/post/list/page/vo", {}).then((res: any) => {
  postList.value = res.records;
});

const route = useRoute();
const router = useRouter();
// 表示当前激活的是哪一个标签栏
const activeName = ref(route.params.category);
const initSearchParams = {
  q: "",
  pageSize: 10,
  pageNum: 1,
};

// 用户输入绑定的变量
const searchParams = ref(initSearchParams);

watchEffect(() => {
  searchParams.value = {
    ...initSearchParams,
    q: route.query.q,
  } as any;
});

// 搜索方法
const onSearch = (value: string) => {
  router.push({
    query: searchParams.value,
  });
};
    
const onChange = (activeName: string) => {
  router.push({
    path: `/${activeName}`,
    query: searchParams.value,
  });
};
</script>

<style>

</style>

```

9. 添加 list 组件，跨组件传递 data

   将 HomeView 组件获取到的数据源 data 传递给 PostList 

   `<PostList :post-list="postList"/>` 

   `<a-list item-layout="horizontal" :data-source="props.postList">` 

   修改描述、图片

   `<a-list-item-meta :description="item.content"> `

   `<a-avatar :src="blue_eye_cat" />` 

   > Bug 6
   >
   > 并没有数据渲染在页面上，但是控制台正确的打印出了数据源
   >
   > 原因：变量写错了
   >
   > Home 组件中的 `const postList = ref([]);` 和Postlist组件中的
   >
   > `interface Props {  postList: any[];}` 和 ` :data-source="props.postList"` 
   >
   > 需要一致。
   >
   > 本来写成了 postlist ，list 没有大写，所以错误了。

   ![image-20230408142257340](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304081422501.png)

10. 修改居中样式

11. 按照同样的方法修改其他列表组件

12. 将 userlist 的列表组件改为卡片的形式

    在 `<a-list-item>` 标签中嵌入 `<a-card>` 标签

    ```vue
    <a-list item-layout="horizontal" :data-source="props.userList">
        <template #renderItem="{ item }">
    <a-list-item>
        <a-card hoverable style="width: 240px">
            <template #cover>
                <img alt="example" :src="blue_eye_cat" />
        </template>
        <a-card-meta :title="item.userName">
            <template #description>{{ item.userProfile }}</template>
        </a-card-meta>
        </a-card>
    </a-list-item>
    </template>
    </a-list>
    ```

    此时会发现卡片是竖着展示的，不好看，需要改为横着的，需要添加属性 `:grid="{ gutter: 16 }` 

    ```vue
    <a-list item-layout="horizontal" :data-source="props.userList" :grid="{ gutter: 16 }">
        ...
    </a-list>    
    ```

    ![image-20230409111707690](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304091117858.png)

    ![image-20230409111646162](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304091116365.png)



## 多数据源获取

### 爬虫

1. 抓包分析：URL、方法、body、response

   第三方请求的 √ 应该取消

   ![image-20230409122316431](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304091223517.png)

   ![image-20230409122359007](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304091223054.png)

   ![image-20230409122434773](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304091224826.png)

   ![image-20230409122523527](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304091225590.png)

   预览和响应都可以

   可以将这些 JSON 数据粘贴在文本编辑器中，方便查看

   ![image-20230409123446909](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304091234002.png)

2. 编写测试类

   1. 获取数据

      1. 使用 [Hutool — 🍬A set of tools that keep Java sweet.](https://hutool.cn/)

         > 其他 HttpClient、OKHttp、RestTemplate 

         ```java
         package com.mykaneki.sobackend;
         
         import cn.hutool.http.HttpRequest;
         import org.junit.jupiter.api.Test;
         import org.springframework.boot.test.context.SpringBootTest;
         
         
         @SpringBootTest
         public class CrawlerTest {
             @Test
             void testFetchPassage() {
                 String json = "{\"current\":1,\"pageSize\":8,\"sortField\":\"createTime\",\"sortOrder\":\"descend\",\"category\":\"文章\",\"reviewStatus\":1}";
                 String url = "https://www.code-nav.cn/api/post/search/page/vo";
                 String result = HttpRequest.post(url).body(json).execute().body();
                 System.out.println(result);
             }
         }
         
         ```

         ![image-20230409123952155](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304091239291.png)

   2. JSON 转对象

      需要根据调试的结果，了解每层是什么类型的数据，才能进行强转

      ![image-20230409145732501](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304091457623.png)

      Post 对象可以使用 ArrayList 容器来装

      `List<Post> postList = new ArrayList<>();` 

      Post 对象的 set 方法可以使用插件 GenerateAllSetter 

      ![image-20230409150044112](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304091500190.png)

      明确我们需要存入的数据，把不需要的删了，比如：CreateTime、UpdateTime、IsDelete 等

      从 temprecord 中取数据 [参考文档](https://hutool.cn/docs/#/json/%E6%A6%82%E8%BF%B0?id=%e4%bb%8b%e7%bb%8d)

      ① 需要注意 tags 如何取：tags 的结构

      ![image-20230409150815690](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304091508738.png)

      ```java
      JSONArray tags = tempRecord.getJSONArray("tags");
      List<String> tagList = tags.toList(String.class);
      post.setTags(JSONUtil.toJsonStr(tagList));
      ```

      ② 在导入文章时设置的 userid 最好是管理员

      ​	并且需要注意 int 类型溢出

      ​		`post.setUserId(1644892968243216386L);` 

   3. 数据入库

      使用 postService 存储数据

      ```java
      boolean b = postService.saveBatch(postList);
      ```

      postService 是需要注入的 bean 

      ```java
      @Resource
      private PostService postService;
      ```

      断言

      ```java
      Assertions.assertTrue(b);
      ```

   4. 查看数据库：导入成功

      > 没有会员，有些地方不能解析（ToDo）

      ![image-20230409152633327](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304091526404.png)

   视频 0.47

## 前后端接口调通

## 聚合搜索业务场景分析

## 聚合搜索接口开发







