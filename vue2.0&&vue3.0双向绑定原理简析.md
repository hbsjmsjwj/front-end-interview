```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Jay</title>
</head>
<body>
    <!--
        实现mvvm的双向绑定，是采用数据劫持结合发布者-订阅者模式的方式，通过Object.defineProperty()来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。就必须要实现以下几点：
            1、实现一个数据监听器Observer，能够对数据对象的所有属性进行监听，如有变动可拿到最新值并通知订阅者
            2、实现一个指令解析器Compile，对每个元素节点的指令进行扫描和解析，根据指令模板替换数据，以及绑定相应的更新函数
            3、实现一个Watcher，作为连接Observer和Compile的桥梁，能够订阅并收到每个属性变动的通知，执行指令绑定的相应回调函数，从而更新视图
            4、mvvm入口函数，整合以上三者
    -->
    <div id="app">
        <div>
            <span>8888</span>
        </div>
        <input type="text" l-model="msg" >
        <p l-html="msg"></p>
        <input type="text" l-model="info" >
        <p l-html="info"></p>
        <button l-on:click="clickMe">点我</button>
        <p>{{msg}}</p>

    </div>

    <script>
        function Vue(option){
            this.$el = document.querySelector(option.el);   //获取挂载节点
            this.$data = option.data;
            this.$methods = option.methods;
            this.deps = {};     //所有订阅者集合 目标格式（一对多的关系）：{msg: [订阅者1, 订阅者2, 订阅者3], info: [订阅者1, 订阅者2]}
            this.observer(this.$data);  //调用观察者
            this.compile(this.$el);     //调用指令解析器

        }


Vue.prototype.observer = function(data){
    for(var key in data){
        (function(that){
            let val = data[key];    //每一个数据的属性值
            that.deps[key] = [];    //初始化所有订阅者对象{msg: [订阅者], info: []}
            var watchers = that.deps[key];
            console.log('data',data)
            console.log('watchers==',watchers)

            /**
             * Object.defineProperty(obj, prop, desc)
             * obj 需要定义属性的当前对象
             * prop 当前需要定义的属性名
             * desc 属性描述符
            */
            Object.defineProperty(data, key, {  //数据劫持
                get: function(){
                    return val;
                },
                set: function(newVal){  //设置值(说明有数据更新)
                    if(val !== newVal){
                        val = newVal;
                    }
                    // 通知订阅者


                    watchers.forEach(watcher=>{
                        console.log('watcher',watcher)
                        watcher.update()
                    })
                }
            })
        })(this)
    }
}
        //解释器
Vue.prototype.compile = function (el) {
    let nodes = el.children; //获取挂载节点的子节点
    console.log('2222222',this.deps)
    console.log('nodes挂载的节点',nodes)
    for (var i = 0; i < nodes.length; i++) {
        var node = nodes[i];
        console.log('node',node)
        if (node.children.length) {
            this.compile(node) //递归获取子节点
        }
        if (node.hasAttribute('l-model')) { //当子节点存在l-model指令
            console.log('l-model',node)
            let attrVal = node.getAttribute('l-model'); //获取属性值
            console.log('attrVal',attrVal)
            node.addEventListener('input', (() => {//添加事件监听,监听input事件

                console.log('deps',this.deps)
                    this.deps[attrVal].push(new Watcher(node, "value", this, attrVal)); //添加一个订阅者
                    let thisNode = node;
                    return () => {
                        this.$data[attrVal] = thisNode.value //更新数据层的数据
                    }
                })()
            )
        }
        if (node.hasAttribute('l-html')) {
            let attrVal = node.getAttribute('l-html'); //获取属性值
            this.deps[attrVal].push(new Watcher(node, "innerHTML", this, attrVal)); //添加一个订阅者
        }
        if (node.innerHTML.match(/{{([^\{|\}]+)}}/)) {
            let attrVal = node.innerHTML.replace(/[{{|}}]/g, '');   //获取插值表达式内容
            this.deps[attrVal].push(new Watcher(node, "innerHTML", this, attrVal)); //添加一个订阅者
        }
        if (node.hasAttribute('l-on:click')) {
            let attrVal = node.getAttribute('l-on:click'); //获取事件触发的方法名
            node.addEventListener('click', this.$methods[attrVal].bind(this.$data)); //将this指向this.$data
        }
    }
}
function Watcher(el, attr, vm, attrVal) {
    this.el = el;//input or p
    this.attr = attr;// value or innerHTML
    this.vm = vm;//vue
    this.val = attrVal;//msg or info
    this.update(); //更新视图
}
Watcher.prototype.update = function () {
    this.el[this.attr] = this.vm.$data[this.val]
}



// \\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\
        var vm = new Vue({
            el: "#app",
            data: {
                msg: "恭喜发财",
                info: "好好学习, 天天向上"
            },
            methods: {
                clickMe(){
                    this.msg = "我爱敲代码";
                }
            }
        })
    </script>
</body>
</html>

```

