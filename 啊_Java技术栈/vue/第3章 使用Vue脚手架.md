# 第三章 使用Vue脚手架

## 一、初始化脚手架

### 1. 脚手架说明

> 1. Vue脚手架：`Vue-cli(command line interface 命令行接口工具)`
> 2. Vue脚手架是Vue官方提供的`标准化开发工具（开发平台）`
> 3. 文档：`https://cli.vuejs.org/zh/`

### 2. 安装步骤

> 1. 第一步（仅第一次执行）
>    - 全局安装`@Vue/cli`包：`npm install -g @vue/cli`
>      - 安装后cmd上可以使用`vue`开头的指令
> 2. 第二步：`切换到你要创建项目的目录`，然后使用命令创建项目
>    - `vue create 项目名称`
> 3. 第三步：启动项目
>    - `npm run serve`

> - 备注
>   - 下载缓慢需配置npm淘宝镜像：`npm config set registry https://registry.npm.taobao.org`
>   - 在cmd页面按两次`ctrl+c`停止serve

### 3. 分析脚手架结构

![image-20230306203005748](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251750340.png)

> 1. `.gitignore`
>    - git的忽略文件，配置不想受git管理的文件和文件夹
> 2. `babel.config.js`
>    - 配置一些规则，把`ES6`转成`ES5	`
> 3. `main.js`
>    - 该文件是整个项目的入口文件

![image-20230311123452777](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251750341.png)

### 4. vm配置对象属性：render函数

> - 关于不同版本的Vue
>   1. vue.js与vue.runtime.xxx.js的区别
>      - vue.js是完整的Vue，包含：`核心功能`+`模板解析器`
>      - vue.runtime.xxx.js是运行版的Vue，只包含：`核心功能`没有模板解析器
>   2. 因为vue.runtime.xxx.js没有模板解析器，所以不能使用`template`配置项，需要使用`render`函数去接收createElement函数去指定具体内容

### 5. vue.config.js配置文件

![image-20230311123516407](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251750342.png)

### 6. 标签属性：ref

![image-20230311174031337](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251750343.png)

```vue
<template>
    <div>
        <h2 v-text="msg" ref="title"></h2>
        <button @click="showDOM" ref="btn">点我获取DOM</button>
        <School ref="sch"></School>
    </div>
</template>
<script>
    import School from "./components/School.vue"
    export default {
        name: 'App',
        components:{School},
        data() {
            return {
                msg: 'hello',
            };
        },
        methods: {
            showDOM(){
                console.log(this.$refs.title); // 真实DOM
                console.log(this.$refs.btn); // 真实DOM
                console.log(this.$refs.sch); // vc组件实例对象
            }
        },
    }
</script>
```

### 7. vc配置对象属性：props

![image-20230311231016503](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251750344.png)

```vue
<template>
    <div>
        <h1>{{msg}}</h1>
        <h2>学生姓名：{{name}}</h2>
        <h2>学生性别：{{sex}}</h2>
        <h2>学生年龄：{{myAge}}</h2>
        <button @click="updateAge">尝试修改收到的年龄</button>
    </div>
</template>
<script>
export default {
    name:'Student',
    data() {
        return {
            msg: 'cjj',
            myAge:this.age
        };
    },
    methods: {
        updateAge(){
            this.myAge++
        }
    },
    // 简单声明接受
    // props:['name','age','sex']
    
    // 接受的同时对数据进行类型限制
    // props:{
    //     name:String,
    //     age:Number,
    //     sex:String
    // }
    
    // 接受的同时对数据：
    // 进行类型限制 + 默认值的指定 + 必要性的限制
    props:{
        name:{
            type:String,
            required:true
        },
        age:{
            type:Number,
            default:99 // 默认值
        },
        sex:{
            type:String,
            required:true
        }
    }
    
}
</script>
```

### 8. mixin(混入)

> - 有点类似与java中的父类，在`mixin.js`中定义组件共有的`data()`属性或者`methods()`方法。
> - 然后在指定组件内`import {mixin} from '.../mixin.js'`混入mixin对象，并在配置对象中用`mixins:[mixin]属性`以数组的形式接收。

