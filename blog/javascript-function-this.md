这些年很多人提出了对Javascript函数调用的疑惑，特别是很多人抱怨函数调用中'this'的语义不易理解。

实际上只要理解了原生(primitive)的核心函数调用,就可以扫除这些疑惑，而且其他函数调用方式都是在这个原生定义基础上进行了包装(sugar)。事实上，ECMAScript标准也是这样考虑的。尽管在某些地方，本文是该标准的简化版本，但是基本思想是一样的。 <!--more-->

#### 原生的核心函数调用

先看看原生的核心函数调用，即call方法[1][1]。

1.  参数列表（argList）列出了函数的所有参数
2.  call第一个参数是thisValue
3.  函数调用时，将thisValue赋值给this，argList作为参数列表

例如：

    function hello(thing) {
      console.log(this + " says hello " + thing);
    }

    hello.call("Yehuda", "world") //=> Yehuda says hello world


从代码中可以看到，调用hello函数时，将"Yehuda"赋给了this，且参数列表(argList)中仅包含唯一参数“world”。这就是原生的Javascript核心函数调用。其他的函数调用方式去除包装(desugar)后，最终都会归根于该原生函数调用。

*[1][2] ES5标准中，call 方法被描述成另一个更低水平的原生函数调用，但是它是针对那个原生函数的很简陋的封装(thin warrper)，因此在这里我简化了一点，详情见文章末尾。*

#### 简单的函数调用

当然如果每次都使用call进行函数调用，将会带来诸多不方便。Javascript可以直接使用()语法执行函数调用(*hello("world")*)，具体调用情况如下：

    function hello(thing) {
      console.log("Hello " + thing);
    }

    // this:
    hello("world")

    // call调用方式:
    hello.call(window, "world");


ECMAScript 5 标准中的**严格模式**[2][2]使用如下调用方法：

    // this:
    hello("world")

    // desugars to:
    hello.call(undefined, "world");


简化后的版本是这样的：**类似于fn(...args)的函数调用等价于 fn.call(window [ES5-strict: undefined], ...args)**。

值得注意的是将函数声明成内联模式是正确的做法，(function() {})() 等价于 (function() {}).call(window [ES5-strict: undefined)。

*[2][2]事实上，我有点说谎了。ECMAScript 5 标准是这样描述的：通常是传入undefined作为thisValue的，但是当非严格模式执行函数调用时，应该将thisValue重置为全局对象。这将确保严格模式下的调用者不会改变非严格模式下的类库。*

#### 成员函数

以对象成员的形式进行调用(person.hello())，是另一个非常常见的函数调用的方式。在这种情况下，该函数调用可以描述为：

    var person = {
      name: "Brendan Eich",
      hello: function(thing) {
          console.log(this + " says hello " + thing);
      }
    }

    // this:
    person.hello("world")

    // desugars to this:
    person.hello.call(person, "world");


值得注意的是，这种形式不管hello方法怎样依附到对象上都是可行的。别忘了我们先前定义的hello方法是单独的函数，让我们看看将hello方法动态赋给对象的情况：

    function hello(thing) {
      console.log(this + " says hello " + thing);
    }

    person = { name: "Brendan Eich" }
    person.hello = hello;

    person.hello("world") // still desugars to person.hello.call(person, "world")

    hello("world") // "[object DOMWindow]world"


需要记住的是function中的this是没有固定值的，它经常在函数调用时被赋值，这取决于调用者调用的方式。

#### 使用 Function.prototype.bind

有时候使用固定的this值来引用函数会很方便，因此人们过去(historically)就使用闭包来确保函数中的this值保持不变。

    var person = {
      name: "Brendan Eich",
      hello: function(thing) {
          console.log(this.name + " says hello " + thing);
      }
    }

    var boundHello = function(thing) { return person.hello.call(person, thing); }

    boundHello("world");


尽管boundHello函数调用仍然会转换成boundHello.call(window, "world")，但是我们转了一圈后又绕回来，使用原生的call方法来将this改变为我们想要的值。

通过细微的调整，可以使得这个伎俩更加通用。

    var bind = function(func, thisValue) {
      return function() {
        return func.apply(thisValue, arguments);
      }
    }

    var boundHello = bind(person.hello, person);
    boundHello("world") // "Brendan Eich says hello world"


为了理解这种方法，还需另外两个知识点。第一，arguments是一个数组对象，描述了传给函数的所有参数。第二，apply方法工作原理和call原生方法一模一样，唯一的不同是apply能够接受一个数组对象参数而call只能一次列出所有的参数。

bind函数简单的返回一个新函数，当被调用时，新函数只调用传入的原始函数，将原始值赋给this，同时传入参数。

因为这也是一个有些常见的术语，所以ECScript 5标准引入一个新方法bind,专门为所有的Function对象实现该行为功能。

    var boundHello = person.hello.bind(person);
    boundHello("world") // "Brendan Eich says hello world"


当你需要传入一个原始函数用做回调函数时，这是最有用的。

    var person = {
      name: "Alex Russell",
      hello: function() { console.log(this.name + " says hello world"); }
    }

    $("#some-div").click(person.hello.bind(person));

    // when the div is clicked, "Alex Russell says hello world" is printed


当然这也是有些笨拙，而且TC39(下一代ECMAScript标准委员会)正继续为构建一个更加简洁、仍然向后兼容的解决方案而努力。

#### 关于jQuery

因为jQuery大量运用匿名回调函数，因此它在内部使用call方法后，将回调函数的this值设为更加有用的值。例如，在所有的事件处理程序中(除非特殊处理)，jQuery没有将window对象赋给this对象，而是在回调函数中调用*call*时，将触发事件处理的元素作为第一个参数传入。

这是相当有用的，因为在匿名回调函数中this的默认值不是特别有用，但是这会给Javascript初学者一个印象，就是一般而言this是一个怪异可变的概念，很难解释清楚。

如果掌握了将包装过的(sugary)函数调用转变为一个原生的函数调用desugared) func.call(thisValue, ...args)的基本规则，那么应该可以趟过Javascript this对象的这趟浑水。

![总结图][2]

原文链接：[Understanding JavaScript Function Invocation and “this”][3]

 [1]: 意思相对直白
 [2]: http://yehudakatz.com/wp-content/uploads/2011/08/this-table2.png
 [3]: http://yehudakatz.com/2011/08/11/understanding-javascript-function-invocation-and-this/
