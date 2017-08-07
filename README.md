# 第一章 Vue.js特性

1. 确实轻量

    除了以MVP模式代表的Riot.js外，Vue.js已经算是前端库里面体积非常小的，但不依赖其他基础库。

2. 数据绑定

    对于一些富交互、状态机类似的前端UI界面，数据绑定非常方便、简单。

3. 指令

    类似于AngularJS，可以使用一些内置的简单指令(v-*)，也可以自定义指令，通过对应表达式值得变化就可以修改对应的DOM。

4. 插件化

    Vue.js核心库不包含Router、AJAX、表单验证等功能，但是可以非常方便地加载对应的插件。

# Vue.js与其他框架的区别

## 与AngularJS的区别

相同点：

- 都支持指令--内置指令和自定义指令。
- 都支持过滤器--内置过滤器和自定义过滤器。
- 都支持双向绑定。
- 都不支持低端浏览器(比如IE6/7/8)
    + Vue.js使用比如Array.isArray的ES5特性。
    + AngularJS 1.3开始不支持IE8。

不同点：

- AungularJS的学习成本比较高，而Vue.js本身提供的API都比较简单、直观。
- 在性能上，AngularJS依赖对数据做脏检查，所以Watcher越多越慢。Vue.js使用基于依赖追踪的观察并且使用异步队列更新，所有的数据都是独立触发的。

## 与React的区别

相同点：

- React采用特殊的JSX语法，Vue.js在组件开发中也推崇编写.vue特殊文件格式，对文件内容都有一些约定，两者都需要编译后使用。
- 中心思想相同：一切都是组件，组件实例之间可以嵌套。
- 都提供合理的钩子函数，可以让开发者定制化地处理需求。
- 都不内置类似AJAX、Router等功能到核心包，而是以其他方式(插件)加载。
- 在组件开发中都支持mixns的特性。

不同点：

- React依赖Virtual DOM，而Vue.js使用的是DOM模版。React采用的Virtual DOM会对渲染出来的结果做脏检查。
- Vue.js在模版中提供了指令、过滤器等，可以非常方便、快捷地操作DOM。

## 安装

- script

    直接通过script加载CDN文件，代码示例如下：

    `<script src="http://apps.bdimg.com/libs/vue/1.0.14/vue.js"></script>`

- npm

    如果项目基于npm管理依赖，则可以使用npm来安装Vue，执行如下命令：

    `$ npm i vue --save-dev`

- bower

    如果项目基于bower管理依赖，则可以使用bower来安装Vue，执行如下命令：

    `$ bower i vue --save-dev`

## 第一个HELLO World程序

代码示例如下：

```html
<div id="didi-navigator">
  <ul>
    <li v-for="tab in tabs">
      {{ tab.text }}
    </li>
  </ul>
</div>

new Vue({
  el: "#didi-navigator",
  data: {
    tabs: [
      { text: '巴士' },
      { text: '快车' },
      { text: '专车' },
      { text: '顺风车' },
      { text: '出租车' },
      { text: '代驾' }
    ]
  }
})
```

# 第二章 数据绑定

参考资料：