![image-20230311233829884](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251750345.png)

### 9. 插件：plugins

![image-20230312001033344](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251750346.png)

```js
export default {
    // 形参为Vue
    install(Vue) {
        console.log("@@install()");
        // 定义全局指令
        Vue.directive('fbind', {
            bind(el, binding) {
                el.value = binding.value
            },
            inserted(el, binding) {
                el.focus()
            },
            update(el, binding){
                el.value = binding.value
            }
        });
    }
}
```

### 10. style标签属性：scoped

> - 作用：让css样式在组件局部生效，防止冲突
> - 写法：`<style scoped></style>`

## 二、案例

> - 组件化编码流程
>   1. 实现静态组件：抽取组件，使用组件实现静态页面效果
>   2. 展示动态数据
>   3. 交互——从绑定事件监听开始

### 1. Todo-List案例

#### 1.1 添加

##### `App.vue`

```vue
<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader :addTodo="addTodo"></MyHeader>
        <MyList :todos="todos"></MyList>
        <MyFooter></MyFooter>
      </div>
    </div>
  </div>
</template>

<script>
  import MyHeader from './components/MyHeader'
  import MyFooter from './components/MyFooter'
  import MyList from './components/MyList'
  export default {
    name: 'App',
    components: {
      MyHeader,
      MyFooter,
      MyList
    },
    data() {
      return {
        // 一堆要做的事
        todos: [{
            id: '001',
            title: '吃饭',
            done: true
          },
          {
            id: '002',
            title: '睡觉',
            done: false
          },
          {
            id: '003',
            title: '打豆豆',
            done: true
          }
        ]
      };
    },
    methods:{
      addTodo(todo){
        // 添加todo
        this.todos.unshift(todo)
      }
    }
  }
</script>
```

##### `MyHeader.vue`

```Vue
<template>
  <div class="todo-header">
    <input type="text" placeholder="请输入你的任务名称，按回车键确认" v-model="title" @keyup.enter="add"/>
  </div>
</template>

<script>
  import {nanoid} from 'nanoid'
  export default {
    name: 'MyHeader',
    props:['addTodo'],
    data() {
      return {
        title:'',
      };
    },
    methods:{
      // 添加
      add(){
        // 数据校验
        if(!this.title.trim()) return alert('输入不能为空') 
        // 将用户的输入包装成一个todo对象
        const todoObj = {id:nanoid(),title:this.title,done:false}
        // 通知App组件去添加一个todo对象
        this.addTodo(todoObj)
        // 清空input框
        this.title = ''
      }
    }
  };
</script>
```

##### `MyList.vue`

```vue
<template>
  <ul class="todo-main">
     <!-- :toto 可以向子组件传值。因为是单向数据绑定，所以可以识别后面的js表达式，此处传值为todoObj对象 -->
    <MyItem v-for="todoObj in todos" :key="todoObj.id" :todo="todoObj"></MyItem>
  </ul>
</template>

<script>
  import MyItem from './MyItem.vue'
  export default {
    name: 'MyList',
    components: {
      MyItem
    },
    // 
    props:['todos'],
    data() {
      return {
      };
    },
  };
</script>
```

##### `MyItem.vue`

```vue
<template>
  <li>
    <label>
      <!--      这里勾选和取消勾选可以使用change和click作为事件处理-->
      <input type="checkbox" :checked="todo.done" />
      <span>{{todo.title}}</span>
    </label>
    <button class="btn btn-danger"">删除</button>
  </li>
</template>

<script>
  export default {
    name: 'MyItem',
    // 接收父组件传的值
    props:['todo'],
    data() {
      return {};
    },
  };
</script>
```

#### 1.2 单选框勾选

##### `App.vue`

