# 路由

## 基本信息

- 该模板的路由和侧栏的高度绑定
- 修改路由就会修改侧边栏
- children属性会包含有

````js
 {
    path: '/china',
    component: Layout,
    redirect: '/china/base_data',
    name: 'china',
    meta: { title: '中国疫情', icon: 'el-icon-s-help' },
    children: [
      {
        path: 'base_data',
        name: 'Base_data',
        component: () => import('@/views/base_data/index'),
        meta: { title: '基本数据', icon: 'table' }
      },
      {
        path: 'data_analysis',
        name: 'Data_analysis',
        component: () => import('@/views/data_analysis/index'),
        meta: { title: '数据分析', icon: 'tree' }
      }
    ]
  },
````

## 属性说明

该模板对路由封装一些属性

````js
// 当设置 true 的时候该路由不会在侧边栏出现
hidden: true 
//当设置 noRedirect 的时候该路由在面包屑导航中不可被点击
redirect: 'noRedirect'
// 当你一个路由下面的 children 声明只有一个时，会将那个子路由当做根路由显示在侧边栏--如引导页面
// 设置 alwaysShow: true，这样它就会忽略之前定义的规则，一直显示根路由
name: 'router-name'
meta: {
  roles: ['admin', 'editor'] // 设置该路由进入的权限，支持多个权限叠加
  title: 'title' // 设置该路由在侧边栏和面包屑中展示的名字
  noCache: true // 如果设置为true，则不会被 <keep-alive> 缓存(默认 false)
  breadcrumb: false //  如果设置为false，则不会在breadcrumb面包屑中显示(默认 true)
  affix: true // 如果设置为true，它则会固定在tags-view中(默认 false)
  // 当路由设置了该属性，则会高亮相对应的侧边栏。
  // 点击文章进入文章详情页，这时候路由为/article/1，但你想在侧边栏高亮文章列表的路由，就可以进行如下设置
  activeMenu: '/article/list'
}
````

````js
{
  path: '/permission',
  component: Layout,
  redirect: '/permission/index', //重定向地址，在面包屑中点击会重定向去的地址
  alwaysShow: true, //一直显示根路由
  meta: { roles: ['admin','editor'] }, //你可以在根路由设置权限，这样它下面所有的子路由都继承了这个权限
  children: [{
    path: 'index',
    component: ()=>import('permission/index'),
    name: 'permission',
    meta: {
      title: 'permission',
      icon: 'lock', //图标
      roles: ['admin','editor'], //或者你可以给每一个子路由设置自己的权限
      noCache: true // 不会被 <keep-alive> 缓存
    }
  }]
}
````

