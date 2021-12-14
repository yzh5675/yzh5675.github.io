---
title: Vue语法及入门基础
date: 2021-11-11 09:13:47
tags: Vue
author: 别听学唱情歌
img: https://yzh5675.oss-cn-beijing.aliyuncs.com/blog/img/gf01
categories: Vue
abbrlink: 3fde
top: true
cover: true
toc: false
copyright: true
toc_number: true
mathjax: false
keywords: vue 
description: Vue-动态构建用户界面的渐进式 JavaScript 框架
summary: Vue-动态构建用户界面的渐进式 JavaScript 框架
---

# Vue

## 一、什么是Vue？

动态构建用户界面的渐进式 JavaScript 框架。



## 二、模板语法

### 1. 插值语法

```vue
// 用于解析标签体内容
<h1>{{xxx}}</h1>
```

### 2. 指令语法

```vue
// 解析标签属性、解析标签体内容、绑定事件
<a v-bind:href="xxx">百度</a>
```



## 三、数据绑定

### 1.单向数据绑定

```vue
<!-- 语法 -->
<!-- 或简写为 :href -->
<p v-bind:href ="xxx"></p>
<!-- 特点：数据只能从 data 流向页面 -->
```

### 2.双向数据绑定

```vue
<!-- 语法 -->
<!-- 或简写为  v-model="xxx" -->
<p v-model:value ="xxx"></p>
<!-- 数据不仅能从 data 流向页面，还能从页面流向 data -->
```



## 四、MVVM 模型 

### 1. 概念

​			M：模型(Model) ：对应 data 中的数据

​			V：视图(View) ：模板

​			VM：视图模型(ViewModel) ： Vue 实例对象

