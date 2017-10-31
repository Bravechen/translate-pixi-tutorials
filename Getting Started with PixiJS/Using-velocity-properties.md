# 使用速度属性

有一种方式可以让你更灵活更好的控制sprite移动的速度，那就是增加2个速度属性：`vx`和`vy`。
`vx`用来设置sprite沿着x轴（水平方向）运动的速度和方向。
`vy`则在y轴（垂直方向）上实现同样的事。
比起直接设置sprite的`x`和`y`属性值，先一步设置速度属性值更具灵活性也更明了。
这是一种额外设置的模块级属性，在制作交互产品、动画和游戏中非常有用。

第一步，我们为sprite实例新增`vx`和`vy`属性，然后给他们一个初始值。

```js
cat.vx = 0;
cat.vy = 0;
```

设置`vx`和`vy`为0，意味着sprite停止不动。

接下来，在游戏循环中，我们更新`vx`和`vy`属性，为他们赋予一个适当的速度值。然后再把这些值分配给sprite的`x`和`y`属性。下面的代码展示了我们应用这项技术来使cat每帧向下并向右各移动1像素的效果：

```js
function setup() {

  //创建cat
  cat = new Sprite(resources["images/cat.png"].texture);
  stage.addChild(cat);

  //初始化cat的速度值
  cat.vx = 0;
  cat.vy = 0;
 
  //启动游戏循环
  gameLoop();
}

function gameLoop(){

  //以60fps运行回调函数
  requestAnimationFrame(gameLoop);

  //更新cat的速度
  cat.vx = 1;
  cat.vy = 1;

  //将速度值应用到cat的位置上
  //以使它真正移动起来
  cat.x += cat.vx;
  cat.y += cat.vy;

  //渲染舞台
  renderer.render(stage);
}
```

运行这段代码，cat就开始向右下方逐帧的移动：

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/16.png)

如果想让cat往相反的方向移动该怎么办？设置`vx`为`-1`可以让其向左移动；设置`vy`为`-1`可以让其向上移动。想让cat移动快点？将`vx`和`vy`设置大一点，比如`3`、`5`、`-2`和`-4`。

在后面的讲解中，你可以了解到更多关于处理有关sprite运行速度的知识。它们将会帮助你更好的完成诸如键盘鼠标控制系统的游戏，又或者轻松的实现一个物理系统。
