# jQuery类库

jQuery如此强大和好用，关键得益于以下特性：

- [x] 丰富强大的语法（CSS选择器），用来查询文档元素
- [x] 高效的查询方法，用来找到与CSS选择器匹配的文档元素集
- [x] 一套有用的方法，用来操作选中的元素
- [x] 强大的函数式编程技巧，用来批量操作元素集，而不是每次只操作单个
- [x] 简洁的语言用法（链式调用），用来表示一系列顺序操作

## jQuery基础知识

jQuery类库定义了一个全局函数：`jQuery()`。该函数使用频繁，因此在类库中还给它定义了一个快捷别名：`$`。这是jQuery在全局命名空间中定义的唯一两个变量。

`jQuery()` 是工厂函数，不是构造函数，它返回一个新创建的对象，但并没有和new关键字一起使用。

“jQuery函数”：jQuery函数指定义在jQuery命名空间中的函数，比如 `$.each(a,fn);` 。jQuery函数也可称为“静态方法”。

“jQuery方法”：jQuery方法是由jQuery函数返回的jQuery对象的方法，比如 `$("a").each(fn);`。jQuery类库最重要的部分就是它定义的这些强大的方法。

`$()` 的返回值是一个jQuery对象。jQuery对象是类数组。jQuery对象还有三个挺有趣的属性。`selector` 属性是创建jQuery对象时的选择器字符串（如果有的话）。`context` 属性是上下文对象，是传递给 `$()` 方法的第二参数，如果没有传递的话，默认是Document对象。最后，所有jQuery对象都有一个名为jquery的属性，检测该属性是否存在可以简单快捷地将jQuery对象与其他类数组对象区分开来。

## jQuery()的4中调用方式

在jQuery类库中，最重要的方法是`jQuery()`方法（也就是 `$()` ）。它的功能很强大，有4种不同的调用方式。

第一种也是最常用的调用方式是传递CSS选择器（字符串）给 `$()` 方法。当通过这种方式调用时，`$()` 方法会返回当前文档中匹配该选择器的元素集。jQuery支持大部分CSS3选择器语法，还支持一些自己的扩展语法。还可以将一个元素或jQuery对象作为第二参数传递给 `$()` 方法，这时返回的是该特定元素或元素集的子元素中匹配选择器的部分。这第二参数是可选的，定义了元素查询的起始点，经常称为上下文（context）。

第二种调用方式是传递一个Element、Document或Window对象给 `$()` 方法。在这种情况下，`$()` 方法只须简单地将该Element、Document或Window对象封装成jQuery对象并返回。这样可以使得能用jQuery方法来操作这些元素而不用使用原生DOM方法。例如，在jQuery程序中，经常可以看见 `$(document)` 或 `$(this)`。jQuery对象可以表示文档中的多个元素，也可以传递一个元素数组给 `$()` 方法。在这种情况下，返回的jQuery对象表示该数组中的元素集。

第三种调用方式是传递HTML文本字符串给 `$()` 方法。在这种调用方式下，jQuery会根据传入的文本创建好HTML元素并封装为jQuery对象返回。jQuery不会将刚创建的元素自动插入文档中，可以使用jQuery方法来轻松地将元素插入想要的地方。注意：在这种调用方式下，不可传入纯文本，因为jQuery会把纯文本当成CSS选择器来解析。当使用这种调用风格时，传递给$()方法的字符串必须至少包含一个带有尖角括号的HTML标签。

通过第三种方式调用时，`$()` 接受可选的第二参数。可以传递Document对象来指定与所创建元素相关联的文档。（比如，当创建的元素需要插入iframe里时，需要显式指定该iframe的document对象。）第二参数还可以是object对象。此时，假设该对象的属性表示HTML属性的键/值对，这些属性将设置到所创建的对象上。当第二参数对象的属性名是"css"、"html"、"text"、"width"、"height"、"offset"、"val"或"data"，或者属性名是jQuery事件处理程序注册方法名时，jQuery将调用新创建元素上的同名方法，并传入属性值。
```js
var img=$(  "<img/>",//新建一个＜img＞元素
            {src:url,//具有HTML属性
 css:{borderWidth:5},//CSS样式
    click:handleClick//和事件处理程序
});
```
最后，第4种调用方式是传入一个函数给$()方法。此时，当文档加载完毕且DOM可操作时，传入的函数将被调用。
```js
jQuery(function(){//文档加载完毕时调用
  //所有jQuery代码放在这里
});
```

