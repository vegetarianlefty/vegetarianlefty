### 条件渲染

##### v-if

写法：v-if="表达式"   v-else-if="表达式"  v-else="表达式"

适用于：切换频率较低的场景。
特点：不展示的DOM元素直接被移除。
注意：v-if可以和:v-else-if、v-else一起使用，但要求结构不能被“打断”。

##### v-show

写法：v-show="表达式"

适用于：切换频率较高的场景。

特点：不展示的DOM元素未被移除，仅仅是使用样式隐藏掉。

注意：使用v-if的时，元素可能无法获取到，而使用v-show一定可以获取到。

	<!DOCTYPE html>
	<html>
		<head>
			<meta charset="utf-8" />
			<title></title>
			<script src="./js/vue.js" type="text/javascript" charset="utf-8"></script>
		</head>
		<body>
			
			<div id="root">
				<h2>当前的年龄值是:{{age}}</h2>
				<button @click="age++">点我年龄+1</button>
				<h1>hello，{{name}},年龄：{{age}}</h1>
				<!-- 使用v-show做条件渲染 -->
				<h2 v-show="true">欢迎你的到来:{{name}}</h2>
				
				<!-- v-else和v-else-if -->
				<div v-if="age === 4">婴儿</div>
				<div v-else-if="age === 7">儿童</div>
				<div v-else-if="age === 10">小孩</div>
			</div>
			<script type="text/javascript">
			Vue.config.productionTip = false
			new Vue({
				el:'#root',
				data:{
					name:"小灰灰",
					age:4
				}
			})
			</script>
		</body>
	</html>

### 列表渲染

v-for指令：

1. 用于展示列表数据。
2. 语法：v-for="(item, index) in object" :key="index"。
3. 可遍历：数组、对象、字符串（用的很少）、指定次数（用的很少）。

key  的作用：

key是虚拟DOM对象的标识，当数据发生变化时，Vue会根据新数据生成新的虚拟DOM, 随后Vue进行新虚拟DOM与旧虚拟DOM的差异比较。

若是旧虚拟DOM中找到了与新虚拟DOM相同的key:

1. 若虚拟DOM中内容没变, 直接使用之前的真实DOM.
2. 若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM。

旧虚拟DOM中未找到与新虚拟DOM相同的key,创建新的真实DOM，随后渲染到到页面。

<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>列表渲染</title>
		<script src="./js/vue.js" type="text/javascript" charset="utf-8"></script>
	</head>
	<body>

	<!DOCTYPE html>
	<html>
		<head>
			<meta charset="utf-8" />
			<title>列表渲染</title>
			<script src="./js/vue.js" type="text/javascript" charset="utf-8"></script>
		</head>
		<body>
			
			<div id="root">
				<!-- 遍历数组 -->
				<h2>人员列表（遍历数组）</h2>
				<ul>
					<li v-for="(p,index) of persons" :key="index">
						{{p.name}}-{{p.age}}
					</li>
				</ul>
				
				<!-- 遍历对象 -->
				<h2>汽车信息（遍历对象）</h2>
				<ul>
					<li v-for="(value,k) of car" :key="k">
						{{k}}-{{value}}
					</li>
				</ul>
				
				<!-- 遍历字符串 -->
				<h2>测试遍历字符串（用得少）</h2>
				<ul>
					<li v-for="(char,index) of str" :key="index">
						{{char}}-{{index}}
					</li>
				</ul>
				
				<!-- 遍历指定次数 -->
				<h2>测试遍历指定次数（用得少）</h2>
				<ul>
					<li v-for="(number,index) of 5" :key="index">
						{{index}}-{{number}}
					</li>
				</ul>
			</div>
			<script type="text/javascript">
			Vue.config.productionTip = false
			new Vue({
					el:'#root',
					data:{
						persons:[
							{id:'001',name:'张三',age:18},
							{id:'002',name:'李四',age:19},
							{id:'003',name:'王五',age:20}
						],
						car:{
							name:'奥迪A8',
							price:'70万',
							color:'黑色'
						},
						str:'hello'
					}
				})
			</script>
		</body>
	</html>

