不学会怎么处理对象，你在 JavaScript 道路就就走不了多远。它们几乎是 JavaScript 编程语言每个方面的基础。事实上，学习如何创建对象可能是你刚开始学习的第一件事。

对象是键/值对。创建对象的最常用方法是使用花括号{}，并使用点表示法向对象添加属性和方法。

```javascript
let animal = {}
animal.name = 'Leo'
animal.energy = 10

animal.eat = function (amount) {
  console.log(`${this.name} is eating.`)
  this.energy += amount
}

animal.sleep = function (length) {
  console.log(`${this.name} is sleeping.`)
  this.energy += length
}

animal.play = function (length) {
  console.log(`${this.name} is playing.`)
  this.energy -= length
}
```
现在，在我们的应用程序中，我们需要创建多个 animal。当然，下一步是将逻辑封装,当我们需要创建新 animal 时，只需调用函数即可，我们将这种模式称为函数的实例化(unctional Instantiation)，我们将函数本身称为“构造函数”，因为它负责“构造”一个新对象。

## 函数的实例化
```javascript
function Animal (name, energy) {
  let animal = {}
  animal.name = name
  animal.energy = energy

  animal.eat = function (amount) {
    console.log(`${this.name} is eating.`)
    this.energy += amount
  }

  animal.sleep = function (length) {
    console.log(`${this.name} is sleeping.`)
    this.energy += length
  }

  animal.play = function (length) {
    console.log(`${this.name} is playing.`)
    this.energy -= length
  }
  return animal
}
const leo = Animal('Leo', 7)
const snoop = Animal('Snoop', 10)
```
现在，无论何时我们想要创建一个新 animal(或者更广泛地说，创建一个新的“实例”)，我们所要做的就是调用我们的 Animal 函数，并传入参数：name 和 energy 。这很有用，而且非常简单。但是，你能说这种模式的哪些缺点吗?

最大的和我们试图解决的问题与函数里面的三个方法有关 - eat，sleep 和 play。这些方法中的每一种都不仅是动态的，而且它们也是完全通用的。这意味着，我们没有理由像现在一样，在创造新animal的时候重新创建这些方法。我们只是在浪费内存，让每一个新建的对象都比实际需要的还大。

你能想到一个解决方案吗？如果不是在每次创建新动物时重新创建这些方法，我们将它们移动到自己的对象然后我们可以让每个动物引用该对象，该怎么办？我们可以将此模式称为函数实例化与共享方法。

函数实例化与共享方法

```javascript
const animalMethods = {
  eat(amount) {
    console.log(`${this.name} is eating.`)
    this.energy += amount
  },
  sleep(length) {
    console.log(`${this.name} is sleeping.`)
    this.energy += length
  },
  play(length) {
    console.log(`${this.name} is playing.`)
    this.energy -= length
  }
}

function Animal (name, energy) {
  let animal = {}
  animal.name = name
  animal.energy = energy
  animal.eat = animalMethods.eat
  animal.sleep = animalMethods.sleep
  animal.play = animalMethods.play

  return animal
}

const leo = Animal('Leo', 7)
const snoop = Animal('Snoop', 10)
```
通过将共享方法移动到它们自己的对象并在 Animal 函数中引用该对象，我们现在已经解决了内存浪费和新对象体积过大的问题。

**Object.create**

```javascript
const parent = {
  name: 'Stacey',
  age: 35,
  heritage: 'Irish'
}

const child = Object.create(parent)
child.name = 'Ryan'
child.age = 7

console.log(child.name) // Ryan
console.log(child.age) // 7
console.log(child.heritage) // Irish
```
因此，在上面的示例中，由于 child 是用 object.create(parent) 创建的，所以每当child 对象上的属性查找失败时，JavaScript 就会将该查找委托给 parent 对象。这意味着即使 child 没有属性 heritage ，当你打印 child.heritage 时，它会从 parent 对象中找到对应 heritage 并打印出来。
现在如何使用 Object.create 来简化之前的 Animal代码？好吧，我们可以使用Object.create 来委托给animalMethods对象，而不是像我们现在一样逐一向 animal 添加所有共享方法。为了B 格一点，就叫做 使用共享方法 和 Object.create 的函数实例化。
**使用共享方法 和 Object.create 的函数实例化
**

