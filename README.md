## 一、安装
### 1、直接引入
```
<!-- 开发环境版本，包含了用帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
或者：
<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

## 2、NPM
```
# 最新稳定版
$ npm install vue
# 全局安装 vue-cli
$ npm install --global vue-cli
# 创建一个基于 webpack 模板的新项目
$ vue init webpack my-project
# 安装依赖，走你
$ cd my-project
$ npm run dev
```

## 二、介绍
### 1、指令
- **v-bind:title="message"**

将这个元素节点的 title 特性和 Vue 实例的 message 属性保持一致
```
<div id="app-2">
  <span v-bind:title="message">
    鼠标悬停几秒钟查看此处动态绑定的提示信息！
  </span>
</div>

var app2 = new Vue({
  el: "#app-2",
  data: {
    message: "页面加载于" + new Date().toLocaleString()
  }
})
```
- **v-if="seen"**

我们不仅可以把数据绑定到 DOM 文本或特性，还可以绑定到 DOM 结构
```
<div id="app-3">
  <p v-if="seen">现在你看到我了</p>
</div>

var app3 = new Vue({
  el: "#app-3",
  data: {
    seen: true
  }
})
```
- **v-for="todo in todos"**

可以绑定数组的数据来渲染一个项目列表
```
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>

var app4 = new Vue({
  el: "#app-4",
  data: {
    todos: [{
        text: "学习JavaScipt"
      },
      {
        text: "学习Vue"
      },
      {
        text: "整个牛项目"
      }
    ]
  }
})
```
- **v-on:click="reverseMessage"**

为了让用户和你的应用进行交互，我们可以用 v-on 指令添加一个事件监听器，通过它调用在 Vue 实例中定义的方法
```
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">逆转消息</button>
</div>

var app5 = new Vue({
  el: "#app-5",
  data: {
    message: "Hello Vue.js!"
  },
  methods: {
    reverseMessage: function() {
      this.message = this.message.split("").reverse().join("")
    }
  }
})
```
- **v-model="message"**

轻松实现表单输入和应用状态之间的双向绑定
```
<div id="app-6">
  <p> {{ message }} </p>
  <input v-model="message">
</div>

var app6 = new Vue({
  el: "#app-6",
  data: {
    message: "Hello Vue.js!"
  }
})
```

### 2、组件
```
<div id="app-7">
  <ol>
    <!--
      现在我们为每个 todo-item 提供 todo 对象
      todo 对象是变量，即其内容可以是动态的。
      我们也需要为每个组件提供一个“key”，稍后再
      作详细解释。
    -->
    <todo-item v-for="item in groceryList" v-bind:todo="item" v-bind:key="item.id"></todo-item>
  </ol>
</div>

Vue.component("todo-item", {
  // todo-item 组件现在接受一个
  // "prop"，类似于一个自定义特性。
  // 这个 prop 名为 todo。
  props: ["todo"],
  template: "<li>{{ todo.text }}</li>"
})

var app7 = new Vue({
  el: "#app-7",
  data: {
    groceryList: [{
        id: 0,
        text: '蔬菜'
      },
      {
        id: 1,
        text: '奶酪'
      },
      {
        id: 2,
        text: '随便其它什么人吃的东西'
      }
    ]
  }
})
```

## 三、Vue实例
所有的 Vue 组件都是 Vue 实例，并且接受相同的选项对象 (一些根实例特有的选项除外)。 

### 1、数据与方法
- **Object.freeze()**

Object.freeze() 会阻止修改现有的属性，也意味着响应系统无法再追踪变化。

```
<div id="app">
  <p>{{ foo }}</p>
  <!-- 这里的 `foo` 不会更新！ -->
  <button v-on:click="foo = 'baz'">Change it</button>
</div>

var obj = {
  foo: 'bar'
}

Object.freeze(obj);

