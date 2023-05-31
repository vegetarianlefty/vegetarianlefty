### Vue 实例

#### 创建一个 Vue 实例

- 每个 Vue 应用都是通过用 Vue 函数创建一个新的 Vue 实例开始的。
- 当创建一个 Vue 实例时，你可以传入一个选项对象。
- 创建的根 Vue 实例，以及可选的嵌套的、可复用的组件树组成。
- 所有的 Vue 组件都是 Vue 实例，并且接受相同的选项对象 (一些根实例特有的选项除外)。

```
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8" />
	<title>初识Vue</title>
	<!-- 引入Vue -->
	<script type="text/javascript" src="../js/vue.js"></script>
</head>

<body>

<!-- 准备好一个容器 -->
<div id="demo">
	<h1>Hello，{{name}}，{{address}}</h1>
</div>

<script type="text/javascript" >
	Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

	//创建Vue实例
	new Vue({
		el:'#demo', //el用于指定当前Vue实例为哪个容器服务，值通常为css选择器字符串。
		data:{ //data中用于存储数据，数据供el所指定的容器去使用，值我们暂时先写成一个对象。
			name:'小灰灰',
			address:'北京'
		}
	})

</script>
</body>
</html>
```

### Vue 模板语法

Vue.js 使用了基于 HTML 的模板语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。所有 Vue.js 的模板都是合法的 HTML，所以能被遵循规范的浏览器和 HTML 解析器解析。

在底层的实现上，Vue 将模板编译成虚拟 DOM 渲染函数。结合响应系统，Vue 能够智能地计算出最少需要重新渲染多少组件，并把 DOM 操作次数减到最少。

#### 插值语法

数据绑定最常见的形式就是使用“Mustache”语法 (双大括号) 的文本插值，用于解析标签体内容：

```
<span>Message: {{ msg }}</span>
```

Mustache 标签将会被替代为对应数据对象上 msg property 的值。无论何时，绑定的数据对象上 msg property 发生了改变，插值处的内容都会更新。

#### 指令语法

Mustache 语法不能作用在 HTML attribute 上，遇到这种情况应该使用 v-bind 指令，用于解析标签（包括：标签属性、标签体内容、绑定事件.....）.v-bind:href="xxx" 或  简写为 :href="xxx"，xxx同样要写js表达式，且可以直接读取到data中的所有属性。

	<!DOCTYPE html>
	<html>
		<head>
			<meta charset="utf-8" />
			<title>模板语法</title>
			<script src="js/vue.js" type="text/javascript" charset="utf-8"></script>
		</head>
		<body>
			
			<div id="root">
				<h1>插值语法</h1>
				<h3>你好，{{name}}</h3>
				<hr/>
				<h1>指令语法</h1>
				<a v-bind:href="url">点我去{{name}}网站</a>
			</div>
			<script type="text/javascript">
			Vue.config.productionTip=false;
			new Vue({
				el:'#root',
				data:{
					name:"吃素的左撇子",
					url:"http://www.wangxiaohuan.com"
				}
			})
			</script>
		</body>
	</html>

<div v-bind:id="dynamicId"></div>

### 数据绑定

#### 单向绑定(v-bind)

数据只能从data流向页面。

#### 双向绑定(v-model)

数据不仅能从data流向页面，还可以从页面流向data。双向绑定一般都应用在表单类元素上（如：input、select等）

<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>数据绑定</title>
		<script src="js/vue.js" type="text/javascript" charset="utf-8"></script>
	</head>
	<body>



	<!DOCTYPE html>
	<html>
		<head>
			<meta charset="utf-8" />
			<title>数据绑定</title>
			<script src="js/vue.js" type="text/javascript" charset="utf-8"></script>
		</head>
		<body>
			
			<div id="root">
				<h1>单向绑定(v-bind)</h1>
				<input type="text" v-bind:value="name">
				<hr/>
				<h1>双向绑定(v-model)</h1>
				<input type="text" v-model:value="name">
			</div>
			<script type="text/javascript">
			Vue.config.productionTip=false;
			new Vue({
				el:'#root',
				data:{
					name:"吃素的左撇子",
					url:"http://www.wangxiaohuan.com"
				}
			})
			</script>
		</body>
	</html>