```vue
<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader :addTodo="addTodo"></MyHeader>
        <MyList :todos="todos" :checkTodo="checkTodo"></MyList>
        <MyFooter></MyFooter>
      </div>
    </div>
  </div>
</template>

<script>
  import MyHeader from './components/MyHeader'
  import MyFooter from './components/MyFooter'
  import MyList from './components/MyList'
  export default {
    name: 'App',
    components: {
      MyHeader,
      MyFooter,
      MyList
    },
    data() {
      return {
        // 一堆要做的事
        todos: [{
            id: '001',
            title: '吃饭',
            done: true
          },
          {
            id: '002',
            title: '睡觉',
            done: false
          },
          {
            id: '003',
            title: '打豆豆',
            done: true
          }
        ]
      };
    },
    methods:{
      addTodo(todo){
        // 添加todo
        this.todos.unshift(todo)
      },
      // 勾选框
      checkTodo(id){
        this.todos.forEach(todo => {
          if(todo.id === id) todo.done = !todo.done
        });
      }
    }
  }
</script>
```

##### `MyList.vue`

```vue
<template>
  <ul class="todo-main">
    <!-- :toto 可以向子组件传值。因为是单向数据绑定，所以可以识别后面的js表达式，此处传值为todoObj对象 -->
    <MyItem v-for="todoObj in todos" :key="todoObj.id" :todo="todoObj" :checkTodo="checkTodo">
    </MyItem>
  </ul>
</template>

<script>
  import MyItem from './MyItem.vue'
  export default {
    name: 'MyList',
    components: {
      MyItem
    },
    // 
    props: ['todos', 'checkTodo'],
    data() {
      return {};
    },
  };
</script>
```

##### `MyItem.vue`

```vue
<template>
  <li>
    <label>
      <!--      这里勾选和取消勾选可以使用change和click作为事件处理-->
      <input type="checkbox" :checked="todo.done" />
      <span>{{todo.title}}</span>
    </label>
    <button class="btn btn-danger"">删除</button>
  </li>
</template>

<script>
  export default {
    name: 'MyItem',
    // 接收父组件传的值
    props:['todo'],
    data() {
      return {};
    },
  };
</script>
```

#### 1.3 删除一个todo

##### `App.vue`

```vue
<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader :addTodo="addTodo"></MyHeader>
        <MyList :todos="todos" :checkTodo="checkTodo" :deleteTodo="deleteTodo"></MyList>
        <MyFooter></MyFooter>
      </div>
    </div>
  </div>
</template>

<script>
  import MyHeader from './components/MyHeader'
  import MyFooter from './components/MyFooter'
  import MyList from './components/MyList'
  export default {
    name: 'App',
    components: {
      MyHeader,
      MyFooter,
      MyList
    },
    data() {
      return {
        // 一堆要做的事
        todos: [{
            id: '001',
            title: '吃饭',
            done: true
          },
          {
            id: '002',
            title: '睡觉',
            done: false
          },
          {
            id: '003',
            title: '打豆豆',
            done: true
          }
        ]
      };
    },
    methods: {
      // 添加todo
      addTodo(todo) {
        this.todos.unshift(todo)
      },
      // 勾选框
      checkTodo(id) {
        this.todos.forEach(todo => {
          if (todo.id === id) todo.done = !todo.done
        });
      },
      // 删除一个todo
      deleteTodo(id){
        this.todos = this.todos.filter((todo)=>{
          return todo.id !==id
        })
      }
    }
  }
</script>
```

##### `MyList.vue`

```vue
<template>
  <ul class="todo-main">
    <!-- :toto 可以向子组件传值。因为是单向数据绑定，所以可以识别后面的js表达式，此处传值为todoObj对象 -->
    <MyItem v-for="todoObj in todos" :key="todoObj.id" :todo="todoObj" :checkTodo="checkTodo" :deleteTodo="deleteTodo">
    </MyItem>
  </ul>
</template>

<script>
  import MyItem from './MyItem.vue'
  export default {
    name: 'MyList',
    components: {
      MyItem
    },
    // 
    props: ['todos', 'checkTodo','deleteTodo'],
    data() {
      return {};
    },
  };
</script>
```