new Vue({
  el: '#app',
  data: obj
})
```

- **实例属性与方法**

除了数据属性，Vue 实例还暴露了一些有用的实例属性与方法。它们都有前缀 $，以便与用户定义的属性区分开来。例如：
```
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch 是一个实例方法
vm.$watch('a', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
})
```

### 2、实例生命周期钩子
每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户在不同阶段添加自己的代码的机会。
![image](https://cn.vuejs.org/images/lifecycle.png)


## 四、模板语法
### 1、插值 

- **v-once**

执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上的其它数据绑定：
```
<div id="app-9">
  <span v-once>这个值将不会改变：{{msg}}</span>
  <button v-on:click="msg = 'baz'">Change it</button>
</div>

var app9 = new Vue({
  el: "#app-9",
  data: {
    msg: "hello vue!"
  }
})
```

- **v-html**

双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 v-html 指令：
```
<div id="app-10">
  <p>Using mustaches: {{ rawHtml }}</p>
  <p>Using v-html directive: <span v-html="rawHtml"></span></p>
</div>

var app10 = new Vue({
  el: "#app-10",
  data: {
    rawHtml: '<span style="color:red">This should be red.</span>'
  }
})
```

- **特性**

Mustache 语法不能作用在 HTML 特性上，遇到这种情况应该使用 v-bind 指令：
```
<div v-bind:id="dynamicId"></div>

<button v-bind:disabled="isButtonDisabled">Button</button>
```

- **使用 JavaScript 表达式**
```
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```
### 2、指令

- **参数**

一些指令能够接收一个“参数”，在指令名称之后以冒号表示。
```
<a v-bind:href="url">...</a>

<a v-on:click="doSomething">...</a>
```

- **修饰符**

修饰符 (Modifiers) 是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，.prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()：
```
<form v-on:submit.prevent="onSubmit">...</form>
```

### 3、缩写
- **v-bind 缩写**
```
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>
```

- **v-on 缩写**
```
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>
```

## 五、计算属性和侦听器
### 1、计算属性

对于任何复杂逻辑，你都应当使用计算属性。

- **基础例子**
```
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>

var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
```

- **计算属性缓存 vs 方法**

计算属性是基于它们的依赖进行缓存的，相比之下，每当触发重新渲染时，调用方法将总会再次执行函数。如果你不希望有缓存，请用方法来替代。

- **计算属性 vs 侦听属性**

Vue 提供了一种更通用的方式来观察和响应 Vue 实例上的数据变动：侦听属性。当你有一些数据需要随着其它数据变动而变动时，你很容易滥用 watch。然而，通常更好的做法是使用计算属性而不是命令式的 watch 回调。

- **计算属性的 setter**

计算属性默认只有 getter ，不过在需要时你也可以提供一个 setter ：
```
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```

### 2、侦听器

当需要在数据变化时执行异步或开销较大的操作时，watch 是最有用的。
```
<div id="watch-example">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>

<!-- 因为 AJAX 库和通用工具的生态已经相当丰富，Vue 核心代码没有重复 -->
<!-- 提供这些功能以保持精简。这也可以让你自由选择自己更熟悉的工具。 -->
<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.getAnswer()
    }
  },
  methods: {
    // `_.debounce` 是一个通过 Lodash 限制操作频率的函数。
    // 在这个例子中，我们希望限制访问 yesno.wtf/api 的频率
    // AJAX 请求直到用户输入完毕才会发出。想要了解更多关于
    // `_.debounce` 函数 (及其近亲 `_.throttle`) 的知识，
    // 请参考：https://lodash.com/docs#debounce
    getAnswer: _.debounce(
      function () {
        if (this.question.indexOf('?') === -1) {
          this.answer = 'Questions usually contain a question mark. ;-)'
          return
        }
        this.answer = 'Thinking...'
        var vm = this
        axios.get('https://yesno.wtf/api')
          .then(function (response) {
            vm.answer = _.capitalize(response.data.answer)
          })
          .catch(function (error) {
            vm.answer = 'Error! Could not reach the API. ' + error
          })
      },
      // 这是我们为判定用户停止输入等待的毫秒数
      500
    )
  }
})
</script>
```

## 六、Class 与 Style 绑定
主要用v-bind去处理。
### 1、绑定 HTML Class
- **对象语法**
```
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>

和如下 data：
data: {
  isActive: true,
  hasError: false
}

