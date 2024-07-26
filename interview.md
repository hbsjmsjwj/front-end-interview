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

## 如何实现⽹⻚加载进度条？

监听静态资源加载情况

可以通过 window.performance 对象来监听⻚⾯资源加载进度。该对象提供了各种⽅法来获取资
源加载的详细信息。
可以使⽤ performance.getEntries() ⽅法获取⻚⾯上所有的资源加载信息。可以使⽤该⽅法
来监测每个资源的加载状态，计算加载时间，并据此来实现⼀个资源加载进度条。
下⾯是⼀个简单的实现⽅式：

```javascript
const resources = window.performance.getEntriesByType("resource");
const totalResources = resources.length;
let loadedResources = 0;
resources.forEach((resource) => {
  if (resource.initiatorType !== "xmlhttprequest") {
    // 排除 AJAX 请求
    resource.onload = () => {
      loadedResources++;
      const progress = Math.round((loadedResources / totalResources) * 100);
      updateProgress(progress);
    };
  }
});

function updateProgress(progress) {
  // 更新进度条
}
```

该代码会遍历所有资源，并注册⼀个 onload 事件处理函数。当每个资源加载完成后，会更新
loadedResources 变量，并计算当前的进度百分⽐，然后调⽤ updateProgress() 函数来更
新进度条。需要注意的是，这⾥排除了 AJAX 请求，因为它们不属于⻚⾯资源。
当所有资源加载完成后，⻚⾯就会完全加载。

### 实现进度条

⽹⻚加载进度条可以通过前端技术实现，⼀般的实现思路是通过监听浏览器的⻚⾯加载事件和资源加
载事件，来实时更新进度条的状态。下⾯介绍两种实现⽅式。

1. 使⽤原⽣进度条
   在 HTML5 中提供了 progress 元素，可以通过它来实现⼀个原⽣的进度条。

```html
<progress id="progressBar" value="0" max="100"></progress>
```

然后在 JavaScript 中，监听⻚⾯加载事件和资源加载事件，实时更新 progress 元素的 value 属性

```javascript
const progressBar = document.getElementById("progressBar");

window.addEventListener("load", () => {
  progressBar.value = 100;
});

document.addEventListener("readystatechange", () => {
  const progress = Math.floor((document.readyState / 4) * 100);
  progressBar.value = progress;
});
```

2. 使⽤第三⽅库
   使⽤第三⽅库可以更加⽅便地实现⽹⻚加载进度条，下⾯以 nprogress 库为例：
1. 安装 nprogress 库

```shell
npm install nprogress --save
```

2. 在⻚⾯中引⼊ nprogress.css 和 nprogress.js

```javascript
 <link rel="stylesheet" href="/node_modules/nprogress/nprogress.css">
 <script src="/node_modules/nprogress/nprogress.js"></script>
```

3. 在 JavaScript 中初始化 nprogress 并监听⻚⾯加载事件和资源加载事件

```javascript
// 初始化 nprogress
NProgress.configure({ showSpinner: false });

// 监听⻚⾯加载事件
window.addEventListener("load", () => {
  NProgress.done();
});

// 监听资源加载事件
document.addEventListener("readystatechange", () => {
  if (document.readyState === "interactive") {
    NProgress.start();
  } else if (document.readyState === "complete") {
    NProgress.done();
  }
});
```

使⽤ nprogress 可以⾃定义进度条的样式，同时也提供了更多的 API 供我们使⽤，⽐如说⼿动控制
进度条的显⽰和隐藏，以及⽀持 Promise 和 Ajax 请求的进度条等等。

## 常⻅图⽚懒加载⽅式有哪些？【热度: 1,001】

图⽚懒加载可以延迟图⽚的加载，只有当图⽚即将进⼊视⼝范围时才进⾏加载。这可以⼤⼤减轻⻚⾯
的加载时间，并降低带宽消耗，提⾼了⽤⼾的体验。以下是⼀些常⻅的实现⽅法：