3.0

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="./vue.js"></script>
</head>
<body>
    <!--
        实现mvvm的双向绑定，是采用数据劫持结合发布者-订阅者模式的方式，通过new Proxy()来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。就必须要实现以下几点：
            1、实现一个数据监听器Observer，能够对数据对象的所有属性进行监听，如有变动可拿到最新值并通知订阅者
            2、实现一个指令解析器Compile，对每个元素节点的指令进行扫描和解析，根据指令模板替换数据，以及绑定相应的更新函数
            3、实现一个Watcher，作为连接Observer和Compile的桥梁，能够订阅并收到每个属性变动的通知，执行指令绑定的相应回调函数，从而更新视图
            4、mvvm入口函数，整合以上三者
    -->
    <div id="app">
        <input type="text" l-model="msg" >
        <p l-html="msg"></p>
        <input type="text" l-model="info" >
        <p l-html="info"></p>
        <button l-on:click="clickMe">点我</button>
        <p>{{msg}}</p>
    </div>

    <script>

        function Vue(option){
            this.$el = document.querySelector(option.el);   //获取挂载节点
            this.$data = option.data;
            this.$methods = option.methods;
            this.deps = {};     //所有订阅者集合 目标格式（一对多的关系）：{msg: [订阅者1, 订阅者2, 订阅者3], info: [订阅者1, 订阅者2]}
            this.observer(this.$data);  //调用观察者
            this.compile(this.$el);     //调用指令解析器
        }

        Vue.prototype.observer = function (data) {
            const that = this;
            for(var key in data){
                that.deps[key] = [];    //初始化所有订阅者对象{msg: [订阅者], info: []}
            }
            let handler = {
                get(target, property) {
                    return target[property];
                },
                set(target, key, value) {
                    let res = Reflect.set(target, key, value);
                    var watchers = that.deps[key];
                    watchers.map(item => {
                        item.update();
                    });
                    return res;
                }
            }
            this.$data = new Proxy(data, handler);
        }

        Vue.prototype.compile = function (el) {
            let nodes = el.children; //获取挂载节点的子节点
            for (var i = 0; i < nodes.length; i++) {
                var node = nodes[i];
                if (node.children.length) {
                    this.compile(node) //递归获取子节点
                }
                if (node.hasAttribute('l-model')) { //当子节点存在l-model指令
                    let attrVal = node.getAttribute('l-model'); //获取属性值
                    node.addEventListener('input', (() => {
                        this.deps[attrVal].push(new Watcher(node, "value", this, attrVal)); //添加一个订阅者
                        let thisNode = node;
                        return () => {
                            this.$data[attrVal] = thisNode.value //更新数据层的数据
                        }
                    })())
                }
                if (node.hasAttribute('l-html')) {
                    let attrVal = node.getAttribute('l-html'); //获取属性值
                    this.deps[attrVal].push(new Watcher(node, "innerHTML", this, attrVal)); //添加一个订阅者
                }
                if (node.innerHTML.match(/{{([^\{|\}]+)}}/)) {
                    let attrVal = node.innerHTML.replace(/[{{|}}]/g, '');   //获取插值表达式内容
                    this.deps[attrVal].push(new Watcher(node, "innerHTML", this, attrVal)); //添加一个订阅者
                }
                if (node.hasAttribute('l-on:click')) {
                    let attrVal = node.getAttribute('l-on:click'); //获取事件触发的方法名
                    node.addEventListener('click', this.$methods[attrVal].bind(this.$data)); //将this指向this.$data
                }
            }
        }

        function Watcher(el, attr, vm, attrVal) {
            this.el = el;
            this.attr = attr;
            this.vm = vm;
            this.val = attrVal;
            this.update(); //更新视图
        }

        Watcher.prototype.update = function () {
            this.el[this.attr] = this.vm.$data[this.val]
        }


        var vm = new Vue({
            el: "#app",
            data: {
                msg: "恭喜发财",
                info: "好好学习, 天天向上"
            },
            methods: {
                clickMe(){
                    this.msg = "我爱敲代码";
                }
            }
        })
    </script>
</body>
</html>

```