结果渲染为：
<div class="static active"></div>
```

绑定的数据对象不必内联定义在模板里：
```
<div v-bind:class="classObject"></div>
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

也可以在这里绑定一个返回对象的计算属性：
```
<div v-bind:class="classObject"></div>

data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

- **数组语法**
```
<div v-bind:class="[activeClass, errorClass]"></div>

data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}

渲染为：
<div class="active text-danger"></div>
```

如果你也想根据条件切换列表中的 class，可以用三元表达式：
```
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```

当有多个条件 class 时这样写有些繁琐。所以在数组语法中也可以使用对象语法：
```
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```

- **用在组件上**
```
如果你声明了这个组件：
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})

然后在使用它的时候添加一些 class：
<my-component class="baz boo"></my-component>

HTML 将被渲染为:
<p class="foo bar baz boo">Hi</p>
```

### 2、绑定内联样式
- **对象语法**
```
<div v-bind:style="styleObject"></div>

data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

- **数组语法**
```
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

- **自动添加前缀**
当 v-bind:style 使用需要添加浏览器引擎前缀的 CSS 属性时，如 transform，Vue.js 会自动侦测并添加相应的前缀。

- **多重值**
```
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```
这样写只会渲染数组中最后一个被浏览器支持的值。在本例中，如果浏览器支持不带浏览器前缀的 flexbox，那么就只会渲染 display: flex。

## 七、条件渲染
### 1、v-if

- **在 template 元素上使用 v-if 条件渲染分组**
```
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

- **v-else**
```
<div v-if="Math.random() > 0.5">
  Now you see me
</div>

<div v-else>
  Now you don't
</div>
```

- **v-else-if**
```
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else>
  Not A/B
</div>
```

- **用 key 管理可复用的元素**
Vue 为你提供了一种方式来表达“这两个元素是完全独立的，不要复用它们”。只需添加一个具有唯一值的 key 属性即可：
```
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
```
现在，每次切换时，输入框都将被重新渲染。

注意，<label> 元素仍然会被高效地复用，因为它们没有添加 key 属性。

- **v-show**
v-show 的元素始终会被渲染并保留在 DOM 中。v-show 只是简单地切换元素的 CSS 属性 display。但 v-show 不支持 <template> 元素，也不支持 v-else。

- **v-if vs v-show**
v-if 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，v-show 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。

### 2、v-if 与 v-for 一起使用
当 v-if 与 v-for 一起使用时，v-for 具有比 v-if 更高的优先级。

## 八、列表渲染
### 1、用 v-for 把一个数组对应为一组元素
```
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>

var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Parent',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```
也可以用 of 替代 in 作为分隔符：
```
<div v-for="item of items"></div>
```

### 2、一个对象的 v-for
```
<ul id="v-for-object" class="demo">
  <li v-for="(key, value, index) in object">
    {{ index }}. {{ key }}: {{ value }}
  </li>
</ul>

new Vue({
  el: '#v-for-object',
  data: {
    object: {
      firstName: 'John',
      lastName: 'Doe',
      age: 30
    }
  }
})
```

### 3、key
为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 key 属性。理想的 key 值是每项都有的且唯一的 id。

建议尽可能在使用 v-for 时提供 key，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。
```
<div v-for="item in items" :key="item.id">
  <!-- 内容 -->
</div>
```

### 4、数组更新检测
- **变异方法**

push()，pop()，shift()，unshift()，splice()，sort()，reverse()

- **替换数组**

filter(), concat() 和 slice()
```
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```

- **注意事项**

由于 JavaScript 的限制，Vue 不能检测以下变动的数组：
1. 当你利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue
1. 当你修改数组的长度时，例如：vm.items.length = newLength

```
var vm = new Vue({
  data: {
    items: ['a', 'b', 'c']
  }
})
vm.items[1] = 'x' // 不是响应性的
vm.items.length = 2 // 不是响应性的

// 下面这3种方式都可以
Vue.set(vm.items, indexOfItem, newValue)
vm.items.splice(indexOfItem, 1, newValue)
vm.$set(vm.items, indexOfItem, newValue)

//为了解决第二类问题，你可以使用 splice：
vm.items.splice(newLength)
```