```javascript
const animalMethods = {
  eat(amount) {
    console.log(`${this.name} is eating.`)
    this.energy += amount
  },
  sleep(length) {
    console.log(`${this.name} is sleeping.`)
    this.energy += length
  },
  play(length) {
    console.log(`${this.name} is playing.`)
    this.energy -= length
  }
}

function Animal (name, energy) {
  let animal = Object.create(animalMethods)
  animal.name = name
  animal.energy = energy

  return animal
}

const leo = Animal('Leo', 7)
const snoop = Animal('Snoop', 10)

leo.eat(10)
snoop.play(5)
```
所以现在当我们调用 leo.eat 时，JavaScript 将在 leo 对象上查找 eat 方法，因为leo 中没有 eat 方法，所以查找将失败，由于 Object.create，它将委托给animalMethods对象，所以会从 animalMethods 对象上找到 eat 方法。
到现在为止还挺好。尽管如此，我们仍然可以做出一些改进。为了跨实例共享方法，必须管理一个单独的对象（animalMethods）似乎有点“傻哈”。我们希望这在语言本身中实现的一个常见特，所以就需要引出下一个属性 - prototype。

那么究竟 JavaScript 中的 prototype 是什么？好吧，简单地说，JavaScript 中的每个函数都有一个引用对象的prototype属性。

```javascript
function doThing () {}
console.log(doThing.prototype) // {}
```
**
原型(prototype)实例化
**

```javascript
function Animal (name, energy) {
  let animal = Object.create(Animal.prototype)
  animal.name = name
  animal.energy = energy

  return animal
}

Animal.prototype.eat = function (amount) {
  console.log(`${this.name} is eating.`)
  this.energy += amount
}

Animal.prototype.sleep = function (length) {
  console.log(`${this.name} is sleeping.`)
  this.energy += length
}

Animal.prototype.play = function (length) {
  console.log(`${this.name} is playing.`)
  this.energy -= length
}

const leo = Animal('Leo', 7)
const snoop = Animal('Snoop', 10)

leo.eat(10)
snoop.play(5)
```
同样，prototype 只是 JavaScript 中的每个函数都具有的一个属性，正如我们前面看到的，它允许我们跨函数的所有实例共享方法。我们所有的功能仍然是相同的，但是现在我们不必为所有的方法管理一个单独的对象，我们只需要使用 Animal 函数本身内置的另一个对象Animal.prototype。
**更进一步
**
现在我们知道三个点：
1如何创建构造函数。

2如何向构造函数的原型添加方法。

3如何使用 Object.create 将失败的查找委托给函数的原型。

这三个点对于任何编程语言来说都是非常基础的。JavaScript 真的有那么糟糕，以至于没有更简单的方法来完成同样的事情吗?正如你可能已经猜到的那样，现在已经有了，它是通过使用new关键字来实现的。

回顾一下我们的 Animal 构造函数，最重要的两个部分是创建对象并返回它。如果不使用Object.create创建对象，我们将无法在失败的查找上委托函数的原型。如果没有return语句，我们将永远不会返回创建的对象。

```javascript
function Animal (name, energy) {
  let animal = Object.create(Animal.prototype) // 1 
  animal.name = name
  animal.energy = energy

  return animal   // 2
}
```
关于 new，有一件很酷的事情——当你使用new关键字调用一个函数时，以下编号为1和2两行代码将隐式地(在底层)为你完成，所创建的对象被称为this。

使用注释来显示底层发生了什么，并假设用new关键字调用了Animal构造函数，可以这样重写它。

```javascript
function Animal (name, energy) {
  // const this = Object.create(Animal.prototype)

  this.name = name
  this.energy = energy
  // return this
}
const leo = new Animal('Leo', 7)
const snoop = new Animal('Snoop', 10)
```
正常如下：

```javascript
function Animal (name, energy) {
  this.name = name
  this.energy = energy
}

Animal.prototype.eat = function (amount) {
  console.log(`${this.name} is eating.`)
  this.energy += amount
}

Animal.prototype.sleep = function (length) {
  console.log(`${this.name} is sleeping.`)
  this.energy += length
}

Animal.prototype.play = function (length) {
  console.log(`${this.name} is playing.`)
  this.energy -= length
}

const leo = new Animal('Leo', 7)
const snoop = new Animal('Snoop', 10)
```
再次说明，之所以这样做，并且这个对象是为我们创建的，是因为我们用new关键字调用了构造函数。如果在调用函数时省略new，则永远不会创建该对象，也不会隐式地返回该对象。我们可以在下面的例子中看到这个问题。

