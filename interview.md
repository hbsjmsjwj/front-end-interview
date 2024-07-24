## 前端如何实现截图？

### 前端实现截图需要使⽤ HTML5 的 Canvas 和相关 API，具体步骤如下：

1. 创建 canvas 元素，并设置其宽和高为需要截屏的元素的宽和高。
2. 使⽤ Canvas API 在 Canvas 上绘制需要截图的内容，⽐如⻚⾯的某个区域、某个元素、图⽚等。使用 canvas 的 getContext 方法获取上下文对象，并设置其属性为 2d。
3. 调⽤ Canvas API 中的 toDataURL() ⽅法将 Canvas 转化为 base64 编码的图⽚数据。 4.将 base64 编码的图⽚数据传递给后端进⾏处理或者直接在前端进⾏显⽰。
   以下是⼀个简单的例⼦，实现了对整个⻚⾯的截图：

```html
<button id="btn">截图</button> <canvas id="canvas"></canvas>
```

```css
#canvas {
  position: fixed;
  left: 0;
  top: 0;
  z-index: 9999;
}
```

```javascript
const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");
const btn = document.getElementById("btn");
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;
btn.addEventListener("click", () => {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  ctx.drawImage(document.documentElement, 0, 0);
  const imgData = canvas.toDataURL();
  console.log(imgData);
});
```

这个例⼦中，在⻚⾯中创建了⼀个 canvas 元素，并设置其宽⾼和样式，将其放在⻚⾯最上⽅。在
点击“截图”按钮时，通过 toDataURL() ⽅法将整个⻚⾯的截图转换为 base64 编码的图⽚数据，
并打印到控制台上。

## 当 QPS 达到峰值时, 该如何处理？

当 QPS 达到峰值时，可以从以下⼏个⽅⾯来进⾏优化：

1. 数据库优化：数据库的优化包括优化 SQL 语句、使⽤索引、避免全表扫描、分表分库等措施，以提
   ⾼数据库的读写性能。
2. 缓存优化：缓存可以降低对数据库的访问频率，提⾼响应速度。可以使⽤ Redis、Memcached 等缓
   存技术，减轻服务器负载。
3. 代码优化：优化代码可以提⾼代码的执⾏效率，减少不必要的开销。可以通过⼀些优化⼿段，如减
   少不必要的代码执⾏、避免循环嵌套、避免不必要的递归调⽤等来提⾼代码的性能。
4. 负载均衡：负载均衡可以将请求分发到多个服务器上，减少单个服务器的负载，提⾼整个系统的性
   能和可⽤性。
5. 异步处理：将⼀些计算量⼤、耗时⻓的操作异步处理，减少对主线程的阻塞，提⾼响应速度。
6. CDN 加速：使⽤ CDN 技术可以将静态资源缓存到 CDN 节点上，提⾼资源的加载速度，减少服务器的负载。
7. 硬件升级：可以通过升级服务器硬件，增加带宽等⽅式来提⾼系统的处理能⼒。
   以上是⼀些常⻅的优化⼿段，需要根据具体情况进⾏选择和实施。

## js 超过 Number 最⼤值的数怎么处理？

在 JavaScript 中，超过 Number.MAX_VALUE 的数值被认为是 Infinity （正⽆穷⼤）。如果要
处理超过 Number.MAX_VALUE 的数值，可以使⽤第三⽅的 JavaScript 库，如 big.js 或
bignumber.js ，这些库可以处理任意精度的数值。
例如，使⽤ big.js 库可以将两个超过 Number.MAX_VALUE 的数相加：

```javascript
const big = require("big.js");

const x = new big("9007199254740993");
const y = new big("100000000000000000");

const result = x.plus(y);

console.log(result.toString()); // 输出：100009007194925474093
```

这⾥创建了两个 big.js 对象 x 和 y ，分别存储超过 Number.MAX_VALUE 的数值。通过
plus ⽅法将它们相加，得到了正确的结果。最后，通过 toString ⽅法将结果转换为字符串。

#### 如果不依赖外部库，咋处理

JavaScript 中，数值超过了 Number 最⼤值时，可以使⽤ BigInt 类型来处理，它可以表⽰任意精度的整数。
使⽤ BigInt 类型时，需要在数值后⾯添加⼀个 n 后缀来表⽰ BigInt 类型。例如：

```javascript
const bigNum = 9007199254740993n; // 注意：数字后⾯添加了 'n' 后缀
```

