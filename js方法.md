# JS方法

## 1. Object.keys()

> - Object.keys()
>   - 作用：把传入对象的所有属性名提取出来，返回一个数组。
>   - 参数
>     - Object.keys(要传的  对象)

## 2. Object.defineProperty()

> - Object.defineProperty()
>   - 作用：给一个对象添加/定义属性
>   - 参数
>     - Object.defineProperty(要添加属性的对象，'属性名'，配置对象`{}`)
>   - 配置对象的属性
>     - `value`：属性值
>     - `enumerable`
>       - true：控制属性是否可以枚举，默认false
>     - `writable`
>       - true：控制属性是否可以被修改，默认为false
>     - `configurable`
>       - true：控制属性是否可以被删除，默认为false
>     - `get(){return 返回值}`：当有人读取age属性时，get函数（getter）会被调用，返回值就是age的值
>     - `set(value){}`：当有人修改age属性时，set函数（setter）就会被调用，且会收到具体修改的值传到value参数中

## 3. event.preventDefault()

> - event.preventDefault()
>   - 作用：阻止事件默认行为

## 4. Array.splice()

> -  Array.splice()
>   - 作用：修改数组内元素
>   - 参数：`Array.splice(第几项, 操作几项, '修改后值(操作几项就写几个用逗号隔开)')`
>   - 例子：`hobby.splice(0,2,'开车','喝酒')`
>     - 把hobby数组的第一个元素修改为'开车'，第二个元素修改为'喝酒'

## 5. Array.prototype.reduce()

> - 参数
>   - pre：初始值为0，函数体循环todos.length()次，pre的值从第二次开始，为上一次循环的return值
>   - current：遍历的数组元素值，此处为todo对象
> - 分析
>   -  todos数组长度为3，函数一共执行三次，最后一次执行函数的返回值作为`x`的返回值。
>   - 每次循环判断todo里的属性done是否为true。为ture返回1，反之为0。从而筛选出属性值done全为true的元素的个数最后作为返回值给x

```vue
<script>
        const x = this.todos.reduce((pre,current)=>{
            return pre + (current.done ? 1 : 0)
        },0)
</script>
```

## 6.filter()

> - 作用：默认筛选结果为true的元素组成新的数组