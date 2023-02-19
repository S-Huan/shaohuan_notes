## vite

vite是什么  [官方文档](https://cn.vitejs.dev/)
-   它是一个更加轻量（热更新速度快，打包构建速度快）的vue项目脚手架工具。
-   相对于vue-cli它默认安装的插件非常少，随着开发过程依赖增多，需要自己额外配置。
-   **所以：** 在单纯学习vue3语法会使用它，后面做项目的时候我们还是使用vue-cli

vite基本使用：
-   创建项目 `npm create vite@latest my-vue-app -- --template vue` 或者 `yarn create vite my-vue-app --template vue`
-   安装依赖 `npm i` 或者 `yarn`
-   启动项目 `npm run dev` 或者 `yarn dev`

## 创建vue应用
基本步骤：
-   在main.js中导入createApp函数
-   定义App.vue根组件，导入main.js
-   使用createApp函数基于App.vue组件创建应用实例
-   挂载至index.html的#app容器
```js
import {createApp} from 'vue'
import App from './App.vue'
const app = createApp(App)
app.mount('#app')
```

与vue-cli的区别：
	1. 使用createApp函数创建app实例，扩展功能将来都是在app上进行。而不是直接用vue实例
	2.  挂载方式不同

## 选项API和组合API
选项API写法：`Options ApI`
-   在vue2.x项目中使用的就是 `选项API` 写法
	-  代码风格：data选项写数据，methods选项写函数...，一个功能逻辑的代码分散。
-   优点：易于学习和使用，写代码的位置已经约定好
-   缺点：代码组织性差，相似的逻辑代码不便于复用，逻辑复杂代码多了不好阅读。
-   补充：虽然提供mixins用来封装逻辑，但是出现数据函数覆盖的概率很大，不好维护。

组合API写法：`Compositon API`
-   在vue3.0项目中将会使用 `组合API` 写法
    -   代码风格：一个功能逻辑的代码组织在一起（包含数据，函数...）
-   优点：功能逻辑复杂繁多情况下，各个功能逻辑代码组织再一起，便于阅读和维护
-   缺点：需要有良好的代码组织能力和拆分逻辑能力，PS：大家没问题。
-   补充：为了能让大家较好的过渡到vue3.0的版本来，`也支持vue2.x选项API写法`

## 组合API-setup函数

基本使用
-   `setup` 是一个新的组件选项，作为组件中使用组合API的起点，所有js代码都可以写在这里。
-   从组件生命周期来看，它的执行在组件实例创建之前`vue2.x的beforeCreate`执行。
-   这就意味着在`setup`函数中 `this` 还不是组件实例，`this` 此时是 `undefined`
-   在模版中需要使用的数据和函数，需要在 `setup` 返回。

## 组合API-生命周期

vue2.x生命周期钩子函数：

-   beforeCreate
-   created
-   beforeMount
-   mounted
-   beforeUpdate
-   updated
-   beforeDestroy
-   destroyed

vue3.0生命周期钩子函数

 >组合API的生命周期钩子有7个，可以多次使用同一个钩子，执行顺序和书写顺序相同。
-   `setup` 创建实例前
-   `onBeforeMount` 挂载DOM前
-   `onMounted` 挂载DOM后
-   `onBeforeUpdate` 更新组件前
-   `onUpdated` 更新组件后
-   `onBeforeUnmount` 卸载销毁前
-   `onUnmounted` 卸载销毁后

## 组合API-reactive函数
>组合API的钩子函数中定义的变量，数组，对象，不是响应式的，更改后不能实时同步到视图中。

定义响应式数据：
-   reactive是一个函数，它可以定义一个复杂数据类型，成为响应式数据。
```vue
<script>
import { reactive } from 'vue'
export default {
  name: 'App',
  setup () {
 
    const obj = reactive({
      name: 'ls',
      age: 18
    })
    // 修改名字
    const updateName = () => {
      console.log('updateName')
      obj.name = 'zs'
    }
    return { obj ,updateName}
  }
}
</script>
```

**总结：** 通常是用来定义响应式**对象数据**

## 组合API-toRef函数
>通过reactive定义的只能是复杂数据类型，当只单独让对象中的某一个属性作为响应式时，就用toRef

定义响应式数据：
- toRef是转换函数，转换**响应式对象**中**某个**属性为单独响应式数据，并且**值是关联的**。
- 转换的属性，一定要是响应式对象中的属性
- 修改响应式属性，修改的就是转换后的==响应式属性的对象==，所以修改是属性.value，
- return的也是==响应式属性对象。==
```vue

<script>
import { reactive, toRef } from 'vue'
export default {
  name: 'App',
  setup () {
    // 1. 响应式数据对象
    const obj = reactive({
      name: 'ls',
      age: 10
    })
    // 2. 模板中只需要使用name数据
    // 注意：从响应式数据对象中解构出的属性数据，不再是响应式数据
    // let { name } = obj 不能直接解构，出来的是一个普通数据
    
    const name = toRef(obj, 'name')   //通过toRef来转换
 
    const updateName = () => {
      console.log('updateName')
     
      name.value = 'zs'   // toRef转换响应式数据包装成对象，value存放值的位置
      obj.name = 'ls'     //因为值是关联的，可以修改对象中的属性，最后也会经过toRef转换
    }

    return {name, updateName}  //返回转换的响应式属性对象
  }
}
</script>
<style scoped lang="less"></style>
```

## 组合API-toRefs函数
>toRef转换函数只能转换一个对象属性为响应式属性
>toRefs函数可以让一个对象中所有属性都转换为响应式属性
>只需要在return响应式属性时，结构响应式对象中的属性即可

定义响应式数据：
- toRefs是函数，转换**响应式对象**中所有属性为单独响应式数据，并封装在新对象中，并且**值是关联的**
- 转换的对象，一定要是响应式对象
- 修改响应式属性，修改的是转换后的==返回的新对象==，所以修改是 obj3.name.value 。
- return的是解构后的新对象
```vue
<script>
import { reactive, toRef, toRefs } from 'vue'
export default {
  name: 'App',
  setup () {
    // 1. 响应式数据对象
    const obj = reactive({
      name: 'ls',
      age: 10
    })
    console.log(obj)
    // 2. 解构或者展开响应式数据对象
    // const {name,age} = obj
    // console.log(name,age)
    // const obj2 = {...obj}
    // console.log(obj2)
    // 以上方式导致数据就不是响应式数据了
    const newobj = toRefs(obj)   //但是经过toRefs函数的转换，响应式对象中的所有属性就转换为了响应式属性对象，并封装在新对象中
  
	const updateName = () => {
	    obj3.name.value = 'zs'   //修改的是新对象中的响应式属性对象
      obj.name = 'zs'
    }

    return {...newobj, updateName}  //结构响应式属性对象中的所有属性。
  }
}
</script>
<style scoped lang="less"></style>
```


## 组合API-ref函数
>前面的 toRef  和 toRefs 都是对复杂数据类型的==转换==函数
>ref是专门针对==定义==简单数据类型的。和reactive函数一个级别。

定义响应式数据：
-   ref函数，常用于简单数据类型定义为响应式数据，同时也可以定义复杂数据类型
-   因为ref是直接定义了简单数据类型，所有就没有了值关联，获取值的时候，需要.value
-   在模板中使用ref申明的响应式数据，可以省略.value(复杂数据类型使用的响应式数据也省略了)

**使用场景：**
-   **当你明确知道需要的是一个响应式数据 _对象_ 那么就使用 reactive 即可**
-   当你不知道是什么数据类型，也要使用ref，因为reactive只能定义复杂数据类型，ref两者都可以。

```vue
<template>
  <div class="container">
    <div>{{name}}</div>
    <div>{{age}}</div>
    <button @click="updateName">修改数据</button>
  </div>
</template>
<script>
import { ref } from 'vue'
export default {
  name: 'App',
  setup () {
    // 1. name数据
    const name = ref('ls')
    console.log(name)
    const updateName = () => {
      name.value = 'zs'
    }
    // 2. age数据
    const age = ref(10)

    // ref常用定义简单数据类型的响应式数据
    // 其实也可以定义复杂数据类型的响应式数据
    // 对于数据未之的情况下 ref 是最适用的
    // const data = ref(null)
    // setTimeout(()=>{
    //   data.value = res.data
    // },1000)

    return {name, age, updateName}
  }
}
</script>
```

## 组合API-computed函数
>vue3的计算属性和2的差不多，都是根据已有数据返回动态的值

普通用法的计算属性：
-  computed函数，是用来定义计算属性的，计算属性一般不能修改。
-  计算属性时响应式的，当计算属性的计算的数据变化时，计算属性也会响应到视图中。
-  给computed传入函数，返回值就是计算属性的值

```vue
<template>
  <div class="container">
    <div>今年：{{age}}岁</div>
    <div>后年：{{newAge}}岁</div>
  </div>
</template>
<script>
import { computed, ref } from 'vue'
export default {
  name: 'App',
  setup () {
    // 1. 计算属性：当你需要依赖现有的响应式数据，根据一定逻辑得到一个新的数据。
    const age = ref(16)
    // 得到后年的年龄
    const newAge = computed(()=>{
      // 该函数的返回值就是计算属性的值
      return age.value + 2
    })

    return {age, newAge}
  }
}
</script>
```

高级用法的计算属性：
- 计算属性本质上就是根据变量的变化而变化，理论上不能修改，你不能修改靠别人定义的值。
- 但是，你可以反向修改变量，让变量随着计算属性的改变而改变。从而支持双向绑定。
- 给computed传入对象，get获取计算属性的值，set监听计算属性改变。

```vue
<template>
  <div class="container">
    <div>今年：{{age}}岁</div>
    <div>后年：{{newAge}}岁</div>
    <!-- 使用v-model绑定计算属性 -->
    <input type="text" v-model="newAge">
  </div>
</template>
<script>
import { computed, ref } from 'vue'
export default {
  name: 'App',
  setup () {
    // 1. 计算属性：当你需要依赖现有的响应式数据，根据一定逻辑得到一个新的数据。
    const age = ref(16)

    // 计算属性高级用法，传入对象
    const newAge = computed({
      // get函数，获取计算属性的值， 此处是变量改变计算属性
      get(){
        return age.value + 2
      },
      // set函数，当你给计算属性设置值的时候触发，此处是计算属性反向修改变量。
      set (value) {   //监听计算属性的变化
        age.value = value - 2
      }
    })
    return {age, newAge}
  }
}
</script>
```

## 组合API-watch函数

定义计算属性：
- watch函数，是用来定义侦听器的，可以监听对象，变量，数组等各种数据的变化
- 理论上你监听的变化要是响应式数据，因为只要变化才能监听到。
- 其监听规则为： 当监听的是基本数据类型的响应式对象时，响应式函数参数就可以获取到变化前后的值。
- 当监听的为复杂数据类型的响应式对象时，函数参数得到的是变化后的响应式对象。

1. 监听ref定义的响应式数据

```vue
<script>
import { reactive, ref, watch } from 'vue'
export default {
  name: 'App',
  setup () {
    const count = ref(0)
    const add = () => {
      count.value++
    }
    // 当你需要监听数据的变化就可以使用watch
    // 1. 监听一个ref数据
    // 1.1 第一个参数  需要监听的目标
    // 1.2 第二个参数  改变后触发的函数
     watch(count, (newVal,oldVal)=>{  //因为监听的是基本数据类型的数据，所以可以获取变化前后的两个数据
      console.log(newVal,oldVal)
    })
</script>
```

2. 监听reactive定义的响应式数据

```vue
 <script>
import { reactive, ref, watch } from 'vue'
export default {
  name: 'App',
  setup () {
  const obj = reactive({
      name: 'ls',
      age: 10,
    })
    // 当你需要监听数据的变化就可以使用watch
    // 1. 监听一个reactive数据
    // 1.1 第一个参数  需要监听的目标
    // 1.2 第二个参数  改变后触发的函数
    watch(obj, (e)=>{    //监听reactive对象时，监听的是复杂数据类型的响应式对象，参数里是变化后的响应式对象
      console.log('数据改变了')
    })
</script>
```

3.  监听多个数据的变化

```vue
 <script>
import { reactive, ref, watch } from 'vue'
export default {
  name: 'App',
  setup () {
  
 const count = ref(0)
  const obj = reactive({
      name: 'ls',
      age: 10,
    })
   
   watch([count, obj], (e)=>{   //监听不同的数据类型，响应式函数参数会把两个数据类型的获取的值存放到数组里，
   
       console.log('监听多个数据改变了')
     }) 

</script>
```

4. 监听对象中某一个属性
- 监听某一属性变化，也可以获取某一属性的响应式对象，监听此对象

```vue
 <script>
import { reactive, ref, watch } from 'vue'
export default {
  name: 'App',
  setup () {
  
 const count = ref(0)
  const obj = reactive({
      name: 'ls',
      age: 10,
    })
   
// 监听属性类的数据，就要写成函数返回值的形式才能监听到

 watch(()=>obj.name,(a, b)=>{ //因为监听的是基本数据类型的数据，所以可以获取变化前后的两个数据

 console.log('监听obj.name改变了')

 })
</script>
```

5. 监听对象的对象中的属性变化(深层监听)
- 一定是==监听==的是深度数据，而不是更改的是深度数据。
```vue
<script>
import { reactive, ref, watch } from 'vue'
export default {
  name: 'App',
  setup () {
  
 const count = ref(0)
  const obj = reactive({
      name: 'ls',
      age: 10,
    })
    // 监听属性类的数据，就要写成函数返回值的形式才能监听到
watch(()=>obj.brand, ()=>{    
      console.log('brand数据改变了')  //因为监听的是赋值数据类型，所以参数获取的还是响应式对象
    },{
      // 5. 需要深度监听
      deep: true, // 监听深层对象属性时，要打开deep
      // 6. 想默认触发
      immediate: true   //网页开启自动触发
    })
</script>
```

## 组合API-ref属性
>ref属性在vue2中被用来获取原生DOM，和vue组件实例。
>在vue3中也是这个作用，只是写法不同

- 获取DOM或者组件实例可以使用ref属性，写法和vue2.0需要区分开
	-   单个元素：先申明ref响应式数据，返回给模版使用，通过ref绑定数据
	-   遍历的元素：先定义一个空数组，定一个函数获取元素，返回给模版使用，通过ref绑定这个函数

- vue3获取单个DOM或者组件
- 因为setup是声明周期不能获得dom，所以要去onMounted的钩子函数里获取
```vue
<template>

<div class="container">

<!-- 单个元素 -->

<div ref="dom">我是box</div>

</div>

</template>
<script>
import { onMounted, ref } from 'vue'
export default {
  name: 'App',
  setup () {
    // 1. 获取单个元素
    // 1.1 先定义一个空的响应式数据 接收ref传来的DOM对象
    // 1.2 setup中返回该数据，你想获取那个dom元素，在该元素上使用ref属性绑定该数据即可。
    const dom = ref(null)
    
    onMounted(()=>{
       console.log(dom.value)
    })
    return {dom}
  }
}
</script>
<style scoped lang="less"></style>
```

- vue3获取遍历的标签DOM

```vue
<template>

<div class="container">

<!-- 被遍历的元素 -->

<ul>
						  <!--绑定ref的是方法，要加：-->
<li v-for="i in 4" :key="i" :ref="setDom">第{{i}}LI</li>

</ul> 

</div>

</template>
<script>
import { onMounted, ref } from 'vue'
export default {
  name: 'App',
  setup () {
  
     // 2. 获取v-for遍历的元素
    // 2.1 定义一个空数组，接收所有的LI
    // 2.2 定义一个函数，往空数组push DOM
    const domList = []
    
    const setDom = (el) => {  //遍历的dom都传参给了el
      domList.push(el)  //给定义好的数组，添加dom
    }
    
    onMounted(()=>{
      console.log(domList)   //在挂载钩子函数里获取
    })

	 // ref获取v-for遍历的DOM元素，需要在组件更新的时候重置接受dom元素的数组。
    onBeforeUpdate(()=>{
      domList = []
    })
    return {dom, setDom}
  }
}
</script>
<style scoped lang="less"></style>
```

## 组合API-父子通讯
- vue3中的父子通信和vue2中的很像，都是用props和emit传
- 只是因为vue3中对于数据层更加的封闭，所以要遵守规则
-   父传子：在setup种使用props数据 `setup(props){ // props就是父组件数据 }`
-   子传父：触发自定义事件的时候emit来自 `setup(props,{emit}){ // emit 就是触发事件函数 }`

- ==父传子（props）==
- 父传值
- 父传子的时候就是数据层要return出去，（记住return出去的要是响应数据对象，不然不能动态响应）
```vue
<template>
  <div class="container">
    <h1>父组件</h1>
    <p>{{money}}</p>
    <hr>
    <Son :money="money" />
  </div>
</template>
<script>
import { ref } from 'vue'
import Son from './Son.vue'
export default {
  name: 'App',
  components: {
    Son
  },
  // 父组件的数据传递给子组件
  setup () {
    const money = ref(100)
    return { money }
  }
}
</script>
```

子接收
- 子组件用props接收值，但是只能在视图层用，想要进入数据层，要经过setup的参数传进去。
```vue
<template>
  <div class="container">
    <h1>子组件</h1>
    <p>{{money}}</p>
  </div>
</template>
<script>
import { onMounted } from 'vue'
export default {
  name: 'Son',
  // 子组件接收父组件数据使用props即可
  props: {
    money: {
      type: Number,
      default: 0
    }
  },
  setup (props) {
    // 获取父组件数据money
    console.log(props.money)
  }
}
</script>
```

 - ==子传父（emit）==
 - 子传值
 - 子传父主要是子组件从setup函数的第二个形参中解构出 ==emit函数== 然后其他和vue2一样，定义事件，传值。

```vue
<template>
  <div class="container">
    <h1>子组件</h1>
    <p>{{money}}</p>
+    <button @click="changeMoney">花50元</button>
  </div>
</template>
<script>
import { onMounted } from 'vue'
export default {
  name: 'Son',

  // emit 触发自定义事件的函数
+  setup (props, {emit}) {
    // 获取父组件数据money
    
    // 向父组件传值
+    const changeMoney = () => {
      // 消费50元
      // 通知父组件，money需要变成50
+      emit('change-money', 50)
+    }
+    return {changeMoney}
  }
}
</script>
```

- 父接收
- 父接收值和vue2一样，接收函数，接收实参。
-  - 注意接收的函数记得return出去，不然没法双向绑定
```vue
<template>
  <div class="container">
    <h1>父组件</h1>
    <p>{{money}}</p>
    <hr>
+    <Son :money="money" @change-money="updateMoney" />
  </div>
</template>
<script>
import { ref } from 'vue'
import Son from './Son.vue'
export default {
  name: 'App',
  components: {
    Son
  },
  // 父组件的数据传递给子组件
  setup () {
    const money = ref(100)
+    const updateMoney = (newMoney) => {
+      money.value = newMoney
+    }
+    return { money , updateMoney}
  }
}
</script>

```

==v-model的父传子==

>在vue2中，v-model有两种方式，可以父传子（前提都是有 子要修改父传给子的变量  的需求）
>-   在vue3.0中 `v-model` 和 `.sync` 已经合并成 `v-model` 指令

```vue
    <!-- <Son :money="money" @update:money="updateMoney" /> -->
    <Son v-model:money="money" />  //在父组件还是一样，emit(update：money, 10)
```

## 组合API-依赖注入
>vue2中也有依赖注入，两个原理差不多，写法上有些差别
-  使用场景：有一个父组件，里头有子组件，有孙组件，有很多后代组件，共享父组件数据。
- 父组件给所有后代组件传递数据

- 父组件用provide(名字，变量)传递
```vue
<script>
import { provide, ref } from 'vue'
import Son from './Son.vue'
export default {
  name: 'App',
  components: {
    Son
  },
  setup () {
    const money = ref(100)  
    // 将数据提供给后代组件 provide
    provide('money', money)
    // 将函数提供给后代组件 provide
    provide('changeMoney', changeMoney)

    return { money }
  }
}
</script>
<style scoped lang="less"></style>
```

- 所有后代组件用inject(名字)接收
```vue
<script>
import { inject } from 'vue'
import GrandSon from './GrandSon.vue'
export default {
  name: 'Son',
  components: {
    GrandSon
  },
  setup () {
    // 接收祖先组件提供的数据
    const money = inject('money')
    return { money }
  }
}
</script>
<style scoped lang="less"></style>
```

- 遵循单向数据流，后代组件不能修改传过来的数据，只能祖先组件传一个函数下来，后代组件告诉祖先组件要修改的值。
```vue
<script>
import { inject } from 'vue'
export default {
  name: 'GrandSon',
  setup () {
    const money = inject('money')
    // 孙组件，消费50，通知父组件App.vue组件，进行修改
    // 不能自己修改数据，遵循单选数据流原则，大白话：数据谁定义谁修改
    const changeMoney = inject('changeMoney')  //子组件接收父组件传的修改函数
    const fn = () => {
      changeMoney(20)
    }
    return {money, fn}
  }
}
</script>
<style scoped lang="less"></style>
```



## 补充-v-model语法糖
>vue3中的v-model语法糖改为

```html
   <Son v-model="count" />   //统一改为只有一种形式
```

```vue
<script>
export default {
  name: 'Son',
  props: {
    modelValue: {
      type: Number,
      default: 0
    }
  },
  setup (props, {emit}) {
    const fn = () => {
      // 改变数据
      emit('update:modelValue', 100)  //和原来v-model一样默认给modelValue传参
    }
    return { fn }
  }
}
</script>
```

## 补充-mixins语法
>-   混入 (mixin) 提供了一种非常灵活的方式，来分发 Vue 组件中的可复用功能。一个混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项。

- mixins是vue2的功能，它还是在使用选项api来混入组件，因为本身原理是混合进入组件本身的选项，所以很容易覆盖。在vue3中尽量不用，而用组合api的形式封装功能。

全局混入 ： 在app实例中挂载
- 在main.js里面app实例中挂载minxin混入对象，就可以在相当于在全局组件的选项api中拥有这些方法
```js
// 全局混入 全局mixin
// vue2.0 写法  Vue.mixin({})
app.mixin({
  methods: {
    say () {
      console.log(this.$el,'在mounted中调用say函数')
    }
  },
  mounted () {
    this.say()
  }
})
```

局部混入：通过mixins选项进行混入
- 在一个js文件中，配置等待混入的mixins对象
```js
// 配置对象
export const followMixin =  {
  data () {
    return {
      loading: false
    }
  },
  methods: {
    followFn () {
      this.loading = true
      // 模拟请求
      setTimeout(()=>{
        // 省略请求代码
        this.loading = false
      },2000)
    }
  }
}
```

- 在一个组件中部分引入mixins对象，并在mixins选项中混入
```vue
<template>
  <div class="container2">
    <h2> 作者：周杰伦  <button @click="followFn">{{loading?'loading...':'关注'}}</button> </h2>
  </div>
</template>
<script>
import {followMixin} from './mixins'
export default {
  name: 'Son',
  mixins: [followMixin]
}
</script>
<style scoped lang="less"></style>
```
## 补充-vueX的新用

>vuex在vue3中的创建方法有看变化，通过函数定义，而不是以前的new一个实例
>而且因为vue3的代码在setup里，获取不到组件this所以也要获取引入仓库模块，通过原始引入方法引入
>辅助函数不在能用，因为辅助函数要定义在计算属性里。

- 创建根仓库
```js
//vue2.0 创建仓库 new Vuex.Store({})
//vue3.0 创建仓库 createStore({})
export default createStore({
  state: {
    username: 'zs'
  },
  getters: {
    newName (state) {
      return state.username + '!!!'
    }
  },
  mutations: {
    updateName (state) {
      state.username = 'ls'
    }
  },
  actions: {
    updateName (ctx) {
      // 发请求
      setTimeout(() => {
        ctx.commit('updateName')
      }, 1000)
    }
  },
  modules: {

  }
})
```

- 使用根的vuex
```js
<template>
  <!-- vue2.0需要根元素，vue3.0可以是代码片段 Fragment -->
  <div>
    App
    <!-- 1. 使用根模块state的数据   -->
    <p>{{$store.state.username}}</p>
    <!-- 2. 使用根模块getters的数据   -->
    <p>{{$store.getters['newName']}}</p>
    <button @click="mutationsFn">mutationsFn</button>
  </div>
</template>
<script>
import { useStore } from 'vuex'
export default {
  name: 'App',
  setup () {
    // 使用vuex仓库
    const store = useStore()
    // 1. 使用根模块state的数据
    console.log(store.state.username)
    // 2. 使用根模块getters的数据
    console.log(store.getters.newName)
    const mutationsFn = () => {
      // 3. 提交根模块mutations函数
      // store.commit('updateName')
      // 4. 调用根模块actions函数
      store.dispatch('updateName')
    }
    return { mutationsFn }
  }
}
</script>
```

- 模块化的VUEX
-   不带命名空间的模块，`state` 区分模块，使用要加模块名，其他 `getters` `mutations` `actions` 都在全局还是一样。
```html
 <!-- 使用模块A的username -->
 <p>A的username --- {{$store.state.a.username}}</p>  //带模块名
```

-  带命名空间 `namespaced: true` 的模块，所有功能区分模块，更高封装度和复用。
- 创建带命名空间的模块仓库
```js
import { createStore } from 'vuex'

const moduleA = {
  // 子模块state建议写成函数，防止冲突
  state: () => {
    return {
      username: '模块A'
    }
  },
  getters: {
    changeName (state) {
      return state.username + 'AAAAAA'
    }
  }
}

const moduleB = {
  // 带命名空间的模块
  namespaced: true,
  // 子模块state建议写成函数
  state: () => {
    return {
      username: '模块B'
    }
  },
  getters: {
    changeName (state) {
      return state.username + 'BBBBBB'
    }
  },
  mutations: {
    // 修改名字的mutation
    update (state) {
      state.username = 'BBBB' + state.username
    }
  },
  actions: {
    update ({ commit }) {
      // 假设请求
      setTimeout(() => {
        commit('update')
      }, 2000)
    }
  }
}

// 创建vuex仓库并导出
export default createStore({
  state: {
    // 数据
    person: [
      { id: 1, name: 'tom', gender: '男' },
      { id: 2, name: 'lucy', gender: '女' },
      { id: 3, name: 'jack', gender: '男' }
    ]
  },
  mutations: {
    // 改数据函数
  },
  actions: {
    // 请求数据函数
  },
  modules: {
    // 分模块
    a: moduleA,   //重命名模块名
    b: moduleB
  },
  getters: {
    // vuex的计算属性
    boys: (state) => {
      return state.person.filter(p => p.gender === '男')
    }
  }
})
```

- 使用带命名空间的模块
```vue
<template>
 <div>APP组件</div>
 <ul>
   <li v-for="item in $store.getters.boys" :key="item.id">{{item.name}}</li>
 </ul>
 <!-- 使用模块A的username -->
 <p>A的username --- {{$store.state.a.username}}</p>
 <p>A的changeName --- {{$store.getters.changeName}}</p>
 <hr>
 <p>B的username --- {{$store.state.b.username}}</p>
 <p>B的changeName --- {{$store.getters['b/changeName']}}</p>
 <button @click="$store.commit('b/update')">修改username</button>
 <button @click="$store.dispatch('b/update')">异步修改username</button>
</template>
```

### VUEX的持久化
1. 首先：我们需要安装一个vuex的插件`vuex-persistedstate`来支持vuex的状态持久化。

```bash
npm i vuex-persistedstate
```

2. 建立业务的vuex

3. 使用vuex-persistedstate插件来进行持久化 

```js
import { createStore } from 'vuex'
+import createPersistedstate from 'vuex-persistedstate'

import user from './modules/user'
import cart from './modules/cart'
import cart from './modules/category'

export default createStore({
  modules: {
    user,
    cart,
    category
  },
 plugins: [
    createPersistedstate({
      key: 'erabbit-client-pc-store',
      paths: ['user', 'cart']
    })
  ]
})
```

**注意：**
===> 默认是存储在localStorage中

===> key是存储数据的键名

===> paths是存储state中的哪些数据，如果是模块下具体的数据需要加上模块名称，如`user.token`

===> 修改state后触发才可以看到本地存储数据的的变化。

## VUE3更新细节
1. vue3的数据层，数据必须要转换才能给视图层用
2. vue3的视图层，不再需要一个根标签，可以定义多个便签
3. vue3的vuex，路由，组件的挂载，都在函数里创建。
```js
createApp(App).use(store).use(router).mount('#app')
```

4. 在setup里获取路由实例和vuex实例，
- 因为数据层在setup获取不到组件this，所以获取不到$router, $store
```js
//获取路由模块
import { useRouter } from 'vue-router'
```

```js
//获取vuex模块 - 方法一
import { useStore } from 'vuex'
 setup () {
    // 获取vuex仓库
    const store = useStore()
    }
```

```js
//获取vuex模块 - 方法二
import store from '@/store'  //在setup外边获取vuex仓库
store.state.profile  //获取vuex仓库
```

5. 获取组件实例
- 在setup获取不到组件实例，因为setup生命周期还没有创建组件。但是有两个方法可以获取
1. 在created 生命周期里获取 (注意要写在setup外边，setup本身就是生命周期， 要分开写)
2. const { proxy } = getCurrentInstance() proxy就是组件的实例

6. ref获取的其他组件实例， 可以获取表单验证相关选项
	```js
	formCom.value.resetForm()  //手动重置校验结果
	const valid = await formCom.value.validate() //对整体表单全局校验, 返回校验结果
	formCom.value.setFieldError('mobile', valid)  //让某一个校验属性， 返回校验值
```

7. vue2-vue3 动态路由激活类名
- 在vue2时， 给动态路由激活类名有两个， 一个模糊匹配激活， 一个精确匹配激活。当url路由的地址是包含组件的to属性的路由地址时， 就会触发模糊匹配（不必是它的子路由） 例：'/user/name' 与 ‘user’  此时'user'就触发了模糊匹配，但'user'的路由不必是'user/name'的父路由。他俩不必有关系，只要路由地址相同就行。

`router-link-active   当你的路由路径包含 router-link组件的to属性值，当前组件会加上它
`router-link-exact-active   当你的路由路径完全和你的router-link组件的to属性值一致，当前组件会加上它

> active-class = ‘class’ / exact-active-class = ‘class’ 两个属性   当触发某种激活时， 给组件添加类名


- 但是当vue3时， 逻辑和vue2是一样的
- 就是当url路由地址和to属性路由地址一致时， 就会触发精确匹配，然后添加`router-link-exact-active `类名, 当触发模糊匹配， 就会添加`router-link-active`类名
- 同时， 当触发某种匹配的同时，也会触发 active-class = ‘class’ / exact-active-class = ‘class’ 两个属性， 给组件动态添加任意类名。
- 注意： ==但是， 触发模糊匹配的条件更苛刻了，当触发模糊匹配时， 不但要求url路由地址包含to属性路由地址， 还要求 url路由地址真的是to属性的路由地址的子路由， 他们必须在路由上有从属关系/ 路由嵌套关系。  ==
![[Pasted image 20221123123230.png]]









## 内置组件

### Teleport 传送门
`<Teleport>` 是一个内置组件，它可以将一个组件内部的一部分模板“传送”到该组件的 DOM 结构外层的位置去。

```html
<Teleport to="#model">   //填写目标结构的id

<OrderLogistics ref="orderLogisticsCom" />

</Teleport>
```

### Suspense
`<Suspense>` 是一个内置组件，用来在组件树中协调对异步依赖的处理。它让我们可以在组件树上层等待下层的多个嵌套异步依赖项解析完成，并可以在等待时渲染一个加载状态。

```vue
<Suspense v-if="[3,4,5].includes(order.orderState)">

<!-- 组件加载完毕 -->

<template #default>

<DetailLogistics :order="order" />  //此子组件时异步时，会阻碍父组件的实例化渲染， 所以用内置组件包裹 

</template>

<!-- 组件加载中显示 -->

<template #fallback>

<div class="loading">loading</div>

</template>

</Suspense>
```