## 5、对象更改检测注意事项
Vue 不能检测对象属性的添加或删除：
```
var vm = new Vue({
  data: {
    userProfile: {
      name: 'Anika'
    }
  }
})
// `vm.userProfile.name` 现在是响应式的

vm.userProfile.age = 27
// `vm.userProfile.age` 不是响应式的

// 下面这2种添加方式都可以
Vue.set(vm.userProfile, 'age', 27)
vm.$set(vm.userProfile, 'age', 27)
```

有时你可能需要为已有对象赋予多个新属性，比如使用 Object.assign() 或 _.extend()。
```
//不要像这样：
Object.assign(vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})

//你应该这样做：
vm.userProfile = Object.assign({}, vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```

### 6、显示过滤/排序结果
```
<li v-for="n in evenNumbers">{{ n }}</li>

data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```
在计算属性不适用的情况下 (例如，在嵌套 v-for 循环中) 你可以使用一个 method 方法：
```
<li v-for="n in even(numbers)">{{ n }}</li>

data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
methods: {
  even: function (numbers) {
    return numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```

### 7、一段取值范围的 v-for
```
<div>
  <span v-for="n in 10">{{ n }} </span>
</div>
```
结果： 1 2 3 4 5 6 7 8 9 10

### 8、v-for on a template
```
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider"></li>
  </template>
</ul>
```

### 9、v-for with v-if
当它们处于同一节点，v-for 的优先级比 v-if 更高:
```
<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo }}
</li>
```
如果你的目的是有条件地跳过循环的执行，那么可以将 v-if 置于外层元素 (或 template)上：
```
<ul v-if="todos.length">
  <li v-for="todo in todos">
    {{ todo }}
  </li>
</ul>
<p v-else>No todos left!</p>
```

### 10、一个组件的 v-for
 todo list 的完整例子：
 ```
 <div id="todo-list-example">
  <input
    v-model="newTodoText"
    v-on:keyup.enter="addNewTodo"
    placeholder="Add a todo"
  >
  <ul>
    <todo-item
      v-for="(todo, index) in todos"
      v-bind:key="todo.id"
      v-bind:title="todo.title"
      v-on:remove="todos.splice(index, 1)"
    ></todo-item>
  </ul>
</div>

Vue.component('todo-item', {
  template: '\
    <li>\
      {{ title }}\
      <button v-on:click="$emit(\'remove\')">X</button>\
    </li>\
  ',
  props: ['title']
})

new Vue({
  el: '#todo-list-example',
  data: {
    newTodoText: '',
    todos: [
      {
        id: 1,
        title: 'Do the dishes',
      },
      {
        id: 2,
        title: 'Take out the trash',
      },
      {
        id: 3,
        title: 'Mow the lawn'
      }
    ],
    nextTodoId: 4
  },
  methods: {
    addNewTodo: function () {
      this.todos.push({
        id: this.nextTodoId++,
        title: this.newTodoText
      })
      this.newTodoText = ''
    }
  }
})
 ```

## 九、事件处理
### 1、监听事件
可以用 v-on 指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码。
```
<div id="example-1">
  <button v-on:click="counter += 1">Add 1</button>
  <p>The button above has been clicked {{ counter }} times.</p>
</div>

var example1 = new Vue({
  el: '#example-1',
  data: {
    counter: 0
  }
})
```

### 2、事件处理方法
然而许多事件处理逻辑会更为复杂，所以直接把 JavaScript 代码写在 v-on 指令中是不可行的。因此 v-on 还可以接收一个需要调用的方法名称。
```
<div id="example-2">
  <!-- `greet` 是在下面定义的方法名 -->
  <button v-on:click="greet">Greet</button>
</div>

var example2 = new Vue({
  el: '#example-2',
  data: {
    name: 'Vue.js'
  },
  // 在 `methods` 对象中定义方法
  methods: {
    greet: function (event) {
      // `this` 在方法里指向当前 Vue 实例
      alert('Hello ' + this.name + '!')
      // `event` 是原生 DOM 事件
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
})

// 也可以用 JavaScript 直接调用方法
example2.greet() // => 'Hello Vue.js!'
```

