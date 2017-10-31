# 实现动画

我们现在已经知晓了如何显示`sprite`，那么怎样让他们动起来呢？很简单，使用`requestAnimationFrame`创建一个帧循环回调函数。我们称它为游戏主循环。任何我们在主循环中的代码，都会以60次每秒的速度循环执行。下面的代码中，我们来看一个让`cat`每帧移动一像素的动画：

```js
function gameLoop() {

  //以60fps的速度执行回到函数
  requestAnimationFrame(gameLoop);

  //每帧让cat向右移动1像素
  cat.x += 1;

  //渲染舞台
  renderer.render(stage);
}

//启动游戏循环
gameLoop();
```

如果执行这个代码片段，将会看到sprite逐渐的向舞台右边缘靠近。

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/15.png)

仅此而已，很简单吧！让sprite的一些属性在循环中慢慢的增长，动画就这样出现了。而如果想让sprite向相反的方向移动，就给予它负值，例如`-1`。你可以在`movingSprites.html`中看到全部的代码，截取如下：

```js
//设置变量
var Container = PIXI.Container,
    autoDetectRenderer = PIXI.autoDetectRenderer,
    loader = PIXI.loader,
    resources = PIXI.loader.resources,
    Sprite = PIXI.Sprite;

//创建pixi舞台和渲染器
var stage = new Container(),
    renderer = autoDetectRenderer(256, 256);
document.body.appendChild(renderer.view);

//加载图片，并运行`setup`函数
loader
  .add("images/cat.png")
  .load(setup);

//定义一些变量，以备使用
var cat;

function setup() {

  //创建`cat`sprite
  cat = new Sprite(resources["images/cat.png"].texture);
  cat.y = 96; 
  stage.addChild(cat);
 
  //启动游戏循环
  gameLoop();
}

function gameLoop(){

  //以60fps的速度执行回调函数
  requestAnimationFrame(gameLoop);

  //每帧移动cat对象1像素
  cat.x += 1;

  //渲染舞台
  renderer.render(stage);
}
```

（主要要在`setup`函数和`gameLoop`函数之外定义变量`cat`，以便你在2个函数中都可以引用它。）

现在我们可以做多种动画啦，无论是缩放、渲染，还是改变大小，都木有问题。后面你还能看到更多做动画的例子。