## 遍历元素的方法分析

想要遍历jQuery对象中的所有元素时，可以调用 `each()` 方法来代替for循环。each()方法有点类似ECMAScript 5(ES5)中的 `forEach()` 数组方法。它接受一个回调函数作为唯一参数，然后它对jQuery对象中的每一个元素（按照在文档中的顺序）调用回调函数。回调函数作为匹配元素的方法来调用，因此在回调函数里 `this` 关键字指代Element对象。`each()` 方法还会将索引值和该元素作为第一个和第二个参数传递给回调函数。注意：`this` 和第二参数都是原生文档元素，而不是jQuery对象；如果想使用jQuery方法来操作该元素，需要先用 `$()` 封装它。jQuery的 `each()` 方法和 `forEach()` 有一个显著区别：如果回调函数在任一个元素上返回false，遍历将在该元素后中止（这就像在普通循环中使用break关键字一样）。`each()` 返回调用自身的jQuery对象，因此它可以用于链式调用。

jQuery的map()方法和 `Array.prototype.map()` 方法很相近。它接受回调函数作为参数，并为jQuery对象中的每一个元素都调用回调函数，同时将回调函数的返回值收集起来，并将这些返回值封装成一个新的jQuery对象返回。`map()` 调用回调函数的方式和 `each()` 方法相同：元素作为this值和第二参数传入，元素的索引值作为第一参数入。如果回调函数返回null或undefined，该值将被忽略，在本次回调中不会有任何新元素添加到新的jQuery对象中。

除了 `each()` 和 `map()` 之外，jQuery的另一个方法是 `index()`。该方法接受一个元素作为参数，返回值是该元素在此jQuery对象中的索引值，如果找不到的话，则返回-1。

## 获取和设置HTML和CSS属性

|方法名称|一个字符串作为参数|两个字符串作为参数|一个对象作为参数|一个字符串和一个函数作为参数|
|-|-|-|-|-|
|attr()|获取HTML属性|设置HTML属性|批量设置HTML属性|调用该函数来计算需要设置的值|
|css()|获取CSS属性|设置CSS属性|批量设置CSS属性|调用该函数来计算需要设置的值|

## 常用方法概述

* 获取和设置HTML属性（attr()/removeAttr()）
* 获取和设置CSS属性（css()）
* 获取和设置CSS类名（addClass()/removeClass()/toggleClass()）
* 获取和设置HTML表单值（val()）
* 获取和设置元素内容（text()/html()）
* 获取和设置元素的位置（offset()/position()）offset()返回元素的绝对位置，用相对于文档的坐标来表示。而position()则返回相对于元素的offsetParent()的偏移量。
* 获取和设置元素数据（data()/removeData()）
* 复制元素（clone()）

## 获取和设置元素的宽高

|方法名称|方法作用|是否包含padding|是否包含border|
|-|-|-|-|
|width()|获取和设置元素的宽|否|否|
|height()|获取和设置元素的高|否|否|
|innerWidth()|获取和设置元素的宽|是:white_check_mark:|否|
|innerHeight()|获取和设置元素的高|是:white_check_mark:|否|
|outerWidth()|获取和设置元素的宽|是:white_check_mark:|是:white_check_mark:|
|outerHeight()|获取和设置元素的高|是:white_check_mark:|是:white_check_mark:|

## 修改和替换元素

|操作|$(target).method(content)|函数参数|$(content).method(target)|函数参数|
|-|-|-|-|-|
|在目标元素的结尾处插入内容|append()|能:white_check_mark:|appendTo()|否|
|在目标元素的起始处插入内容|prepend()|能:white_check_mark:|prependTo()|否|
|在目标元素的后面处插入内容|after()|能:white_check_mark:|insertAfter()|否|
|在目标元素的前面处插入内容|before()|能:white_check_mark:|insertBefore()|否|
|在目标元素替换为指定的内容|replaceWith()|能:white_check_mark:|relpaceAll()|否|

对于 `append()`、`prepend()` 和 `replaceWith()`，第二参数将是该元素当前内容的HTML字符串形式。对于 `before()` 和 `after()` ，该函数在调用时没有第二参数。

第二列中的方法返回调用自身的jQuery对象。该jQuery对象中的元素有可能有新内容或新兄弟节点，但这些元素自身并没有修改。第四列中的方法在插入的内容上调用，返回一个新的jQuery对象，表示插入操作后的新内容。特别注意，当内容被插入多个地方时，返回的jQuery对象将为每一个地方保留一个元素。

