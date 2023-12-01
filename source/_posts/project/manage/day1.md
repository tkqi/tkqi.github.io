---
title: day1
date: 2023-12-01 18:14:33
tags:
- Project
categories:
- programming
- project
- manage
---

## day1 for manageSystem

总结

- 多看element-ui官方文档，留意每个组件属性的使用
- 使用路由，进一步了解路由嵌套、工程化使用路由跳转、了解路由实例(router)和页面路由(route)的区别

### 路由配置

- index文件配置

  ```js
  // 引入路由并注入到全局
  import Vue from 'vue'
  import VueRouter from 'vue-router'
  // 引入路由配置
  import routes from './routes'
  Vue.use(VueRouter)
  
  // 新建路由实例
  const router = new VueRouter({
      // 参数配置
    routes: routes
  })
  
  // 暴露router实例
  export default router
  ```

  

- routes文件：路由参数配置

  ```js
  export default [
      // 主路由
      {
          path: '/',
          component: () => import('../views/Main.vue'),
          redirect:'/home',// 重定向
          // 子路由
          children: [
              {
                  // 首页
                  path: '/home',
                  // 路由组件懒加载
                  component: () => import('../views/Home.vue')
              },
              {
                  // 用户管理
                  path: '/user',
                  component: () => import('../views/User.vue')
              },
              {
                  // 商品管理
                  path: '/mall',
                  component: () => import('../views/Mall.vue')
              },
              {
                  // 其他/页面1
                  path: '/page1',
                  component: () => import('../views/Page1.vue')
              },
              {
                  // 其他/页面2
                  path: '/page2',
                  component: () => import('../views/Page2.vue')
              },
          ]
      },
      {
          path: '/home',
          // 路由组件懒加载
          component: () => import('../views/Home.vue')
      },
      {
          path: '/user',
          component: () => import('../views/User.vue')
      }
  ]
  ```

  

  - `component: () => import('../views/Main.vue'),`：重定向引入组件，利于组件间跳转
  - `redirect:'/home'`：重定向，跳转到此对象的path会替换为重定向路径的组件
  - `children`：子路由，用于路由嵌套

### 整体页面布局

- 使用el-container布局页面

  ![image-20231201182852520](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202312011828658.png)

- 整体页面文件--Main.vue

- 在整体页面的main部分嵌套一个路由，用于页面之间切换

  ```vue
  <template>
      <div>
          <el-container>
              <el-aside width="auto">
                  <sidebar />
              </el-aside>
              <el-container>
                  <el-header><topbar/></el-header>
                  <el-main><router-view /></el-main>
              </el-container>
          </el-container>
      </div>
  </template>
  ```

  - sidebar：左侧边栏组件
  - topbar：顶栏组件
  - router-view：嵌套的子路由入口

### 左侧边栏

![image-20231201185139919](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202312011851955.png)

- 使用element的el-menu组件

- 子菜单处理问题

- 使用相对于视窗高度`100vh`，使得高度占满屏高