##### `MyItem.vue`

```vue
<template>
  <li>
    <label>
      <!--      这里勾选和取消勾选可以使用change和click作为事件处理-->
      <input type="checkbox" :checked="todo.done" @change="handleCheck(todo.id)" />
      <!--v-model数据的双向绑定，checkbox使用v-model来双向绑定其是否被勾选,也可以实现效果但不推荐(因为其实修改了props中的数据)-->
      <!--这里修改了从List修改过来的props,这里的不允许改是浅层次，就是如果props是一个对象则这个修改这个对象的某一个属性vue是放行的-->
      <!-- <input type=" checkbox" v-model="todo.done" />-->
      <span>{{todo.title}}</span>
    </label>
    <button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
  </li>
</template>

<script>
  export default {
    name: 'MyItem',
    // 接收父组件传的值
    props: ['todo', 'checkTodo'],
    data() {
      return {};
    },
    props:['deleteTodo'],
    methods: {
      handleCheck(id) {
        // 通知App组件将对应的todo对象的done值取反
        this.checkTodo(id)
      },
      handleDelete(id) {
        if (confirm('确定删除吗？')) {
          // 通知App删除对应todo
          this.deleteTodo(id)
        }
      }
    }
  }
</script>
```

#### 1.4 底部统计

##### `MyFooter.vue`

```vue
<template>
  <!--隐式类型转换-->
  <div class="todo-footer">
    <label>
      <!--这里也可用v-model来替代，此时不需要计算属性了-->
      <!--      <input type="checkbox" :checked="isAll" @change="checkAll"/>-->
      <input type="checkbox"/>
    </label>
    <span>
      <span>已完成{{doneTotal}}</span> / 全部{{todos.length}}
    </span>
    <button class="btn btn-danger">清除已完成任务</button>
  </div>
</template>

<script>
  export default {
    name: 'MyFooter',
    props:['todos'],
    computed:{
      doneTotal(){
        const x = this.todos.reduce((pre,current)=>{
            return pre + (current.done ? 1 : 0)
        },0)
        return x
      }
    },
  };
</script>
```

#### 1.5 底部交互

##### `MyFooter.vue`

```vue
<template>
  <!--隐式类型转换-->
  <!-- 没有就不显示底部 -->
  <div class="todo-footer" v-show="total">
    <label>
      <!--这里也可用v-model来替代，此时不需要计算属性了-->
      <input type="checkbox" v-model="isAll"/>
      <!-- <input type="checkbox" :checked="isAll" @change="checkAll"/> -->
    </label>
    <span>
      <span>已完成{{doneTotal}}</span> / 全部{{total}}
    </span>
    <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
  </div>
</template>

<script>
  export default {
    name: 'MyFooter',
    props:['todos','checkAllTodo','clearAllTodo'],
    computed:{
      doneTotal(){
        const x = this.todos.reduce((pre,current)=>{
            return pre + (current.done ? 1 : 0)
        },0)
        return x
      },
      total(){
        return this.todos.length
      },
      isAll:{
        get(){
          return this.doneTotal === this.total && this.doneTotal > 0
        },
        set(value){
          this.checkAllTodo(value)
        }
      }
    },
    methods:{
      clearAll(){
        this.clearAllTodo()
      }
    }
  };
</script>
```

### 2. Todo-List案例总结

![image-20230314174406175](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251750348.png)

### 3. TodoList_本地存储

#### `App.vue`

