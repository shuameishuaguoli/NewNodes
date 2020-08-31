# 三、Layout 嵌套路由

## 准备

1、创建 `src/components/layout-header.vue` 并写入以下内容

```html
<template>
  <!-- 头部组件 el-row布局 -->
  <el-row type='flex' justify="space-between" align="middle">
    <!-- 左侧 -->
    <el-col :span="6" class='left'>
      <!-- 左侧 -->
      <i class='el-icon-s-fold'></i>
      <span>江苏传智播客教育科技股份有限公司</span>
    </el-col>
    <!-- 右侧 -->
    <el-col :span="3" class='right'>
      <!-- 头像 -->
      <img src="../assets/img/avatar.jpg" alt="">
      <!-- 下拉菜单 -->
      <el-dropdown trigger="click">
        <span>水若寒宇</span>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item>账户信息</el-dropdown-item>
          <el-dropdown-item>git地址</el-dropdown-item>
          <!--
            如果想要给一个组件注册一个原生 JavaScript 事件
            使用 .native 修饰符
           -->
          <el-dropdown-item @click.native="onLogout">退出</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
    </el-col>
  </el-row>
</template>
<script>
export default {
  methods: {
    onLogout () {
      this.$confirm('确认推出吗？', '退出提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        // 确认执行这里
        // 删除 token
        window.localStorage.removeItem('user-token')

        // 跳转到登录页
        this.$router.push('/login')

        this.$message({
          type: 'success',
          message: '退出成功!'
        })
      }).catch(() => {
        // 取消执行这里
        this.$message({
          type: 'info',
          message: '已取消退出'
        })
      })
    }
  }
}

</script>
<style lang='less' scoped>
.left {
  display: flex;
  align-items: center;

  i {
    font-size: 24px;
  }

  span {
    margin-left: 4px;
  }
}

.right {
  display: flex;
  align-items: center;

  img {
    border-radius: 50%;
    margin-right: 5px;
  }
}

</style>

```



2、创建 `src/views/layout/index.vue` 并写入以下内容

```html
<template>
  <!-- 布局 -->
  <el-container>
    <!-- 左右布局 -->
    <el-aside class="left" style="width:220px">
      <div class="menu-title" style="background-color:#2e2f32">
        <img src="../../assets/img/logo_admin.png" alt="" />
      </div>
      <!-- 左侧菜单 -->
      <el-menu
        style="width:221px"
        background-color="#353b4e"
        text-color="#adafb5"
        active-text-color="#ffd04b"
      >
        <!-- 一级菜单 -->
        <el-menu-item>首页</el-menu-item>
        <!-- 二级菜单 -->
        <el-submenu>
          <!-- 具名插槽 -->
          <template slot="title">内容管理</template>
          <el-menu-item>发布文章</el-menu-item>
          <el-menu-item>内容列表</el-menu-item>
          <el-menu-item>评论列表</el-menu-item>
          <el-menu-item>素材管理</el-menu-item>
        </el-submenu>
        <!-- 二级菜单 -->
        <el-submenu>
          <template slot="title">粉丝管理</template>
          <el-menu-item>图文数据</el-menu-item>
          <el-menu-item>粉丝概况</el-menu-item>
          <el-menu-item>粉丝画像</el-menu-item>
          <el-menu-item>粉丝列表</el-menu-item>
        </el-submenu>
        <!--一级菜单 -->
        <el-menu-item>账户信息</el-menu-item>
      </el-menu>
    </el-aside>
    <!-- 大容器 -->
    <el-container>
      <el-header>
        <layout-header></layout-header>
      </el-header>
      <el-main>
        <!-- 二级路由容器，子路由会渲染到这里 -->
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>
<script>
import layoutHeader from '../../components/layout-header.vue'
export default {
  name: 'home',
  components: {
    'layout-header': layoutHeader
  }
}
</script>
<style lang="less" scoped>
.left {
  width: 220px;
  height: 100vh;
  background-color: #353b4e;
  overflow: hidden;

  .menu-title {
    padding: 15px 0;
    text-align: center;

    img {
      height: 30px;
    }
  }
}
</style>

```

3、配置路由

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
// import Home from '../views/home/index.vue'  完整路径
+ import Layout from '../views/layout' // 简写路径
import Login from '../views/login' // 简写路径

Vue.use(VueRouter)

const routes = [
  // {
  //   path: '/',
  //   redirect: '/home'
  // },
  // 一级路由 主页，它是最外面的那个壳子
+  {
+    path: '/',
+    component: Layout
+  },
  {
    // 一级路由 登录页
    path: '/login',
    component: Login
  }
]

const router = new VueRouter({
  routes
})

export default router

```



## 创建并配置子路由页面

### 创建首页并配置为 Layout 的子路由

1、创建 `src/views/home/index.vue` 并写入以下内容

```html
<template>
  <div class="home">首页</div>