1. Intersection Observer API
   Intersection Observer API 是⼀种⽤于异步检查⽂档中元素与视⼝叠加程度的 API。可以将
   其⽤于检测图⽚是否已经进⼊视⼝，并根据需要进⾏相应的处理。

```javascript
let observer = new IntersectionObserver(function (entries) {
  entries.forEach(function (entry) {
    if (entry.isIntersecting) {
      const lazyImage = entry.target;
      lazyImage.src = lazyImage.dataset.src;
      observer.unobserve(lazyImage);
    }
  });
});

const lazyImages = [...document.querySelectorAll(".lazy")];
lazyImages.forEach(function (image) {
  observer.observe(image);
});
```

2.  ⾃定义监听器
    可以通过⾃定义监听器来实现懒加载。其中，应该避免在滚动事件处理程序中频繁进⾏图⽚加
    载，因为这可能会影响性能。相反，使⽤⾃定义监听器只会在滚动停⽌时进⾏图⽚加载。

```javascript
function lazyLoad() {
  const images = document.querySelectorAll(".lazy");
  const scrollTop = window.pageYOffset;
  images.forEach((img) => {
    if (img.offsetTop < window.innerHeight + scrollTop) {
      img.src = img.dataset.src;
      img.classList.remove("lazy");
    }
  });
}

let lazyLoadThrottleTimeout;
document.addEventListener("scroll", function () {
  if (lazyLoadThrottleTimeout) {
    clearTimeout(lazyLoadThrottleTimeout);
  }
  lazyLoadThrottleTimeout = setTimeout(lazyLoad, 20);
});
```

在这个例⼦中，我们使⽤了 setTimeout() 函数来延迟图⽚的加载，以避免在滚动事件的频繁触发
中对性能的影响。
⽆论使⽤哪种⽅法，都需要为需要懒加载的图⽚设置占位符，并将未加载的图⽚路径保存在 data 属
性中，以便在需要时进⾏加载。这些占位符可以是简单的 div 或样式类，⽤于预留图⽚的空间，避免⻚
⾯布局的混乱。

```html
<!-- 占位符⽰例 -->
<div
  class="lazy-placeholder"
  style="background-color:#ddd;height: 500px;"
></div>
<!-- 图⽚⽰例 -->
<img class="lazy" data-src="path/to/image.jpg" alt="预览图" />
```

总体来说，图⽚懒加载是⼀种这很简单，但⾮常实⽤的优化技术，能够显著提⾼⽹⻚的性能和⽤⼾体
验。

## cookie 构成部分有哪些【热度: 598】

在 HTTP 协议中，cookie 是⼀种包含在请求和响应报⽂头中的数据，⽤于在客⼾端存储和读取信息。
cookie 是由服务器发送的，客⼾端可以使⽤浏览器 API 将 cookie 存储在本地进⾏后续使⽤。
⼀个 cookie 通常由以下⼏个部分组成：

1. 名称：cookie 的名称（键），通常是⼀个字符串。
2. 值：cookie 的值，通常也是⼀个字符串。
3. 失效时间：cookie 失效的时间，过期时间通常存储在⼀个 expires 属性中，以便浏览器⾃动清
   除失效的 cookie。
4. 作⽤路径：cookie 的作⽤路径，只有在指定路径下的请求才会携带该 cookie。
5. 作⽤域：cookie 的作⽤域，指定了该 cookie 绑定的域名，可以使⽤ domain 属性来设置。
   例如，以下是⼀个设置了名称为 "user"、值为 "john"、失效时间为 2022 年 1 ⽉ 1 ⽇，并且作⽤于全
   站的 cookie：

```shell
Set-Cookie: user=john; expires=Sat, 01 Jan 2022 00:00:00 GMT; path=/;
domain=example.com
```

其中， Set-Cookie 是响应报⽂头，⽤于设置 cookie。在该响应报⽂中，将 cookie 数据设置为
"user=john"，失效时间为 "2022 年 1 ⽉ 1 ⽇"，作⽤路径为全站，作⽤域为 "example.com" 的域名。这
个 cookie 就会被存储在客⼾端，以便在以后的请求中发送给服务器。

