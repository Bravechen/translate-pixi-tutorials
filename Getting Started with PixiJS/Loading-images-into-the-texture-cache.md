# 将图片加载到纹理缓存

因为在底层Pixi采用WebGL在GPU中进行渲染图像的操作，所以图像数据需要被转变成一种GPU可处理的格式，被称为**纹理**。
在使用sprite承载一个图片数据之前，我们需要把图片文件转化为一份WebGL纹理。为了保证底层处理工作的快速和高效，Pixi使用了**纹理缓存**来存储和引用所有我们将要用到的纹理。这些纹理的名称与文件的地址相匹配，以字符串的形式被存储。这意味着如果你有一个从`"images/cat.png"`加载的纹理，那么可以像这样在纹理缓存中找到它:

```js
PIXI.utils.TextureCache["images/cat.png"];
```

在内部，纹理会被存储为一种WebGL兼容格式，以便Pixi渲染器能够高效的对它进行处理。而我们可以使用`Sprite`类来创建一个使用纹理的sprite实例。

```js
var texture = PIXI.utils.TextureCache["images/anySpriteImage.png"];
var sprite = new PIXI.Sprite(texture);
```

究竟图片是如何被加载又被转换为纹理的呢？Pixi库中内置的`loader`对象的功能正在于此。

Pixi强大的`loader`对象可以满足你加载任意类型图片的需求。下面的代码展示了如何加载一个图片并在加载完成时调用回调函数：

```js
PIXI.loader
  .add("images/anyImage.png")
  .load(setup);

function setup() {
  //这里可以运行当图片加载完成后的代码
}
```

值得注意的是，[Pixi开发团队建议](http://www.html5gamedevs.com/topic/16019-preload-all-textures/?p=90907)在使用`loader`时，应该利用`loader`对象中`resources`属性所指向的纹理来创建sprite实例，像这样:

```js
var sprite = new PIXI.Sprite(
  PIXI.loader.resources["images/anyImage.png"].texture
);

```

下面的例子是一段完整的代码，展示了如何通过一个名为`setup`的回调函数，利用加载好的图片创建sprite：

```js
PIXI.loader
  .add("images/anyImage.png")
  .load(setup);

function setup() {
  var sprite = new PIXI.Sprite(
    PIXI.loader.resources["images/anyImage.png"].texture
  );
}
```
以上介绍了一种利用加载到的图片创建sprite的普通方法。

更进一步，还可以使用链式操作同时加载多张图片，如下所示：

```js
PIXI.loader
  .add("images/imageOne.png")
  .add("images/imageTwo.png")
  .add("images/imageThree.png")
  .load(setup);

```

当然，还有更好的做法，比如把所有的图片放在一个数组中传入方法中：

```js
PIXI.loader
  .add([
    "images/imageOne.png",
    "images/imageTwo.png",
    "images/imageThree.png"
  ])
  .load(setup);
```

有没有想到`loader`对象还允许你加载`JSON`文件？在后面的文章中你会看到的。

