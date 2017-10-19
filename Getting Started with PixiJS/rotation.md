# 旋转

我们也可以使用`sprite`的`rotation`属性，来让它旋转。仅仅需要一个弧度值。

```js
cat.rotation = 0.5;
```
思考一下，旋转围绕着那个点进行呢？

我们之前了解到sprite的左上角的点代表了它的`x`和`y`的位置。这个点被称为锚点(`anchor point`)。如果我们为sprite的`rotation`属性设置一个值，比如说0.5，那么sprite将会绕着锚点旋转。下文的图表展示了cat旋转后的效果。

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/07.png)

我们注意到锚点，也就是猫的左耳，正处于`cat`绕转的那个虚拟圆形的中点。话说应该让sprite绕着自己的中心旋转更好吧？我们可以通过改变sprite的锚点来实现：

```js
cat.anchor.x = 0.5;
cat.anchor.y = 0.5;
```

`anchor.x`和`anchor.y`的值表示纹理维度的百分比，值域从0到1（0%-100%）。将他们设置为0.5之后，纹理将会以锚点为中心。锚点的位置本身并没有改变，只是纹理的位置发生了变化。

下面的两幅图展示了设置了锚点之后sprite旋转的变化。

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/08.png)

我们注意到纹理已经向左上移动了。效果令人印象深刻！

类似位置和缩放，你也可以使用一个简洁的方法同时设置锚点的`x`和`y`坐标：

```js
sprite.anchor.set(x, y)
```

sprite还有一个类似锚点的属性叫`pivot`。`pivot`可以设置sprite原点的坐标。如果你更改了`pivot`的值然后旋转sprit，它将会围绕着原点旋转。我们设置sprite的`pivot.x`和`pivot.y`为32，看看会发生什么：

```js
cat.pivot.set(32, 32)
```
假设sprite的尺寸是64x64像素，它将会绕着自己的中点进行旋转。谨记这一点：当你改变了sprite的`pivot`点，也就改变它的`x/y`的原点。

讲了这么多，锚点(`anchor`)和`pivot`之间的区别到底是什么呢？它们真的很相似！`anchor`改变的是sprite图片纹理的原点，使用一种值域为0-1的标准值。`pivot`改变的是sprite的`x`和`y`的原点，使用像素值。到底该如何用？这取决于你。你可以对他们进行轮番的尝试，看看究竟哪种更符合你的需求。