插入HTML文档的另一种类型涉及在一个或多个元素中包装新元素。jQuery定义了3个包装函数。`wrap()` 包装每一个选中元素。`wrapInner()` 包装每一个选中元素的内容。`wrapAll()` 则将选中元素作为一组来包装。这些方法通常传入一个新创建的包装元素或用来创新包装元素的HTML字符串。

```js
//用＜i＞元素包装所有＜h1＞元素
$("h1").wrap(document.createElement("i"));
//产生＜i＞＜h1＞...＜/h1＞＜/i＞
//包装所有＜h1＞元素的内容，使用字符串参数更简单
$("h1").wrapInner("<i/>");
//产生＜h1＞＜i＞...＜/i＞＜/h1＞
```
## 删除元素

|方法名|函数作用|是否会修改元素自身|是否会删除元素的数据和绑定的事件|
|-|-|-|-|
|empty()|删除选中元素的所有子节点|否|是:white_check_mark:|
|remove()|删除选中元素|是:white_check_mark:|是:white_check_mark:|
|detach()|删除选中元素|是:white_check_mark:|否|
|unwrap()|删除选中元素的父元素|否|是:white_check_mark:|

## 事件处理程序

`focus` 和 `blur` 事件不支持冒泡，但 `focusin` 和 `focusout` 事件支持。

`mouseover` 和 `mouseout` 事件支持冒泡，`mouseenter` 和 `mouseleave` 是非冒泡事件。

`resize` 和 `unload` 事件类型只在 `Window` 对象中触发。

`scroll()` 方法经常也用于 `$(window)` 对象上，但它也可以用在有滚动条的任何元素上（比如，当CSS的overflow属性设置为"scroll"或"auto"时）。

`hover()` 方法用来给 `mouseenter` 和 `mouseleave` 事件注册处理程序。调用 `hover(f,g)` 就和调用 `mouseenter(f)` 然后调用 `mouseleave(g)` 一样。如果仅传入一个参数给 `hover()`，该参数函数会同时用做 `enter` 和 `leave` 事件的处理程序。

另一个特殊的事件注册方法是 `toggle()`。该方法将事件处理程序函数绑定到单击事件。可指定两个或多个处理程序函数，当单击事件发生时，jQuery每次会调用一个处理程序函数。

## 注册事件

事件注册的简单方法是使用的 `on()`。

调用 `on()` 时还可以带有三个参数。在这种形式下，事件类型是第一个参数，处理程序函数是第三个参数。在这两个参数中间可以传入任何值，jQuery会在调用处理程序前，将指定的值设置为Event对象的 `data` 属性。通过这种方式传递额外的数据给处理程序，不需要使用闭包，有时很有用。

`on()` 还有其他高级特性。如果第一个参数是由空格分隔的事件类型列表，则处理程序函数会为每一个命名的事件类型注册。

`on()` 的另一个重要特性是允许为注册的事件处理程序指定命名空间。这使得可以定义处理程序组，能方便后续触发或卸载特定命名空间下的处理程序。处理程序的命名空间对于开发可复用jQuery代码的类库或模块的程序员来说特别有用。事件命名空间类似CSS的类选择器。要绑定事件处理器到命名空间中时，添加句点（`.`）和命名空间名到事件类型字符串中即可。

## 注销事件

用 `on()`（或任何更简单的事件注册方法）注册事件处理程序后，可以使用 `off()` 来注销它，以避免在将来的事件中触发它。带有一个字符串参数时，由该字符串指明的事件类型（可以是多个，当字符串含有多个名字时）的所有处理程序会从jQuery对象的所有元素中取消绑定。

注意，`off()` 只注销用 `on()` 和相关jQuery方法注册的事件处理程序。通过 `addEventListener()` 或IE的 `attachEvent()` 方法注册的处理器不会注销，并且不会移除通过 `onclick` 和 `onmouseover` 等元素属性定义的处理程序。

如果模块使用命名空间来注册事件处理程序，则可以使用 `off()`，传入一个参数，来做到只注销命名空间下的处理程序。

如果想小心地只取消绑定自己注册的事件处理程序，但没有使用命名空间，必须保留事件处理程序函数的一个引用，并使用 `off()` 带两个参数的版本。在这种形式下，第一个参数是事件类型字符串（不带命名空间），第二个参数是处理程序函数。

Query也定义了一个更通用的 `trigger()` 方法。通常，调用 `trigger()` 时会传入事件类型字符串作为第一个参数，`trigger()` 会在jQuery对象中的所有元素上触发为该类型事件注册的所有处理程序。