```javascript
function Animal (name, energy) {
  this.name = name
  this.energy = energy
}

const leo = Animal('Leo', 7)
console.log(leo) // undefined
```
这种模式称为 伪类实例化。

对于那些不熟悉的人，类允许你为对象创建蓝图。然后，每当你创建该类的实例时，你可以访问这个对象中定义的属性和方法。

听起来有点熟？这基本上就是我们对上面的 Animal 构造函数所做的。但是，我们只使用常规的旧 JavaScript 函数来重新创建相同的功能，而不是使用class关键字。当然，它需要一些额外的工作以及了解一些 JavaScript “底层” 发生的事情，但结果是一样的。

这是个好消息。JavaScript 不是一种死语言。TC-39委员会不断改进和补充。这意味着即使JavaScript的初始版本不支持类，也没有理由将它们添加到官方规范中。事实上，这正是TC-39委员会所做的。2015 年，发布了EcmaScript（官方JavaScript规范）6，支持类和class关键字。让我们看看上面的Animal构造函数如何使用新的类语法。

```javascript
class Animal {
  constructor(name, energy) {
    this.name = name
    this.energy = energy
  }
  eat(amount) {
    console.log(`${this.name} is eating.`)
    this.energy += amount
  }
  sleep(length) {
    console.log(`${this.name} is sleeping.`)
    this.energy += length
  }
  play(length) {
    console.log(`${this.name} is playing.`)
    this.energy -= length
  }
}

const leo = new Animal('Leo', 7)
const snoop = new Animal('Snoop', 10)
```
这个相对前面的例子，是相对简单明了的。

因此，如果这是创建类的新方法，为什么我们花了这么多时间来复习旧的方式呢？原因是因为新方法（使用class关键字）主要只是我们称之为伪类实例化模式现有方式的“语法糖”。为了完全理解 ES6 类的便捷语法，首先必须理解伪类实例化模式。

**数组方法**
我们在上面深入讨论了如何在一个类的实例之间共享方法，你应该将这些方法放在类（或函数）原型上。如果我们查看Array类，我们可以看到相同的模式。

```javascript
const friends = []
```
以为是代替使用 new Array() 的一个语法糖。
```javascript
const friendsWithSugar = []
const friendsWithoutSugar = new Array()
```
你可能从未想过的一件事是，数组的每个实例如何具有所有内置方法 (splice, slice, pop 等)？

正如你现在所知，这是因为这些方法存在于 Array.prototype 上，当你创建新的Array实例时，你使用new关键字在失败的查找中将该委托设置为 Array.prototype。

我们可以打印 Array.prototype 来查看有哪些方法：

```javascript
console.log(Array.prototype)

/*
  concat: ƒn concat()
  constructor: ƒn Array()
  copyWithin: ƒn copyWithin()
  entries: ƒn entries()
  every: ƒn every()
  fill: ƒn fill()
  filter: ƒn filter()
  find: ƒn find()
  findIndex: ƒn findIndex()
  forEach: ƒn forEach()
  includes: ƒn includes()
  indexOf: ƒn indexOf()
  join: ƒn join()
  keys: ƒn keys()
  lastIndexOf: ƒn lastIndexOf()
  length: 0n
  map: ƒn map()
  pop: ƒn pop()
  push: ƒn push()
  reduce: ƒn reduce()
  reduceRight: ƒn reduceRight()
  reverse: ƒn reverse()
  shift: ƒn shift()
  slice: ƒn slice()
  some: ƒn some()
  sort: ƒn sort()
  splice: ƒn splice()
  toLocaleString: ƒn toLocaleString()
  toString: ƒn toString()
  unshift: ƒn unshift()
  values: ƒn values()
*/
```
对象也存在完全相同的逻辑。所有的对象将在失败的查找后委托给 Object.prototype，这就是所有对象都有 toString 和 hasOwnProperty 等方法的原因
**静态方法
**
到目前为止，我们已经讨论了为什么以及如何在类的实例之间共享方法。但是，如果我们有一个对类很重要的方法，但是不需要在实例之间共享该方法怎么办?例如，如果我们有一个函数，它接收一系列 Animal 实例，并确定下一步需要喂养哪一个呢?我们这个方法叫做 nextToEat。