注意，BigInt 类型是 ECMAScript 2020 新增的特性，因此在某些浏览器中可能不被⽀持。如果需要在
不⽀持 BigInt 的环境中使⽤ BigInt，可以使⽤ polyfill 或者第三⽅库来实现。

## 使⽤同⼀个链接， 如何实现 PC 打开是 web 应⽤、⼿机打开是⼀个 H5 应⽤？

可以通过根据请求来源（User-Agent）来判断访问设备的类型，然后在服务器端进⾏适配。例如，可
以在服务器端使⽤ Node.js 的 Express 框架，在路由中对不同的 User-Agent 进⾏判断，返回不同的⻚⾯或数据。具体实现可以参考以下步骤：

1. 根据 User-Agent 判断访问设备的类型，例如判断是否为移动设备。可以使⽤第三⽅库如 ua-parser-js 进⾏ User-Agent 的解析。
2. 如果是移动设备，可以返回⼀个 H5 ⻚⾯或接⼝数据。
3. 如果是 PC 设备，可以返回⼀个 web 应⽤⻚⾯或接⼝数据。具体实现⽅式还取决于应⽤的具体场景和需求，以上只是⼀个⼤致的思路。

## 如何保证⽤⼾的使⽤体验?

【如何保证⽤⼾的使⽤体验】这个也是⼀个较为复杂的话题， 这个也不是问题了， 这个算是话题吧；
主要从以下⼏个⽅⾯思考问题：

1. 性能⽅向的思考
2. ⽤⼾线上问题反馈，线上 on call 的思考
3. ⽤⼾使⽤体验的思考， 交互体验使⽤⽅向
4. 提升⽤⼾能效⽅向思考

## 如何解决⻚⾯请求接⼝⼤规模并发问题?

如何解决⻚⾯请求接⼝⼤规模并发问题， 不仅仅是包含了接⼝并发， 还有前端资源下载的请求并发。
应该说这是⼀个话题讨论了；
个⼈认为可以从以下⼏个⽅⾯来考虑如何解决这个并发问题:

1. 后端优化：可以对接⼝进⾏优化，采⽤缓存技术，对数据进⾏预处理，减少数据库操作等。使⽤集
   群技术，将请求分散到不同的服务器上，提⾼并发量。另外可以使⽤反向代理、负载均衡等技术，
   分担服务器压⼒。
2. 做 BFF 聚合：把所有⾸屏需要依赖的接⼝， 利⽤服务中间层给聚合为⼀个接⼝。
3. CDN 加速：使⽤ CDN 缓存技术可以有效减少服务器请求压⼒，提⾼⽹站访问速度。CDN 缓存可以将
   接⼝的数据存储在缓存服务器中，减少对原始服务器的访问，加速数据传输速度。
4. 使⽤ WebSocket：使⽤ WebSocket 可以建⽴⼀个持久的连接，避免反复连接请求。WebSocket
   可以实现双向通信，⼤幅降低服务器响应时间。
5. 使⽤ HTTP2 及其以上版本， 使⽤多路复⽤。
6. 使⽤浏览器缓存技术：强缓存、协商缓存、离线缓存、Service Worker 缓存 等⽅向。
7. 聚合⼀定量的静态资源： ⽐如提取⻚⾯公⽤复⽤部分代码打包到⼀个⽂件⾥⾯、对图⽚进⾏雪碧图
   处理， 多个图⽚只下载⼀个图⽚。
8. 采⽤微前端⼯程架构： 只是对当前访问⻚⾯的静态资源进⾏下载， ⽽不是下载整站静态资源。
9. 使⽤服务端渲染技术： 从服务端把⻚⾯⾸屏直接渲染好返回， 就可以避免掉⾸屏需要的数据再做
   额外加载和执⾏。

## 设计⼀套全站请求耗时统计⼯具？

#### ⾸先我们要知道有哪些⽅式可以统计前端请求耗时

从代码层⾯上统计全站所有请求的耗时⽅式主要有以下⼏种：

1. Performance API：Performance API 是浏览器提供的⼀组 API，可以⽤于测量⽹⻚性能。通过
   Performance API，可以获取⻚⾯各个阶段的时间、资源加载时间等。其中，Performance
   Timing API 可以获取到每个资源的加载时间，从⽽计算出所有请求的耗时。
