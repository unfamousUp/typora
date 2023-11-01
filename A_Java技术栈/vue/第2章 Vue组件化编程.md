# 第二章 Vue组件化编程

## 一、模块与组件、模块化与组件化

### 1. 模块与组件

![image-20230305190839761](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251749925.png)

### 2. 模块化与组件化

![image-20230305190903356](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251749927.png)

## 二、非单文件组件

### 1. 使用步骤

> - Vue中使用组件的三大步骤
>   - 定义组件：使用`Vue.extend(options)`创建，其中`options`和`new Vue(options)`时传入的那个`options`几乎一样，区别如下：
>     - 定义组件options配置对象内`el`不要写，因为最终所有组件都由一个vm管理，由vm中的el决定服务哪个容器
>     - 定义组件options配置对象内的`data`必须写成函数形式，避免组件被复用时，数据存在引用关系
>     - 注意：配置对象options使用`template属性`可以配置组件结构
>   - 注册组件
>     - 局部注册：使用`new Vue()`传入配置对象的`components`属性
>     - 全局注册：`Vue.component('组件名', 组件)`
>   - 编写组件标签
>     - `<school></school>`

```vue
<div id="root">
    // 3. 编写组件标签
    <school></school>
    <hr>
    <student></student>
    <hr>
    <hello></hello>
</div>
<script>
    Vue.config.productionTip = false
    // 1. 创建组件
    const school = Vue.extend({
        template:`
            <div>
                <h2>学校名称：{{schoolName}}</h2>
                <h2>学校地址：{{schoolAdress}}</h2>
            </div>
        `
        ,
        data(){
            return{
                schoolName:'chzu',
                schoolAdress:'滁州'
            }
        }    
    })
    const student = Vue.extend({
    const hello = Vue.extend({
        template:`
            <div>
                <h2>学校名称：{{studentName}}</h2>
                <h2>学校地址：{{age}}</h2>
            </div>
        `
        ,
        data(){
            return{
                studentName:'cjj',
                age:18
            }
        }    
    })
    // 全局注册组件
    Vue.component('hello',hello)
    new Vue({
        el: '#root',
        data: {
            
        },
        // 2. 注册组件（局部注册）
        components:{
            school,
            student
        }
    })
</script>
```

### 2. 注意事项

![image-20230305215230355](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251749928.png)

### 3. 组件的嵌套

```vue
<div id="root">
    <app></app>
</div>
<script>
    Vue.config.productionTip = false
    const student = Vue.extend({
        template: `
            <div>
                <h2>学校名称：{{studentName}}</h2>
                <h2>学校地址：{{age}}</h2>
            </div>
        `,
        data() {
            return {
                studentName: 'cjj',
                age: 18
            }
        }
    })
    // 1. 创建组件
    const school = Vue.extend({
        template: `
            <div>
                <h2>学校名称：{{schoolName}}</h2>
                <h2>学校地址：{{schoolAdress}}</h2>
                <student></student>
            </div>
        `,
        data() {
            return {
                schoolName: 'chzu',
                schoolAdress: '滁州'
            }
        },
        components: {
            student
        }
    })
    const app = Vue.extend({
        template: `
            <div>
                <school></school>
            </div>
        `,
        components: {
            school
        }
    })
    // 全局注册组件
    new Vue({
        el: '#root',
        data: {
        },
        // 2. 注册组件（局部注册）
        components: {
            app
        }
    })
</script>
```

### 4. VueComponent构造函数

![image-20230306154900063](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251749929.png)

### 5. 一个重要的内置关系`☆`

> - 一个重要的内置关系：
>   - `Vue.prototype === Vuecomponent.prototype.__proto__`
> - 作用
>   - 让`组件实例对象（vc）`可以访问到Vue`原型`上的属性和方法

![image-20230306162239440](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251749930.png)

## 三、单文件组件

> School.vue