```javascript
function nextToEat (animals) {
  const sortedByLeastEnergy = animals.sort((a,b) => {
    return a.energy - b.energy
  })

  return sortedByLeastEnergy[0].name
}
```
因为我们不希望在所有实例之间共享 nextToEat，所以在 Animal.prototype上使用nextToEat 是没有意义的。相反，我们可以将其视为辅助方法。
所以如果nextToEat不应该存在于Animal.prototype中，我们应该把它放在哪里？显而易见的答案是我们可以将nextToEat放在与我们的Animal类相同的范围内，然后像我们通常那样在需要时引用它。

```javascript
class Animal {
  constructor(name, energy) {
    this.name = name
    this.energy = energy
  }
  eat(amount) {
    console.log(`${this.name} is eating.`)
    this.energy += amount
  }
  sleep(length) {
    console.log(`${this.name} is sleeping.`)
    this.energy += length
  }
  play(length) {
    console.log(`${this.name} is playing.`)
    this.energy -= length
  }
}

function nextToEat (animals) {
  const sortedByLeastEnergy = animals.sort((a,b) => {
    return a.energy - b.energy
  })

  return sortedByLeastEnergy[0].name
}

const leo = new Animal('Leo', 7)
const snoop = new Animal('Snoop', 10)

console.log(nextToEat([leo, snoop])) // Leo
```
这是可行的，但是还有一个更好的方法。
**只要有一个特定于类本身的方法，但不需要在该类的实例之间共享，就可以将其定义为类的静态属性。
**

```javascript
class Animal {
  constructor(name, energy) {
    this.name = name
    this.energy = energy
  }
  eat(amount) {
    console.log(`${this.name} is eating.`)
    this.energy += amount
  }
  sleep(length) {
    console.log(`${this.name} is sleeping.`)
    this.energy += length
  }
  play(length) {
    console.log(`${this.name} is playing.`)
    this.energy -= length
  }
  static nextToEat(animals) {
    const sortedByLeastEnergy = animals.sort((a,b) => {
      return a.energy - b.energy
    })

    return sortedByLeastEnergy[0].name
  }
}
```
现在，因为我们在类上添加了nextToEat作为静态属性，所以它存在于Animal类本身（而不是它的原型）上，并且可以使用Animal.nextToEat进行调用 。

```javascript
const leo = new Animal('Leo', 7)
const snoop = new Animal('Snoop', 10)
console.log(Animal.nextToEat([leo, snoop])) // Leo
```
因为我们在这篇文章中都遵循了类似的模式，让我们来看看如何使用 ES5 完成同样的事情。在上面的例子中，我们看到了如何使用 static 关键字将方法直接放在类本身上。使用 ES5，同样的模式就像手动将方法添加到函数对象一样简单。

```javascript
function Animal (name, energy) {
  this.name = name
  this.energy = energy
}

Animal.prototype.eat = function (amount) {
  console.log(`${this.name} is eating.`)
  this.energy += amount
}

Animal.prototype.sleep = function (length) {
  console.log(`${this.name} is sleeping.`)
  this.energy += length
}

Animal.prototype.play = function (length) {
  console.log(`${this.name} is playing.`)
  this.energy -= length
}

Animal.nextToEat = function (nextToEat) {
  const sortedByLeastEnergy = animals.sort((a,b) => {
    return a.energy - b.energy
  })

  return sortedByLeastEnergy[0].name
}

const leo = new Animal('Leo', 7)
const snoop = new Animal('Snoop', 10)

console.log(Animal.nextToEat([leo, snoop])) // Leo
```

**获取对象的原型**
无论您使用哪种模式创建对象，都可以使用Object.getPrototypeOf方法完成获取该对象的原型。

```javascript
function Animal (name, energy) {
  this.name = name
  this.energy = energy
}
Animal.prototype.eat = function (amount) {
  console.log(`${this.name} is eating.`)
  this.energy += amount
}
Animal.prototype.sleep = function (length) {
  console.log(`${this.name} is sleeping.`)
  this.energy += length
}
Animal.prototype.play = function (length) {
  console.log(`${this.name} is playing.`)
  this.energy -= length
}
const leo = new Animal('Leo', 7)
const proto  = Object.getPrototypeOf(leo)
console.log(proto )
// {constructor: ƒ, eat: ƒ, sleep: ƒ, play: ƒ}
proto === Animal.prototype // true
```
上面的代码有两个重要的要点。

首先，你将注意到 proto 是一个具有 4 个方法的对象，constructor、eat、sleep和play。这是有意义的。我们使用getPrototypeOf传递实例，leo取回实例原型，这是我们所有方法的所在。

