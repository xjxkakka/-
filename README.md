### vite 的基本使用    components自动导入的后缀 .vue 一定要自己加上
1.创建 vite 的项目
按照顺序执行如下的命令，即可基于vite创建 vue 3.x 的工程化项目:
1.`npm create vite`

2.`cd 项目名称`
3.`npm install`
4.`npm run dev`
5.看需要，安装less语法包 `npm i less -D`
##组件的基本使用
##3.组件的props
为了提高组件的复用性，在封装vue组件时需要遵守如下we的原则:
·组件的DOM结构、Style 样式要尽量复用
·组件中要展示的数据，尽量由组件的使用者提供

为了方便使用者为组件提供要展示的数据，vue组件提供了props 的概念 

##3.1 什么是组件的 props
 props是组件的自定义属性，组件的使用者可以通过 props 把数据传递到子组件的内部，供子组件内部进行使用。
 代码示例如下:
 <!-- 通过自定义 props,把文章的标题和作者，传递到 my-article 组件中 -->
 ` <my-article title="面朝大海，春暖花开" author="海子"</my-article> `
 props 的作用：父组件通过 props 向子组件传递要展示的数据。
 props 的好处：提高了组件的复用性。
 
##3.2 在组件中声明 props
在封装 vue 组件时，可以把动态的数据项声明为 props 自定义属性。自定义属性可以在当前组件的模板结构中被直接使用。
示例代码如下：
````<!-- my-article 组件的定义如下: -->
<template>
<h3>标题：{{title}}</h3>
<h5>标题：{{author}}</h5>
</template>
<script>
export default {
props:['title','author'], //父组件传递给 my-article 组件的数据，必须在 props 节点中声明
}
</script>
````

路由重定向: redirect
###4.1 动态绑定HTML 的 class
可以通过三元表达式，动态的为元素绑定 class 的类名。 示例代码如下:
```
<h3 class="thin" :class="isItalic ? 'italic' : "'">MyDeep组件</h3>
<button @click=" isItalic=!isItalic ">Toggle Italic</button>

setup() {
let isItalic = ref(true);
return { isItalic}
}

.thin { //字体变细
font-weight: 200;
}
.italic {//倾斜字体
font-style: italic;
}
```

###4.2 以数组语法绑定 HTML 的class 不推荐使用
如果元素要动态绑定多个class的类名，此时可以使用数组的语法格式：
`<h3  class="thin" :class="[isShow ? 'italic':'' , isItalic ? 'isItalic':'']">MyStyle 组件</h3>
`

###4.3 以对象语法绑定 HTML 的class
使用数组语法动态绑定 class 会导致模板结构臃肿的问题。此时可以使用对象语法进行简化：
```
<h3 class="thin" :class="classobj">MyDeep组件</h3>
<button @click="class0bj.italic = !class0bj.italic">Toggle Italic</button>
<button @click="class0bj.delete = !classObj.delete">Toggle Delete</button>

setup() {
let classObj = reactive{(
    italic:false,
    delete:false
)};
return {
class0bj
}
```

###4.4 以对象语法绑定内联的 style
:style的对象语法十分直观——看着非常像CSS，但其实是一个JavaScript对象。
CSS property名可以用驼峰式(camelCase)或短横线分隔(kebab-case，记得用引号括起来)来命名:
```<div :style=" {color: active,fontSize: fsize + 'px ','background-color ': bgcolor}">
黑马程序员</div>
<button @click="fsize += 1">字号+1</button>
<button @click="fsize -= 1">字号-1</button>
setup() {
return {
active: 'red' ,
fsize: 30,
bgcolor : 'pink ',
}
}
这里不完整，记得添加 reactive
```

## Code2
## props验证
1.什么是 props 验证
指的是：在封装组件时对外界传递过来的 props 数据进行了合法性的校验，从而防止数据不合法的问题。
    使用数组类型的 props 节点的缺点：无法为每个 prop 指定具体的数据类型。

2.对象类型的 props 节点
使用对象类型的 props节点，可以对每个 prop 进行数据类型的校验。
示例如下：
````
props:{
count:Number,
state:Boolean
}
````

