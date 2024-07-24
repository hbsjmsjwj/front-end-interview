## 何理解 JS 中的this关键字？
JS 初学者总是对 this 关键字感到困惑，因为与其他现代编程语言相比，JS 中的这this关键字有点棘手。“this” 一般是表示当前所在的对象，但是事情并没有像它应该的那样发生。JS中的this关键字由函数的调用者决定，谁调用就this就指向哪个。如果找不到调用者，this将指向windows对象。

- 来几个粟子
**第一个例子很简单。调用 test对象中的 func()，因此func() 中的'this'指向的是 test 对象，所以打印的 prop 是 test 中的 prop，即 42。**

```javascript
var test = {
 prop: 42,
func: function(){
 return this.prop;
},
 };
console.log (test.func()); // 42
```
**
如果我们直接调用getFullname函数，第二个例子将打印出'David Jones'，因为此时 this 找不到调用者，所以默认就为 window 对象，打印的 fullname 即是全局的。
**

```javascript
var fullname = ‘David Jones’
var obj ={
fullname: ‘Colin Brown’,
prop:{
  fullname:’Aurelio Deftch’,
  getFullname: function(){
    return this.fullname;
  }
 }
}
var test = obj.prop.getFullname
console.log(test()) // David Jones
obj.prop.getFullname() // ‘Aurelio Deftch’
```
## 2. 由于 this 关键字很混乱，如何解决这个问题
有很多方法可以解决这个问题; 但是，无论你选择哪种解决方案，最重要的是要知道你决定让 this 指向哪个对象。
一旦你弄清楚了this指向的对象，你就可以直接将它改成对象名。否则，使用bind，call，apply函数也可以解决问题。
## 3.什么是闭包
当我第一次解释闭包时，我常说函数中的函数;但是，它没有正确地描述闭包的确切含义。

闭包是在另一个作用域内创建一个封闭的词法范围。它通常会自动返回来生成这个词法环境。这个环境由创建闭包时在作用域内的任何局部变量组成。它就像一个微型工厂，用这些原料生产出具有特定功能的产品。

```javascript
function add(n){
  var num = n
  return function addTo(x){
    return x + num
  }
}
addTwo = add(2)
addTwo(5)
```
闭包的另一个应用是创建私有变量和方法。JavaScript不像Java那样可以很好地支持oop。在JS中没有明确的方法来创建私有方法，但是闭包可以私有方法。

## 4.解释一下变量的提升
变量的提升是JavaScript的默认行为，这意味着将所有变量声明移动到当前作用域的顶部，并且可以在声明之前使用变量。初始化不会被提升，提升仅作用于变量的声明。

```javascript
var x = 1
console.log(x + '——' + y) // 1——undefined
var y = 2
```
## 5. JavaScript如何处理同步和异步情况
尽管JavaScript是一种只有一个调用堆栈的单线程编程语言，但它也可以使用一个称为**事件循环(event loop)**的机制来处理一些异步函数。从基本级别了解JavaScript如何工作是理解JS如何处理异步的关键部分。
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/d7949750f9d314a1a621425e95814605.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/457b4fdb170aeadaffcea5e159e359ab.png)
如图所示，调用堆栈是定位函数的位置。一旦函数被调用，函数将被推入堆栈。然而，异步函数不会立即被推入调用堆栈，而是会被推入任务队列(Task Queue)，并在调用堆栈为空后执行。将事件从任务队列传输到调用堆栈称为事件循环。
## 6. 如何理解事件委托
在DOM树上绑定事件监听器并使用JS事件处理程序是处理客户端事件响应的典型方法。从理论上讲，我们可以将监听器附加到HTML中的任何DOM元素，但由于事件委派，这样做是浪费而且没必要的。
**什么是事件委托？**
这是一种让父元素上的事件监听器也影响子元素的技巧。通常，事件传播（捕获和冒泡）允许我们实现事件委托。冒泡意味着当触发子元素（目标）时，也可以逐层触发该子元素的父元素，直到它碰到DOM绑定的原始监听器（当前目标）。捕获属性将事件阶段转换为捕获阶段，让事件下移到元素; 因此，触发方向与冒泡阶段相反。捕获的默认值为false。
## 7. 如何理解高阶函数
JavaScript中的一切都是对象，包括函数。我们可以将变量作为参数传递给函数，函数也是如此。我们调用接受和或返回另一个函数称为高阶函数的函数。
## 8. 如何区分声明函数和表达式函数 

```javascript
// 声明函数
function hello() {
  return "HELLO"
}
// 表达式函数
var h1 = function hello() {
  return "HELLO"
}
```
两个函数将在不同的时期定义。在解析期间定义声明，在运行时定义表达式;因此，如果我们控制台打印 h1，它将显示HELLO。

## 9.解释原型继承是如何工作的
JavaScript不是一种面向对象的友好编程语言，但它仍然使用继承的思想来实现依赖关系，并使用许多内置函数使其灵活使用。了解原型继承的工作原理将使你很好地理解JavaScript知识，从而避免概念上的误用。

最好在大脑中描绘一下JavaScript的整个机制，以了解原型继承。

JavaScript中有一个超级对象，所有对象都将从中继承。 '__ proto__'指向的对象的Prototype内部属性。原型(prototype )包含一个构造函数，使对象能够从中创建实例。 __proto__始终存在于对象中，并且分层指向它所属的原型，直到null，这称为原型链。
## 10. 解释一下严格模式(strict mode)
严格模式用于标准化正常的JavaScript语义。严格模式可以嵌入到非严格模式中，关键字 ‘use strict’。使用严格模式后的代码应遵循JS严格的语法规则。例如，分号在每个语句声明之后使用。