[Vue.js之数据绑定](http://www.cnblogs.com/fly_dragon/p/6218675.html "Vue.js之数据绑定")

## 插值

文本插值是最基本的形式，使用双大括号{{}}，代码示例如下：

`<span>Text: {{ text }}</span>`

只需要渲染一次数据，后续数据变化不再关心，可以通过"*"实现，代码示例如下：

`<span>Text: {{ *text }}</span>`

双大括号标签会把里面的值全部当做字符串来处理，如果是HTML片段，则可以使用三个大括号来绑定，代码示例如下：

```html
<div>Logo: {{{ logo }}}</div>
logo: '<span>DDFE</span>'
```

双大括号还可以放在HTML标签内，示例如下：

`<li data-id='{{ id }}'></li>`

## 表达式

Mustache标签也接受表达式形式的值，表达式可由JavaScript表达式和过滤器构成。过滤器可以没有，也可以有多个。

表达式是各种数值、变量、运算符的综合体。简单的表达式可以是常量或者变量名称。表达式的值是其运算结果，代码示例如下：

```html
<!-- JS表达式 -->
{{ cents/100 }}     //在原值的基础上除以100
{{ true ? 1 : 0 }}  //值为真，则渲染出1，否则渲染出0
{{ example.split(",") }}

<!-- 无效示例 -->
{{ var logo = 'DDFE' }}       //这是语句，不是表达式
{{ if(true) return 'DDFE' }}  //条件控制语句是不支持的，可以使用三元式
```

类似于Linux中的管道，Vue.js允许在表达式后面添加过滤符，代码示例如下：

`{{ example | toUpperCase }}`

## 指令

![修改绑定语法](images/1.png)

# 第三章 指令

## 内部指令

### v-if

v-if指令可以完全根据表达式的值在DOM中生成或移除一个元素。如果v-if表达式赋值为false，那么对应的元素就会从DOM中删除；否则，对应元素的一个克隆将被重新插入DOM中。代码示例如下：

```html
<div id="example">
	<span>nothing</span>
	<p v-if="greeting">Hello</p>
</div>
<script>
	var app = new Vue({
		el: "#example",
		data: {
			greeting: true
		}
	})
</script>
```

因为v-if是一个指令，需要将它添加到一个元素上。但是如果想切换多个元素，则可以把&lt;template&gt;元素当做包装元素，并在其上使用v-if，最终的渲染结果不会包含它。代码示例如下：

```html
<template v-if="ok">
	<h1>Title</h1>
	<p>Paragraph 1</p>
	<p>Paragraph 2</p>
</template>
```

### v-show

v-show指令是根据表达式的值来显示或者隐藏HTML元素。当v-show赋值为false时，元素将被隐藏。查看DOM时，会发现元素上多了一个内联样式`style="display: none;"`。代码示例如下：

```html
<input type="text" v-model="message" placeholder="edit me">
<div id="example">
  <p v-show="greeting">Hello!</p>
</div>
<script>
  var vm = new Vue({
    el: "#example",
    data: {
      greeting: false
    }
  })
</script>
```

**注：v-show不支持\<template\>语法，也不支持`v-else`**

在切换v-if模块时，Vue.js有一个局部编译/卸载的过程，因为v-if中的模版可能包括数据绑定或子组件。v-if是真实的条件渲染，因为它会确保条件块在切换时合适地销毁与重建条件块内的事件监听器和子组件。

v-if是惰性的---如果初始渲染时条件为假，则什么也不做，在条件第一次变为真时才开始局部编译(编译会被缓存起来)。

相比之下，v-show简单得多---元素始终被编译并保留，只是简单的基于CSS切换。

一般来说，v-if有更高的切换消耗，而v-show有更高的初始渲染消耗。因此，如果需要频繁地切换，则使用v-show较好；如果在运行条件不大可能改变，则使用v-if较好。

### v-else

就是JavaScript中else的意思，它必须跟着v-if或v-show，充当else功能。代码示例如下：

```html
<body>
  <div id="example">
    <p v-if="ok">我是对的</p>
    <p v-else="ok">我是错的</p>
  </div>
</body>
<script>
  var exampleVM2 = new Vue({
    el: '#example',
    data: {
      ok: false
    }
  })
</script>
```

将v-show用在组件上时，因为指令的优先级v-else会出现问题，所以不要这样做。代码示例如下：

```html
<custom-component v-show="condition"></custom-component>
<p v-else>这可能也是一个组件</p>
```

我们可以用另一个v-show替换v-else，代码示例如下：

```html
<custom-component v-show="condition"></custom-component>
<p v-show="!condition">这可能也是一个组件</p>
```

### v-model

v-model指令用来在input、select、text、checkbox、radio等表单控件元素上创建双向数据绑定。根据控件类型v-model自动选取正确的方法更新元素。但是v-model不过是语法糖，在用户输入时间中更新元素，以及特别处理一些极端的例子。代码示例如下：

```html
<div id="example">
  <form>
  	姓名：
  	<input type="text" v-model="data.name" placeholder="">
  	<br>
  	性别：
  	<input type="radio" id="man" value="One" v-model="data.sex">
  	<label for="man">男</label>
  	<input type="radio" id="male" value="Two" v-model="data.sex">
  	<label for="male">女</label>
  	<br>
  	兴趣：
  	<input type="checkbox" id="book" value="book" v-model="data.interest">
  	<label for="book">阅读</label>
  	<input type="checkbox" id="swim" value="swim" v-model="data.interest">
  	<label for="swim">游泳</label>
  	<input type="checkbox" id="game" value="game" v-model="data.interest">
  	<label for="game">游戏</label>
  	<input type="checkbox" id="song" value="song" v-model="data.interest">
  	<label for="song">唱歌</label>
  	<br>
  	身份：
  	<select v-model="data.identity">
  		<option value="teacher" selected="selected">教师</option>
  		<option value="doctor">医生</option>
  		<option value="lawyer">律师</option>
  	</select>
  </form>
  {{data.name}}
  <br>
  {{data.sex}}
  <br>
  {{data.interest}}
  <br>
  {{data.identity}}
  <br>
</div>
<script>
var vm = new Vue({
	el: "#example",
	data: {
		data: {
			name: '',
			sex: '',
			interest: [],
			identity: ''
		}
	}
})
</script>
```

除了以上用法，在v-model指令后面还可以添加多个参数(number、lazy、debounce)。

1. number

   如果想将用户的输入自动转换为Number类型(如果原值的转换结果为NaN，则返回原值)，则可以添加一个number特性。

2. lazy

    在默认情况下，v-model在input事件中同步输入框的值与数据，我们可以添加一个lazy特性，从而将数据改到change事件中发生。代码示例如下：

    ```html
    <body id="example">
      <input v-model="msg" lazy>
      <br>
      {{ msg }}
      <script>
        var exampleVM2 = new Vue({
          el: '#example',
          data: {
            msg: '内容是在change事件后才改变的~'
          }
        })
      </script>
    </body>
    ```

3. debounce

    设置一个最小的延时，在每次敲击之后延时同步输入框的值与数据。如果每次更新都要进行高耗操作(例如，在input中输入内容时要随时发送AJAX请求)，那么它较为有用。代码示例如下：

    ```html
    <body id="example">
    	<input type="text" v-model="msg" debounce="5000">
    	<br>
    	{{ msg }}
    	<script>
    		var vm = new Vue({
    			el: "#example",
    			data: {
    				msg: '内容是在5000ms后才改变的~'
    			}
    		})
    	</script>
    </body>
    ```

### v-for

我们可以使用v-for指令基于源数据重复渲染元素。我们也可以使用$index来呈现相对应的数据索引，代码示例如下：

```html
<body id="example">
  <ul id="demo">
    <li v-for="item in items" class="item-{{$index}}">
	  {{ $index }} - {{ parentMssage }}  {{ item.msg }}
	</li>
  </ul>
  <script>
    var vm = new Vue({
	  el: "#example",
	    data: {
	      parentMssage:'滴滴',
		  items: [
		    { msg: '滴滴顺风车' },
		    { msg: '滴滴专车' }
	      ]
	    }
	})
  </script>
</body>
```

v-for需要特殊的别名，形式为`item in items`(items是数据数组，item是当前数组元素的别名)。Vue.js 1.0.17及以后版本支持of分隔符，用法如下：

`<div v-for="item of items"></div>`

使用v-for，将得到一个特殊的作用域，类似于AngularJS的隔离作用域，我们需要明确指定prop属性传递数据，否则在组件内将获取不到数据。对于组件内的&lt;p&gt;标签，我们可以使用&lt;slot&gt;

```html
<my-item v-for="item in items" :item="item" :index="$index">
  <p>{{item.text}}</p>
<my-item>
```

Vue.js包装了被观察数组的变异方法，它们能触发视图更新。被包装的方法有：

- push()
- pop()
- shift()
- unshift()
- splice()
- sort()
- reverse()

Vue.js还增加了两个方法来观测变化：$set、$remove。

我们应该尽量避免直接设置数据绑定的数组元素，因为这些变化不会被Vue.js检测到，因而也不会更新视图渲染。这时，我们可以使用$set方法：

```javascript
// same as `demo.items[0] = ...` but triggers view update
demo.item.$set(0, { childMsg: 'Changed!' })
```

$remove是splice的语法糖，用于从目标数组中查找并删除元素。因此，不必这样：

```javascript
var index = this.items.indexOf(item)
if(index !== -1) {
  this.item.splice(index, 1)
}
```

只用这样：

```javascript
demo.item.$remove(item)
```

另外，也可以使用filter、concat、slice方法，返回的数组将是一个不同的实例。我们可以用新的数组替换原来的数组。

```javascript
demo.items = demo.items.filter(function(item) {
  return items.childMsg.match(/Hello/)
})
```

在某些情况下，我们有时可能需要用全新对象(例如，通过API调用创建的对象)来替换数组。因为在默认情况下，v-for通过数据对象的特征来决定对已有作用域和DOM元素的复用程度，这可能导致重新渲染整个列表。但是，如果每个对象都有一个唯一的ID属性，便可以使用track-by特性给Vue.js一个提示，因而Vue.js能尽可能地复用已有实例。假定数据为：

```json
{
  items: {
    {_uid: '88f869d', ...},
    {_uid: '7496c10', ...}
  }
}
```

可以这样给出提示，代码示例如下：

```html
<div v-for="item in items" track-by="_uid">
<!-- content -->
</div>
```

在替换数组items时，如果Vue.js遇到一个包含有`_uid: '88f869d'`的新对象，那么它知道可以复用这个已有对象的作用域与DOM元素。

如果没有唯一的键跟踪，则可以使用`track-by="$index"`，它强制让v-for进入原位更新模式：片段不会被移动，而是简单的以对应索引的新值刷新。这种模式也能处理数据数组中重复的值。

这让数据替换非常高效，但是也会付出一定的代价。因为这时DOM节点不再映射数组元素顺序的改变，不能同步临时状态(比如&lt;input&gt;元素的值)，以及组件的私有状态。因此，如果v-for块包含&lt;input&gt;元素或子组件，则要小心使用`track-by="$index"`。

因为JavaScript的限制，Vue.js不能检测到下面数组的变化：

- 直接用索引设置元素，如`vm.items[0] = {}`。
- 修改数据的长度，如`vm.items.length = 0`。

为了解决前一个问题，Vue.js扩展了观察数组，我们可以使用上面讲过的$set方法：

```javascript
// 与`example1.items[0] = ...`相同，但是能触发视图更新
vm.items.$set(0, { childMsg: 'Changed!' })
```

至于后一个问题，只需用一个空数组替换items即可。

有时我们可能想重复一个包含多个DOM元素的块，在这种情况下，则可以使用&lt;template&gt;标签来包装重复片段。这里的&lt;template&gt;标签只充当一个语义包装器。代码示例如下：

```html
<ul>
  <template>
    <li>{{list.msg}}</li>
    <li class="divider"></li>
  </template>
</ul>
```

我们也可以使用v-for遍历一个对象，每一个重复的实例都将有一个特殊的属性$key，或者给对象的键值提供一个别名。代码示例如下：

```html
<body id="example">
	<ul id="repeat-object">
		<li v-for="value in primitiveValues">{{$key}} : {{value}}</li>
		<li>===</li>
		<li v-for="(key, item) in objectValues">{{key}} : {{item.msg}}</li>
	</ul>
	<script>
		var vm = new Vue({
		
			el: "#repeat-object",
			data: {
				primitiveValues: {
					FirstName: 'DIDI',
					LastName: 'FE',
					Age: 4
				},
				objectValues: {
					one: {
						msg: 'Hello'
					},
					two: {
						msg: 'DIDI FE'
					}
				}
			}
		})
	</script>
</body>
```

**注：ECMAScript5无法检测到新属性添加到一个对象上或者在对象中删除。要处理这种情况，Vue.js增加了三种方法：`$add(key, value)`、`$set(key, value)`和`$delete(key)`，这些方法可以用来添加和删除属性，同时触发视图更新。**

v-for也支持整数。代码示例如下：

```html
<div id="range">
  <div v-for="n in 10">Hi! {{$index}}</div>
</div>
```

将模版重复整数次。

v-for同时还可以和Vue.js提供的内置过滤器或排序数据一起使用。

1. filterBy

语法： `filterBy searchKey[in dataKey...]`

用法：

```html
<input type="text" v-model="searchText">
<ul>
  <li v-for="user in users | filterBy searchText in 'name'">{{user.name}}</li>
</ul>
```

数据如下：

```json
users: [
  {
    name: '快车',
    tag: '1'
  },
  {
    name: '出租车',
    tag: '2'
  },
  {
    name: '顺风车',
    tag: '3'
  },
  {
    name: '专车',
    tag: '4'
  },
]
```

在&lt;ul&gt;标签中展示滴滴业务类型，在输入框中输入“快车”，&lt;ul&gt;中数据或根据所输入的“快车”，在user的name字段中过滤出我们需要的信息，并展示出来。

2. orderBy

语法：`orderBy sortKey[reverseKey]`

用法： 

```html
<body id="example">
	<ul>
		<li v-for="user in users | orderBy field reverse">{{user.name}}</li>
	</ul>
	<script>
		var vm = new Vue({
		
			el: "#example",
			data: {
				field: 'tag',
				reverse: false,
				users: [
					{
						name: '快车',
						tag: 1
					},
					{
						name: '出租车',
						tag: 3
					},
					{
						name: '顺风车',
						tag: 2
					},
					{
						name: '专车',
						tag: 0
					}
				]
			}
		
		})
	</script>
</body>
```

在&lt;ul&gt;标签中根据field变量代表的tag字段正序排列数据。

### v-text

v-text指令可以更新元素的textContent。在内部，{{ Mustache }}插值也被编译为textNode的一个v-text指令。代码示例如下：

```html
<span v-text="msg"></span>
<!-- same as -->
<span>{{ msg }}</span>
```

### v-html

v-html指令可以更新元素的innerHTML。内容按普通HTML插入——数据绑定被忽略。如果想复用模版片段，则应当使用partials。

在内部，{{{ Mustache }}}插值也会被编译为锚节点上的一个v-html指令。

**注： 不建议在网站上直接动态渲染任意HTML片段，很容易导致XSS攻击。**

```html
<div v-html="html"></html>
<!-- same as -->
<div>{{{ html }}}</div>
```