### 收集表单数据

#### 文本

```
<input type="text" v-model="name">
```

#### 多行文本 

```
<textarea v-model="message"></textarea>
#注意在 <textarea> 中是不支持插值表达式的。请使用 v-model 来替代：
```

#### 复选框 

单一的复选框，绑定布尔类型值：

```
<input type="checkbox" id="checkbox" v-model="checked" />
```

多个复选框绑定到同一个数组或集合的值:

```
  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames" />
  <label for="jack">Jack</label>
 
  <input type="checkbox" id="john" value="John" v-model="checkedNames" />
  <label for="john">John</label>
 
  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames" />
  <label for="mike">Mike</label>
```

#### 单选按钮 

```
<div>Picked: {{ picked }}</div>

<input type="radio" id="one" value="One" v-model="picked" />
<label for="one">One</label>

<input type="radio" id="two" value="Two" v-model="picked" />
<label for="two">Two</label>
```

#### 下拉框

```
<div>Selected: {{ selected }}</div>

<select v-model="selected">
  <option disabled value="">Please select one</option>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
```

#### 修饰符 

```
#lazy
默认情况下，v-model 会在每次 input 事件后更新数据 (IME 拼字阶段的状态例外)。你可以添加 lazy 修饰符来改为在每次 change 事件后更新数据：
<!-- 在 "change" 事件后同步更新而不是 "input" -->
<input v-model.lazy="msg" />

#number
如果你想让用户输入自动转换为数字，你可以在 v-model 后添加 .number 修饰符来管理输入：
<input v-model.number="age" />
如果该值无法被 parseFloat() 处理，那么将返回原始值。
number 修饰符会在输入框有 type="number" 时自动启用。

#trim
如果你想要默认自动去除用户输入内容中两端的空格，你可以在 v-model 后添加 .trim 修饰符：
<input v-model.trim="msg" />
```



```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>收集表单数据</title>
		<script src="./js/vue.js" type="text/javascript" charset="utf-8"></script>
	</head>
	<body>
		<!-- 准备好一个容器-->
		<div id="root">
			<form @submit.prevent="demo">
				账号：	 <br/><br/>
				密码：<input type="password" v-model="userInfo.password"> <br/><br/>
				年龄：<input type="number" v-model.number="userInfo.age"> <br/><br/>
				性别：
				男<input type="radio" name="sex" v-model="userInfo.sex" value="male">
				女<input type="radio" name="sex" v-model="userInfo.sex" value="female"> <br/><br/>
				爱好：
				学习<input type="checkbox" v-model="userInfo.hobby" value="study">
				打游戏<input type="checkbox" v-model="userInfo.hobby" value="game">
				吃饭<input type="checkbox" v-model="userInfo.hobby" value="eat">
				<br/><br/>
				所属校区
				<select v-model="userInfo.city">
					<option value="">请选择校区</option>
					<option value="beijing">北京</option>
					<option value="shanghai">上海</option>
					<option value="shenzhen">深圳</option>
					<option value="wuhan">武汉</option>
				</select>
				<br/><br/>
				其他信息：
				<textarea v-model.lazy="userInfo.other"></textarea> <br/><br/>
				<input type="checkbox" v-model="userInfo.agree">阅读并接受<a href="http://www.bai.com">《用户协议》</a>
				<button>提交</button>
			</form>
		</div>
		<script type="text/javascript">
		Vue.config.productionTip = false
		new Vue({
			el:'#root',
			data:{
				userInfo:{
					account:'',
					password:'',
					age:18,
					sex:'female',
					hobby:[],
					city:'beijing',
					other:'',
					agree:''
				}
			},
			methods: {
				demo(){
					console.log(JSON.stringify(this.userInfo))
				}
			}
		})
		</script>
	</body>
</html>
```