```vue
<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader :addTodo="addTodo"></MyHeader>
        <MyList :todos="todos" :checkTodo="checkTodo" :deleteTodo="deleteTodo"></MyList>
        <MyFooter :todos="todos" :checkAllTodo="checkAllTodo" :clearAllTodo="clearAllTodo"></MyFooter>
      </div>
    </div>
  </div>
</template>

<script>
  import MyHeader from './components/MyHeader'
  import MyFooter from './components/MyFooter'
  import MyList from './components/MyList'
  export default {
    name: 'App',
    components: {
      MyHeader,
      MyFooter,
      MyList
    },
    data() {
      return {
        // 一堆要做的事
        todos:JSON.parse(localStorage.getItem('todos')) || []
      };
    },
    methods: {
      // 添加todo
      addTodo(todo) {
        this.todos.unshift(todo)
      },
      // 勾选框
      checkTodo(id) {
        this.todos.forEach(todo => {
          if (todo.id === id) todo.done = !todo.done
        });
      },
      // 删除一个todo
      deleteTodo(id){
        this.todos = this.todos.filter((todo)=>{
          return todo.id !==id
        })
      },
      // 全选or取消全选
      checkAllTodo(done){
        this.todos.forEach((todo)=>{
          todo.done = done
        })
      },
      // 清除所有已完成的todo
      clearAllTodo(){
        this.todos = this.todos.filter((todo)=>{
          return !todo.done
        })
      }
    }
    ,
    watch:{
      todos:{
        // 深度监视
        deep:true,
        handler(value){
          localStorage.setItem('todos',JSON.stringify(value))
        }
      }
    }
  }
</script>
```

### 4. TodeList_自定义事件

#### `App.vue`

```vue
<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader @addTodo="addTodo"></MyHeader>
        <MyList :todos="todos" :checkTodo="checkTodo" :deleteTodo="deleteTodo"></MyList>
        <MyFooter :todos="todos" @checkAllTodo="checkAllTodo" @clearAllTodo="clearAllTodo"></MyFooter>
      </div>
    </div>
  </div>
</template>

<script>
  import MyHeader from './components/MyHeader'
  import MyFooter from './components/MyFooter'
  import MyList from './components/MyList'
  export default {
    name: 'App',
    components: {
      MyHeader,
      MyFooter,
      MyList
    },
    data() {
      return {
        // 一堆要做的事
        todos:JSON.parse(localStorage.getItem('todos')) || []
      };
    },
    methods: {
      // 添加todo
      addTodo(todo) {
        this.todos.unshift(todo)
      },
      // 勾选框
      checkTodo(id) {
        this.todos.forEach(todo => {
          if (todo.id === id) todo.done = !todo.done
        });
      },
      // 删除一个todo
      deleteTodo(id){
        this.todos = this.todos.filter((todo)=>{
          return todo.id !==id
        })
      },
      // 全选or取消全选
      checkAllTodo(done){
        this.todos.forEach((todo)=>{
          todo.done = done
        })
      },
      // 清除所有已完成的todo
      clearAllTodo(){
        this.todos = this.todos.filter((todo)=>{
          return !todo.done
        })
      }
    }
    ,
    watch:{
      todos:{
        // 深度监视
        deep:true,
        handler(value){
          localStorage.setItem('todos',JSON.stringify(value))
        }
      }
    }
  }
</script>
```

#### `MyHeader.vue`

```vue
<template>
  <div class="todo-header">
    <input type="text" placeholder="请输入你的任务名称，按回车键确认" v-model="title" @keyup.enter="add"/>
  </div>
</template>

<script>
  import {nanoid} from 'nanoid'
  export default {
    name: 'MyHeader',
    props:['addTodo'],
    data() {
      return {
        title:'',
      };
    },
    methods:{
      // 添加
      add(){
        // 数据校验
        if(!this.title.trim()) return alert('输入不能为空') 
        // 将用户的输入包装成一个todo对象
        const todoObj = {id:nanoid(),title:this.title,done:false}
        // 通知App组件去添加一个todo对象
        this.$emit('addTodo',todoObj)
        // 清空input框
        this.title = ''
      }
    }
  };
</script>
```

#### `MyFooter.vue`