## 动画效果

jQuery定义了 `fadeIn()` 和 `fadeOut()` 等简单方法来实现常见视觉效果。除了简单动画方法，jQuery还定义了一个 `animate()` 方法，用来实现更复杂的自定义动画。

jQuery动画方法经常使用动画时长来作为可选的第一个参数。如果省略时长参数，通常会得到默认值400ms。注意，省略时长时，有部分方法会立刻跳到最后一帧，没有中间的动画效果。

jQuery动画是异步的。调用 `fadeIn()` 等动画方法时，它会立刻返回，动画则在“后台”执行。由于动画方法会在动画完成之前返回，因此可以向很多jQuery动画方法传入第二个参数（也是可选的），该参数是一个函数，会在动画完成时调用。该函数在调用时不会有任何参数传入，但 `this` 值会设置为发生动画的文档元素。对于每个选中元素都会调用一次该回调函数。

通用使用 `animate()` 方法时，还可以传入一个对象来调用动画方法，该对象的属性指定动画选项。第一个参数是必需的：它必须是一个对象，该对象的属性指定要变化的CSS属性和它们的目标值。第二个参数是可选的，可以传入一个选项对象给animate()方法，除了将选项对象作为第二个参数传入，`animate()` 方法还允许将三个最常用的选项作为参数传入。可以将动画时长（数值或字符串）作为第二个参数传入。可以指定缓动函数名为第三个参数。最后可以将回调函数指定为第四个参数。

## 简单动画

|方法名|方法名|方法名|实现原理|是否在文档中占位|
|-|-|-|-|-|
|fadeIn()|fadeOut()|fadeTo()|改变透明度|是:white_check_mark:|
|show()|hide()|toggle()|改变透明度后display:none|否|
|slideDown()|slideUp()|slideToggle()|改变高度后display:none|否|

## 动画的取消和延迟

`stop()` 方法：它用来停止选中元素上的当前正在执行的任何动画。`top()` 方法接受两个可选的布尔值参数。如果第一个参数是true，会清除该选中元素上的动画队列：除了停止当前动画，还会取消任何等待执行的动画。第一个参数的默认值是false:如果忽略该参数，等待执行的动画不会被取消。第二个参数用来指定正在连续变化的CSS属性是否保留当前值，还是应该变化到最终目标值。传入 `true` 可以让它们变化到最终值。传入 `false`（或省略该参数）会让它们保持为当前值。

与动画相关的第二个方法是 `delay()`。这会直接添加一个时间延迟到动画队列中：第一个参数是时长（以毫秒为单位的数值或字符串），第二个参数是队列名，是可选的（通常并不需要第二个参数）

## jQuery中的Ajax


`jQuery.getScript()` 函数的第一个参数是JavaScript代码文件的URL。它会异步加载文件，加载完成后在全局作用域执行该代码。它能同时适用于同源和跨源脚本。

`jQuery.getJSON()` 与 `jQuery.getScript` 类似：它会获取文本，然后特殊处理一下，再调用指定的回调函数。`jQuery.getJSON()` 获取到文本后，不会将其当做脚本执行，而会将其解析为JSON。`jQuery.getJSON()` 只有在传入了回调参数时才有用。当成功加载URL，以及将内容成功解析为JSON后，解析结果会作为第一个参数传入回调函数中。

`jQuery.get()` 使用HTTP GET请求来实现，`jQuery.post()` 使用HTTP POST请求，其他两者则都是一样的。

上面四个Ajax工具最后都会调用 `jQuery.ajax()`——这是整个类库中最复杂的函数。`jQuery.ajax()` 仅接受一个参数：一个选项对象，该对象的属性指定Ajax请求如何执行的很多细节。

## 推荐阅读