2. XMLHttpRequest 的 load 事件：在发送 XMLHttpRequest 请求时，可以为其添加 load 事件，在请
   求完成时执⾏回调函数，从⽽记录请求的耗时。
3. fetch 的 Performance API：类似 XMLHttpRequest，fetch 也提供了 Performance API，可以通过
   Performance API 获取请求耗时。
4. ⾃定义封装的请求函数：可以⾃⼰封装⼀个请求函数，在请求开始和结束时记录时间，从⽽计算请
   求耗时。

#### 设计⼀套前端全站请求耗时统计⼯具

可以遵循以下步骤：

1. 实现⼀个性能监控模块，⽤于记录每个请求的开始时间和结束时间，并计算耗时。
2. 在应⽤⼊⼝处引⼊该模块，将每个请求的开始时间记录下来。
3. 在每个请求的响应拦截器中，记录响应结束时间，并计算请求耗时。
4. 将每个请求的耗时信息发送到服务端，以便进⾏进⼀步的统计和分析。
5. 在服务端实现数据存储和展⽰，可以使⽤图表等⽅式展⽰请求耗时情况。
6. 对于请求耗时较⻓的接⼝，可以进⾏优化和分析，如使⽤缓存、使⽤异步加载、优化查询语句等。
7. 在前端应⽤中可以提供开关，允许⽤⼾⾃主开启和关闭全站请求耗时统计功能。
   以下是⼀个简单的实现⽰例：

```javascript
// performance.js
const performance = {
  timings: {},
  config: {
    reportUrl: "/report",
  },
  init() {
    // 监听所有请求的开始时间
    window.addEventListener("fetchStart", (event) => {
      this.timings[event.detail.id] = {
        startTime: Date.now(),
      };
    });
    // 监听所有请求的结束时间，并计算请求耗时
    window.addEventListener("fetchEnd", (event) => {
      const id = event.detail.id;
      if (this.timings[id]) {
        const timing = this.timings[id];
        timing.endTime = Date.now();
        timing.duration = timing.endTime - timing.startTime;
        // 将耗时信息发送到服务端
        const reportData = {
          url: event.detail.url,
          method: event.detail.method,
          duration: timing.duration,
        };
        this.report(reportData);
      }
    });
  },
  report(data) {
    // 将耗时信息发送到服务端
    const xhr = new XMLHttpRequest();
    xhr.open("POST", this.config.reportUrl);
    xhr.setRequestHeader("Content-Type", "application/json");
    xhr.send(JSON.stringify(data));
  },
};

export default performance;
```

在应⽤⼊⼝处引⼊该模块：

```javascript
// main.jsimport
performance from './performance';
performance.init();
```

在每个请求的响应拦截器中触发 fetchEnd 事件：

```javascript
// fetch.js
import EventBus from "./EventBus";

const fetch = (url, options) => {
  const id = Math.random().toString(36).slice(2);
  const fetchStartEvent = new CustomEvent("fetchStart", {
    detail: {
      id,
      url,
      method: options.method || "GET",
    },
  });
  EventBus.dispatchEvent(fetchStartEvent);
  return window.fetch(url, options).then((response) => {
    const fetchEndEvent = new CustomEvent("fetchEnd", {
      detail: {
        id,
        url,
        method: options.method || "GET",
      },
    });
    EventBus.dispatchEvent(fetchEndEvent);
    return response;
  });
};

export default fetch;
```

在服务端实现数据存储和展⽰，可以使⽤图表等⽅式展⽰请求耗

## ⼤⽂件上传了解多少?

#### ⼤⽂件分⽚上传

如果太⼤的⽂件，⽐如⼀个视频 1g 2g 那么⼤，直接上传可能会出链接现超时
的情况，⽽且也会超过服务端允许上传⽂件的⼤⼩限制，所以解决这个问题我们可以将⽂件进⾏分⽚
上传，每次只上传很⼩的⼀部分 ⽐如 2M。
Blob 它表⽰原始数据, 也就是⼆进制数据，同时提供了对数据截取的⽅法 slice ,⽽ File 继承
了 Blob 的功能，所以可以直接使⽤此⽅法对数据进⾏分段截图。
过程如下：

- 把⼤⽂件进⾏分段 ⽐如 2M，发送到服务器携带⼀个标志，暂时⽤当前的时间戳，⽤于标识⼀个完
  整的⽂件