3.3 必填项校验
如果组件的某个 prop 属性是必填项，必须让组件的使用者为其传递属性的值，
此时可以通过如下的方式将其设置为必填项：
```
props:{
    //通过 配置对象 的形式，来定义 propB 属性的验证规则
    propB:{
        type:String, //当前属性的值必须是 String 字符串类型
        required:true //当前属性的值是必填项，如果使用者没指定 propB 属性的值，则在终端进行警告提示
}
}
```
####3.4 属性默认值
在封装组件时，可以为某个 prop 属性指定默认值。示例代码如下：
```
props:{
    //通过配置对象的形式，来定义 propC 属性的 验证规则
    propC:{
        type:Number,
        default:100  //如果使用者没有指定 propC 的值，则 propC 属性的默认值为 100
    }
}
```
3.5 自定义验证函数
在封装组件时，可以为prop属性指定自定义的验证函数，从而对 prop 属性的值进行更加精确的控制：
```
props:{
    //通过 配置对象的形式，来定义 propD 属性的 验证规则
    propD:{
        //通过 validator 函数，对 propD 属性的值进行校验，属性的值可以通过形参 value 进行接收
        validator(value){
        // propD 属性的值，必须匹配下列字符串中的一个
        // validator 函数的返回值为 true 表示验证通过， false 表示验证失败
        // indexOf 判断有没value是否等于数组中的值，false返回-1
        return ['success','warning','danger'].indexOf(value) !== -1
    }
    }
}
```
###计算属性
1.什么是计算属性
计算属性本质上就是一个function函数，它可以实时监听data中数据的变化，
并return一个计算后的新值，供组件渲染DOM时使用。

2.如何声明计算属性
计算属性需要以function函数的形式声明到组件的computed选项中，示例代码如下:
```
<template>
  <h1>第一个数字</h1>
  <input type="text" v-model="number.a">
  <h1>第二个数字</h1>
  <input type="text" v-model="number.b">
  <h1>a+b: {{ number.sum }}</h1>
</template>

 setup() {
    let number = reactive({
      a: 0,
      b: 0,
      //computed是一个函数
      sum: computed(() => {
        return parseInt(number.a) + parseInt(number.b)
        // return number.a * number.b
      })
    })
    return {number}
  }
```
4.计算属性 vs 方法 
相对于方法来说，计算属性会缓存计算的结果，只有计算属性的依赖项发生变化时，才会重新进行运算。因此计算属性的性能更好:

##自定义事件
####1.什么是自定义事件
在封装组件时，为了让组件的使用者可以监听到组件内状态的变化，此时需要用到组件的自定义事件
####2.自定义事件的3个使用步骤
在封装组件时：
1、声明自定义事件
2、触发自定义事件

在使用组件时：
3、监听自定义事件
####2.1声明自定义事件
开发者为自定义组件封装的自定义事件，必须事先在emits节点中声明，示例代码如下：
```
export default{
    //my-counter 组件的自定义事件，必须事先声明到 emits 节点中
    emits:['change']
}
```
####2.2触发自定义事件
在 emits 节点下声明的自定义事件，Vue3可以通过 context.emit('自定义事件的名称') 方法进行触发，
示例代码如下： 
```
<template>
<div>
  <p>count 的值是: {{count}}</p>
  <button @click="add">+1</button>
</div>
</template>

<script>
import {ref} from 'vue'
export default {
  name: "Counter",
  emits:['countChange'],
  setup(props,context){
    let count = ref(0);
    function add(){
      count++;
      // Vue3中需要context.emit() 触发自定义事件
      //当按钮点击时就会触发自定义事件 countChange
      context.emit('countChange')
    }
    return{add,count}
  }
}
</script>

```
####2.3监听自定义事件
在使用自定义的组件时，可以通过v-on的形式监听自定义事件。示例代码如下：
```
<! --使用v-on指令绑定事件监听-->
//这里的change事件什么时候触发是由上面点击的按钮来决定的
//@countChange="getCount",这里千万不要写@countChange="getCount(value)" 写括号接收参数，如果有传参这里不需要接收
  <Counter @countChange="getCount"></Counter>

setup(){
    function getCount(){
      console.log('自定义事件被触发了')
    }
    return {getCount}
  }
}
```
####2.4自定义事件传参，需要自寻找资料