### data与el的2种写法

#### el有2种写法

- new Vue时候配置el属性。
- 先创建Vue实例，随后再通过vm.$mount('#root')指定el的值。

#### data有2种写法

- 对象式
- 函数式

**如何选择：目前哪种写法都可以，以后学习到组件时，data必须使用函数式，否则会报错。**

***一个重要的原则：***
***由Vue管理的函数，一定不要写箭头函数，一旦写了箭头函数，this就不再是Vue实例了。***



```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>el与data的两种写法</title>
		<script src="js/vue.js" type="text/javascript" charset="utf-8"></script>
	</head>
	<body>
		
		<div id="root1">
			<h1>{{name}}</h1>
		</div>
		<div id="root2">
			<h1>{{name}}</h1>
		</div>
		<div id="root3">
			<h1>{{name}}</h1>
		</div>
		<div id="root4">
			<h1>{{name}}</h1>
		</div>
		<script type="text/javascript">
		Vue.config.productionTip=false;
		//el的两种写法
		new Vue({
			el:'#root1', //第一种写法
			data:{
				name:'吃素的左撇子1'
			}
		})
		const v = new Vue({
			data:{
				name:'吃素的左撇子2'
			}
		})
		console.log(v)
		v.$mount('#root2') //第二种写法 */
		
		//data的两种写法
		new Vue({
			el:'#root3',
			//data的第一种写法：对象式
		    data:{
				name:'吃素的左撇子3'
			} 
		})
		new Vue({
			el:'#root4',
			data(){
				console.log('@@@',this) //此处的this是Vue实例对象
				return{
					name:'吃素的左撇子4'
				}
			}
		})
		</script>
	</body>
</html>

```

### MVVM 模型

M:模型(Model) :对应 data 中的数据

 V:视图(View) :模板

VM:视图模型(ViewModel) : Vue 实例对象