- 服务端保存各段⽂件
- 浏览器端所有分⽚上传完成，发送给服务端⼀个合并⽂件的请求
- 服务端根据⽂件标识、类型、各分⽚顺序进⾏⽂件合并
- 删除分⽚⽂件

客⼾端 JS 代码实现如下

```javascript
function submitUpload() {
  var chunkSize = 210241024; //分⽚⼤⼩ 2M
  var file = document.getElementById("f1").files[0];
  var chunks = [], //保存分⽚数据
    token = +new Date(), //时间戳
    name = file.name,
    chunkCount = 0,
    sendChunkCount = 0;
  //拆分⽂件 像操作字符串⼀样
  if (file.size > chunkSize) {
    //拆分⽂件var start = 0,
    end = 0;
    while (true) {
      end += chunkSize;
      var blob = file.slice(start, end);
      start += chunkSize;
      //截取的数据为空 则结束
      if (!blob.size) {
        //拆分结束break;
      }
      chunks.push(blob); //保存分段数据
    }
  } else {
    chunks.push(file.slice(0));
  }
  chunkCount = chunks.length;
  //分⽚的个数
  //没有做并发限制，较⼤⽂件导致并发过多，tcp链接被占光 ，需要做下并发控制，⽐如只有4个在请求在发送
  for (var i = 0; i < chunkCount; i++) {
    //构造FormData对象
    var fd = new FormData();
    fd.append("token", token);
    fd.append("f1", chunks[i]);
    fd.append("index", i);
    xhrSend(fd, function () {
      sendChunkCount += 1;
      if (sendChunkCount === chunkCount) {
        //上传完成，发送合并
        请求console.log("上传完成，发送合并请求");
        var formD = new FormData();
        formD.append("type", "merge");
        formD.append("token", token);
        formD.append("chunkCount", chunkCount);
        formD.append("filename", name);
        xhrSend(formD);
      }
    });
  }
}

function xhrSend(fd, cb) {
  var xhr = new XMLHttpRequest(); //创建对象
  xhr.open("POST", "http://localhost:8100/", true);
  xhr.onreadystatechange = function () {
    console.log("state change", xhr.readyState);
    if (xhr.readyState == 4) {
      console.log(xhr.responseText);
      cb && cb();
    }
  };
  xhr.send(fd); //发送
}

//绑定提交事件
document.getElementById("btn-submit").addEventListener("click", submitUpload);
```

服务端 node 实现代码如下： 合并⽂件这⾥使⽤ stream pipe 实现，这样更节省内存，边读边写⼊，
占⽤内存更⼩，效率更⾼，代码⻅ fnMergeFile ⽅法。

```javascript
 //⼆次处理⽂件，修改名称
app.use((ctx) => {
  var body = ctx.request.body;
  var files = ctx.request.files ? ctx.request.files.f1 : [];
  //得到上传⽂件的数组
  var result = [];
  var fileToken = ctx.request.body.token;
  // ⽂件标识
  var fileIndex = ctx.request.body.index;//⽂件顺序
  if (files && !Array.isArray(files)) {
     //单⽂件上传容错
     files = [files];
  }
  files && files.forEach(item => {
    var path = item.path;
    var fname = item.name;
    //原⽂件名称
    var nextPath = path.slice(0, path.lastIndexOf('/') + 1) + fileIndex +'-'+fileToken;
    if (item.size > 0 && path) {
      //得到扩展名
      var extArr =fname.split('.');
      var ext = extArr[extArr.length - 1];
      var nextPath = path +'.' + ext;//重命名⽂件
     fs.renameSync(path, nextPath);
     result.push(uploadHost + nextPath.slice(nextPath.lastIndexOf('/') + 1));
    }
  });
 if (body.type === 'merge') {
  //合并分⽚⽂件
  var filename = body.filename,
   chunkCount = body.chunkCount,
   folder = path.resolve(__dirname, '../static/uploads') + '/';
  var writeStream = fs.createWriteStream( ${folder}${filename} );
 var cindex = 0;
 //合并⽂件
 function fnMergeFile() {
  var fname =${folder}${cindex}-${fileToken};var readStream = fs.createReadStream(fname);
   readStream.pipe(writeStream, { end: false });
   readStream.on("end", function() {
   fs.unlink(fname,
   function(err) {
    if (err) {
      throw err;
    }
  });
  if (cindex + 1 < chunkCount) {
    cindex += 1;
    fnMergeFile();
  }
  });
 }
 fnMergeFile();
   ctx.body = 'merge ok 200';
 }
 });
```

