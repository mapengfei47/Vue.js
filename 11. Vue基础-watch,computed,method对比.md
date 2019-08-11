# watch,computed,methods对比

[TOC]

> 分别使用method,watch,computed三种方法
>
> 来实现`firstName`文本框 + `lastName`文本框自动生成`fullName`文本框

## 一.methods

~~~html
<div id="app">
    <input type="text" placeholder="firstName" v-model="firstName" @keyup="getFullName">+
    <input type="text" placeholder="lastName" v-model="lastName" @keyup="getFullName">=
    <input type="text" placeholder="fullName" v-model="fullName" disabled>
</div>
<script type="text/javascript">
    var v = new Vue( {
        el: "#app",
        data: {
            firstName:'',
            lastName:'',
            fullName:''
        },
        methods: {
            getFullName(){
                this.fullName = this.firstName + this.lastName;
            }
        }
    } )
</script>
~~~



## 二.watch

~~~html
<div id="app">
    <input type="text" v-model="firstName">+
    <input type="text" v-model="lastName">=
    <input type="text" v-model="fullName" disabled>
</div>
<script type="text/javascript">
    var v = new Vue( {
        el: "#app",
        data: {
            firstName:'',
            lastName:'',
            fullName:''
        },
        // methods: {
        //     getFullName(){
        //         this.fullName = this.firstName + this.lastName;
        //     }
        // }
        watch: {
            firstName:function(newName,oldName){
                this.fullName = newName + this.lastName;
            },
            lastName:function(newLast){
                this.fullName = this.firstName + newLast;
            }
        }
    } )
</script>
~~~



## 三.computed

~~~html
<div id="app">
    <input type="text" v-model="firstName">+
    <input type="text" v-model="lastName">=
    <input type="text" v-model="fullName" disabled>
</div>
<script type="text/javascript">
    var v = new Vue( {
        el: "#app",
        data: {
            firstName:'',
            lastName:''
            // fullName:''
        },
        // methods: {
        //     getFullName(){
        //         this.fullName = this.firstName + this.lastName;
        //     }
        // }
        // watch: {
        //     firstName:function(newName,oldName){
        //         this.fullName = newName + this.lastName;
        //     },
        //     lastName:function(newLast){
        //         this.fullName = this.firstName + newLast;
        //     }
        // }
        computed: {
            fullName:function(){
                return this.firstName + this.lastName;
            }
        }
    } )
</script>
~~~



## 四.三种方式对比

### 不同点：

- **<font color='red'>computed：</font>**
  - `computed`**一定会有一个返回值**
  - `computed`定义的函数用来**当作属性来用**
  - `computed`主要用来**监听现有的属性**，并且将它们进行拼装，**返回一个新的值，可通过函数名调用该属性**
  - `computed`属性的结果会被缓存，**除非依赖的响应式属性发生变化，才会重新计算值**
- **<font color='red'>watch：</font>**
  - 主要是用来**监听数据的变化**
  - 第一个是新值，第二个入参是之前的值
  - 可以看作**computed和methods的组合体**
  - 使用**键值对**的方式定义，键是要监听的属性名，值是一个方法
- **<font color='red'>methods：</font>**
  - `methods`方法表示一个具体的操作，主要书写业务逻辑；

### 相同点：

- 都可以用来监听数据的变化，并且及时的对数据的改变做出响应