![MVVM模型](https://yzh5675.oss-cn-beijing.aliyuncs.com/blog/img/typora-user-images/image-20211021192325645.png)

解释：

![模型解释](https://yzh5675.oss-cn-beijing.aliyuncs.com/blog/img/typora-user-images/image-20211021192350130.png)

## 五、事件处理和事件、按键修饰符

### 1. 事件处理

```vue
<!-- 语法 -->
<p v-on:事件名="函数名(参数或无参)"></p>
<!-- 简略写法 -->
<p @事件名="函数名(参数或无参)"></p>
```

### 2. 事件修饰符

```vue
<!-- prevent阻止事件的默认行为 -->
<a href="https://www.baidu.com" @click.prevent="函数名()">点我到百度</a>

<!-- 阻止事件冒泡 -->
<div class="apps" @click="showInfo()">
    <!-- stop：阻止事件冒泡 -->
    <!-- 链式写法 -->
    <button type="button" @click.stop.prevent="函数名()">点我</button>
</div>

<!-- once：事件只能触发一次 -->
<button type="button" @click.once="函数名()">点我</button>
```

### 3. 按键修饰符

```vue
<!-- 指定按键，键码或者键名都行,键的别名也行 -->
<!-- 
	Event 对象代表事件的状态，比如事件在其中发生的元素、键盘按键的状态、鼠标的位置、鼠标按钮的状态
	事件通常与函数结合使用，函数不会在事件发生前被执行 
-->
<input type="text" name="" id="" value="" @keydown.键名="showInfo($event)" placeholder="按下回车提示输入....." />

<!-- 自定义键名 -->
<!-- 声明一个键别名，键码为13 -->
Vue.config.keyCodes.huiche = 13;
new Vue({
	......
});
```



## 六、计算属性

### 1. 概念

​			**在一个计算属性里可以完成各种复杂的逻辑，包括运算、函数调用等，只要最终返回一个结果就可以**，要显示的数据不存在，要通过计算得来，计算属性其实和函数没什么区别

### 2. 声明和使用计算属性

```vue
<script type="text/javascript">
    new Vue({
        el : "#app",
        // 每个计算属性中默认有get和set方法，可以写也可以不写
        computed:{		// 计算属性
            fullName : {
                get(){		// 当使用fullName这个计算属性时就会调用此get方法
                    console.log("get方法被调用了!");
                    return this.fistName +"-"+ this.lastName;
                },
                set(value){		// fullName这个计算属性发生发生改变时就会调用此set方法
                    console.log("set方法", value);
                }
            }
       },
    });

    // 使用计算属性：{{计算属性名}}		可以触发get方法
    // v-model="计算属性名"			 可以触发set方法
</script>
```

### 3. 计算属性缓存和函数（方法）的区别

​			两者最主要的区别：computed 是**可以缓存的**，methods **不能缓存**；**只要相关依赖没有改变，多次访问计算属性得到的值是之前缓存的计算结果，就不会多次执行。**

### 4. 计算属性通过闭包方式传参

```vue
<script type="text/javascript">
    computed: {
        closure () {
            return function (a, b, c) {
                .......
                return data;
            }
        }
    }
</script>
```



## 七、监听（侦听）属性

### 1. 概念

​			顾名思义，监听某个属性的变化并对其进行一些操作，Watch中可以执行任何逻辑，如函数节流、Ajax异步获取数据，甚至操作 DOM（不建议）

### 2. 声明及用法

```vue
<!-- HTML -->
<div id="app">
    姓：<input type="text" name="" id="" value="" v-model="fistName" />		<br />
    名：<input type="text" name="" id="" value="" v-model="lastName" />		<br />
    <p>全名：{{fistName}} - {{lastName}} </p>
</div>

<script type="text/javascript">
    new Vue({
        el : '#app',
        data : {
            fistName : "阿",
            lastName : "龟"
        },
        watch:{			// 监视属性，当fistName或lastName发生改变时就会触发监视属性
            fistName : {
                deep : true,	//  deep：需要监听的数据的深度，一般用来监听对象中某个属性的变化,数组字符串一般不需要
                // 在选项参数中指定 immediate: true将立即以表达式的当前值触发回调,
                immediate : false,		// 初始化，调用一次handler
                // 第一个参数为新值，第二个参数为旧值，如果调用的旧值会发生无限更新值的情况
                handler(newValue, oldValue){			// handle：watch中需要具体执行的方法
                    console.log(oldValue, newValue);
                    this.fistName = newValue;
                }
            },
            lastName : {
                handler(newValue, oldValue){
                    console.log(oldValue, newValue);
                    this.lastName = newValue;
                }
            }
        }
    });
</script>
```



## 八、计算属性和监视属性的总结

### 1.计算属性和监视属性的区别

​			① computed：监测的是依赖值，依赖值不变的情况下其会直接读取缓存进行复用，变化的情况下才会重新计算。

​			② watch：监测的是属性值， 只要属性值发生变化，其都会触发执行回调函数来执行一系列操作

​			主要区别：

​				**计算属性不能执行异步任务，计算属性必须同步执行**，而监视属性可以实现异步

### 2. 总结

​			① 计算属性能做的监视属性也能做，反之则不行

​			② 尽量使用计算属性来替代监视属性



## 九、class 与 style 绑定 

### 1.概念：字面意思

### 2.使用

```vue
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>class 与 style 绑定</title>
		<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
		<style>
			.c1{
				color: red;
			}
			.c2{
				font-size: 30px;
			}
			.c3{
				font-weight: 600;
			}
		</style>
	</head>
	<body>
		<div id="app">
			<ul>
				<li :class="cl1">aguiagu</li>
				<li :class="['c1', 'c2', 'c3']">fas</li>
				<li :style="{color: colors, 'font-weight': fonts}">erwe</li>
				<li :style="styleList">fsa</li>
				<li :class="{c1:t}">gsdfg</li>
			</ul>
		</div>
	</body>
	<script type="text/javascript">
		new Vue({
			el : "#app",
			data : {
				cl1 : 'c1',
				cl2 : 'c2',
				cl3 : 'c3',
				t : true,
				f : false,
				colors : 'blue',
				fonts : 800,
				styleList : {		// 属性集合
					color : 'green',
					fontWeight : 900
				}
			}
		})
	</script>
</html>

```



## 十、条件渲染

### 1. v-if和v-show

​		① 概念：都相当于相当于Java中的if-else

​		② 原理：

​				v-show指令：元素始终被渲染到HTML，它只是简单的伪元素设置css的style属性，当不满足条件的元素被设置style=“display：none”的样，是通过修改元素的的CSS属性(display)来决定实现显示还是隐藏

​				v-if指令：满足条件是会渲染到html中，不满足条件时是不会渲染到html中的，是通过操纵dom元素来进行切换显示

​		③ 使用

```vue
<div id="app">
    <p v-if="ok">表白成功</p>
    <p v-else>表白失败</p>

    <hr />

    <p v-show="ok">求婚成功</p>
    <p v-show="!ok">求婚失败</p>

    <button type="button" @click="ok = !ok">切换</button>
</div>
<script type="text/javascript">
    new Vue({
        el:'#app',
        data:{
            ok: true
        }
    })
</script>
```

### 2. 总结

​			如果频繁切换建议使用v-show



## 十一、列表渲染

### 1. v-for

​			① 概念：相当于Java中的for循环

​			② 使用

```vue
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>列表渲染</title>
		<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
	</head>
	<body>
		<div id="app">
			<!-- 数组遍历 -->
			<h1>遍历人员</h1>
			<table border="" cellspacing="" cellpadding="">
				<tr>
					<th>id</th>
					<th>name</th>
					<th>age</th>
				</tr>
				
				<tr v-for="(p, index) of persons" :key="index">
					<td>{{p.id}}</td>
					<td>{{p.name}}</td>
					<td>{{p.age}}</td>
				</tr>
			</table>
			
			<!-- 遍历对象 -->
			<h1>遍历对象</h1>
			<table border="" cellspacing="" cellpadding="">
				<tr>
					<th>name</th>
					<th>price</th>
					<th>color</th>
				</tr>
				
				<tr v-for="(c, key, i) in cars" :key="key">		<!-- i是索引 -->
					<td>{{c}}</td>
					<td>{{c}}</td>
					<td>{{c}}</td>
				</tr>
			</table>
			
			<!-- 遍历字符串 -->
			
		</div>
	</body>
	<script type="text/javascript">
		new Vue({
			el : "#app",
			data : {
				persons : [
					{id : "001", name : "Tom", age : 20},
					{id : "002", name : "Jack", age : 20},
					{id : "003", name : "Lisa", age : 20},
					{id : "004", name : "Dawei", age : 20},
					{id : "005", name : "agui", age : 20}
				],
				cars : {
					name : "比亚迪",
					price : 380000,
					color : "red"
				}
			},
			methods:{
				
			}
		})
	</script>
</html>

```

### 2. 数据过滤和排序

```vue
<script type="text/javascript">
    new Vue({
        el : "#app",
        data : {
            persons : [
                {id : "001", name : "Tom", age : 20},
                {id : "002", name : "Jack", age : 20},
                {id : "003", name : "Lisa", age : 20},
                {id : "004", name : "Dawei", age : 20},
                {id : "005", name : "agui", age : 20}
            ],
            keyWorld : ""
        },
        computed : {
            fn(v){
                // 使用filter过滤函数实现数据过滤
                return this.persons.filter (k => {		// k代表this
                    return k.name.indexOf(this.keyWorld) !== -1;
                });
                
                // 排序
                if(orderType){
                    persons.sort(function (a,b) {
                        if(orderType===1){
                            return a.age-b.age;
                        }else{
                            return b.age-a.age;
                        }
                    });
                }
            }
        }
    })
</script>
```



## 十二、自定义指令

### 1. 简介

​			① 一个指令对象的定义可以使用如下的几种钩子函数：

​					`bind`：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置

​					`inserted`：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)

​					`update`：所在组件的 VNode 更新时调用，**但是可能发生在其子 VNode 更新之前**。指令的值可能发生了改变，也可能没有

​					`componentUpdated`：指令所在组件的 VNode **及其子 VNode** 全部更新后调用

​					`unbind`：只调用一次，指令与元素解绑时调用

### 2.定义自定义指令

```vue
<script type="text/javascript">
    // 全局自定义指令，el指：指令所在的标签dom
    Vue.directive('指令名', function(el, bind){
        el.innerText = bind.value*100;
    });
   	new Vue({
        // 自定义局部指令
        directives : {
        	big(el, bind){
				element.innerText = bind.value*100;
			}
		}
    });
</script>
```



## 十二、页面优化

```html
<div id="app">
    <!-- v-once所在节点的视为静态属性，只渲染一次，常用于优化页面 -->
    <h2 v-once>n的值为：{{n}}</h2>
    
    <!-- 跳过这个元素和它的子元素的编译过程。一些静态的内容不需要编辑加这个指令可以加快编辑，常用于页面优化 -->
    <h2 v-pre>{{n}}</h2>
    <h2>n的值为：{{n}}</h2>
    <button type="button" @click="n++">点我n+1</button>
</div>
```



## 十三、Vue的生命周期

- ![Vue生命周期](https://yzh5675.oss-cn-beijing.aliyuncs.com/blog/img/typora-user-images/image-20211022164544216.png)





# Vue-cli：vue脚手架

## 一、Vue组件化编程

### 1. 模块与组件

​			1）模块

​				① 理解: 向外提供特定功能的 JS 程序, 一般就是一个 JS 文件

​				② 为什么？ JS 文件很多很复杂

​				③ 作用：复用 JS, 简化 JS 的编写, 提高 JS 运行效率

​			2）组件

​				① 理解：用来实现局部(特定)功能效果的代码集合(html/css/js/image…..)

​				③ 为什么：一个界面的功能很复杂

​				② 作用：复用编码, 简化项目编码, 提高运行效率

### 2. 模块化与组件化 

​			1）模块化

​				当应用中的 JS 都以模块来编写的, 那这个应用就是一个模块化的应用

​			2）组件化

​				当应用中的功能都是多组件的方式来编写的， 那这个应用就是一个组件化的应用



## 二、vue脚手架

### 1. 概念

​			Vue 脚手架是 Vue 官方提供的标准化开发工具（开发平台）

### 2. 安装

```shell
# 如果安装了旧版的vue/cli，先卸载
# 方法一
npm uninstall vue-cli -g
# 方法二
yarn global remove vue-cli

# 安装
npm install -g @vue/cli

# 检查是否安装完成 --->  查看vue/cli版本
vue --version

# 升级vue/cli版本（直接到最新版本）
npm update -g @vue/cli
```

### 3. 使用vue/cli创建vue项目

​			1）cmd方式

```shell
# 第一步：切换到你要创建项目的目录，然后使用命令创建项目
vue create helloworld

# 第二步：选择 vue2 或者 vue3 回车，会在当前目录下创建名为 helloworld 的项目（建议vue2）

# 第三步：进入项目目录

# 第四步：运行项目
npm run serve

# 第五步：浏览器运行初始化项目,vue默认访问端口8080
```

​			2）图形化界面创建

```shell
# 输入回车后会自动打开vue的图形化界面
vue ui
```

​			3）如果出现下载缓慢解决方法

```shell
# 配置淘宝镜像
npm config set registry https://registry.npm.taobao.org
```

​			4）如果Vue 脚手架隐藏了所有 webpack 相关的配置解决方法

```shell
# 查看具体的 webpack 配置
vue inspect > output.js
```

### 4. 模板项目的结构

![项目结构](https://yzh5675.oss-cn-beijing.aliyuncs.com/blog/img/typora-user-images/image-20211025181237348.png)

## 三、ref 与 props

### 1.ref 

​			1）作用：用于给节点打标识

```vue
<template>
	<!-- 定义ref -->
	<h2	ref="tilte" @click="showInfo()">阿龟</h2>
</template>

<script>
    export default {
        name: 'App',
    	methods:{
			showInfo(){
				// 读取ref，取出标记的内容，获取整个被标记的标签
				console.log(this.$refs.tilte);
			}
        }
	}
</script>
```

### 2. props

 			1）作用：用于父组件给子组件传递数据（父子组件间通信）

​			 2）使用

```vue
<!-- 父组件 -->
<template>
	<div>
        <!-- 数据传输 -->
        <HelloWorld :name="name" :age="age" :adr="adr" :fn="fn"></HelloWorld>
    </div>
</template>

<!-- 子组件 -->

<script>
	export default {
		// 第一种定义属性方式，常用
		props:['age', 'adr'],
		// 第二种方式定义属性，常用
		props:{
		 	age : Number,
		 	adr : String,
		 	name: String,
			fn: Function
		},
		// 第三种定义属性的方式
		props:{
			age : {type:Number, required:true, default:18},
			adr : {type:String, required:true, default:'湖南长沙'},
			name: {type:String, required:true, default:'阿龟'},
			fn: {type:Function, required:true, default:function fn(){
				console.log(465123);
			}}
		}
	}
</script>
```



## 四、混入

### 1.概念

​			将组件的选项中的一部分分离出去，单独管理

### 2. 使用

​			1）全局混入（不推荐 ）

```vue
<!-- 第一步：创建mixin.js文件，文件名可以随便取 -->
export const hello ={
	methods : {
		hello(){
			console.log("混入");
		}
	}
}

<!-- 第二步：在main.js中引入混入的js文件 -->
import {hello} from './mixin.js'

<!-- 第三步：在main.js中使用混入 -->
Vue.mixin(hello)

<!-- 第四步：使用混入 -->
<template>
	<div>
		<button type="button" @click="hello">混入</button>		<!-- 混入调用的函数名 -->
	</div>
</template>


<script>
    export default {
		mixins:['hello'],		// 混入
    }
</script>
```

​		2）局部混入

```vue
<!-- 第一步：创建mixin.js文件，文件名可以随便取 -->
export const hello ={
	methods : {
		hello(){
			console.log("混入");
		}
	}
}


<!-- 使用混入 -->
<template>
	<div>
		<button type="button" @click="hello">混入</button>		<!-- 混入调用的函数名 -->
	</div>
</template>

<!-- 在需要的组件里引入minxin.js文件 -->
<script>
	// 引入minxing.js文件
    
    // 使用的话和全局差不多，就是引入不同
     export default {
		mixins:['hello'],		// 混入
    }
</script>
```



## 五、插件

### 1. 概念

​			插件通常用来为 Vue 添加全局功能

### 2. 使用

```vue
<!-- 第一步：创建并编写plug.js文件，文件名随便取 -->
export default{
	install (Vue, x, y, z){
		console.log(x, y, z);

		// 定义全局指令
		// 全局自定义指令
		Vue.directive('big', function(ele, bind){
			ele.innerText = bind.value*100;
		});
		
		// 定义混入
		Vue.mixin({
			data(){
				return{
					
				}
			},
			methods:{
				
			}
			
		});
		
		// 定义全局函数
		Vue.prototype.hi = ()=>{
			alert("hello");
		}
	}
	
}

<!-- 第二步：在main.js中引入并使用插件插件 -->
// 引入插件
import plugin from './plugin.js'

// 使用插件
Vue.use(plugin);

<!-- 使用插件 -->
<div>
    <!-- 插件调用的函数名 -->
    <button type="button" @click="hi">插件</button>		
</div>
```



## 六、自定义事件

### 1. 概念

​			自己定义一个事件，使其完成预想的操作

### 2. 使用

```vue
<script>
    export default {
        // 第一步
        mounted(){
            // $on：创建自定义事件		$once：只能触发一次的自定义事件
            
            // 通过ref标记一个节点来进行创建自定义事件
			this.$refs.school.$on('diy', data);		// data可以是任何数据类型包括函数
            
            // 不使用ref创建自定义事件
            this.$on('diy', data);			// data可以是任何数据类型包括函数
        }
        
    }
</script>

<!-- 第二步：在组件中使用自定义事件 -->
<template>
	<div>
		<button type="button" @click="diyEvent">使用自定义事件</button>
		<button type="button" @click="unbind">解绑自定义事件</button>
	</div>
</template>
<script>
	export default{
		data() {
			return{
				name : 'agui',
				adress : '湖南长沙'
			}
		},
		methods : {
			diyEvent(){
				// 使用自定义事件 ---  $emit
				this.$emit('diy', this.name);
			},
			unbind(){
				// 解绑事件
				this.$off('diy');
			}
		}
		
	}
</script>
```

### 3.全局事件总线

​			1）概念：一种组件间的通信方式，适用于任意组件间通信

​			2）使用

```vue
<!-- 第一步：在全局事件总线上定义一个函数hello -->
<template>
	<div>
		<h2>{{name}}</h2>
	</div>
</template>

<script>
	export default{
		data(){
			return{
				name : "阿君"
			}
		}，
		mounted(){
			// 在全局事件总线上定义一个函数hello
			this.$bus.$on("hello", (data) => {
				this.stuName = data;
			});
		}
	}
</script>

<!-- 第二步：在其他组件中使用全局事件总线 -->
<script>
	export default{
    	methods : {
            sendBusValue(){		// 使用事件总线
				this.$bus.$emit('hello', this.name);
			}
        }
    }
</script>
```



### 4. 其他

​			子向父传递数据、销毁组件代码见 **customassembly项目**



## 七、消息订阅及发布

### 1. 理解

​			1）这种方式的思想与全局事件总线很相似

​			2）它包含以下操作: 

​				① 订阅消息 --对应绑定事件监听

​				② 发布消息 --分发事件

​				③ 取消消息订阅 --解绑事件监听

​			3）实现需要引入一个消息订阅与发布的第三方实现库: PubSubJS

### 2. 使用

```shell
# 第一步：在对应的项目下安装PubSubJS
npm install -S pubsub-js
```

```vue
<script>
	// 第二步：引入PubSubJS
    import PubSub from 'pubsub-js'
    
   	// 发布订阅消息，发布之后会自动回调接收订阅的函数
	PubSub.publish("msgName", data);
    
    // 接收订阅消息的回调函数
    PubSub.subscribe("msgName", functon(msgName, data){ });
    
    // 当用完消息就一定要取消消息的订阅
   	PubSub.unsubscribe(发布事件唯一ID);
</script>
```



## 八、Vue中的Ajax

### 1. 解决开发环境 Ajax 跨域问题

​			1）cors配置响应头

​				缺点：麻烦后端人员，不安全，任何人都能访问

​			2）JSONP jQuery

​			3）使用代理服务器

```vue
<script>
	// 在项目跟目录下创建vue.config.js文件
    module.exports = {
	pages: {
		index: {
			// 入口
			entry: 'src/main.js',
		},
	},
	lintOnSave : false,		// 关闭语法检查
	// 开启代理服务器(一)
  	/**
	devServer: {
		proxy : '要代理的地址'
	}*/
        
    // 开启代理服务器(二)
    devServer: {		
		proxy : {		// 根据访问api的名字进行代理
            '/admin' : {
                target : '要代理的地址',
                pathRewrite : {
                    '^/admin' : ''
                },
                ws : true,			// 是否支持websocket
                changeOrigin : true	// 是否控制请求头中host值
            }
        }
	} 
}	
</script>
```

### 2.VUE 项目中常用的二个 Ajax 库 ----- axios

```shell
# 把axios安装在项目下
npm i –save axios

# 安装qs，qs作业将参数序列化成url 如；uid=cs11&pwd=abc&username=cs11
npm install qs
```

```vue
<script>
	// 在需要的组件中导入axios
    import axios from 'axios'
    
    // 使用
    export default {
        methods: {
            methodName01(){
                // data可以不写，data：传的参数（只适用于get，post方式不适用）
                axios
                .get("/api/emp", data)
                .then(resp => {
                    // 回调函数
                })
                .catch(erro => {
                    // 错误处理
                });
            },
            methodName02(){					// post方式
                // data可以不写，data：传的参数（只适用于get，post方式不适用）
                axios
                .put("/api/emp", qs.stringify(data))
                .then(resp => {
                    // 回调函数
                })
                .catch(erro => {
                    // 错误处理
                });
            }
            
        }
    }
</script>
```



## 九、slot 插槽 

### 1. 插槽内可以是任意内容

​			父组件向子组件传递带数据的标签，当一个组件有不确定的结构时, 就需要使用 slot 技术，注意：插槽内容是在父组件中编译后, 再传递给子组件的

### 2.插槽的分类及使用

​			1）默认插槽

```vue
<template>
	<div>
        <!-- 定义一个插槽 -->
        <slot></slot>
    </div>
</template>

<template>
	<!-- 使用 -->
	<组件名>
    	插槽内容
    </组件名>
</template>
```

​		2）命名插槽

```vue
<template>
	<div>
        <!-- 定义一个命名插槽 -->
        <slot name="game"></slot>
    </div>
</template>

<template>
	<!-- 使用 -->
	<组件名>
        插槽内容...
        <a slot="game" href="#">更多游戏</a>
    </组件名>
</template>
```

​		3）作用域插槽

```vue
<template>
	<div>
       	<!-- 作用域插槽 -->
		<slot :games="games"></slot>
    </div>
</template>

<template>
	<!-- 使用 -->
	<组件名>
        <!-- 作用域插槽scope -->
        <template scope="game">
            <ul>
                <li>dfasd</li>
    		</ul>
		</template>
    </组件名>
</template>
```



## 十、Vuex

### 1. 概念

​				专门在 Vue 中实现集中式状态（数据）管理的一个 Vue 插件，对 VUE 应用中多 个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且 适用于任意组件间通信。

### 2. Vuex的使用场景

​				1）多个组件依赖于同一状态

​				2）来自不同组件的行为需要变更同一状态

### 3. 项目下安装Vuex

```shell
npm i –save vuex
```

### 4. 核心概念和API

```vue

```