## ⼤⽂件上传断点续传

在上⾯我们实现了⽂件分⽚上传和最终的合并，现在要做的就是如何检测这些分⽚，不再重新上传即可。
这⾥我们可以在本地进⾏保存已上传成功的分⽚，重新上传的时候使⽤ spark-md5 来⽣成⽂件
hash，区分此⽂件是否已上传。

- 为每个分段⽣成 hash 值，使⽤ spark-md5 库
- 将上传成功的分段信息保存到本地
- 重新上传时，进⾏和本地分段 hash 值的对⽐，如果相同的话则跳过，继续下⼀个分段的上传

### ⽅案⼀： 保存在本地 indexDB/localStorage 等地⽅， 推荐使⽤ localForage 这个库

npm install localforage
客⼾端 JS 代码：

```javascript
//获得本地缓存的数据
function getUploadedFromStorage() {
  return JSON.parse(localforage.getItem(saveChunkKey) || "{}");
}

//写⼊缓存
function setUploadedToStorage(index) {
  var obj = getUploadedFromStorage();
  obj[index] = true;
  localforage.setItem(saveChunkKey, JSON.stringify(obj));
}

//分段对⽐
var uploadedInfo = getUploadedFromStorage();
//获得已上传的分段信息
for (var i = 0; i < chunkCount; i++) {
  console.log("index", i, uploadedInfo[i] ? "已上传过" : "未上传");
  if (uploadedInfo[i]) {
    //对⽐分段
    sendChunkCount = i + 1; //记录已上传的索引continue;//如果已上传则跳过
  }
  var fd = new FormData(); //构造FormData对象
  fd.append("token", token);
  fd.append("f1", chunks[i]);
  fd.append("index", i);
  (function (index) {
    xhrSend(fd, function () {
      sendChunkCount += 1; //将成功信息保存到本地
      setUploadedToStorage(index);
      if (sendChunkCount === chunkCount) {
        console.log("上传完成，发送合并请求");
        var formD = new FormData();
        formD.append("type", "merge");
        formD.append("token", token);
        formD.append("chunkCount", chunkCount);
        formD.append("filename", name);
        xhrSend(formD);
      }
    });
  })(i);
}
```

### ⽅案 2：服务端⽤于保存分⽚坐标信息， 返回给前端

需要服务端添加⼀个接⼝只是服务端需要增加⼀个接⼝。 基于上⾯⼀个栗⼦进⾏改进，服务端已保存
了部分⽚段，客⼾端上传前需要从服务端获取已上传的分⽚信息（上⾯是保存在了本地浏览器），本
地对⽐每个分⽚的 hash 值，跳过已上传的部分，只传未上传的分⽚。
⽅法 1 是从本地获取分⽚信息,这⾥只需要将此⽅法的能⼒改为从服务端获取分⽚信息就⾏了。

## H5 如何解决移动端适配问题

移动端适配问题是指如何让⽹⻚在不同的移动设备上显⽰效果相同。下⾯是⼀些常⻅的 H5 移动端适配
⽅案：