![image-20221220174656715](http://image.wangxiaohuan.com/blog/image/202212201747509.png)

### 数据代理

1.Vue 中的数据代理，通过 vm 对象来代理 data 对象中属性的操作（读/写）

2.Vue 中数据代理的好处，更加方便的操作 data 中的数据

3.基本原理：

- 通过 Object.defineProperty() 把 data 对象中所有属性添加到 vm 上。
- 为每一个添加到 vm 上的属性，都指定一个 getter/setter。
- 在 getter/setter 内部去操作（读/写）data 中对应的属性。

### 事件处理

事件的基本使用：

1.使用 v-on:xxx 或 @xxx 绑定事件，其中 xxx 是事件名；
2.事件的回调需要配置在 methods 对象中，最终会在 vm 上；
3.methods 中配置的函数，不要用箭头函数！否则 this 就不是 vm 了；
4.methods 中配置的函数，都是被 Vue 所管理的函数，this 的指向是 vm 或 组件实例对象；

<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>事件处理</title>
		<script src="./js/vue.js" type="text/javascript" charset="utf-8"></script>
	</head>
	<body>


	<!DOCTYPE html>
	<html>
		<head>
			<meta charset="utf-8" />
			<title>事件处理</title>
			<script src="./js/vue.js" type="text/javascript" charset="utf-8"></script>
		</head>
		<body>
			
			<div id="root">
				<h1>hello，{{name}},你好</h1>
				<button v-on:click="shouInfo1">点击提示信息1</button>
				<button @click="shouInfo2(66,$event)">点击提示信息2</button>
			</div>
			<script type="text/javascript">
			Vue.config.productionTip = false
			const vm = new Vue({
				el:'#root',
				data:{
					name:"小灰灰"
				},
				methods:{
					shouInfo1(){
						alert("欢迎光临！")
					},
					shouInfo2(value,event){
						console.log(value,event)
						alert("欢迎光临！"+value)
					}
				}
			})
			</script>
		</body>
	</html>

Vue 中的事件修饰符：

1. prevent：阻止默认事件（常用）
2. stop：阻止事件冒泡（常用）
3. once：事件只触发一次（常用）
4. capture：使用事件的捕获模式
5. self：只有 event.target 是当前操作的元素时才触发事件
6. passive：事件的默认行为立即执行，无需等待事件回调执行完毕


	<!DOCTYPE html>
	<html>
		<head>
			<meta charset="utf-8" />
			<title>事件修饰符</title>
			<script src="./js/vue.js" type="text/javascript" charset="utf-8"></script>
		    <style>
		    	*{
		    		margin-top: 20px;
		    	}
		    	.demo1{
		    		height: 50px;
		    		background-color: skyblue;
		    	}
		    	.box1{
		    		padding: 5px;
		    		background-color: skyblue;
		    	}
		    	.box2{
		    		padding: 5px;
		    		background-color: orange;
		    	}
		    	.list{
		    		width: 200px;
		    		height: 200px;
		    		background-color: peru;
		    		overflow: auto;
		    	}
		    	li{
		    		height: 100px;
		    	}
		    </style> 
		</head>
		<body>
			<!--
					Vue中的事件修饰符：
							1.prevent：阻止默认事件（常用）；
							2.stop：阻止事件冒泡（常用）；
							3.once：事件只触发一次（常用）；
							4.capture：使用事件的捕获模式；
							5.self：只有 event.target 是当前操作的元素时才触发事件；
							6.passive：事件的默认行为立即执行，无需等待事件回调执行完毕；		
			-->
			<div id="root">
				<h1>hello，{{name}},你好</h1>
				<!-- 阻止默认事件（常用） -->
				<a href="http://www.wangxiaohuan.com" @click.prevent="showInfo">点我提示信息</a>
			
				<!-- 阻止事件冒泡（常用） -->
				<div class="demo1" @click="showInfo">
					<button @click.stop="showInfo">点我提示信息</button>
				</div>
			
				<!-- 事件只触发一次（常用） -->
				<button @click.once="showInfo">点我提示信息</button>
			
				<!-- 使用事件的捕获模式 -->
				<div class="box1" @click.capture="showMsg(1)">
					div1
					<div class="box2" @click="showMsg(2)">
						div2
					</div>
				</div>
			
				<!-- 只有event.target是当前操作的元素时才触发事件； -->
				<div class="demo1" @click.self="showInfo">
					<button @click="showInfo">点我提示信息</button>
				</div>
			
				<!-- 事件的默认行为立即执行，无需等待事件回调执行完毕； -->
				<ul @wheel.passive="demo" class="list">
					<li>1</li>
					<li>2</li>
					<li>3</li>
					<li>4</li>
				</ul>
			
			</div>
			<script type="text/javascript">
			Vue.config.productionTip = false
			const vm = new Vue({
				el:'#root',
				data:{
					name:"小灰灰"
				},
				methods:{
					showInfo(e){
						alert("欢迎光临！")
						// console.log(e.target)
					},
					showMsg(msg){
						console.log(msg)
					},
					demo(){
						for (let i = 0; i < 1000; i++) {
							console.log('#')
						}
						console.log('累坏了')
					}
				}
			})
			</script>
		</body>
	</html>

### 计算属性

1. 定义：要用的属性不存在，要通过已有属性计算得来。

2. 原理：底层借助了 Objcet.defineproperty 方法提供的 getter 和 setter。

3. get 函数什么时候执行？
   (1).初次读取时会执行一次。
   (2).当依赖的数据发生改变时会被再次调用。

4. 优势：与 methods 实现相比，内部有缓存机制（复用），效率更高，调试方便。

5. 计算属性最终会出现在vm上，直接读取使用即可。

6. 如果计算属性要被修改，那必须写set函数去响应修改，且set中要引起计算时依赖的数据发生改变。

   <!DOCTYPE html>
   <html>
   	<head>
   		<meta charset="utf-8" />
   		<title>计算属性</title>
   		<script src="./js/vue.js" type="text/javascript" charset="utf-8"></script>
   	</head>
   	<body>
   		<!-- 插值语法实现 -->
   		<div id="root1">
   			姓：<input type="text" v-model:value="firstName"><br>
   			名：<input type="text" v-model:value="lastName"><br>
   		   全名：<span>{{firstName.slice(0,3)}}-{{lastName}}</span><br>
   		</div>
   		<!-- methods实现 -->
   		<div id="root2">
   			姓：<input type="text" v-model:value="firstName"><br>
   			名：<input type="text" v-model:value="lastName"><br>
   		   全名：<span>{{fullName()}}</span><br>
   		</div>
   		<!-- 计算属性实现 -->
   		<div id="root3">
   			姓：<input type="text" v-model:value="firstName"><br>
   			名：<input type="text" v-model:value="lastName"><br>
   		   全名：<span>{{fullName}}</span><br>
   		</div>
   		<script type="text/javascript">
   		Vue.config.productionTip = false
   		new Vue({
   			el:'#root1',
   			data:{
   				firstName:'张',
   				lastName:'三'
   			}
   		})


   		new Vue({
   			el:'#root2',
   			data:{
   				firstName:'张',
   				lastName:'三'
   			},
   			methods:{
   				fullName(){
   					return this.firstName+"-"+this.lastName;
   				}
   			}
   		})
   		
   		new Vue({
   			el:'#root3',
   			data:{
   				firstName:'张',
   				lastName:'三'
   			},
   			computed: {
   				//复杂写法
   				fullName:{
   					get(){
   						console.log('get被调用了');
   						return this.firstName+'-'+this.lastName;
   					},
   					//set什么时候调用? 当fullName被修改时。
   					set(value){
   						console.log('set',value);
   						const arr = value.split('-');
   						this.firstName = arr[0];
   						this.lastName = arr[1];
   					}
   				},
   				//简单写法
   				fullName(){
   					console.log('get1被调用了');
   					return this.firstName+'-'+this.lastName;
   				}
   			}
   		})
   		</script>
   	</body>

   </html>

### 监视属性 watch：

1. 当被监视的属性变化时, 回调函数自动调用, 进行相关操作
2. 监视的属性必须存在，才能进行监视

##### 深度监视

1. Vue中的watch默认不监测对象内部值的改变（一层）。
2. 配置deep:true可以监测对象内部值改变（多层）。

注意⚠️：

*Vue自身可以监测对象内部值的改变，但Vue提供的watch默认不可以！*

*使用watch时根据数据的具体结构，决定是否采用深度监视。*

	<!DOCTYPE html>
	<html>
		<head>
			<meta charset="utf-8" />
			<title>监视属性</title>
			<script src="./js/vue.js" type="text/javascript" charset="utf-8"></script>
		</head>
		<body>
			
			<div id="root">
				<h1>你今天很{{info}}</h1>
				<button @click="change">切换</button>
			</div>
			<script type="text/javascript">
			Vue.config.productionTip = false
			new Vue({
				el:'#root',
				data:{
					isFace:true
				},
				computed:{
					info(){
						return this.isFace ? '漂亮':'丑'
					}
				},
				methods:{
					change(){
						this.isFace = !this.isFace
					}
				},
				watch:{
					isFace:{
						handler(newValue,oldValue){
							console.log("isFace被修改了",newValue,oldValue)
						}
					}
				}
				
			})
			</script>
		</body>
	</html>

#### computed 和 watch 之间的区别：

1. computed 能完成的功能，watch 都可以完成。
2. watch 能完成的功能，computed 不一定能完成，例如：watch 可以进行异步操作。

*备注：*

*所被Vue管理的函数，最好写成普通函数，这样this的指向才是vm 或 组件实例对象。*

*所有不被Vue所管理的函数（定时器的回调函数、ajax的回调函数等、Promise的回调函数），最好写成箭头函数，*
*这样this的指向才是vm 或 组件实例对象。*

### 绑定样式

- class样式：写法:class="xxx" xxx可以是字符串、对象、数组。

1. 字符串写法适用于：类名不确定，要动态获取。
2. 对象写法适用于：要绑定多个样式，个数不确定，名字也不确定。
3. 数组写法适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用。

- style样式

1. :style="{fontSize: xxx}"其中xxx是动态值。

2. :style="[a,b]"其中a、b是样式对象。

   <!DOCTYPE html>
   <html>
   	<head>
   		<meta charset="utf-8" />
   		<title></title>
   		<style>
   			.basic{
   				width: 400px;
   				height: 100px;
   				border: 1px solid black;
   			}


   	<!DOCTYPE html>
   	<html>
   		<head>
   			<meta charset="utf-8" />
   			<title></title>
   			<style>
   				.basic{
   					width: 400px;
   					height: 100px;
   					border: 1px solid black;
   				}
   				
   				.happy{
   					border: 4px solid red;;
   					background-color: rgba(255, 255, 0, 0.644);
   					background: linear-gradient(30deg,yellow,pink,orange,yellow);
   				}
   				.sad{
   					border: 4px dashed rgb(2, 197, 2);
   					background-color: gray;
   				}
   				.normal{
   					background-color: skyblue;
   				}
   			
   				.xiaohuihui1{
   					background-color: yellowgreen;
   				}
   				.xiaohuihui2{
   					font-size: 30px;
   					text-shadow:2px 2px 10px red;
   				}
   				.xiaohuihui3{
   					border-radius: 20px;
   				}
   			</style>
   			<script src="./js/vue.js" type="text/javascript" charset="utf-8"></script>
   		</head>
   	<body>
   		
   		<div id="root">
   			<!-- 绑定class样式--字符串写法，适用于：样式的类名不确定，需要动态指定 -->
   			<div class="basic" :class="mood" @click="changeMood">{{name}}</div> <br/><br/>
   			
   			<!-- 绑定class样式--数组写法，适用于：要绑定的样式个数不确定、名字也不确定 -->
   			<div class="basic" :class="classArr">{{name}}</div> <br/><br/>
   				<!-- 绑定class样式--对象写法，适用于：要绑定的样式个数确定、名字也确定，但要动态决定用不用 -->
   	​			<div class="basic" :class="classObj">{{name}}</div> <br/><br/>
   	​			
   	​			<!-- 绑定style样式--对象写法 -->
   	​			<div class="basic" :style="styleObj">{{name}}</div> <br/><br/>
   	​			
   	​			<!-- 绑定style样式--数组写法 -->
   	​			<div class="basic" :style="styleArr">{{name}}</div>
   	​		</div>
   	​		<script type="text/javascript">
   	​		Vue.config.productionTip = false
   	​		new Vue({
   	​			el:'#root',
   	​			data:{
   	​				name:"小灰灰",
   	​				mood:'normal',
   	​				classArr:['xiaohuihui1','xiaohuihui2','xiaohuihui3'],
   	​				classObj:{
   	​					xiaohuihui1:false,
   	​					xiaohuihui2:false,
   	​				},
   	​				styleObj:{
   	​					fontSize: '40px',
   	​					color:'red',
   	​				},
   	​				styleArr:[
   	​					{
   	​						fontSize: '40px',
   	​						color:'blue',
   	​					},
   	​					{
   	​						backgroundColor:'gray'
   	​					}
   	​				]
   	​			},
   	​			methods:{
   	​				changeMood(){
   	​					const arr = ['happy','sad','normal']
   	​					const index = Math.floor(Math.random()*3)
   	​					this.mood = arr[index]
   	​				}
   	​			}
   	​		})
   	​		</script>
   	​	</body>
   	​	</html>


   ​		