```vue
<template>
  <!--隐式类型转换-->
  <!-- 没有就不显示底部 -->
  <div class="todo-footer" v-show="total">
    <label>
      <!--这里也可用v-model来替代，此时不需要计算属性了-->
      <input type="checkbox" v-model="isAll"/>
      <!-- <input type="checkbox" :checked="isAll" @change="checkAll"/> -->
    </label>
    <span>
      <span>已完成{{doneTotal}}</span> / 全部{{total}}
    </span>
    <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
  </div>
</template>

<script>
  export default {
    name: 'MyFooter',
    props:['todos',],
    computed:{
      doneTotal(){
        const x = this.todos.reduce((pre,current)=>{
            return pre + (current.done ? 1 : 0)
        },0)
        return x
      },
      total(){
        return this.todos.length
      },
      isAll:{
        get(){
          return this.doneTotal === this.total && this.doneTotal > 0
        },
        set(value){
          this.$emit('checkAllTodo',value)
        }
      }
    },
    methods:{
      clearAll(){
        this.$emit('clearAllTodo')
      }
    }
  };
</script>
```

### 5. TodoList_全局事件总线

#### `App.vue`

```vue
<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader @addTodo="addTodo"></MyHeader>
        <MyList :todos="todos"></MyList>
        <MyFooter :todos="todos" @checkAllTodo="checkAllTodo" @clearAllTodo="clearAllTodo"></MyFooter>
      </div>
    </div>
  </div>
</template>

<script>
  import MyHeader from './components/MyHeader'
  import MyFooter from './components/MyFooter'
  import MyList from './components/MyList'
  export default {
    name: 'App',
    components: {
      MyHeader,
      MyFooter,
      MyList
    },
    data() {
      return {
        // 一堆要做的事
        todos:JSON.parse(localStorage.getItem('todos')) || []
      };
    },
    methods: {
      // 添加todo
      addTodo(todo) {
        this.todos.unshift(todo)
      },
      // 勾选框
      checkTodo(id) {
        this.todos.forEach(todo => {
          if (todo.id === id) todo.done = !todo.done
        });
      },
      // 删除一个todo
      deleteTodo(id){
        this.todos = this.todos.filter((todo)=>{
          return todo.id !== id
        })
      },
      // 全选or取消全选
      checkAllTodo(done){
        this.todos.forEach((todo)=>{
          todo.done = done
        })
      },
      // 清除所有已完成的todo
      clearAllTodo(){
        this.todos = this.todos.filter((todo)=>{
          return !todo.done
        })
      }
    }
    ,
    watch:{
      todos:{
        // 深度监视
        deep:true,
        handler(value){
          localStorage.setItem('todos',JSON.stringify(value))
        }
      }
    }
    ,
    mounted(){
      this.$bus.$on('checkTodo',this.checkTodo)
      this.$bus.$on('deleteTodo',this.deleteTodo)
    },
    beforeDestroy(){
      this.$bus.$off('checkTodo')
      this.$bus.$off('deleteTodo')
    }
  }
</script>
```

#### `MyItem.vue`

```vue
<template>
  <li>
    <label>
      <!--      这里勾选和取消勾选可以使用change和click作为事件处理-->
      <input type="checkbox" :checked="todo.done" @change="handleCheck(todo.id)" />
      <!--v-model数据的双向绑定，checkbox使用v-model来双向绑定其是否被勾选,也可以实现效果但不推荐(因为其实修改了props中的数据)-->
      <!--这里修改了从List修改过来的props,这里的不允许改是浅层次，就是如果props是一个对象则这个修改这个对象的某一个属性vue是放行的-->
      <!-- <input type=" checkbox" v-model="todo.done" />-->
      <span>{{todo.title}}</span>
    </label>
    <button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
  </li>
</template>

<script>
  export default {
    name: 'MyItem',
    // 接收父组件传的值
    props: ['todo'],
    data() {
      return {};
    },
    methods: {
      handleCheck(id) {
        // 通知App组件将对应的todo对象的done值取反
        this.$bus.$emit('checkTodo', id)
      },
      handleDelete(id) {
        if (confirm('确定删除吗？')) {
          // 通知App删除对应todo
          this.$bus.$emit('deleteTodo', id)
        }
      }
    }
  }
</script>
```

### 6. TodoList_pubsub

#### `App.vue`

