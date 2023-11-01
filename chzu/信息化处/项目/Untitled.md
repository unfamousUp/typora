# 互联网执法系统

> 滁州市网信办

## 一、互联网抓取系统

## 二、互联网监测（纠错）系统

> 网站的一些敏感词汇

基于纠错，把数据提供给相关部门

## 服务器

> IP：192.168.85.107
> 用户：administrator
> 密码：chzu@20230330
> 远程端口号：5107

![img](file:///G:\文件\QQ\消息记录\510618293\Image\C2C\74D8DBDB2D51986ECD3F6EA947D8FC8B.png)

> - Login.vue（初始页面，判断并记录用户是否为管理员，跳转到index.vue）
>
> - Index.vue（主页面 100vh）
>
>   - Header.vue(顶部导航栏)
>
>     - 注销：点击toLogin
>
>     - 设置：下拉菜单（1.修改个人信息 2.修改密码）
>       - 1. 修改个人信息：to edit-personal-info
>       - 2. 修改密码：to edit password
>
>   - Main.vue
>
>     - <router-view></router-view>
>       - Content.vue(路由组件，默认跳转到首页内容区，80vh)
>       - edit-personal-info.vue(路由组件，修改个人信息)
>       - edit-password.vue(路由组件，修改密码)
>       - system-administration.vue(路由组件，`1.系统管理模块`)
>         - sa-aside.vue(系统管理模块-侧边栏)
>           -  institution-management.vue(系统模块子路由，机构管理)
>           - user-management.vue(系统模块子路由，用户管理)
>             - 可尝试抽屉组件
>           - role-management.vue(系统模块子路由，角色管理)
>             - 默认路由页面
>             - 页面...
>             - 页面...
>         - sa-content（系统管理模块-内容区）
>       -  Supervision.vue(路由组件，`2.督办模块`)
>         - s-aside.vue(督办模块-侧边栏)
>         - a-content.vue(督办模块-内容区)
>       - laws-regulations.vue(路由组件，`3.法律法规模块`)
>         - 默认路由页面
>         - laws-regulations-management(法律法规模块子路由-法律法规内容管理)
>       - content-management.vue(路由组件，`4.内容管理模块`)
>       - statistic-analysis.vue(路由组件，`5.统计分析模块`)
>       - my-workbench.vue(路由组件，`6.我的工作台模块`)

## 一、登录模块

### 1. 登录页面

![image-20230410142531001](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230410142531001.png)

## 二、主页面

