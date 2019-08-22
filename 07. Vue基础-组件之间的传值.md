# Vue基础-组件之间的传值

## 一.父组件向子组件传值

> 在组件中，可以通过 props属性来接收通过 v-bind传递进来的参数

~~~html
<div id="app">
    <son v-bind:parentparam="message"></son>
</div>
<script type="text/javascript">

    var son = {
        props: ["parentparam"],
        template:`<h1>这是子组件----{{parentparam}}</h1>`
    }

    var vm = new Vue({
        el: "#app",
        data: {
            message: "hello world!"
        },
        components: {
            son
        }
    })
~~~

## 二.子组件向父组件传值

> 子组件向父组件传值相对麻烦一些

~~~HTML
<div id="app">
    <son v-bind:parentparam="message" @func="getSonMsg"></son>
</div>

<template id="temp">
    <div>
        <h1>这是子组件---{{parentparam}}</h1>
        <input type="button" value="向父组件传值" @click="sendMsg">
    </div>

</template>

<script type="text/javascript">

    var son = {
        props: ["parentparam"],
        template:'#temp',
        data () {
            return {
                sonMsg:"好好学习，天天向上！"
            }
        },
        methods:{
            sendMsg(){
                this.$emit("func",this.sonMsg);
            }
        }
    }

    var vm = new Vue({
        el: "#app",
        data: {
            message: "hello world!",
            msgFromSon:""
        },
        methods: {
            getSonMsg(data){
                this.msgFromSon = data;
                console.log(data);
            }
        },
        components: {
            son
        }
    })
</script>
~~~

**<font color='red'>重要传值步骤</font>**

~~~html
//子组件事件方法触发的事件，给父组件用来调用接受数据的方法
<div id="app">
        <son v-bind:parentparam="message" @func="getSonMsg"></son>
    </div>

//子组件模版中定义触发传值的事件
<template id="temp">
    <div>
        <h1>这是子组件---{{parentparam}}</h1>
        <input type="button" value="向父组件传值" @click="sendMsg">
    </div>
</template>

<script>
    //子组件中定义触发事件，并把子组件中的值传递过去
	var son = {
        props: ["parentparam"],
        template:'#temp',
        data () {
            return {
                sonMsg:"好好学习，天天向上！"
            }
        },
        methods:{
            sendMsg(){
                this.$emit("func",this.sonMsg);
            }
        }
    }
    
    //父组件中定义接收方法
    var vm = new Vue({
        el: "#app",
        data: {
            message: "hello world!",
            msgFromSon:""
        },
        methods: {
            getSonMsg(data){
                this.msgFromSon = data;
                console.log(data);
            }
        },
        components: {
            son
        }
    })
</script>
~~~