### 3、内联处理器中的方法
除了直接绑定到一个方法，也可以在内联 JavaScript 语句中调用方法：
```
<div id="example-3">
  <button v-on:click="say('hi', $event)">Say hi</button>
  <button v-on:click="say('what', $event)">Say what</button>
</div>

new Vue({
  el: '#example-3',
  methods: {
    say: function (message, event) {
      alert(message + event)
    }
  }
})
```

### 4、事件修饰符
- .stop
- .prevent
- .capture
- .self
- .once

```
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即元素自身触发的事件先在此处处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>

<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>

<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
```
使用修饰符时，**顺序很重要**；相应的代码会以同样的顺序产生。因此，用 v-on:click.prevent.self 会阻止**所有的点击**，而 v-on:click.self.prevent 只会阻止对元素自身的点击。

不要把 .passive 和 .prevent 一起使用，因为 .prevent 将会被忽略，同时浏览器可能会向你展示一个警告。请记住，.passive 会告诉浏览器你**不想阻止事件的默认行为**。


### 5、按键修饰符
- .enter
- .tab
- .delete (捕获“删除”和“退格”键)
- .esc
- .space
- .up
- .down
- .left
- .right

```
<!-- 只有在 `keyCode` 是 13 时调用 `vm.submit()` -->
<input v-on:keyup.13="submit">

<!-- 同上 -->
<input v-on:keyup.enter="submit">

<!-- 缩写语法 -->
<input @keyup.enter="submit">
```

可以通过全局 config.keyCodes 对象自定义按键修饰符别名：
```
// 可以使用 `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112
```

- **系统修饰键**
- .ctrl
- .alt
- .shift
- .meta

请注意修饰键与常规按键不同，在和 keyup 事件一起用时，事件触发时修饰键必须处于按下状态。换句话说，只有在按住 ctrl 的情况下释放其它按键，才能触发 keyup.ctrl。而单单释放 ctrl 也不会触发事件。如果你想要这样的行为，请为 ctrl 换用 keyCode：keyup.17。


- **.exact 修饰符**
.exact 修饰符允许你控制由精确的系统修饰符组合触发的事件。
```
<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button @click.ctrl="onClick">A</button>

<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button @click.exact="onClick">A</button>
```

- **鼠标按钮修饰符**
- .left
- .right
- .middle

## 十、表单输入绑定
v-model 会忽略所有表单元素的 value、checked、selected 特性的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 data 选项中声明初始值。

对于需要使用输入法 (如中文、日文、韩文等) 的语言，你会发现 v-model 不会在输入法组合文字过程中得到更新。如果你也想处理这个过程，请使用 input 事件。
 ## 1、修饰符
  - **.lazy**
  ```
  <!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg" >
  ```
  
 - **.number**
 ```
 <input v-model.number="age" type="number">
 ```
这通常很有用，因为即使在 type="number" 时，HTML 输入元素的值也总会返回字符串。

- **.trim**
如果要自动过滤用户输入的首尾空白字符，可以给 v-model 添加 trim 修饰符：
```
<input v-model.trim="msg">
```

## 十一、组件
### 1、使用组件
- **全局注册**
注意确保在初始化根实例之前注册组件：
```
<div id="example">
  <my-component></my-component>
</div>

// 注册
Vue.component('my-component', {
  template: '<div>A custom component!</div>'
})

// 创建根实例
new Vue({
  el: '#example'
})
```

- **局部注册**
```
var Child = {
  template: '<div>A custom component!</div>'
}

new Vue({
  // ...
  components: {
    // <my-component> 将只在父组件模板中可用
    'my-component': Child
  }
})
```

- **data 必须是函数**
```
Vue.component('simple-counter', {
  template: '<button v-on:click="counter += 1">{{ counter }}</button>',
  data: function () {
    return {
      counter: 0
    }
  }
})

