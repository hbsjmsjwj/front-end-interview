kvue.js

```javascript
// 定义KVue构造函数
class KVue {
  constructor(options) {
    // 保存选项
    this.$options = options;

    // 传入data
    this.$data = options.data;

    // 响应化处理
    this.observe(this.$data);

    // new Watcher(this, "foo");
    // this.foo; //读一次，触发依赖收集
    // new Watcher(this, "bar.mua");
    // this.bar.mua;

    new Compile(options.el, this);

    if (options.created) {
        options.created.call(this);
    }
  }

  observe(value) {
    if (!value || typeof value !== "object") {
      return;
    }

    // 遍历value
    Object.keys(value).forEach(key => {
      // 响应式处理
      this.defineReactive(value, key, value[key]);
      // 代理data中的属性到vue根上
      this.proxyData(key);
    });
  }

  defineReactive(obj, key, val) {
    // 递归遍历
    this.observe(val);

    // 定义了一个Dep
    const dep = new Dep(); // 每个dep实例和data中每个key有一对一关系

    // 给obj的每一个key定义拦截
    Object.defineProperty(obj, key, {
      get() {
        // 依赖收集
        Dep.target && dep.addDep(Dep.target);
        return val;
      },
      set(newVal) {
        if (newVal !== val) {
          val = newVal;
          //   console.log(key + "属性更新了");
          dep.notify();
        }
      }
    });
  }

  // 在vue根上定义属性代理data中的数据
  proxyData(key) {
    // this指的KVue实例
    Object.defineProperty(this, key, {
      get() {
        return this.$data[key];
      },
      set(newVal) {
        this.$data[key] = newVal;
      }
    });
  }
}

// 创建Dep：管理所有Watcher
class Dep {
  constructor() {
    // 存储所有依赖
    this.watchers = [];
  }

  addDep(watcher) {
    this.watchers.push(watcher);
  }

  notify() {
    this.watchers.forEach(watcher => watcher.update());
  }
}
// 创建Watcher：保存data中数值和页面中的挂钩关系
class Watcher {
  constructor(vm, key, cb) {
    // 创建实例时立刻将该实例指向Dep.target便于依赖收集
    this.vm = vm;
    this.key = key;
    this.cb = cb;

    //触发依赖收集
    Dep.target = this;
    this.vm[this.key];//触发依赖收集
    Dep.target = null;
  }

  // 更新
  update() {
    // console.log(this.key + "更新了！");
    this.cb.call(this.vm, this.vm[this.key])
  }
}

```
compile.js

```javascript
// 遍历dom结构，解析指令和插值表达式
class Compile {
  // el-带编译模板，vm-KVue实例
  constructor(el, vm) {
    this.$vm = vm;
    this.$el = document.querySelector(el);

    // 把模板中的内容移到片段操作
    this.$fragment = this.node2Fragment(this.$el);
    // 执行编译
    this.compile(this.$fragment);
    // 放回$el中
    this.$el.appendChild(this.$fragment);
  }

  //
  node2Fragment(el) {
    // 创建片段
    const fragment = document.createDocumentFragment();
    //
    let child;
    while ((child = el.firstChild)) {
      fragment.appendChild(child);
    }
    return fragment;
  }

  compile(el) {
    const childNodes = el.childNodes;
    Array.from(childNodes).forEach(node => {
      if (node.nodeType == 1) {
        // 元素
        // console.log('编译元素'+node.nodeName);
        this.compileElement(node);
      } else if (this.isInter(node)) {
        // 只关心{{xxx}}
        // console.log('编译插值文本'+node.textContent);
        this.compileText(node);
      }

      // 递归子节点
      if (node.children && node.childNodes.length > 0) {
        this.compile(node);
      }
    });
  }
  isInter(node) {
    return node.nodeType == 3 && /\{\{(.*)\}\}/.test(node.textContent);
  }

  // 文本替换
  compileText(node) {
    console.log(RegExp.$1);
    console.log(this.$vm[RegExp.$1]);

    // 表达式
    const exp = RegExp.$1;
    this.update(node, exp, "text"); // v-text
  }

  update(node, exp, dir) {
    const updator = this[dir + "Updator"];
    updator && updator(node, this.$vm[exp]); // 首次初始化
    // 创建Watcher实例，依赖收集完成了
    new Watcher(this.$vm, exp, function(value) {
      updator && updator(node, value);
    });
  }

  textUpdator(node, value) {
    node.textContent = value;
  }

  compileElement(node) {
    // 关心属性
    const nodeAttrs = node.attributes;
    Array.from(nodeAttrs).forEach(attr => {
      // 规定：k-xxx="yyy"
      const attrName = attr.name; //k-xxx
      const exp = attr.value; //yyy
      if (attrName.indexOf("k-") == 0) {
        // 指令
        const dir = attrName.substring(2); //xxx
        // 执行
        this[dir] && this[dir](node, exp);
      }
    });
  }

  text(node, exp) {
    this.update(node, exp, 'text')
  }
}

```
index.js

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>


<body>
    <div id="app">
        <p>{{name}}</p>
        <p k-text="name"></p>
        <p>{{age}}</p>
        <input type="text" k-model="name">
        <button @click="changeName">呵呵</button>
        <div k-html="html"></div>
    </div>
    <script src='./compile.js'></script>
    <script src='./kvue.js'></script>

    <script>
        const kaikeba = new KVue({
            el: '#app',
            data: {
                name: "I am test.",
                age: 12,
                html: '<button>这是一个按钮</button>'
            },
            created() {
                console.log('开始啦')
                setTimeout(() => {
                    this.name = '我是测试'
                }, 1500)
            },
            methods: {
                changeName() {
                    this.name = '哈喽，开课吧'
                    this.age = 1
                }
            }
        })
    </script>
</body>
<!-- <script>
        const app = new KVue({
            data: { foo: 'foo', bar: { mua: 'mua' } }
        })   
        app.foo = 'aaa' 
        app.bar.mua = 'aaa' 

        console.log(app.$data);

        
    </script> -->


</html>
```
.....?

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Object.defineProperty</title>
</head>
<body>
    <div id="app">
        <p>你好，<span id="name"></span></p>
    </div>
    <script>
        var obj = {};
        
        // 数据拦截
        Object.defineProperty(obj, 'name', {
            get(){
                console.log('有人想要获取name属性');
                return document.getElementById('name').innerHTML
            },
            set(nick){
                console.log('有人想要修改name属性');
                document.getElementById('name').innerHTML = nick;
            },
        })

        obj.name = 'jerry'
        console.log(obj.name);
        
    </script>
</body>
</html>
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/478633be6442d99c061626ae280eaa3c.png)