```javascript
console.log('script start')
setTimeout(function(){
	console.log('setTimeout')
},0)

Promise.resolve()
.then(function(){
	console.log('promise1')
})
.then(function(){
	console.log('promise2')
})

console.log('script end')
```
以上代码运行结果

```javascript
script start
script end
promise1
promise2
setTimeput
```
Microsoft Edge，Firefox 40，iOS Safari和桌面Safari 8.0.8setTimeout之前promise1和之后都进行了日志记录promise2-尽管这似乎是一种竞争状况。这真的很奇怪，因为Firefox 39和Safari 8.0.7始终如一地正确

**为什么**
每个‘线程’都有自己的事件循环，因此每个web工作者都有自己的事件循环，因此可以独立执行，而同一源的所有窗口都可以共享事件循环，因为他们可以同步通信，事件循环持续运行，执行所有排队的任务。事件循环具有多个任务源，这些任务源保证了该源中的执行顺序（如indexedDB之类的规范了他们的执行顺序），但是浏览器可以在每个循环中选择哪个源执行任务。这使浏览器可以优先执行针对性能敏感的任务，例如用户输入

计划了任务，以便浏览器可以从其内部进入javascript/DOM领域，并确保这些操作有序的发生。在任务之间，浏览器可以呈现更新。从鼠标单击到事件回调，需要安排任务，如同解析html一样，在上面的示例中，该任务也需要安排setTimeout

setTimeout等待给定的延迟，然后为其回调安排新任务。这就是为什么setTimeout在之后script end进行的原因，因为log script end是第一个任务的一部分，并且setTimeout记录在单独的任务中

Microtasks通常安排事情，应改当前执行脚本之后发生，如反应批量的行动，或使一些异步而不采取一个全新的任务，。只要没有其他JavaScript在执行中间，微任务队列就会在回调之后进行处理，并且在每个任务结束时进行处理。在微任务期间排队的任何其他微任务都将添加到队列的末尾并进行处理。**微任务包括 变异观察者回调 并如上例所示Promise回调**

一旦promise得以解决，或者promise已经解决，他就会将微任务排入其反动回调中。这样可以确保即使promise已经解决，promise回调也是异步的。因此 .then(yey, nay)对已解决的promise进行调用会立即使微任务排队。这就是为什么promsie1和promis2在script end后，因为当前正在运行的脚本必须在微任务之前完成。promise1和promsie2在之前记录setTimeout，因为微任务总在下一个任务之前发生
[查看事件循环，动画演示](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)

```javascript
<div class="outer">
  <div class="inner"></div>
</div>

// Let's get hold of those elements
var outer = document.querySelector('.outer');
var inner = document.querySelector('.inner');

// Let's listen for attribute changes on the
// outer element
new MutationObserver(function () {
  console.log('mutate');
}).observe(outer, {
  attributes: true,
});

// Here's a click listener…
function onClick() {
  console.log('click');

  setTimeout(function () {
    console.log('timeout');
  }, 0);

  Promise.resolve().then(function () {
    console.log('promise');
  });

  outer.setAttribute('data-random', Math.random());
}

// …which we'll attach to both elements
inner.addEventListener('click', onClick);
outer.addEventListener('click', onClick);

点击内部 
click
promise
mutate
click
promise
mutate
timeout
timeout

外部
click
promise
mutate
timeout
```