new Vue({
  el: '#example-2'
})
```

- **组件组合**
父组件通过 **prop** 给子组件下发数据，子组件通过**事件**给父组件发送消息。看看它们是怎么工作的。

### 2、Prop
- **用 Prop 传递数据**
```
Vue.component('child', {
  // 声明 props
  props: ['message'],
  // 就像 data 一样，prop 也可以在模板中使用
  // 同样也可以在 vm 实例中通过 this.message 来使用
  template: '<span>{{ message }}</span>'
})

<child message="hello!"></child>
```

- **动态 Prop**
使用v-bind：
```
<div id="prop-example-2">
  <input v-model="parentMsg">
  <br>
  <child v-bind:my-message="parentMsg"></child>
</div>

new Vue({
  el: '#prop-example-2',
  data: {
    parentMsg: 'Message from parent'
  }
})
```

- **字面量语法 vs 动态语法**
使用v-bind：
```
<!-- 传递了一个字符串 "1" -->
<comp some-prop="1"></comp>

<!-- 传递真正的数值 -->
<comp v-bind:some-prop="1"></comp>
```

- **单向数据流**
在 JavaScript 中对象和数组是引用类型，指向同一个内存空间，如果 prop 是一个对象或数组，在子组件内部改变它会影响父组件的状态。

- **Prop 验证**
要指定验证规则，需要用对象的形式来定义 prop，而不能用字符串数组：

```
Vue.component('example', {
  props: {
    // 基础类型检测 (`null` 指允许任何类型)
    propA: Number,
    // 可能是多种类型
    propB: [String, Number],
    // 必传且是字符串
    propC: {
      type: String,
      required: true
    },
    // 数值且有默认值
    propD: {
      type: Number,
      default: 100
    },
    // 数组/对象的默认值应当由一个工厂函数返回
    propE: {
      type: Object,
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        return value > 10
      }
    }
  }
})
```

type 可以是下面原生构造器：

- String
- Number
- Boolean
- Function
- Object
- Array
- Symbol

type 也可以是一个自定义构造器函数，使用 instanceof 检测。


### 3、非 Prop 特性
所谓非 prop 特性，就是指它可以直接传入组件，而不需要定义相应的 prop。
```
<bs-date-input data-3d-date-picker="true"></bs-date-input>
```

### 4、自定义事件
子组件跟父组件通信。

- **使用 v-on 绑定自定义事件**
每个 Vue 实例都实现了事件接口，即：
1. 使用 $on(eventName) 监听事件
1. 使用 $emit(eventName, optionalPayload) 触发事件

父组件可以在使用子组件的地方直接用 v-on 来监听子组件触发的事件。但不能用 $on 监听子组件释放的事件，必须在模板里直接用 v-on 绑定。
```
<div id="counter-event-example">
  <p>{{ total }}</p>
  <button-counter v-on:increment="incrementTotal"></button-counter>
  <button-counter v-on:increment="incrementTotal"></button-counter>
</div>

Vue.component('button-counter', {
  template: '<button v-on:click="incrementCounter">{{ counter }}</button>',
  data: function () {
    return {
      counter: 0
    }
  },
  methods: {
    incrementCounter: function () {
      this.counter += 1
      this.$emit('increment')
    }
  },
})

new Vue({
  el: '#counter-event-example',
  data: {
    total: 0
  },
  methods: {
    incrementTotal: function () {
      this.total += 1
    }
  }
})
```
还可以通过载荷（payload）传给父组件额外的参数，最好是对象的形式。

- **给组件绑定原生事件**
```
<my-component v-on:click.native="doTheThing"></my-component>
```

- **.sync 修饰符**
它只是作为一个编译时的语法糖存在。它会被扩展为一个自动更新父组件属性的 v-on 监听器。
```
<comp :foo.sync="bar"></comp>

当子组件需要更新 foo 的值时，它需要显式地触发一个更新事件：
this.$emit('update:foo', newValue)

当使用一个对象一次性设置多个属性的时候，这个 .sync 修饰符也可以和 v-bind 一起使用：
<comp v-bind.sync="{ foo: 1, bar: 2 }"></comp>
```

- **使用自定义事件的表单输入组件**
```
<currency-input v-model="price"></currency-input>

