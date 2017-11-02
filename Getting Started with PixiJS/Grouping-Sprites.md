# 编组sprite

编组可以让我们在创建游戏场景和管理一群sprite时，可以像独立的集合一样整体操作。Pixi有一个名为`Container`的对象可以做到。下面就来具体了解下。

想象一下，我们要显示三个sprite:`cat`、`hedgehog`和`tiger`。首先得把他们创建出来——*但是不要把他们添加到舞台上*。

```js

//创建并设置cat
var cat = new Sprite(id["cat.png"]);
cat.position.set(16, 16);

//创建并设置hedgehog
var hedgehog = new Sprite(id["hedgehog.png"]);
hedgehog.position.set(32, 32);

//创建并设置tiger
var tiger = new Sprite(id["tiger.png"]);
tiger.position.set(64, 64);

```

接下来，创建一个名为`animals`的容器，用来承载它们成为一组：

```js
var animals = new Container();
```

使用`addChild`方法将那些sprite加入到这个组里。

```js
animals.addChild(cat);
animals.addChild(hedgehog);
animals.addChild(tiger);
```

最后将这个组*加入到`stage`上*。

```js
stage.addChild(animals);
renderer.render(stage);
```

(其实大家都明白，`stage`对象本身也是一个容器。它是所有sprite的根级容器)

运行之后的效果，如下：

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/18.png)

从视觉上来说，包裹这些动物的容器是不可见的。

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/19.png)

现在我们可以把这些动物的组视为一个独立的集合了。更形象点说，容器(`Container`)就像是一个没有纹理的特殊sprite。

如果想要获取容器中含有的子sprite，可以使用容器的`children`属性，它会返回一个数组，包含了所有子对象。

```js
console.log(animals.children)
//Displays: Array [Object, Object, Object]
```

这段代码的结果，告诉我们`animals`含有三个子`sprite`。


由于`animals`几乎与其他sprite相同，所以我们也可以修改它的`x`和`y`的值，还有`alpha`、`scale`和其他的`sprite`属性。对父级容器属性的任何修改，都将间接的影响到子级对象。所以如果修改了组容器的`x`和`y`的值，所有的子sprite都会移动，并分布在相对于组容器的左上角原点的合理位置。
那么，如果将`animals`的`x`和`y`修改为64会发生什么呢？

```js
animals.position.set(64, 64);
```

结果是整个组中的sprite都向右下方移动了64个像素。

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/20.png)

`animals`组容器也有自己的空间维度，这取决于它所包含的子容器所占据的区域。我们可以发现他的宽和高是这样确定的：

```js
console.log(animals.width);
//Displays: 112

console.log(animals.height);
//Displays: 112
```

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/21.png)

如果我们修改组容器的宽和高会发生什么呢？

```js
animals.width = 200;
animals.height = 200;
```

可以发现，所有的子sprite都发生了缩放以适应容器尺寸的改变。

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/22.png)

在开发中，你可以按照需要采用容器套容器的方式，建立一个复杂的层级结构。不过，一个显示对象(`DisplayObject`，比如`Sprite`和`Container`)始终只有一个父级容器（无论父容器的引用指向谁）。如果你使用`addChild`方法把一个别的容器的子对象加载到另一个容器中，那么Pixi会首先将这个子容器从它原先的父容器中移除，然后再添加到目标的容器中。这是非常有用的内部管理机制，省心多了，不是吗？