###组件上的 v-model
####1.为什么需要在组件上给你使用 v-model
我们v-model 是双向数据绑定指令，当需要维护组件内外数据的同步时，可以在组件上使用v-model指令。
####2.在组件上使用v-model的步骤
1.父组件通过 v-bind:属性绑定的形式，把数据传递给子组件
2.子组件中，通过props接收父组件传递过来的数据

## watch 侦听器

```<script>
    import {ref, watch, reactive} from 'vue';

    export default {
        name: "Demo",
        setup() {
            let sum = ref(0);
            let msg = ref('你好啊');
            let person = reactive({
                name: '张三',
                age: 18,
                job: {
                    j1: {
                        salary: 20
                    }
                }
            });
            //情况一:监视ref所定义的一个响应式数据
            //第一个参数:监视的数据,第二个参数：函数返回数据，
            // 第三个可以配置, immediate:true表示开始的时候执行一次
            /*     watch(sum, (newValue, oldValue) => {
                     console.log('sum变了', newValue, oldValue)
                 },{immediate:true});*/

            //情况二:监视ref所定义的多个响应式数据
            /* watch([sum,msg],(newValue,oldValue)=>{
                 console.log('sum或msg变了',newValue,oldValue)
             });*/

            //一般情况下都是用这个，如果特别需要old值，重新定义个ref值
            /*
             情况三:监视reactive所定义的一个响应式数据,注意:此处无法正确的获取oldValue
                 1.注意：此处无法正确的获取oldValue
                 2.注意：强制开启了深度监视(deep配置无效)
            * */
            /*  watch(person, (newValue, oldValue) => {
                  console.log('person变化',newValue, oldValue);
              },{deep:false});//此处的deep配置无效*/

            //情况四：监视reactive所定义的一个响应式数据中的某个属性
            //这里单独监控age，可以获取到新老值
            //   watch(()=>person.age,(newValue, oldValue)=>{
            //       console.log('person的age发生了变化',newValue, oldValue)
            //   });

            //情况五：监视reactive所定义的一个响应式数据中的某个属性
           /* watch([() => person.age, () => person.name], (newValue, oldValue) => {
                console.log('person的age或name发生了变化',newValue,oldValue)
            });*/

           //特殊情况
            //这里监视的job层次比较深，这里需要开启deep
            watch(() => person.job, (newValue, oldValue) => {
                //检测不到老值
                console.log('person的job发生了变化',newValue.j1,oldValue.j1)
            },{deep:true});//此处由于监视的是reactive所定义的对象中的某个属性，所以deep有效
            return {sum, msg, person}
        }
    }
</script>
```