```vue
<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader @addTodo="addTodo"></MyHeader>
        <MyList :todos="todos"></MyList>
        <MyFooter :todos="todos" @checkAllTodo="checkAllTodo" @clearAllTodo="clearAllTodo"></MyFooter>
      </div>
    </div>
  </div>
</template>

<script>
  import pubsub from 'pubsub-js'
  import MyHeader from './components/MyHeader'
  import MyFooter from './components/MyFooter'
  import MyList from './components/MyList'
  export default {
    name: 'App',
    components: {
      MyHeader,
      MyFooter,
      MyList
    },
    data() {
      return {
        // 一堆要做的事
        todos:JSON.parse(localStorage.getItem('todos')) || []
      };
    },
    methods: {
      // 添加todo
      addTodo(todo) {
        this.todos.unshift(todo)
      },
      // 勾选框
      checkTodo(id) {
        this.todos.forEach(todo => {
          if (todo.id === id) todo.done = !todo.done
        });
      },
      // 删除一个todo
      deleteTodo(msgName,id){
        this.todos = this.todos.filter((todo)=>{
          return todo.id !== id
        })
      },
      // 全选or取消全选
      checkAllTodo(done){
        this.todos.forEach((todo)=>{
          todo.done = done
        })
      },
      // 清除所有已完成的todo
      clearAllTodo(){
        this.todos = this.todos.filter((todo)=>{
          return !todo.done
        })
      }
    }
    ,
    watch:{
      todos:{
        // 深度监视
        deep:true,
        handler(value){
          localStorage.setItem('todos',JSON.stringify(value))
        }
      }
    }
    ,
    mounted(){
      this.$bus.$on('checkTodo',this.checkTodo)
      this.pubId = pubsub.subscribe('deleteTodo',this.deleteTodo)
    },
    beforeDestroy(){
      this.$bus.$off('checkTodo')
      pubsub.unsubscribe(thie.pubId)
    }
  }
</script>
```

#### `MyItem.vue`

```vue
<template>
  <li>
    <label>
      <!--      这里勾选和取消勾选可以使用change和click作为事件处理-->
      <input type="checkbox" :checked="todo.done" @change="handleCheck(todo.id)" />
      <!--v-model数据的双向绑定，checkbox使用v-model来双向绑定其是否被勾选,也可以实现效果但不推荐(因为其实修改了props中的数据)-->
      <!--这里修改了从List修改过来的props,这里的不允许改是浅层次，就是如果props是一个对象则这个修改这个对象的某一个属性vue是放行的-->
      <!-- <input type=" checkbox" v-model="todo.done" />-->
      <span>{{todo.title}}</span>
    </label>
    <button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
  </li>
</template>

<script>
  import pubsub from 'pubsub-js'
  export default {
    name: 'MyItem',
    // 接收父组件传的值
    props: ['todo'],
    data() {
      return {};
    },
    methods: {
      handleCheck(id) {
        // 通知App组件将对应的todo对象的done值取反
        this.$bus.$emit('checkTodo', id)
      },
      handleDelete(id) {
        if (confirm('确定删除吗？')) {
          // 通知App删除对应todo
          pubsub.publish('deleteTodo',id)
        }
      }
    }
  }
</script>
```

## 三、浏览器本地存储

![image-20230314204954192](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251750349.png)

### 1. localStorage

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>localStorage</title>
</head>
<body>
    <h2>localStorage</h2>
    <button onclick="saveData()">点我保存一个数据</button>
    <button onclick="readData()">点我读取一个数据</button>
    <button onclick="deleteData()">点我删除一个数据</button>
    <button onclick="clearData()">点我清空数据</button>
    <script>
        let p = {name:'cjj',age:18}
        function saveData(){
            localStorage.setItem('msg', 'helloWorld')
            localStorage.setItem('msg2', 321)
            localStorage.setItem('person', JSON.stringify(p))
        }

        function readData(){
            console.log(localStorage.getItem('msg'));
            console.log(localStorage.getItem('msg2'));
            const result = localStorage.getItem('person')
            console.log(JSON.parse(result));
        }
        
        function deleteData(){
            localStorage.removeItem('msg')
        }
        
        function clearData(){
            localStorage.clear()
        }
    </script>