* [工具函数](http://jquery.cuishifeng.cn/jQuery.Ajax.html)
* [jQuery选择器](http://jquery.cuishifeng.cn/jQuery.Ajax.html)

## 模仿代码
```js
let utils = (function () {
    /**
     * 元素距离body的偏移量
     * @param {element} ele 元素
     * @returns 对象中包含左偏移和上偏移
     */
    function offset(ele) {
        let left, top, par;
        left = ele.offsetLeft;
        top = ele.offsetTop;
        par = ele.offsetParent;
        while (par) {
            left += par.offsetLeft;
            top += par.offsetTop
            left += par.clientLeft;
            top += par.clientTop;
            par = par.offsetParent;
        }
        return {
            left,
            top
        };
    }
    /**
     * 元素距离父元素的偏移量
     * @param {element} ele 元素
     * @returns 对象中包含左偏移和上偏移
     */
    function position(ele) {
        let left, top;
        left = ele.offsetLeft;
        top = ele.offsetTop;
        return {
            left,
            top
        };
    }
    /**
     * 获取或设置页面的滚动的高度
     * @param {element} ele 元素
     * @param {number} val 值
     * @returns 页面已滚动的高度
     */
    function scrollTop(ele, val) {
        if (val == undefined) {
            return ele.scrollTop;
        } else {
            ele.scrollTop = val;
        }
    }
    /**
     * 获取或设置页面的滚动的宽度
     * @param {element} ele 元素
     * @param {number} val 值
     * @returns 页面已滚动的高度
     */
    function scollLeft(ele, val) {
        if (val == undefined) {
            return ele.scollLeft;
        } else {
            ele.scollLeft = val;
        }
    }
    /**
     * 获取或设置元素内容的宽度
     * @param {element} ele 元素
     * @param {number} val 值
     * @returns 
     */
    function width(ele, val) {
        if (val == undefined) {
            return window.getComputedStyle(ele).width;
        } else {
            ele.style.width = val;
        }
    }
    /**
     * 获取或设置元素内容的高度
     * @param {element} ele 元素
     * @param {number} val 值
     * @returns 
     */
    function height(ele, val) {
        if (val == undefined) {
            return window.getComputedStyle(ele).height;
        } else {
            ele.style.height = val;
        }
    }
    /**
     * 获取元素内容加补白的高度
     * @param {element} ele 元素
     * @returns 
     */
    function innerHeight(ele) {
        return ele.clientHeight;
    }
    /**
     * 获取元素内容加补白的宽度
     * @param {element} ele 元素
     * @returns 
     */
    function innerWidth(ele) {
        return ele.clientWidth;
    }
    /**
     * 获取元素内容加补白加边框的高度
     * @param {element} ele 元素
     * @returns 
     */
    function outerHeight(ele) {
        return ele.offsetHeight;
    }
    /**
     * 获取元素内容加补白加边框的宽度
     * @param {element} ele element
     * @returns 
     */
    function outerWidth(ele) {
        return ele.offsetWidth;
    }
    /**
     * 在容器的末尾添加元素
     * @param {element} con 容器
     * @param {element} ele 元素
     */
    function append(con, ele) {
        con.appendChild(ele);
    }
    /**
     * 元素向容器末尾添加
     * @param {element} ele 元素
     * @param {element} con 容器
     */
    function appendTo(ele, con) {
        con.appendChild(ele);
    }
    /**
     * 在容器的开头添加元素
     * @param {element} con 容器
     * @param {element} ele 元素
     */
    function prepend(con, ele) {
        con.insertBefore(ele, con.firstElementChild)
    }
    /**
     * 元素向容器开头添加
     * @param {element} ele 元素
     * @param {element} con 容器
     */
    function prependTo(ele, con) {
        con.insertBefore(ele, con.firstElementChild)
    }
    /**
     * 在旧元素前面添加新元素
     * @param {element} ele 旧元素
     * @param {element} ctx 新元素
     */
    function after(ele, ctx) {
        ele.parentNode.insertBefore(ctx, ele.nextElementSibling);
    }
    /**
     * 在旧元素后面添加新元素
     * @param {element} ele 旧元素
     * @param {element} ctx 新元素
     */
    function before(ele, ctx) {
        ele.parentNode.insertBefore(ctx, ele);
    }
    /**
     * 新元素添加到旧元素前面
     * @param {element} ctx 新元素
     * @param {element} ele 旧元素
     */
    function insertAfter(ctx, ele) {
        ele.parentNode.insertBefore(ctx, ele.nextElementSibling);
    }
    /**
     * 新元素添加到旧元素后面
     * @param {element} ctx 新元素
     * @param {element} ele 旧元素
     */
    function insertBefore(ctx, ele) {
        ele.parentNode.insertBefore(ctx, ele.nextElementSibling);
    }
    return {
        offset,
        position,
        scrollTop,
        scollLeft,
        width,
        height,
        innerHeight,
        innerWidth,
        outerHeight,
        outerWidth,
        append,
        appendTo,
        prepend,
        prependTo,
        after,
        before,
        insertAfter,
        insertBefore
    }
})()
```