## 扫码登录实现⽅式【热度: 734】

扫码登录的实现原理核⼼是基于⼀个中转站，该中转站通常由应⽤提供商提供，⽤于维护⼿机和 PC 之
间的会话状态。
整个扫码登录的流程如下：

1. ⽤⼾在 PC 端访问应⽤，并选择使⽤扫码登录⽅式。此时，应⽤⽣成⼀个随机的认证码，并将该认证
   码通过⼆维码的形式显⽰在 PC 端的⻚⾯上。
2. ⽤⼾打开⼿机上的应⽤，并选择使⽤扫码登录⽅式。此时，应⽤会打开⼿机端的相机，⽤⼾可以对
   着 PC 端的⼆维码进⾏扫描。
3. ⼀旦⽤⼾扫描了⼆维码，⼿机上的应⽤会向应⽤提供商的中转站发送⼀个请求，请求包含之前⽣成
   的随机认证码和⼿机端的⼀个会话 ID。
4. 中转站验证认证码和会话 ID 是否匹配，如果匹配成功，则该中转站将⽤⼾的⾝份信息发送给应⽤，
   并创建⼀个 PC 端和⼿机端之间的会话状态。
5. 应⽤使⽤收到的⾝份信息对⽤⼾进⾏认证，并创建⼀个与该⽤⼾关联的会话状态。同时，应⽤返回
   ⼀个通过认证的响应给中转站。
6. 中转站将该响应返回给⼿机端的应⽤，并携带⼀个⽤于表⽰该会话的令牌，此时⼿机和 PC 之间的认
   证流程就完成了。
7. 当⽤⼾在 PC 端进⾏其他操作时，应⽤将会话令牌附加在请求中，并通过中转站向⼿机端的应⽤发起
   请求。⼿机端的应⽤使⽤会话令牌（也就是之前⽣成的令牌）来识别并验证会话状态，从⽽允许⽤
   ⼾在 PC 端进⾏需要登录的操作。

## DNS 协议了解多少【热度: 712】

#### DNS 基本概念

DNS（Domain Name System，域名系统）是因特⽹上⽤于将主机名转换为 IP 地址的协议。它是⼀个
分布式数据库系统，通过将主机名映射到 IP 地址来实现主机名解析，并使⽤⼾能够通过更容易识别的
主机名来访问互联⽹上的资源。
在使⽤ DNS 协议进⾏主机名解析时，系统⾸先查询本地 DNS 缓存。如果缓存中不存在结果，系统将
向本地 DNS 服务器发出请求，并逐级向上查找，直到找到权威 DNS 服务器并获得解析结果。在域名
解析的过程中，DNS 协议采⽤了分级命名空间的结构，不同的域名可以通过点分隔符分为多个级别，
例如 www.example.com 可以分为三个级别： www 、 example 和 com 。
除了将域名映射到 IP 地址之外，DNS 协议还⽀持多种其他功能：

1. 逆向映射：将 IP 地址解析为域名。
2. 邮件服务器设置：⽀持邮件服务器的⾃动发现和设置。
3. 负载均衡：DNS 还可以实现简单的负载均衡，通过将相同 IP 地址的主机名映射到不同的 IP 地址来
   分散负载。
4. 安全：DNSSEC（DNS Security Extensions，DNS 安全扩展）可以提供对域名解析的认证和完整
   性。

#### 如何加快 DNS 的解析？

有以下⼏种⽅法可以加快 DNS 的解析：

1. 使⽤⾼速 DNS 服务器：默认情况下，⽹络服务提供商（ISP）为其⽤⼾提供 DNS 服务器。但是，
   这些服务器不⼀定是最快的，有时会出现瓶颈。如果您想加快 DNS 解析，请尝试使⽤其他⾼速
   DNS 服务器，例如 Google 的公共 DNS 服务器或 OpenDNS。
2. 缓存 DNS 记录：在本地计算机上缓存 DNS 记录可以⼤⼤加快应⽤程序的响应。当您访问特定的⽹
   站时，计算机会⾃动缓存该⽹站的 DNS 记录。如果您再次访问该⽹站，则计算机将使⽤缓存的
   DNS 记录。