</template>

<script>
export default {
  name: '',
  data () {
    return {}
  }
}
</script>

<style scoped>
</style>

```

2、把首页配置到路由中

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
// import Home from '../views/home/index.vue'  完整路径
import Layout from '../views/layout' // 简写路径
import Login from '../views/login' // 简写路径

// @ 是 VueCLI 中提供的一种特殊的路径规则，它直接指向 src 目录路径
// 注意：在 VueCLI 创建的项目中，无论你在哪里使用 @ 符号，它永远指向 src
+ import Home from '@/views/home'

Vue.use(VueRouter)

const routes = [
  // {
  //   path: '/',
  //   redirect: '/home'
  // },
  // 一级路由 主页，它是最外面的那个壳子
  {
    path: '/',
    component: Layout,
+    children: [
+      { // 首页
+        path: '', // 默认子路由，只能有一个
+        component: Home
+      }
    ]
  },
  {
    // 一级路由 登录页
    path: '/login',
    component: Login
  }
]

const router = new VueRouter({
  routes
})

export default router

```

最后，访问 `/` 测试是否能看到首页。

### 创建文章列表并配置为 Layout 的子路由

1、创建 `src/views/article/index.vue` 并写入以下内容

```html
<template>
  <div class="article">文章列表</div>
</template>

<script>
export default {
  name: '',
  data () {
    return {}
  }
}
</script>

<style scoped>
</style>

```

2、配置到路由中

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
// import Home from '../views/home/index.vue'  完整路径
import Layout from '../views/layout' // 简写路径
import Login from '../views/login' // 简写路径

// @ 是 VueCLI 中提供的一种特殊的路径规则，它直接指向 src 目录路径
// 注意：在 VueCLI 创建的项目中，无论你在哪里使用 @ 符号，它永远指向 src
import Home from '@/views/home'
+ import Article from '@/views/article'

Vue.use(VueRouter)

const routes = [
  // {
  //   path: '/',
  //   redirect: '/home'
  // },
  // 一级路由 主页，它是最外面的那个壳子
  {
    path: '/',
    component: Layout,
    children: [
      { // 首页
        path: '', // 默认子路由，只能有一个
        component: Home
      },
+      { // 文章列表
+        path: '/article',
+        component: Article
+      }
    ]
  },
  {
    // 一级路由 登录页
    path: '/login',
    component: Login
  }
]

const router = new VueRouter({
  routes
})

export default router

```

最后，访问 `/article` 测试。

### 其它

1、创建页面组件

2、配置为 Layout 的子路由

## 路由设计

| 路径     | 说明     | 备注                   |
| -------- | -------- | ---------------------- |
| /login   | 登录     | 一级路由               |
| /        | Layout   | 一级路由               |
|          | 首页     | 二级路由，**默认路由** |
| /publish | 发布文章 | 二级路由               |
| /article | 文章列表 | 二级路由               |
| /comment | 评论列表 | 二级路由               |
| /account | 账户设置 | 二级路由               |
| /image   | 素材管理 | 二级路由               |
|          |          |                        |

## 让路由页面和导航菜单对应

1、`el-menu` 组件有个选项 `router`，用于控制路由模式，启用该模式会在激活导航时以 index 作为 path 进行路由跳转 。所以，在 Layout 组件中找到你的 `el-menu` 菜单组件，启用路由模式

```html
<el-menu
  style="width:221px"
  background-color="#353b4e"
  text-color="#adafb5"
  active-text-color="#ffd04b"
+  router
>
  ...中间内容
</el-menu>
```

2、给你的菜单项设置 index，导航到具体的页面路径

```html
<!-- 左侧菜单 -->
<el-menu
  style="width:221px"
  background-color="#353b4e"
  text-color="#adafb5"
  active-text-color="#ffd04b"
  router
>
  <!-- 一级菜单 -->
+  <el-menu-item index="/">首页</el-menu-item>
  <!-- 二级菜单 -->
+  <el-submenu index="1">
    <!-- 具名插槽 -->
    <template slot="title">内容管理</template>
+    <el-menu-item index="/publish">发布文章</el-menu-item>
+    <el-menu-item index="/article">内容列表</el-menu-item>
    <el-menu-item>评论列表</el-menu-item>
    <el-menu-item>素材管理</el-menu-item>
  </el-submenu>
  <!-- 二级菜单 -->
+  <el-submenu index="2">
    <template slot="title">粉丝管理</template>
    <el-menu-item>图文数据</el-menu-item>
    <el-menu-item>粉丝概况</el-menu-item>
    <el-menu-item>粉丝画像</el-menu-item>
    <el-menu-item>粉丝列表</el-menu-item>
  </el-submenu>
  <!--一级菜单 -->
  <el-menu-item>账户信息</el-menu-item>
</el-menu>
```

最后回到浏览器中，点击不同的菜单项导航测试。