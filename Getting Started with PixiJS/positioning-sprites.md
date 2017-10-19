# 定位sprite 

现在你已经学会了如何创建并显示sprite了，我们来了解一下如何移动sprite的位置和改变他的大小。

在之前的例子中，我们把一个名为`cat`的sprite放置在了舞台的左上角位置。`cat`的`x`和`y`属性都为`0`。我们可以改变他们的`x`和`y`属性来让`cat`的位置发生变化。下面的例子中，我们通过设置`cat`的`x`和`y`都为96，以使它在舞台上处于居中的位置。

```js
cat.x = 96;
cat.y = 96;
```

把这2行代码放入到`setup`函数中，当sprite创建完成时，他们就可以生效了。

```js
function setup() {

  //创建名为`cat`的sprite
  var cat = new Sprite(resources["images/cat.png"].texture);

  //改变sprite的位置
  cat.x = 96;
  cat.y = 96;

  //把cat添加到stage上
  stage.addChild(cat);

  //渲染stage
  renderer.render(stage);
}
```

>注意：在这个例子中，`Sprite`其实是`PIXI.Sprite`的别名，`TextureCache`是`PIXI.utils.TextureCache`的别名，而`resources`则是`PIXI.loader.resources`的别名。约定一下，从现在开始我将会在所有的例子中使用这些别名指代`Pixi`的对象和方法。

这两行代码将会使`cat`向右移动96像素，向下移动96像素。结果如下:

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/03.png)

`cat`的左上角(猫的左耳)代表了它的的`x`和`y`的原点。增大`x`属性的值可以使`cat`向右移动；增大`y`属性值可以使`cat`想下移动。如果`cat`的`x`值为0，它将会处于`stage`对象的左边缘。如果`y`值为0，它将处于`stage`的上边缘。

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/04.png)

Pixi也提供了一个方法，可以对sprite的`x`和`y`属性集中设置。

```js
sprite.position.set(x, y)
```
