# jQuery类库

jQuery如此强大和好用，关键得益于以下特性：

* 丰富强大的语法（CSS选择器），用来查询文档元素
* 高效的查询方法，用来找到与CSS选择器匹配的文档元素集
* 一套有用的方法，用来操作选中的元素
* 强大的函数式编程技巧，用来批量操作元素集，而不是每次只操作单个
* 简洁的语言用法（链式调用），用来表示一系列顺序操作

## 基础

jQuery类库定义了一个全局函数：`jQuery()`。该函数使用频繁，因此在类库中还给它定义了一个快捷别名：`$`。这是jQuery在全局命名空间中定义的唯一两个变量。

`jQuery()` 是工厂函数，不是构造函数，它返回一个新创建的对象，但并没有和new关键字一起使用。

“jQuery函数”：jQuery函数指定义在jQuery命名空间中的函数，比如 `$.each(a,fn);` 。jQuery函数也可称为“静态方法”。

“jQuery方法”：jQuery方法是由jQuery函数返回的jQuery对象的方法，比如 `$("a").each(fn);`。jQuery类库最重要的部分就是它定义的这些强大的方法。

`$()` 的返回值是一个jQuery对象。jQuery对象是类数组。jQuery对象还有三个挺有趣的属性。`selector` 属性是创建jQuery对象时的选择器字符串（如果有的话）。`context` 属性是上下文对象，是传递给 `$()` 方法的第二参数，如果没有传递的话，默认是Document对象。最后，所有jQuery对象都有一个名为jquery的属性，检测该属性是否存在可以简单快捷地将jQuery对象与其他类数组对象区分开来。

## jQuery()

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

## 遍历

想要遍历jQuery对象中的所有元素时，可以调用 `each()` 方法来代替for循环。each()方法有点类似ECMAScript 5(ES5)中的 `forEach()` 数组方法。它接受一个回调函数作为唯一参数，然后它对jQuery对象中的每一个元素（按照在文档中的顺序）调用回调函数。回调函数作为匹配元素的方法来调用，因此在回调函数里 `this` 关键字指代Element对象。`each()` 方法还会将索引值和该元素作为第一个和第二个参数传递给回调函数。注意：`this` 和第二参数都是原生文档元素，而不是jQuery对象；如果想使用jQuery方法来操作该元素，需要先用 `$()` 封装它。jQuery的 `each()` 方法和 `forEach()` 有一个显著区别：如果回调函数在任一个元素上返回false，遍历将在该元素后中止（这就像在普通循环中使用break关键字一样）。`each()` 返回调用自身的jQuery对象，因此它可以用于链式调用。

jQuery的map()方法和 `Array.prototype.map()` 方法很相近。它接受回调函数作为参数，并为jQuery对象中的每一个元素都调用回调函数，同时将回调函数的返回值收集起来，并将这些返回值封装成一个新的jQuery对象返回。`map()` 调用回调函数的方式和 `each()` 方法相同：元素作为this值和第二参数传入，元素的索引值作为第一参数入。如果回调函数返回null或undefined，该值将被忽略，在本次回调中不会有任何新元素添加到新的jQuery对象中。

除了 `each()` 和 `map()` 之外，jQuery的另一个方法是 `index()`。该方法接受一个元素作为参数，返回值是该元素在此jQuery对象中的索引值，如果找不到的话，则返回-1。

## 获取和设置属性

|方法名称|一个字符串作为参数|两个字符串作为参数|一个对象作为参数|一个字符串和一个函数作为参数|
|-|-|-|-|-|
|attr()|获取HTML属性|设置HTML属性|批量设置HTML属性|调用该函数来计算需要设置的值|
|css()|获取CSS属性|设置CSS属性|批量设置CSS属性|调用该函数来计算需要设置的值|

## 归纳

* 获取和设置HTML属性（attr()/removeAttr()）
* 获取和设置CSS属性（css()）
* 获取和设置CSS类名（addClass()/removeClass()/toggleClass()）
* 获取和设置HTML表单值（val()）
* 获取和设置元素内容（text()/html()）
* 获取和设置元素的位置高宽（offset()/position()/width()/height()/innerWidth()/innerHeight()/outerWidth()/outerHeight()）offset()返回元素的绝对位置，用相对于文档的坐标来表示。而position()则返回相对于元素的offsetParent()的偏移量。width()和height()方法返回基本的宽度和高度，不包含内边距、边框和外边距。innerWidth()和innerHeight()返回元素的宽度和高度，包含内边距的宽度和高度（“内”表示这些方法度量的是边框以内的尺寸）。outerWidth()和outerHeight()通常返回的是包含元素内边距和边框的尺寸。
* 获取和设置元素数据（data()/removeData()）
* 复制元素（clone()）

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
