## 本地和全局坐标系

当我们把一个sprite添加到容器中，它的`x`和`y`的位置是相对于容器的左上角原点的。这是sprite的本地坐标。那么在下图中，`cat`的位置你认为是多少呢？

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/20.png)

让我们输入一下:

```js
console.log(cat.x);
//Displays: 16
```

16?没错！这是因为相对于组容器的左上角原点，`cat`只是偏移了16像素。16即是`cat`的本地坐标位置`(16,16)`。

`sprite`还具有**全局坐标**。sprite的全局坐标是指它的锚点（通常在sprite的左上角）到stage左上角点的距离。我们可以通过调用一个`sprite`的`toGlobal`方法获取它的全局坐标。来看一下：

```js
parentSprite.toGlobal(childSprite.position)
```

如此，我们便可以获取身在`animals`组中的`cat`的全局坐标：

```js
console.log(animals.toGlobal(cat.position));
//Displays: Object {x: 80, y: 80...};
```

我们可以得到它的位置为`(80,80)`。这正好是`cat`相对于`stage`左上角点的全局坐标位置。

问题来了，如果不知道一个sprite的父容器是谁，怎么获取它的全局坐标呢？`sprite`的`parent`属性恰好提供了一个可以访问父容器的引用。如果一个`sprite`直接添加在了`stage`上，那么它的`parent`就指向`stage`。上面的例子中，`cat`的`parent`指向`animals`，所以我们也可以这样获取`cat`的全局坐标:

```js
cat.parent.toGlobal(cat.position);
```

现在，即使我们不知道`cat`的父容器是谁，也可以获取它的全局坐标啦。

其实还有另一个计算全局坐标的方法！而且或许是最好的，看仔细了！如果想知道一个sprite距离画布的左上角有多远，但并不关心sprite的父级到底是谁，我们可以使用`getGlobalPosition`方法。下面我们展示一下如何获取`tiger`的全局坐标：

```js
tiger.getGlobalPosition().x
tiger.getGlobalPosition().y
```

我们会得到它的坐标是`(128,128)`。`getGlobalPosition`的特性是具有很高的精度：它能在sprite的本地坐标发生改变时精确的计算它的全局坐标。我曾提出过要求，建议`Pixi`的开发团队加入这个特性，从而有利于游戏中精确地碰撞检测计算。

话说回来，如何将全局坐标转换为本地坐标呢？我们可以用`toLocal`方法。它的工作原理很类似，不过格式通常是这样的：

```js
sprite.toLocal(sprite.position, anyOtherSprite)
```

`toLocal`可以用来计算任意两个sprite之间的距离。下面的代码使用该方法获取了`tiger`相对于`hedgehog`的本地坐标。

```js
tiger.toLocal(tiger.position, hedgehog).x
tiger.toLocal(tiger.position, hedgehog).y
```

运行之后我们获得了一组`(32,32)`的坐标。对比例子中的图片，可以观察到`tiger`位于`hedgehog`左上角点左下方的位置，x方向上相距32像素，y方向也相距32像素。