这也告诉了我们关于 prototype 的另一件事，我们还没有讨论过。默认情况下，prototype对象将具有一个 constructor 属性，该属性指向初始函数或创建实例的类。这也意味着因为 JavaScript 默认在原型上放置构造函数属性，所以任何实例都可以通过。

第二个重要的点是： Object.getPrototypeOf(leo) === Animal.prototype。这也是有道理的。 Animal 构造函数有一个prototype属性，我们可以在所有实例之间共享方法，getPrototypeOf 允许我们查看实例本身的原型。

```javascript
function Animal (name, energy) {
  this.name = name
  this.energy = energy
}
const leo = new Animal('Leo', 7)
console.log(leo.constructor) // Logs the constructor function
```
为了配合我们之前使用 Object.create 所讨论的内容，其工作原理是因为任何Animal实例都会在失败的查找中委托给Animal.prototype。因此，当你尝试访问leo.constructor时，leo没有 constructor 属性，因此它会将该查找委托给 Animal.prototype，而Animal.prototype 确实具有构造函数属性。

你之前可能看过使用** __proto__** 用于获取实例的原型，这是过去的遗物。相反，如上所述使用 Object.getPrototypeOf(instance)

**判断原型上是否包含某个属性
**
在某些情况下，你需要知道属性是否存在于实例本身上，还是存在于对象委托的原型上。我们可以通过循环打印我们创建的leo对象来看到这一点。使用for in 循环方式如下：

```javascript
function Animal (name, energy) {
  this.name = name
  this.energy = energy
}

Animal.prototype.eat = function (amount) {
  console.log(`${this.name} is eating.`)
  this.energy += amount
}

Animal.prototype.sleep = function (length) {
  console.log(`${this.name} is sleeping.`)
  this.energy += length
}

Animal.prototype.play = function (length) {
  console.log(`${this.name} is playing.`)
  this.energy -= length
}

const leo = new Animal('Leo', 7)

for(let key in leo) {
  console.log(`Key: ${key}. Value: ${leo[key]}`)
}
```
我所期望的打印结果可能如下：

```javascript
Key: name. Value: Leo
Key: energy. Value: 7
```
然而，如果你运行代码，看到的是这样的-

```javascript
Key: name. Value: Leo
Key: energy. Value: 7
Key: eat. Value: function (amount) {
  console.log(`${this.name} is eating.`)
  this.energy += amount
}
Key: sleep. Value: function (length) {
  console.log(`${this.name} is sleeping.`)
  this.energy += length
}
Key: play. Value: function (length) {
  console.log(`${this.name} is playing.`)
  this.energy -= length
}
```
这是为什么？对于for in循环来说，循环将遍历对象本身以及它所委托的原型的所有可枚举属性。因为默认情况下，你添加到函数原型的任何属性都是可枚举的，我们不仅会看到name 和energy，还会看到原型上的所有方法 -eat，sleep，play。

要解决这个问题，我们需要指定所有原型方法都是不可枚举的，或者只打印属性位于 leo 对象本身而不是leo委托给失败查找的原型。这是 hasOwnProperty 可以帮助我们的地方。

```javascript
...

const leo = new Animal('Leo', 7)

for(let key in leo) {
  if (leo.hasOwnProperty(key)) {
    console.log(`Key: ${key}. Value: ${leo[key]}`)
  }
}

```
现在我们看到的只是leo对象本身的属性，而不是leo委托的原型。

```javascript
Key: name. Value: Leo
Key: energy. Value: 7
```
如果你仍然对 hasOwnProperty 感到困惑，这里有一些代码可以帮你更好的理清它。

```javascript
function Animal (name, energy) {
  this.name = name
  this.energy = energy
}

Animal.prototype.eat = function (amount) {
  console.log(`${this.name} is eating.`)
  this.energy += amount
}

Animal.prototype.sleep = function (length) {
  console.log(`${this.name} is sleeping.`)
  this.energy += length
}

Animal.prototype.play = function (length) {
  console.log(`${this.name} is playing.`)
  this.energy -= length
}

const leo = new Animal('Leo', 7)

leo.hasOwnProperty('name') // true
leo.hasOwnProperty('energy') // true
leo.hasOwnProperty('eat') // false
leo.hasOwnProperty('sleep') // false
leo.hasOwnProperty('play') // false
```

**检查对象是否是类的实例
**
有时你想知道对象是否是特定类的实例。为此，你可以使用instanceof运算符。用例非常简单，但如果你以前从未见过它，实际的语法有点奇怪。它的工作方式如下

