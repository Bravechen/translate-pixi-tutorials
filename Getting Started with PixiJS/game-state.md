# 游戏状态

作为一种代码风格，并且有利于模块化我们的代码，我建议你可以参考如下的方式处理游戏主循环的结构：

```js
//将游戏当前状态设置为`play`:
var state = play;

function gameLoop() {

  //以60fps执行回调函数
  requestAnimationFrame(gameLoop);

  //更新当前的游戏状态
  state();

  //渲染舞台
  renderer.render(stage);
}

function play() {

  //每帧使cat向右移动1个像素
  cat.x += 1;
}
```

我们看到在`gameLoop`函数中调用了名为`state`的函数，也就是说每秒60次的调用`state`函数。那么`state`函数是什么呢？它是指向`play`函数的引用。这意味着`play`函数中的代码也会获得60fps的执行次数。

下面我们使用这个模式来重构之前例子的代码：

```js
//定义一些变量
var cat, state;

function setup() {

  //创建`cat`sprite
  cat = new Sprite(resources["images/cat.png"].texture);
  cat.y = 96; 
  cat.vx = 0;
  cat.vy = 0;
  stage.addChild(cat);

  //设置游戏状态
  state = play;
 
  //启动游戏主循环
  gameLoop();
}

function gameLoop(){

  //以60fps执行回调函数
  requestAnimationFrame(gameLoop);

  //更新游戏状态
  state();

  //渲染舞台
  renderer.render(stage);
}

function play() {

  //每帧向右移动1像素
  cat.vx = 1
  cat.x += cat.vx;
}
```

哈哈，我知道，这有点[小癫狂](http://www.amazon.com/Electric-Psychedelic-Sitar-Headswirlers-1-5/dp/B004HZ14VS)[1]！但不要害怕，放松，给自己几分钟在脑海中思考一下这些函数是如何联系起来的。后面的讲解中，你能感受到他们的莫大益处。当我们按照这样的思路重新结构化代码之后，会非常非常容易做到一些事情。例如：游戏转场和升级。

（[1]--译注：作者在这段打了个比喻，说到了`head-swirler`这个词，意思是迷幻乐队的电声西塔琴摇滚专辑。代表了60年代美国等国家兴起的嬉皮士摇滚乐的一种，这种音乐来源于模拟人在服药之后产生的[迷幻效果](https://www.zhihu.com/question/21217362)。其实作者在这段主要是希望我们使用状态机模式来结构化代码。）