</body>
</html>
```

### 2. sessionStorage

```html
<!DOCTYPE html>re
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>sessionStorage</title>
</head>
<body>
    <h2>sessionStorage</h2>
    <button onclick="saveData()">点我保存一个数据</button>
    <button onclick="readData()">点我读取一个数据</button>
    <button onclick="deleteData()">点我删除一个数据</button>
    <button onclick="clearData()">点我清空数据</button>
    <script>
        let p = {name:'cjj',age:18}
        function saveData(){
            sessionStorage.setItem('msg', 'helloWorld')
            sessionStorage.setItem('msg2', 321)
            sessionStorage.setItem('person', JSON.stringify(p))
        }

        function readData(){
            console.log(sessionStorage.getItem('msg'));
            console.log(sessionStorage.getItem('msg2'));
            const result = sessionStorage.getItem('person')
            console.log(JSON.parse(result));
        }
        
        function deleteData(){
            sessionStorage.removeItem('msg')
        }
        
        function clearData(){
            sessionStorage.clear()
        }
    </script>
</body>
</html>
```

## 四、组件的自定义事件

### 1. 绑定

```vue
<template>
  <div class="app">
    <h1>{{msg}}</h1>
    <!-- 通过父组件给子组件传递函数类型的props实现：子给父传数据 -->
    <School :getSchoolName="getSchoolName" />
    <!-- 通过父组件给子组件绑定一个自定义事件实现：子给父传数据（第一种写法：使用@或者v-on） -->
    <!-- <Student v-on:cjj="getStudentName" /> -->
    
    <!-- 通过父组件给子组件绑定一个自定义事件实现：子给父传数据（第二种写法：使用ref） -->
    <Student ref="student" />
  </div>
</template>

<script>
  import Student from "@/components/Student";
  import School from "@/components/School";
  export default {
    name: "App",
    components: {
      School,
      Student,
    },
    data() {
      return {
        msg: 'hello',
      };
    },
    methods:{
      getSchoolName(name){
        console.log(name);
      },
      getStudentName(name){
        console.log('app收到了学生名：'+name);
      }
    },
    mounted(){
      this.$refs.student.$on('cjj',this.getStudentName)
      // 绑定一次事件
      this.$refs.student.$once('cjj',this.getStudentName)
    }
  }
</script>
```

### 2. 解绑

```vue
<template>
  <div class="student">
    <h2 class="title">姓名:{{  name }}</h2>
    <h2>性别: {{ sex }}</h2>
    <h2>{{number}}</h2>
    <button @click="add">number++</button>
    <button @click="sendStudentName">把学生名给App</button>
    <button @click="unbind" >解绑cjj事件</button>
    <button @click="death" >销毁组件实例对象（vc）</button>
  </div>
</template>

<script>
  export default {
    name: "Student",
    data() {
      return {
        name: '张三',
        sex: '男',
        number: 0
      }
    },
    methods:{
      sendStudentName(){
        this.$emit('cjj',this.name)
        this.$emit('demo')
      },
      unbind(){
        // 解绑一个自定义事件
        this.$off('cjj')
        // 解绑多个自定义事件
        // this.$off(['cjj','demo'])
        // 解绑所有的事件
        // this.$off()
      },
      add(){
        this.number++
      },
      death(){
        // 销毁了当前Student组件的实例，销毁后所有Student实例的自定义事件全都不奏效
        this.$destroy();
      }
    }
  }
</script>
```

### 3. 总结

![image-20230314230956034](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251750350.png)

## 五、全局事件总线（GlobalEventBus）

![image-20230315122742323](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251750351.png)

## 六、消息订阅与发布_pubsub

> - pubsub.js
>   - publish：发布
>   - subscribe：订阅

![image-20230315131036198](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251750352.png)

## 七、过渡和动画