3. 减少 DNS 查找：当您访问⼀个⽹站时，您的计算机将会查找该域名的 IP 地址。如果⽹站有很多域
   名，则查找过程可能会变得⾮常缓慢。因此，尽可能使⽤较少的域名可以减少 DNS 查找的数量，
   并提⾼响应速度。
4. 使⽤ CDN：CDN（内容分发⽹络）是⼀种将内容存储在全球多个位置的系统。这些位置通常都有专
   ⽤的 DNS 服务器，可以⼤⼤加快站点的加载速度。
5. 使⽤ DNS 缓存⼯具：⼀些辅助⼯具可以帮助您优化与 DNS 相关的设置，例如免费的 DNS Jumper
   软件和 Namebench ⼯具，它们可以测试您的 DNS 响应时间并为您推荐最佳配置。
   通过使⽤⾼速 DNS 服务器、缓存 DNS 记录、减少 DNS 查找、使⽤ CDN 和 DNS 缓存⼯具等⽅法，可
   以显著提⾼ DNS 解析速度，从⽽加快应⽤程序响应时间

## 函数式编程了解多少？【热度: 1,789】

#### 函数式编程的核⼼概念

函数式编程是⼀种编程范式，它将程序看做是⼀系列函数的组合，函数是应⽤的基础单位。函数式编
程主要有以下核⼼概念：

1. 纯函数：函数的输出只取决于输⼊，没有任何副作⽤，不会修改外部变量或状态，所以对于同样的
   输⼊，永远返回同样的输出值。因此，纯函数可以有效地避免副作⽤和竞态条件等问题，使得代码
   更加可靠、易于调试和测试。
2. 不可变性：在函数式编程中，数据通常是不可变的，即不允许在内部进⾏修改。这样可以避免副作
   ⽤的发⽣，提⾼代码可靠性。
3. 函数组合：函数可以组合成复杂的函数，从⽽减少重复代码的产⽣。
4. ⾼阶函数：⾼阶函数是指可以接收其他函数作为参数，也可以返回函数的函数。例如，函数柯⾥化
   和函数的组合就是⾼阶函数的应⽤场景。
5. 惰性计算：指在必要的时候才计算（执⾏）函数，⽽不是在每个可能的执⾏路径上都执⾏，从⽽提
   ⾼性能。
   函数式编程的核⼼概念是将函数作为基本构建块来组合构建程序，通过纯函数、不可变性、函数组
   合、⾼阶函数和惰性计算等概念来实现代码的简洁性、可读性和可维护性，以及⾼效的性能运⾏。

#### 函数式编程的优势

1. 易于理解和维护：函数式编程强调数据不变性和纯函数概念，可以提⾼代码的可读性和可维护性，
   因为它避免了按照顺序对变量进⾏修改，并强调函数⾏为的确定性。
2. 更少的 bug：由于函数式编程强调纯函数的概念，它可以消除由于副作⽤引起的 bug。因为纯函数
   不会修改外部状态或数据结构，只是将输⼊转换为输出。这么做有助于保持代码更加可靠。
3. 更好的可测试性：由于纯函数不具有副作⽤，它更容易测试，因为测试数据是预测性的。
4. 更少的重构：函数式编程使⽤函数组合和柯⾥化等⽅法来简化代码。它将⼤型问题分解为微⼩问
   题，从⽽减少了代码重构的需要。
5. 避免并发问题：由于函数式编程强调不变性和纯函数的概念，这使得并发问题变得更简单。纯函数
   允许并⾏运⾏，因此，当程序在不同的线程上执⾏时，它更容易保持同步。
6. 代码复⽤：由于函数是基本构建块，并且可以组合成更⾼级别的功能块，使⽤函数式编程可以更⼤
   程度上推崇代码复⽤，减少代码冗余。
   函数式编程通过强调纯函数、不可变数据结构和函数组合等概念，可以提⾼代码可读性和可维护性，
   降低程序 bug 出现的⻛险，更容易测试，并且更容易将问题分解为更容易处理的⼩部分，更好地应对并
   发和可扩展性。