Vue.component('currency-input', {
  template: '\
    <span>\
      $\
      <input\
        ref="input"\
        v-bind:value="value"\
        v-on:input="updateValue($event.target.value)"\
      >\
    </span>\
  ',
  props: ['value'],
  methods: {
    // 不是直接更新值，而是使用此方法来对输入值进行格式化和位数限制
    updateValue: function (value) {
      var formattedValue = value
        // 删除两侧的空格符
        .trim()
        // 保留 2 位小数
        .slice(
          0,
          value.indexOf('.') === -1
            ? value.length
            : value.indexOf('.') + 3
        )
      // 如果值尚不合规，则手动覆盖为合规的值
      if (formattedValue !== value) {
        this.$refs.input.value = formattedValue
      }
      // 通过 input 事件带出数值
      this.$emit('input', Number(formattedValue))
    }
  }
})
```

- **自定义组件的 v-model**
默认情况下，一个组件的 v-model 会使用 value prop 和 input 事件。但是诸如单选框、复选框之类的输入类型可能把 value 用作了别的目的。model 选项可以避免这样的冲突：
```
Vue.component('my-checkbox', {
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean,
    // 这样就允许拿 `value` 这个 prop 做其它事了
    value: String
  },
  // ...
})

<my-checkbox v-model="foo" value="some value"></my-checkbox>
```

- **非父子组件的通信**
```
var bus = new Vue()

// 触发组件 A 中的事件
bus.$emit('id-selected', 1)

// 在组件 B 创建的钩子中监听事件
bus.$on('id-selected', function (id) {
  // ...
})
```

### 5、使用插槽分发内容
- **编译作用域**
父组件模板的内容在父组件作用域内编译；子组件模板的内容在子组件作用域内编译。

- **具名插槽**
```
假定我们有一个 app-layout 组件，它的模板为：
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>

父组件模板：
<app-layout>
  <h1 slot="header">这里可能是一个页面标题</h1>

  <p>主要内容的一个段落。</p>
  <p>另一个主要段落。</p>

  <p slot="footer">这里有一些联系信息</p>
</app-layout>

渲染结果为：
<div class="container">
  <header>
    <h1>这里可能是一个页面标题</h1>
  </header>
  <main>
    <p>主要内容的一个段落。</p>
    <p>另一个主要段落。</p>
  </main>
  <footer>
    <p>这里有一些联系信息</p>
  </footer>
</div>
```

- **作用域插槽**
作用域插槽是一种特殊类型的插槽，用作一个 (能被传递数据的) 可重用模板，来代替已经渲染好的元素。
```
<div class="child">
  <slot text="hello from child"></slot>
</div>

<div class="parent">
  <child>
    <template slot-scope="props">
      <span>hello from parent</span>
      <span>{{ props.text }}</span>
    </template>
  </child>
</div>

如果我们渲染上述模板，得到的输出会是：
<div class="parent">
  <div class="child">
    <span>hello from parent</span>
    <span>hello from child</span>
  </div>
</div>
```

### 6、动态组件
通过使用保留的 component 元素，并对其 is 特性进行动态绑定，你可以在同一个挂载点动态切换多个组件：
```
var vm = new Vue({
  el: '#example',
  data: {
    currentView: 'home'
  },
  components: {
    home: { /* ... */ },
    posts: { /* ... */ },
    archive: { /* ... */ }
  }
})
  
<component v-bind:is="currentView">
  <!-- 组件在 vm.currentview 变化时改变！ -->
</component>
```

- **keep-alive**
如果把切换出去的组件保留在内存中，可以保留它的状态或避免重新渲染。为此可以添加一个 keep-alive 指令参数：
```
<keep-alive>
  <component :is="currentView">
    <!-- 非活动组件将被缓存！ -->
  </component>
</keep-alive>
```

###  7、杂项
- **编写可复用组件**

Vue 组件的 API 来自三部分——prop、事件和插槽：
1. **Prop** 允许外部环境传递数据给组件；
1. **事件** 允许从组件内触发外部环境的副作用；
1. **插槽** 允许外部环境将额外的内容组合在组件中。
























