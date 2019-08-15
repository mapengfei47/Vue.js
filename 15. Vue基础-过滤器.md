# Vue-过滤器

在显示参数前，为参数做统一的过滤处理

## 1. 全局注册过滤器

**全局注册的过滤器，所有Vue实例都可以共享**

- 回调函数的**第一个参数默认**为是过滤对应的数据
- 过滤器其它的参数可通过调用过滤器的时候当做参数传递进来

~~~HTML
<p>{{item | itemFilter("extraMsg")}}</p>
~~~

~~~js
Vue.Filter("过滤器名字"，function（item，extraMsg）{
           //过滤逻辑
           })
~~~



## 2. 局部注册的过滤器

**局部注册的过滤器，只能在实例内部使用，别的实例无法使用**

过滤器遵循**就近原则**，如果局部和全部同时拥有相同名字的过滤器，则实例采用局部的过滤器

~~~HTML
<div id="app">
    <p>
       {{msg | msgFilter}}
    </p>
</div>
~~~

~~~js
var app = new Vue({
    el:"#app",
    data:{
        msg:"filter test"
    },
    filters:{
        msgFilter:function(msg){
            //处理逻辑
        }
    }
})
~~~



## 3. 过滤器的传值

过滤器可以通过在过滤方法的括号中传递参数，来达到传值的目的

过滤器的第一个参数默认为添加过滤器的元素，即如下的msg

~~~HTML
<div id="app">
    <p>
       {{msg | msgFilter(param)}}
    </p>
</div>
~~~

~~~js
var app = new Vue({
    el:"#app",
    data:{
        msg:"filter test"
    },
    filters:{
        msgFilter:function(msg,param){
            //处理逻辑
        }
    }
})
~~~