```javascript
object instanceof Class
```
如果 object 是Class的实例，则上面的语句将返回 true，否则返回 false。回到我们的 Animal 示例，我们有类似的东西：

```javascript
function Animal (name, energy) {
  this.name = name
  this.energy = energy
}

function User () {}

const leo = new Animal('Leo', 7)

leo instanceof Animal // true
leo instanceof User // false
```
 instanceof 的工作方式是检查对象原型链中是否存在 constructor.prototype。在上面的例子中，leo instanceof Animal 为 true，因为 Object.getPrototypeOf(leo) === Animal.prototype。另外，leo instanceof User 为 false，因为Object.getPrototypeOf(leo) !== User.prototype。

**创建新的不可知的构造函数**
你能找出下面代码中的错误吗

```javascript
function Animal (name, energy) {
  this.name = name
  this.energy = energy
}
const leo = Animal('Leo', 7)
```
即使是经验丰富的 JavaScript 开发人员有时也会被上面的例子绊倒。因为我们使用的是前面学过的伪类实例模式，所以在调用Animal构造函数时，需要确保使用new关键字调用它。如果我们不这样做，那么 this 关键字就不会被创建，它也不会隐式地返回。

作为复习，注释掉的行是在函数上使用new关键字时背后发生的事情。

```javascript
unction Animal (name, energy) {
  // const this = Object.create(Animal.prototype)
  this.name = name
  this.energy = energy
  // return this
}
```
让其他开发人员记住，这似乎是一个非常重要的细节。假设我们正在与其他开发人员合作，我们是否有办法确保始终使用new关键字调用我们的Animal构造函数？事实证明，可以通过使用我们之前学到的instanceof运算符来实现的。

如果使用new关键字调用构造函数，那么构造函数体的内部 this 将是构造函数本身的实例。

```javascript
function Aniam (name, energy) {
  if (this instanceof Animal === false) {
     console.warn('Forgot to call Animal with the new keyword')
  }
  this.name = name
  this.energy = energy
}
```
现在，如果我们重新调用函数，但是这次使用 new 的关键字，而不是仅仅向函数的调用者打印一个警告呢？
```javascript
function Animal (name, energy) {
  if (this instanceof Animal === false) {
    return new Animal(name, energy)
  }
  this.name = name
  this.energy = energy
}
```
现在，不管是否使用new关键字调用Animal，它仍然可以正常工作。

**重写 Object.create
**
在这篇文章中，我们非常依赖于Object.create来创建委托给构造函数原型的对象。此时，你应该知道如何在代码中使用Object.create，但你可能没有想到的一件事是Object.create实际上是如何工作的。为了让你真正了解Object.create的工作原理，我们将自己重新创建它。首先，我们对Object.create的工作原理了解多少？
1 它接受一个对象的参数。

2 它创建一个对象，在查找失败时委托给参数对象

3 它返回新创建的对象。
Object.create = function (objToDelegateTo) {

}
**现在，我们需要创建一个对象，该对象将在失败的查找中委托给参数对象。这个有点棘手。为此，我们将使用 new 关键字相关的知识。
**
首先，在 Object.create 主体内部创建一个空函数。然后，将空函数的 prototype 设置为等于传入参数对象。然后，返回使用new关键字调用我们的空函数。

```javascript
Object.create = function (objToDelegateTo) {
  function Fn(){}
  Fn.prototype = objToDelegateTo
  return new Fn()
}
```
当我们在上面的代码中创建一个新函数Fn时，它带有一个prototype属性。当我们使用new关键字调用它时，我们知道我们将得到的是一个将在失败的查找中委托给函数原型的对象。

如果我们覆盖函数的原型，那么我们可以决定在失败的查找中委托哪个对象。所以在上面的例子中，我们用调用Object.create时传入的对象覆盖Fn的原型，我们称之为objToDelegateTo。

请注意，我们只支持 Object.create 的单个参数。官方实现还支持第二个可选参数，该参数允许你向创建的对象添加更多属性。

**箭头函数
**
箭头函数没有自己的this关键字。因此，箭头函数不能是构造函数，如果你尝试使用new关键字调用箭头函数，它将引发错误。

```javascript
const Animal = () => {}
const leo = new Animal() // Error: Animal is not a constructor
```
另外，因为我们在上面说明了伪类实例模式不能与箭头函数一起使用，所以箭头函数也没有原型属性。

```javascript
const Animal = () => {}
console.log(Animal.prototype) // undefine
```