## 前端⽔印了解多少？【热度: 641】

#### 明⽔印和暗⽔印的区别

前端⽔印可以分为明⽔印和暗⽔印两种类型。它们的区别如下：

1. 明⽔印：明⽔印是通过在⽂本或图像上覆盖另⼀层图像或⽂字来实现的。这种⽔印会明显地出现在
   ⻚⾯上，可以⽤来显⽰版权信息或其他相关信息。
2. 暗⽔印：暗⽔印是指在⽂本或图像中隐藏相关信息的⼀种技术。这种⽔印不会直接出现在⻚⾯上，
   只有在特殊的程序或⼯具下才能被检测到。暗⽔印通常⽤于保护敏感信息以及追踪⽹⻚内容的来源
   和版本。

添加明⽔印⼿段有哪些
可以参考这个⽂档： https://zhuanlan.zhihu.com/p/374734095
总计⼀下：

1. 重复的 dom 元素覆盖实现： 在⻚⾯上覆盖⼀个 position:fixed 的 div 盒⼦，盒⼦透明度设置较低，设
   置 pointer-events: none;样式实现点击穿透，在这个盒⼦内通过 js 循环⽣成⼩的⽔印 div，每个⽔印
   div 内展⽰⼀个要显⽰的⽔印内容
2. canvas 输出背景图： 绘制出⼀个⽔印区域，将这个⽔印通过 toDataURL ⽅法输出为⼀个图⽚，将
   这个图⽚设置为盒⼦的背景图，通过 backgroud-repeat：repeat；样式实现填满整个屏幕的效果。
3. svg 实现背景图： 与 canvas ⽣成背景图的⽅法类似，只不过是⽣成背景图的⽅法换成了通过 svg ⽣
   成
4. 图⽚加⽔印
   css 添加⽔印的⽅式， 如何防⽌⽤⼾删除对应的 css ， 从⽽达到去除⽔印的⽬的
   使⽤ CSS 添加⽔印的⽅式本⾝并不能完全防⽌⽤⼾删除对应的 CSS 样式，从⽽删除⽔印。但是，可以
   采取⼀些措施来增加删除难度，提⾼⽔印的防伪能⼒。以下是⼀些常⻅的⽅法：
5. 调⽤外部 CSS ⽂件：将⽔印样式单独设置在⼀个 CSS ⽂件内，并通过外链的⽅式在⽹站中调⽤，可
   以避免⽤⼾通过编辑⻚⾯ HTML ⽂件或内嵌样式表的⽅式删除⽔印。
6. 设置样式为 !important：在 CSS 样式中使⽤ !important 标记可以避免被覆盖。但是，这种⽅式会
   影响⽹⻚的可读性，需慎重考虑。
7. 添加⾃定义类名：通过在 CSS 样式中加⼊⾃定义的 class 类名，可以防⽌⽤⼾直接删掉该类名，进⽽
   删除⽔印。但是，⽤⼾也可以通过重新定义该类名样式来替换⽔印。
8. 将⽔印样式应⽤到多个元素上：将⽔印样式应⽤到多个元素上，可以使得⽤⼾删除⽔印较为困难。
   例如，在⽹站的多个位置都加上"Power by XXX"的⽔印样式。
9. 使⽤ JavaScript 动态⽣成 CSS 样式：可以监听挂载⽔印样式的 dom 节点， 如果⽤⼾改变了该 dom
   , 重新⽣成 对应的⽔印挂载上去即可。 这种⽅法可通过 JS 动态⽣成 CSS 样式，从⽽避免⽤⼾直接在
   ⽹⻚源⽂件中删除 CSS 代码。但需要注意的是，这种⽅案会稍稍加重⽹⻚的加载速度，需要合理权
   衡。
10. 混淆 CSS 代码：通过多次重复使⽤同⼀样式，或者采⽤ CSS 压缩等混淆⼿段，可以使 CSS 样式表变得
    复杂难懂，增加⽔印被删除的难度。
