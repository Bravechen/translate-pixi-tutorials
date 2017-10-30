# 从瓦片集中的图片创建sprite

我们已经掌握了如何利用一张图片创建sprite的方法。不过，在游戏开发中，我们更多用到的是瓦片集（或者称为雪碧图`spritesheet`）。Pixi库中内置了几种方法可以帮助你轻松使用它。瓦片集是一张含有多个子图形的图片文件。这些子图形基本涵盖了我们的游戏中用到的图像。下面是一张瓦片集的例子，可以在其中找到游戏的角色和其他游戏图像资源。

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/09.png)

上图的整个瓦片集图片尺寸是192x192。每个自图片都被放置在32x32的单元格中。像这样把游戏中的图形放置在瓦片集中，是一种对性能和内存使用都很好的手段。而Pixi也能很高效的使用它们。

我们可以通过在瓦片集上定义一个矩形的区域以囊括我们需要的子图形。下面的例子中我们从瓦片集中提取一个火箭的子图片。

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/10.png)

让我们看看代码应该怎么做。首先，跟之前类似，我们使用Pixi的`loader`来加载`tileset.png`图片。

```js
loader
  .add("images/tileset.png")
  .load(setup);
```

下一步，图片加载完毕后，我们创建一个sprite以呈现一个从瓦片集中设定的矩形区域捕获的图像。下面的代码展示捕获子图像、创建`rocket`sprite、确定位置和呈现图像的过程。

```js
function setup() {

  //从纹理图片中创建`tileset`sprite
  var texture = TextureCache["images/tileset.png"];

  //创建一个矩形，大小和位置符合你所要提取的子图片的要求。
  var rectangle = new Rectangle(192, 128, 64, 64);

  //将矩形对象赋值给纹理属性
  texture.frame = rectangle;

  //从纹理缓存创建sprite对象
  var rocket = new Sprite(texture);

  //设定rocket在canvas中的位置
  rocket.x = 32;
  rocket.y = 32;

  //将rocket添加到舞台上
  stage.addChild(rocket);
  
  //渲染stage对象   
  renderer.render(stage);
}
```
代码是如何工作的？

Pixi库内置的矩形对象(`PIXI.Rectangle`)是一个有广泛用途的对象。它需要4个参数。前两个参数定义了矩形的`x`和`y`位置。后面的2个参数定义了宽度`width`和高度`height`。下面展示了具体用法：

```js
var rectangle = new PIXI.Rectangle(x, y, width, height);
```

矩形对象是一种数据结构；他的用途取决于你想在哪用。在我们的例子中，矩形对象主要帮助我们定义了想要抽取的子图形在瓦片集中的位置和区域。
Pixi的纹理缓存有一个非常重要的属性`frame`就可以被设置为一个矩形对象。`frame`属性可以利用矩形数据来设定纹理数据。下面我们就来看看，`frame`属性是如何影响rocket纹理的位置和大小的。

```js
var rectangle = new Rectangle(192, 128, 64, 64);
texture.frame = rectangle;
```
然后我们使用这片纹理来创建sprite:

```js
var rocket = new Sprite(texture);
```
过程很简单有木有！

由于通过瓦片集创建sprite所用纹理的方法使用频率很高，Pixi库专门提供了一个简便的方式。具体内容且看下回的讲解。