1. 使⽤ viewport 标签
   通过设置 viewport 标签的 meta 属性，来控制⻚⾯的缩放⽐例和宽度，以适配不同的设备。例如：

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
```

其中 width=device-width 表⽰设置 viewport 的宽度为设备宽度，
initial-scale=1.0 表⽰初始缩放⽐例为 1。 2. 使⽤ CSS3 的媒体查询
通过 CSS3 的媒体查询，根据不同的设备宽度设置不同的样式，以适配不同的设备。例如：

```css
@media screen and (max-width: 640px) {
  /* 样式 */
}
```

其中 max-width 表⽰最⼤宽度，当屏幕宽度⼩于等于 640px 时，应⽤这些样式。

3. 使⽤ rem 单位
   通过将 px 转化为 rem 单位，根据不同的设备字体⼤⼩设置不同的样式，以适配不同的设备。例如：

```css
html {
  font-size: 16px;
}
@media screen and (max-width: 640px) {
  html {
    font-size: 14px;
  }
}
div {
  width: 10rem;
}
```

其中 font-size: 16px 表⽰将⽹⻚的基准字体⼤⼩设置为 16px， font-size: 14px 表⽰在
屏幕宽度⼩于等于 640px 时将基准字体⼤⼩设置为 14px， div 元素的 width: 10rem 表⽰该元
素的宽度为 10 个基准字体⼤⼩。

4. 使⽤ flexible 布局⽅案
   通过使⽤ flexible 布局⽅案，将 px 转化为 rem 单位，并且动态计算根节点的字体⼤⼩，以适配不同的
   设备。例如使⽤ lib-flexible 库：

```javascript
// index.html
<script src="https://cdn.bootcdn.net/ajax/libs/lib-flexible/0.3.4/flexible.js"></script>;
// index.js
import "lib-flexible/flexible.js";
```

其中 flexible.js 会在⻚⾯加载时动态计算根节点的字体⼤⼩，并将 px 转化为 rem 单位。在样
式中可以直接使⽤ px 单位，例如：

```css
div {
  width: 100px;
  height: 100px;
}
```

## 站点⼀键换肤的实现⽅式有哪些？

#### ⽹站⼀键换肤实现⽅式有以下⼏种

1. 使⽤ CSS 变量：通过定义⼀些变量来控制颜⾊、字体等，然后在切换主题时动态修改这些变量的值。
2. 使⽤ class 切换：在 HTML 的根元素上添加不同的 class 名称，每个 class 名称对应不同的主题样
   式，在切换主题时切换根元素的 class 名称即可。
3. 使⽤ JavaScript 切换：使⽤ JavaScript 动态修改⻚⾯的样式，如修改元素的背景颜⾊、字体颜⾊
   等。
4. 使⽤ Less/Sass 等 CSS 预处理器：通过预处理器提供的变量、函数等功能来实现主题切换。
   需要注意的是，⽆论采⽤哪种⽅式实现，都需要在设计⻚⾯样式时尽量遵循⼀些规范，如不使⽤绝对
   的像素值，使⽤相对单位等，以便更好地适应不同的屏幕⼤⼩和分辨率。

### 以 less 举例， 详细讲述⼀下具体操作流程

通过 Less 实现⽹⻚换肤可以使⽤ CSS 变量和 Less 变量。
CSS 变量的语法如下：

```less
:root {
  --primary-color: #007bff;
}
.btn {
  background-color: var(--primary-color);
}
```

⽽ Less 变量则是通过 Less 预编译器提供的变量语法来实现的，如下所⽰：

```less
@primary-color: #007bff;

.btn {
  background-color: @primary-color;
}
```

通过 Less 变量来实现⽹⻚换肤的⽅式可以在运⾏时使⽤ JavaScript 来修改 Less 变量的值，从⽽实现
换肤效果。具体步骤如下：

1. 使⽤ Less 预编译器来编译 Less ⽂件为 CSS ⽂件。
2. 在 HTML ⽂件中引⼊编译后的 CSS ⽂件。
3. 在 JavaScript 中动态修改 Less 变量的值。
4. 使⽤ JavaScript 将新的 Less 变量值注⼊到编译后的 CSS ⽂件中。
5. 将注⼊后的 CSS 样式应⽤到⻚⾯上。

   ##### 以下是⼀段实现通过 Less 变量来实现⽹⻚换肤的⽰例代码：

```scss
// base.less ⽂件@primary-color: #007bff;

.btn {
  background-color: @primary-color;
}

// dark.less ⽂件@primary-color: #343a40;
```

```html
<!-- index.html ⽂件 -->
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>⽹⻚换肤⽰例</title>
    <link rel="stylesheet/less" type="text/css" href="base.less" />
    <link rel="stylesheet/less" type="text/css" href="dark.less" />
  </head>
  <body>
    <button class="btn">按钮</button>
    <script src="less.min.js"></script>
    <script>
      function changeSkin() {
        less
          .modifyVars({
            "@primary-color": "#28a745",
          })
          .then(() => {
            console.log("换肤成功");
          })
          .catch(() => {
            console.error("换肤失败");
          });
      }
    </script>
  </body>
</html>
```

在上⾯的⽰例代码中，我们引⼊了两个 Less ⽂件，⼀个是 base.less ，⼀个是 dark.less 。其
中 base.less 定义了⼀些基础的样式，⽽ dark.less 则是定义了⼀个暗⿊⾊的主题样式。在
JavaScript 中，我们使⽤ less.modifyVars ⽅法来修改 Less 变量的值，从⽽实现了换肤的效
果。当然，这只是⼀个简单的⽰例代码，实际的换肤功能还需要根据实际需求来进⾏设计和实现。

##  如何实现⽹⻚加载进度条？