###插槽
####1.什么是插槽
插槽（Slot)是vue为组件的封装者提供的能力。允许开发者在封装组件时，把不确定的、希望由用户指定的部分定义为插槽。
可以把插槽认为是组件封装期间,为用户预留的内容的占位符
####2.体验插槽的基本用法
在封装组件时，可以通过<slot>元素定义插槽，从而为用户预留内容占位符。示例代码如下:
```
<template>
<p>这是 MyCom1组件的第1个p标签</p>
<!--通过 slot标签，为用户预留内容占位符（插槽）-->
<slot></slot>
<p>这是MyCom1组件最后一个p标签</p>
</template>

```
```
<my-com-1>
<!--在使用MyCom1 组件时，为插槽指定具体的内容-->
<p>~~~用户自定义的内容~~~</p>
</my-com-1>
```
####2.1 没有预留插槽的内容会被丢弃
如果在封装组件时没有预留任何<slot>插槽，则用户提供的任何自定义内容都会被丢弃。示例代码如下:
####2.2 后被内容
封装组件时，可以为预留的<slot>插槽提供后备内容（默认内容）。如果组件的使用者没有为插槽提供任何内容，则后备内容会生效。示例代码如下:
```
<template>
<div class="container">
  <h3>Myslot 组件 --- 插槽的基础用法</h3>
  <hr>
  <p>这是第一个p标签</p>
<!--  <p>这里内容不确定就给个slot</p>-->
  <slot>这是后备内容,如果用户没填写则会显示</slot>
  <p>这是最后一个p标签</p>
</div>
</template>
```
####3.具名插槽
如果在封装组件时需要预留多个插槽节点，则需要为每个<slot>插槽指定具体的name 名称。这种带有具体名称的插槽叫做“具名插槽”。示例代码如下:
```
<template>
<div>
<!--  我们希望把页头放到这里-->
  <header>
    <slot name="header"></slot>
  </header>
<!--  我们希望把主要内容放到这里-->
  <main>
<!--    不写name,默认是default-->
    <slot></slot>
  </main>
<!--  我们希望把页脚放到这里-->
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
</template>
```
####3.1 为具名插槽提供内容
在向具名插槽提供内容的时候，我们可以在一个<template>元素上使用v-slot 指令，并以v-slot的参数的形式提供其名称。示例代码如下:
```
  <my-article>
<!--    使用具名插槽-->
    <template #header>
      <h1>头部</h1>
    </template>
<!--    那边定义有个没写name的 就是default-->
    <template v-slot:default>
      我在中间
    </template>
    <template v-slot:footer>
      <h1>底部</h1>
    </template>
  </my-article>
```
####3.2 具名插槽的简写形式
跟v-on和v-bind 一样，v-slot也有缩写，即把参数之前的所有内容(v-slot:)替换为字符#。例如v-slot,header可以被重写为#header
####4.作用域插槽
在封装组件的过程中，可以为预留的<slot>插槽绑定props 数据，这种带有props 数据的<slot>叫做“作用域插槽”。示例代码如下:
```
<!--MtTest组件的代码-->
<template>
<div>
  <h3>这是TEST 组件</h3>
<!--info自定义名字-->
  <slot :info="infomation"  :xingming="name"></slot>
</div>
</template>

<!-- App组件中使用slot的代码 -->
<template>
 <my-test>
<!-- 接收那边传递进来的参数 可以自定义接收名字-->
<!--    自定义的名字可以接收所有传过来的参数-->
    <template #default="scope">
      <p>{{scope.info.address}}</p>
      <p>姓名:{{scope.xingming}}</p>
    </template>
  </my-test>
  </template>

```
####4.1 解构作用域插槽的Prop
同上
```
 <my-test>
    <template #default="{xingming,info}">
      <p>{{info.address}}</p>
      <p>姓名:{{xingming}}</p>
      <p>电话:{{info.phone}}</p>
    </template>
  </my-test>
```

### ref引用
1.什么是ref引用
ref 用来辅助开发者在不依赖于jQuery的情况下，获取DOM元素或组件的引用。
获取代码如下:
```
<template>
  <div>
  <!--    首先用ref绑定一个变量-->
    <h1 ref="myh1">App 跟组件</h1>
    <hr>
    <button class="btn btn-primary" @click="getRefs">获取 ref 引用</button>
  </div>
</template>

<script>
import {ref} from 'vue';
export default {
  name: "App",
  setup() {
    //创建一个ref为 myh1的变量
    //通过ref.value就可以操作DOM了
    let myh1 = ref(null)
    let getRefs = ()=>{
      console.log(myh1.value)
      myh1.value.style.color='red'
    }
    return {getRefs,myh1}
  }
}
</script>
```
#### 使用 ref 引用组件示例
如果想要使用ref引用页面上的组件实例，则可以按照如下的方式进行操作:
```
App页的代码
<template>
  <button class="btn btn-primary" @click="getRefs">获取 ref 引用</button>
<!--    获取子组件中的DOM结构、数据、及方法-->
    <Counter ref="counterRef"></Counter>
    </template>
    
    <script>
     //创建子组件的ref，获取子组件一定要等页面挂载完后才能获取，否则会为空
    let counterRef = ref(null)
    let getRefs = ()=>{
      console.log(counterRef)
      console.log(counterRef.value.reset)
      //当父组件的按钮点击时候执行，子组件的reset方法， 让count 等于0
      //reset()是子组件那边定义的方法，详情请看 09-ref
      counterRef.value.reset();
    }
    </script>
```
####4.控制文本框和按钮的按需切换
通过布尔值inputVisible来控制组件中的文本框与按钮的按需切换。示例代码如下:
[详情请看 09-ref中的RefFocus.vue](day_html/vite/code2/src/components/09-ref/RefFocus.vue)
```
  <input type="text" class="form-control" v-if="inputVisible" ref="ipt">
  <button class="btn btn-primary" v-else @click="showInput">展示 input 输入框</button>

<script>
 let showInput = ()=>{
      inputVisible.value = true
      //通过ref获取DOM节点然后添加获取焦点事件，
      //如果不写定时器，是为null的，因为用了v-if还没刷新出来
      setTimeout((()=>{ipt.value.focus();}),1)
    }
</script>
```
#####5.让文本框自动获得焦点
当文本框展示出来之后，如果希望它立即获得焦点，则可以为其添加ref引用，并调用原生 DOM 对象的 .focus()方法即可。
示例代码如下:
```
 let showInput = ()=>{
      inputVisible.value = true
      //通过ref获取DOM节点然后添加获取焦点事件，
      //如果不写定时器，是为null的，因为用了v-if还没刷新出来
      setTimeout((()=>{ipt.value.focus();}),1)
      使用 nextTIck()方法，不要写定时器
```