11. 采⽤图⽚⽔印的⽅式：将⽔印转化为⼀个透明的 PNG 图⽚，然后将其作为⽹⻚的背景图⽚，可以更
    有效地防⽌⽔印被删除。
12. 使⽤ SVG 图形：可以将⽔印作为 SVG 图形嵌⼊到⽹⻚中进⾏展⽰。由于 SVG 的⽮量性质，这种⽅式
    可以保证⽔印在缩放或旋转后的清晰度，同时也增加了删除难度。
    暗⽔印是如何把⽔印信息隐藏起来的
    暗⽔印的基本原理是在原始数据（如⽂本、图像等）中嵌⼊信息，从⽽实现版权保护和溯源追踪等功
    能。暗⽔印把信息隐藏在源数据中，使得⼈眼难以察觉，同时对源数据的影响尽可能⼩，保持其⾃⾝
    的特征。
    ⼀般来说，暗⽔印算法主要包括以下⼏个步骤：
13. ⽔印信息处理：将待嵌⼊的信息经过处理和加密后，转化为⼆进制数据。
14. 源数据处理：遍历源数据中的像素或⼆进制数据，根据特定规则对其进⾏调整，以此腾出空间插⼊
    ⽔印⼆进制数据。
15. 嵌⼊⽔印：将⽔印⼆进制数据插⼊到源数据中的指定位置，以某种⽅式嵌⼊到源数据之中。
16. 提取⽔印：在使⽤暗⽔印的过程中，需要从带⽔印的数据中提取出隐藏的⽔印信息。提取⽔印需要
    使⽤特定的解密算法和提取密钥。
    暗⽔印的⼀个关键问题是在嵌⼊⽔印的过程中，要保证⽔印对源数据的伤害尽可能的⼩，同时嵌⼊⽔
    印后数据的分布、统计性质等不应发⽣明显变化，以更好地保持数据的质量和可视效果。

## 什么是领域模型【热度: 1,092】

#### 什么是领域模型

领域模型是软件开发中⽤于描述领域（业务）概念和规则的⼀种建模技术。它通过定义实体、值对
象、关联关系、⾏为等元素，抽象出领域的核⼼概念和业务规则，帮助开发⼈员理解和设计软件系
统。
以下是领域模型中常⻅的⼀些元素：

1. 实体（Entity）：实体是领域模型中具有唯⼀标识的对象，通常代表领域中的具体事物或业务对
   象。实体具有属性和⾏为，并且可以通过其标识进⾏唯⼀标识和识别。
2. 值对象（Value Object）：值对象是没有唯⼀标识的对象，通常⽤于表⽰没有明确⽣命周期的属性
   集合。值对象的相等性通常基于其属性值，⽽不是标识。例如，⽇期、时间、货币等都可以作为值
   对象。
3. 关联关系（Association）：关联关系描述了不同实体之间的关系和连接。关联关系可以是⼀对
   ⼀、⼀对多、多对多等不同类型。关联关系可以带有⽅向和导航属性，⽤于表⽰实体之间的关联和
   导航。
4. 聚合（Aggregation）：聚合是⼀种特殊的关联关系，表⽰包含关系，即⼀个实体包含其他实体。
   聚合关系是⼀种强关联，被包含实体的⽣命周期受到包含实体的控制。
5. 领域事件（Domain Event）：领域事件表⽰领域中发⽣的具体事件或状态变化。它可以作为触发
   业务逻辑的信号，通常⽤于解耦和处理领域中的复杂业务流程。
6. 聚合根（Aggregate Root）：聚合根是聚合中的根实体，它代表整个聚合的⼀致性边界。通过聚合
   根，可以对整个聚合进⾏操作和维护。
7. 领域服务（Domain Service）：领域服务是⼀种封装了领域逻辑的服务，⽤于处理领域中的复杂业
   务操作或跨实体的操作。它通常与具体实体⽆关，提供⼀些⽆状态的操作。
   通过建⽴领域模型，开发⼈员可以更好地理解和表达领域的业务需求和规则，从⽽指导软件系统的设
   计和实现。领域模型可以作为开发团队之间沟通的⼯具，也可以⽤于⽣成代码、进⾏⾃动化测试等。

