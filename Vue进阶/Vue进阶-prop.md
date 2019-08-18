## prop

### 1. prop属性值的大小写

HTML中属性名的大小写是**不敏感**的，浏览器会将标签中的所有大写字符解释为小写，这就意味着，在使用DOM模板时，驼峰命令法的属性名要**转换为对应的分隔线的小写形式**

~~~HTML
<div id="app">
    <prop-test some-attr="message"></prop-test>
</div>
~~~

~~~js
Vue.component("prop-test",{
				props:["someAttr"],
				template:"<h3>{{someAttr}}</h3>"
			})
var app = new Vue({
    el:"#app",
    data:{
        message:"hello world!"
    }
})
~~~



### 2. prop类型

目前为止，我们前面使用的prop类型都为字符串类型，如下所示

~~~js
props:["id","title","firstName","lastName"]
~~~

但是，某些时候，我们需要给参数指定具体的数值类型，这个时候，可以以对象的形式列出这些prop，这些属性的名称和值分别是prop各自的名称和属性

~~~js
props:{
    title:String,
    likes:Number,
    author:Object,
    callback:Function
}
~~~



### 3. prop传值

prop 传值的方式有两种，静态传值和动态传值

- 静态传值：直接在模板上设置相应的属性，并且传递值
- 动态传值：通过v-bind：指令，动态绑定属性和值

~~~HTML
<div id="app">
    <!-- 静态传值 -->
    <prop-test some-attr="hello message!"></prop-test>

    <!-- 动态绑定传值 -->
    <prop-test v-bind:some-attr="message"></prop-test>
</div>
~~~

根据传值的类型，prop存在一些特殊的处理

1. **布尔类型**

   - 只输入属性名，不指定值的时候，默认为true

     ~~~html
     <!-- 包含该 prop 没有值的情况在内，都意味着 `true`。-->
     <blog-post is-published></blog-post>
     ~~~

2. **数组**

   -  传入数组的时候，可以将数组直接指定在标签里面

     ~~~html
     <blog-post ids="["123","abc","666"]"></blog-post>
     ~~~

3. **对象**

   - 对象传入方式类似于数组

     ~~~html
     <blog-post author="{
                name:'鲁迅',
                country:'china'
                }"></blog-post>
     ~~~

4. **传入一个对象的所有属性**

   - 如果想要将一个对象的所有属性都当做prop传入，则可以使用 `v-bind` 取代 `v-bind：ObjectName`

     ~~~js
     post:{
         id:'001',
         name:'Vue'
     }
     ~~~

     ~~~HTML
     <blog-post v-bind="post"></blog-post>
     ~~~

     等价于：

     ~~~HTML
     <blog-post
       v-bind:id="post.id"
       v-bind:name="post.name"
     ></blog-post>
     ~~~



### 4. 单向数据流

所有的prop都使得其父子prop之间形成一个**单向下行绑定**：父级prop的更新会下流到子组件中，反过来则不行。这样会防止子组件意外改变父组件的状态

**eg1：这个prop用来传递一个初始值，这个子组件希望将这个初始值作为组件私有的属性来使用，**这总情况下，最好为组件定义一个data属性，并将这个prop作为其初始值，而不是直接使用该值

~~~js
props:["initCounter"],
data:function(){
    return {
        conunter:this.initCounter
    }
}
~~~

**eg2：这个prop以一种原始的值传入，并且需要转换**，这种情况下，最好使用这个prop的值来定义一个计算属性

~~~js
props:["size"],
computed:{
    normalizedSize:function(){
        return this.size.trim().toLowerCase() 
    }
}
~~~

**注意：**对象和数组是通过**引用传递**的，所以对于一个对象或者数组类型的prop来说，在子组件中改变这个对象或者数组**本身**会影响到父组件的状态



### 5. prop验证

我们可以为组件的prop指定验证要求，当需求没有被满足时，会在控制台中发出警告

为了定制prop的验证方式，你可以为props中的值提供一个带有验证需求的对象

~~~js
Vue.component('my-component', {
  props: {
    // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
~~~

当验证失败的时候，Vue会在控制台产生一个警告

**注意：**哪些prop会在一个组件实例创建之前验证，所以实例的属性（如data，computed等）在default或者validator函数中是不可用的



**类型检查：**type可以是一下原生构造函数中的一个

- String
- Number
- Boolean
- Array
- Object
- Date
- Function
- Symbol

额外的，type还可以是一个自定义的构造函数，并且通过 `instanceof`来进行确认检查

eg：验证author的prop值是否通过new Person 创建的

~~~js
function Person(fristName,lastName){
    this.firstName = fristName;
    this.lastName = lastName;
}
~~~

~~~js
 Vue.component("blog-post",{
     props:{
         author:Person
     }
 })
~~~



### 6. 非Prop特性

一个非prop特性是指传向一个组件，但是该组件内并没有相应prop定义的特性。