###6. nextTick(cb)方法
组件的nextTick(cb)方法，会把 cb 回调推迟到下一个DOM更新周期之后执行。
通俗的理解是∶等组件的DOM异步地重新渲染完成后，再执行cb回调函数。从而能保证cb回调函数可以操作到最新的DOM元素。
```
<script>
 let showInput = ()=>{
      inputVisible.value = true
      //通过ref获取DOM节点然后添加获取焦点事件，
//把对 input 文本款的操作，推迟到下次 DOM 更新之后，否则页面上根本不存在文本框元素
      nextTick(()=>{
      ipt.value.focus();
    })
    }
</script>
```

### 自定义指令
```
定义完后使用
<input type="text" class="form-control"  v-focus>
//自定义指令
  directives:{
    //出现输入框后自动获取焦点 v-focus 自定义
    focus(el){
      el.focus();
    }
  }
```
###路由的详情
[请开todolist3案例，view组件下有路由传参、跳转 在view/Star和view/Home](day_html/vite/todolist3/src/views/Start.vue)
####6.全局控制路由的访问权限
1.在router.js模块中，通过router路由实例对象，全局挂载路由导航守卫:
[全局路由导航守卫详情请看](https://www.jb51.net/article/159959.htm)
```
//全局路由导航守卫
router.beforeEach((to, from, next) => {
    // 如果用户访问的是登录页面，直接放行
    if (to.path === '/login') return next()
    // 获取 前面用户登录后定义的 Token 值
    const token = localStorage.getItem('token')
    if (!token){
        //Token 值不存在，强制跳转登录页面
        return next('/login')
    }
    //存在 Token 值，直接放行
    next();
})
```

路由元信息 meta 自定义对象:
```
{
    path: '/home',
    name: 'Home',
    component: Home,
    meta:{
      show:true
    }
```
```
router.beforeEach((to, from) => {
isShow.value = to.meta.show
})
```

####Proxy 跨域代理
#####1.回顾:接口的跨域问题
vue项目运行的地址: http://localhost:8080/
API 接口运行的地址: https://www.escook.cn/api/users
由于当前的API 接口没有开启 CORS 跨域资源共享，因此默认情况下，上面的接口无法请求成功
vue项目运行在: http://localhost:8080
axios.defaults.baseURL='https://www.escook.cn'
存在跨域问题获取不到数据
#####2.通过代理解决接口的跨域问题
通过 vue-cli 创建的项目在遇到接口跨域问题时，可以通过代理的方式来解决:
1.把axios的请求根路径设置为vue项目的运行地址(接口请求不再跨域)
2.vue 项目发现请求的接口不存在，把请求转交给proxy代理
3.代理把请求根路径替换为 devServer.proxy属性的值，发起真正的数据请求
4.代理把请求到的数据，转发给axios
#####3.在项目中配置 proxy 代理
步骤2，在项目根目录下创建 vue.config.js 的配置文件，并声明如下的配置:
```
module.exports={
    devServer: {
        //当前项目在开发调是阶段，
        //会将任务未知请求(没有匹配到静态的请求) 代理到 https://www.escook.cn
        proxy:'https://www.escook.cn'
    }
}
```
注意:
1.devServer.proxy提供的代理跟你，仅在开发调试阶段生效
2.项目上线发布时，依旧需要API接口服务器开启CORS跨域资源共享

###2.3解决跨域请求限制
[通过 vue.config.js 中的 devServer.proxy 即可在开发环境下将 API 请求代理到 API服务器](day_html/vite/heima-users/vue.config.js)
[调用在 UserList.vue](day_html/vite/heima-users/src/components/UserList.vue)
```
module.exports={
    devServer:{
        //基础设置
        open:true,
        host:'localhost',
        port:3000,
        https:false,
        //开启代理
            proxy:{
            //代理设置，我们不可能所以请求都找代理
            //当访问/api地址的时候都使用代理操作
                '/api':{
                    //写真正的服务器
                    target:'https://www.escook.cn',
                    ws:true,
                    changeOrigin:true,
                    //因为没有访问api，所以要把路径重写，把 /api去掉
                    /*pathRewrite:{
                        '^/api':''
                    }*/
                }
}
    }
}
```
####[Element plus加载动画效果 以及 axios封装的使用](vue3-ts-master3/src/http.ts)

#### [Vuex的使用可以看](day_html/vite/todolist3/src/store/index.js)在TypeNav组件中有使用 [以及](gouwu-app/src/store/index.js)

######Vue3.0项目中使用事件总线传参，
[//先引入第三方插件mitt](gouwu-app/src/components/Header/index.vue)
[详情看这两个组件](gouwu-app/src/views/Search/Search.vue)
```
npm install mitt
```
1.先建立EventBus.js的文件
```
import mitt from 'mitt'
export default new mitt();
```
2.//引入EventBus.js  全局组件通信插件
```
import EventBus from "@/EventBus";

//使用 emit	调用方法发起数据传递
  EventBus.emit("clear")
```
//3.另一个组件监听事件，用于接收数据
```
//on  注册一个监听事件，用于接收数据
  EventBus.on('clear',()=>{
      keyword.value = '';
    })
```
4.off   用来移除监听事件

###### swiper的使用
[swiper的使用可以看gouwu-app](gouwu-app/src/views/ListContainer/ListContainer.vue)

###### 分页器的手写
[分页器的具体代码看gouwu-app](gouwu-app/src/components/Pagination/index.vue)


######父子传参因为vuex中发送请求不能实时获取到可以参考
[可以参考gouwu-app](gouwu-app/src/views/Detail/Detail.vue)

#####watch监听props
直接写props.skuImageList是监听不到的
```
 watch(()=>props.skuImageList,(newValue,oldValue)=>{
      console.log(newValue,oldValue)
      //如果传完数据回来，等渲染完在执行下面的函数
      nextTick(()=>{
        new Swiper (cur.value, {
          slidesPerView : 2,

          // 如果需要前进后退按钮
          navigation: {
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev',
          },
        })
      })
    })
```

#####放大镜代码可以看gouwu-app
[代码看这里](gouwu-app/src/views/Detail/Zoom/Zoom.vue)

#####浏览器存储功能:HTML5中新增的，本地存储和会话存储
本地存储:持久化的----5M
会话存储:并非持久----会话结束就消失

#####全局方法在main.js中设置
```
//统一引入接口api文件夹里面全部请求函数
*号代表引入里面所有的方法
import * as API from '@/api/index'

app.config.globalProperties.$API = API

//组件代码:         
const internalInstance  = getCurrentInstance();
console.log(internalInstance.appContext.config.globalProperties.$API)
```

#######Element-plus ui自动导入   
1.安装element plus
```
npm install element-plus --save
```
2.自动导入  首先你需要安装unplugin-vue-components和unplugin-auto-import.
```
npm install -D unplugin-vue-components unplugin-auto-import
```
3.然后将下面的代码添加到您的Vite或Webpack配置文件中。
vite
```
// vite.config.ts
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

export default {
  plugins: [
    // ...
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    Components({
      resolvers: [ElementPlusResolver()],
    }),
  ],
}
```
webpack     [gouwu-app使用的是webpack这段代码,但是是写在了vue.config.js里面详情自己进去看](gouwu-app/vue.config.js)
[CSDN网站详情:](https://blog.csdn.net/weixin_44013946/article/details/121206708?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-1-121206708.pc_agg_new_rank&utm_term=element-plus+%E6%8C%89%E9%9C%80%E5%AF%BC%E5%85%A5&spm=1000.2123.3001.4430)
```
// vue.config.js
const AutoImport = require('unplugin-auto-import/webpack')
const Components = require('unplugin-vue-components/webpack')
const { ElementPlusResolver } = require('unplugin-vue-components/resolvers')

module.exports = {
  // ...
  plugins: [
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    Components({
      resolvers: [ElementPlusResolver()],
    }),
  ],
}
```
##### vee-validate 表单验证插件  去npm或者github上搜索下载使用

#####事件绑定自定义事件
```
//Event1组件：Event1非原生DOM节点，而绑定的click并非原生DOM事件，而是自定义事件
//@click.native可以把自定义事件变为原生DOM事件
//当前原生DOMclick事件，其实是给子组件的根节点绑定了点击事件---利用到事件委派
//点击组件任何地方都能触发handler1
<Event1 @click.native="handler1"></Event1>
```

#####移动端的rem配置在mussicap下的public下js的rem.js

######module变量代表当前模块。这个变量是一个对象，module对象会创建一个叫exports的属性，这个属性的默认值是一个空的对象：
```
module.exports.Name="我是电脑"；
module.exports.Say=function(){
  console.log("我可以干任何事情")；  
}


//上边这段代码就相当于一个对象

{
  "Name":" 我是电脑"，
  "Say"    :function(){
         　　console.log("我可以干任何事情")；  
     　　　}
}
```
######require方法用于加载模块。
```
var req=require("./app.js");

req.Name      //这个值是 "我是电脑"
req.Say()      //这个是直接调用Say方法，打印出来 "我可以干任何事情"
```

#####getCurrentInstance()中的ctx 生产环境不可以使用  建议使用 proxy  ，跟ctx效果一样

##### 打包移除项目中所有console.log()
npm i babel-plugin-transform-remove-console
在babel.config.js中
```
module.exports = {
  plugins: [
      'transform-remove-console'
  ]
}
```
##### 打包后不显示试试加上这个  vue.config.js
```
module.exports = {
    publicPath: process.env.NODE_ENV === 'production' ? '' : './',
}
```

#####动态组件 component
1.什么是动态组件
动态组件指的是动态切换组件的显示与隐藏。vue提供一个内置的<component>组件，专门用来实现组件的动态渲染。
1.<component>是组件的占位符
2.通过 is属性动态指定要渲染的组件名称
3.<component is="要渲染的组件的名称"></component>
使用的场景，比如 后台管理动态路由，返回的数据有icon图标的名字，elementplus先在main.js注册完后使用
```
 <el-icon :size="30" >
   <component :is="item.icon" />
 </el-icon>
```
##### keep-alive 缓存状态
作用:避免多次重复渲染降低性能
常用知识点：
匹配首先检查组件自身的 name 选项，如果 name 选项不可用，则匹配它的局部注册名称 (父组件 components 选项的键值)。匿名组件不能被匹配。
1、include - 字符串或正则表达式。只有名称匹配的组件会被缓存
```
<keep-alive include="Home">
<router-view/>
</keep-alive>
```
2、exclude - 字符串或正则表达式。任何名称匹配的组件不会被缓存
```
<keep-alive exclude="Home">
<router-view/>
</keep-alive>
```
3、max - 数字。最多可以缓存多少组件实例
```
<keep-alive :max="10">
<component :is="view"></component>
</keep-alive>
```
4、结合Router,缓存部分页面
```
<keep-alive>
<router-view v-if="route.meta.keepAlive"></router-view>
</keep-alive>
// 外面这里也要留一个 不缓存的路由
<router-view v-if="!route.meta.keepAlive"></router-view>
```

// 示例代码
```
<keep-alive>
<component :is="home"></component>
</keep-alive>

// <component :is="home"></component> 相当于 <home></home> 这个组件被缓存了 离开时没有被销毁
```

##### 使用Echarts
```
npm i --save echarts
```
然后在 APP.vue中 通过 provider 传递给每个组件
```
import * as echarts from 'echarts'
import {provide} from 'vue'
provide("echarts",echarts)
```