#### 前端系统应该如何划分领域模型

在前端系统中划分领域模型的⽅式可以根据具体业务需求和系统复杂性进⾏灵活调整。以下是⼀些常
⻅的划分领域模型的⽅式：

1. 模块划分：将前端系统按照模块进⾏划分，每个模块对应⼀个领域模型。模块可以根据功能、业务
   领域或者⻚⾯进⾏划分。每个模块可以有⾃⼰的实体、值对象、关联关系和业务逻辑。
2. ⻚⾯划分：将前端系统按照⻚⾯进⾏划分，每个⻚⾯对应⼀个领域模型。每个⻚⾯可以有⾃⼰的实
   体、值对象和关联关系，以及与⻚⾯相关的业务逻辑。
3. 组件划分：将前端系统按照组件进⾏划分，每个组件对应⼀个领域模型。每个组件可以有⾃⼰的实
   体、值对象和关联关系，以及与组件相关的业务逻辑。组件可以是⻚⾯级别的，也可以是更细粒度
   的功能组件。
4. 功能划分：将前端系统按照功能进⾏划分，每个功能对应⼀个领域模型。功能可以是⽤⼾操作的具
   体功能模块，例如登录、注册、购物⻋等。每个功能可以有⾃⼰的实体、值对象和关联关系，以及
   与功能相关的业务逻辑。
   在划分领域模型时，需要根据具体业务的复杂性和团队的组织⽅式进⾏调整。重要的是识别系统中的
   核⼼业务概念和规则，并将其抽象成适当的实体和值对象。同时，要保持领域模型的聚合性和⼀致
   性，避免出现过于庞⼤和紧耦合的领域模型。划分的领域模型应该易于理解、扩展和维护，以⽀持前
   端系统的开发和演进。

## ⼀直在 window 上⾯挂东西是否有什么⻛险

在前端开发中，将内容或应⽤程序运⾏在浏览器的全局 window 对象上可能会带来⼀些潜在的⻛险。
以下是⼀些需要注意的⻛险：

1. 命名冲突： window 对象是浏览器的全局对象，它包含许多内置属性和⽅法。如果您在全局命名
   空间中定义的变量或函数与现有的全局对象属性或⽅法发⽣冲突，可能会导致意外⾏为或错误。
2. 安全漏洞：在全局 window 对象上挂载的代码可以访问和修改全局的数据和功能。这可能导致安
   全漏洞，特别是当这些操作被恶意利⽤时。攻击者可能通过篡改全局对象来窃取⽤⼾敏感信息或执
   ⾏恶意代码。
3. 代码维护性：过多地依赖全局 window 对象可能导致代码的维护困难。全局状态的过度共享可能
   导致代码变得难以理解和调试，尤其在⼤型应⽤程序中。
   为了减轻这些⻛险，建议采⽤以下最佳实践：
4. 使⽤模块化开发：将代码模块化，避免对全局 window 对象的直接依赖。使⽤模块加载器（如 ES
   Modules、CommonJS、AMD）来管理模块之间的依赖关系，以减少全局命名冲突和代码冗余。
5. 使⽤严格模式：在 JavaScript 代码中使⽤严格模式（ "use strict" ），以启⽤更严格的语法检
   查和错误处理。严格模式可以帮助捕获潜在的错误和不安全的⾏为。
6. 显式访问全局对象：如果确实需要访问全局 window 对象的属性或⽅法，请使⽤显式访问⽅式，
   如 window.localStorage 、 window.setTimeout() 等。避免直接引⽤全局属性，以减少
   冲突和误⽤的⻛险。
7. 谨慎处理第三⽅代码：在使⽤第三⽅库或框架时，注意审查其对全局 window 对象的使⽤⽅式。
   确保库或框架的操作不会产⽣潜在的安全⻛险或全局命名冲突。
