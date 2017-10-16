
## 创建渲染器和舞台

现在你终于可以使用`Pixi`啦！

那么，应该如何开始呢？

第一步，让我们创建一个矩形的区域来显示图形。Pixi的渲染器对象(`renderer object`)可以做到这一点。渲染器对象能够自动生成一个`<canvas>`标签并插入到html当中，之后将你的图形显示在`canvas`上。
另外还需要创建一个特殊的Pixi容器对象(`Pixi Container object`)，我们称他为`stage`。它将会作为根级容器承载你所创建的所有显示对象。


下面的代码展示了如何创建渲染器和stage对象。你可以把它们添加到HTML的文档中，放在`<script>`标签之间:

```js

//创建renderer对象
var renderer = PIXI.autoDetectRenderer(256, 256);

//将canvas实例对象添加到html的document中
document.body.appendChild(renderer.view);

//创建stage根级容器
var stage = new PIXI.Container();

//告诉render对象渲染stage
renderer.render(stage);

```

以上几乎是使用Pixi功能最简单的代码。我们在html文档中创建了一个256x256大小的canvas，并整体填充为黑色。下面就是运行后，你在浏览器中看到的效果：

![image](https://github.com/kittykatattack/learningPixi/blob/master/examples/images/screenshots/01.png)

哇，一个[黑块儿](http://rampantgames.com/blog/?p=7745)!

Pixi的`autoDetectRenderer(width,height,options)`方法可以决定渲染图像是使用`Canvas Drawing API`，还是`WebGL`，这取决于运行时系统上哪个是可用的。方法的前两个参数是`width`和`height`，指定了`canvas`的大小。
此外，你还可以设置第三个可选参数，用来进行附加设置。第三个参数是一个纯对象(`object literal`)，你可以在其中设置诸如抗锯齿，背景透明度和分辨率等参数：

```js

renderer = PIXI.autoDetectRenderer(
  256, 256,
  {antialias: false, transparent: false, resolution: 1}
);

```

第三个参数（`options`）是可选的。如果你满足于Pixi的默认设置，完全可以不管它，通常也确实这么做。当然，如果你真的需要设置他们，可以去查看Pixi的文档中关于[Canvas Renderer](http://pixijs.download/release/docs/PIXI.CanvasRenderer.html)和[WebGLRenderer](http://pixijs.download/release/docs/PIXI.WebGLRenderer.html)部分，获取相关信息。

刚才那些附加设置能做到什么呢？

```js
{antialias: false, transparent: false, resolution: 1}
```

`antialias`(抗锯齿)可以让文字的边缘和图形基元变得平滑（`WebGL`的抗锯齿功能并不是在所有平台都可用的，所以你需要先测试一下游戏的目标平台是否支持这项功能）。
`transparent`可以设置`canvas`的背景是否透明。
`resolution`可以让设置不同分辨率和像素密度更轻松。关于设置`resolution`的细节其实略略超出了本手册的内容，如果你想了解更多详情，可以看看Mat Grove的这篇[文章](http://www.goodboydigital.com/pixi-js-v2-fastest-2d-webgl-renderer/)。通常来讲，把`resolution`设置为1，就可以满足大多数项目的需要了。

（注意：`renderer`对象还有第四个可选的参数，是`preserveDrawingBuffer`，默认值为`false`。只有当需要在`WebGL`绘图上下文中使用Pixi专门的`dataToURL`方法时，才设置为`true`）

Pixi的渲染器对象会默认使用`WebGL`，因为他性能不错，可以让你做出一些令人惊艳的视觉效果。不过，如果你的关注点诉求对于`Canvas Drawing API`多过WebGL，你也可以转换底层渲染器:

```js
renderer = new PIXI.CanvasRenderer(256, 256);
```

只需设置两个必要的参数:`width`和`height`。

如果你想切换到WebGL渲染器，可以这样:

```js
renderer = new PIXI.WebGLRenderer(256, 256);
```

`renderer.view`返回的是一个`<canvas>`元素节点(`Element Node`)对象。所以你可以像控制其他canvas对象那样使用它，比如下面这段css，我们为canvas设置了一个虚线边框:

```js
renderer.view.style.border = "1px dashed black";
```

如果你需要在canvas创建之后改变它的背景色，可以使用渲染器对象的`backgroundColor`属性，赋予任意一个十六进制的数值:

```js
renderer.backgroundColor = 0x061639;    //注意:不是"#061639"
```

如果你想要获取渲染器的宽度和高度，可以使用`renderer.view.width`和`renderer.view.height`。

（重点来了：尽管stage也有宽度和高度属性，但是它们指的并不是渲染窗口的尺寸。stage的宽度和高度只是指明了你放入其中的各个显示对象所占用的区域大小，所以也可能更大！）

想要改变canvas的尺寸，可以使用渲染器的`resize`方法，传入新的宽度和高度值即可。不过，为了确保canvas能够适应分辨率(`resolution`)，请把`autoResize`设置为`true`。

```js
renderer.autoResize = true;
renderer.resize(512, 512);
```

如果你希望canvas能充满整个窗口，可以使用如下css样式改变渲染器的尺寸以适应浏览器的窗口尺寸。

```js
renderer.view.style.position = "absolute";
renderer.view.style.display = "block";
renderer.autoResize = true;
renderer.resize(window.innerWidth, window.innerHeight);
```

此外，你可能还需要把HTML元素的padding和margin设置为0，就像下面这段css代码一样：

```html

<style>
    * {
        padding: 0; 
        margin: 0;
    }
</style>

```

(上面代码中的星号，`*`，在css中称作“通配符选择器”，会查找HTML文档中的所有元素并应用样式。) 

如果你想要canvas按照比例适应浏览器窗口，可以参考[自定义`scaleToWindow`函数](https://github.com/kittykatattack/scaleToWindow)。