- 使用element自带图标：el-icon

  - `vue`部分

    ```vue
        <el-menu class="menu" @open="handleOpen" @close="handleClose" :collapse="isCollapse" background-color="#545c64"
            text-color="#fff" active-text-color="#ffd04b">
            <h3>{{ !isCollapse?'后台管理系统':'后台' }}</h3>
            <el-menu-item v-for="item in noChildren" :key="item.name" :index="item.name" @click="clickMenu(item)">
                <i :class="`el-icon-${item.icon}`"></i>
                <span slot="title">{{ item.label }}</span>
            </el-menu-item>
            <el-submenu v-for="item in hasChildren" :key="item.label" :index="item.label">
                <template slot="title">
                    <i :class="`el-icon-${item.icon}`"></i>
                    <span slot="title">{{ item.label }}</span>
                </template>
                <el-menu-item-group v-for="subitem in item.children" :key="subitem.name">
                    <el-menu-item :index="subitem.name" @click="clickSubmenu(subitem)">
                        <!-- <template slot="title"> -->
                        <i :class="`el-icon-${subitem.icon}`"></i>
                        <span slot="title">{{ subitem.name }}</span>
                        <!-- </template> -->
                    </el-menu-item>
                </el-menu-item-group>
            </el-submenu>
        </el-menu>
    ```

    - collapse属性：菜单是否折叠，利用vuex控制折叠状态

    - 子菜单：

      ```vue
      <el-submenu>
      	<!--上级目录信息-->
          <template></template>
          <el-menu-item-grop>
              <!--子菜单位置-->
          <el-menu-item></el-menu-item>
          </el-menu-item-grop>
      </el-submenu>
      ```

    - 在el-menu-item使用v-for渲染多个菜单选项

  - `js`部分

    ```js
    export default {
        computed: {
            // 无子菜单
            noChildren() {
                return this.menuData.filter(o => !o.children);
            },
            // 有子菜单
            hasChildren() {
                return this.menuData.filter(o => o.children);
            },
            isCollapse() {
               return this.$store.state.tab.isCollapse;
            }
        },
        data() {
            return {
                // 使用store实时改变属性需要定义到computed
                // isCollapse: this.$store.getters('getisCollapse'),
                menuData: [
                    {
                        path: '/',
                        name: 'home',
                        label: "首页",
                        icon: 's-home',
                        url: 'Home/Home'
                    },
                    {
                        path: '/mall',
                        name: 'mall',
                        label: "商品管理",
                        icon: 's-shop',
                        url: 'MallManage/MallManage'
                    },
                    {
                        path: '/user',
                        name: 'user',
                        label: "用户管理",
                        icon: 's-custom',
                        url: 'UserManage/UserManage'
                    },
                    {
                        label: '其他',
                        icon: 'menu',
                        children: [
                            {
                                path: '/page1',
                                name: 'page1',
                                label: "页面1",
                                icon: 's-claim',
                                url: 'Other/PageOne'
                            },
                            {
                                path: '/page2',
                                name: 'page2',
                                label: "页面2",
                                icon: 's-claim',
                                url: 'Other/PageTwo'
                            },
                        ]
                    }
                ]
            };
        },
        methods: {
            clickMenu(item) {
                console.log(this.$router.currentRoute.path);
                // 判断当前路由于点击链接的路由不一致才跳转
                if (item.path != this.$route.path && !(this.$route.path == '/home' && item.path == '/'))
                    this.$router.push(item.path);
            },
            clickSubmenu(subitem) {
                // 判断当前路由于点击链接的路由不一致才跳转
                if (subitem.path != this.$route.path)
                    this.$router.push(subitem.path);
            }
        }
    }
    ```

    - `menuData`：路由信息，通过有无属性children判断是否有子菜单，在computed计算属性实时改变有、无子菜单的目录
    - `clickMenu`、`clickSubmenu`：点击触发路由跳转，传递的值为路由信息；路由跳转时要注意处理跳转重复路由路径的情况
    - isCollapse：通过vuex管理菜单是否折叠的状态，在顶栏的按钮中改变该状态

### 顶栏

![image-20231201185116503](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202312011851551.png)

- 使用flex布局，使得按钮和头像处于中心对称
- 使用一个div标签包含两个div标签的格式
  - `<div><div/><div/></div>`
  - 左边为折叠菜单按钮和面包屑
  - 右边为个人头像及信息等

- 点击按钮时通过vuex的mutation方法改变折叠菜单的属性的值，达到折叠菜单的效果

### 有关样式

```vue
<style lang="less" scoped>
    div{
		height:100%;
        /*仅div标签中的class为style1的元素生效*/
        .style1{
			height:100%;
        }
    }
</style>
```

- lang：标记css使用less插件
- scoped：样式仅在此组件生效

