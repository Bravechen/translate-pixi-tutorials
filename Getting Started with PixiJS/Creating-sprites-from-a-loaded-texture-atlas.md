# 利用加载到的纹理地图创建sprite

通常有三种方式利用纹理地图中的数据创建`sprite`:

1. 使用`TextureCache`：

```js
var texture = TextureCache["frameId.png"],
    sprite = new Sprite(texture);
```

2. 在使用Pixi的`loader`加载纹理地图完成之后，使用`loader`的`resources`:

```js
var sprite = new Sprite(
  resources["images/treasureHunter.json"].textures["frameId.png"]
);
```

3. 写到这里我觉得创建一个`sprite`需要输入的字符太多了点啊！现在我就推荐给你一个简单的方式，使用名为`id`的别名指代纹理地图的`textures`对象，像这样：

```js
var id = PIXI.loader.resources["images/treasureHunter.json"].textures; 
```

之后我们创建每一个`sprite`时，只需这样：

```js
let sprite = new Sprite(id["frameId.png"]);
```
感觉手指舒服多啦！

下面我们在`setup`函数，分别使用刚才介绍的三种方式，将`dungeon`、`explorer`和`treasure`三种`sprite`创建出来并呈现它们：

```js
//定义一些可能用到的变量，以便在多个函数中使用它们
var dungeon, explorer, treasure, door, id;

function setup() {

  //三种方式使用纹理地图的帧数据创建sprite

  //1. 直接访问`TextureCache`
  var dungeonTexture = TextureCache["dungeon.png"];
  dungeon = new Sprite(dungeonTexture);
  stage.addChild(dungeon);

  //2. 使用loader的`resources`访问纹理数据
  explorer = new Sprite(
    resources["images/treasureHunter.json"].textures["explorer.png"]
  );
  explorer.x = 68;

  //将explorer放在居中位置
  explorer.y = stage.height / 2 - explorer.height / 2;
  stage.addChild(explorer);

  //3. 使用id别名指代所有的纹理地图帧数据集合
  id = PIXI.loader.resources["images/treasureHunter.json"].textures; 
  
  //创建宝箱，并赋值给变量名
  treasure = new Sprite(id["treasure.png"]);
  stage.addChild(treasure);

  //将宝箱放置在画布右侧的边缘位置
  treasure.x = stage.width - treasure.width - 48;
  treasure.y = stage.height / 2 - treasure.height / 2;
  stage.addChild(treasure);

  //渲染舞台
  renderer.render(stage);
}
```

代码运行后的结果：

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/13.png)

在上面的代码中，也展示了如何利用`stage.width`和`stage.height`属性，让众多`sprite`在大小为512x512像素的舞台中保持对齐。下面就是让`explorer`在`y`方向垂直居中的代码：

```js
explorer.y = stage.height / 2 - explorer.height / 2;
```

学会使用纹理地图创建和显示`sprite`是一个重要的里程碑。那么在继续学习之前，让我们补充一些代码，把剩余的`sprite`创建完成。你将看到，我们增加了`blob`怪物和出口，这样也就完成了一个场景：

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/14.png)

终于完成了所有代码。我将包含`HTML`代码的内容一并放在下面展示给你，以便能看到全部内容（你可以在代码仓库中找到这个文件，路径是`examples/spriteFromTextureAtlas.html`）。
注意那些`blob`怪的`sprite`我是用一个循环随机放置在舞台上的。

```html
<!doctype html>
<meta charset="utf-8">
<title>Make a sprite from a texture atlas</title>
<body>
<script src="../pixi.js/bin/pixi.js"></script>
<script>

//定义变量名
var Container = PIXI.Container,
    autoDetectRenderer = PIXI.autoDetectRenderer,
    loader = PIXI.loader,
    resources = PIXI.loader.resources,
    TextureCache = PIXI.utils.TextureCache,
    Texture = PIXI.Texture,
    Sprite = PIXI.Sprite;

//创建一个Pixi的stage和renderer渲染器，
//然后将renderer.view加入到DOM中。
var stage = new Container(),
    renderer = autoDetectRenderer(512, 512);
document.body.appendChild(renderer.view);

//加载一份JSON文档并在完成时调用`setup`函数
loader
  .add("images/treasureHunter.json")
  .load(setup);

//定义一些变量，以便用在多个函数中
var dungeon, explorer, treasure, door, id;

function setup() {

  //3种从纹理地图帧数据中创建sprite的方法：

  //1. 直接访问`TextureCache`
  var dungeonTexture = TextureCache["dungeon.png"];
  dungeon = new Sprite(dungeonTexture);
  stage.addChild(dungeon);

  //2. 通过loader对象的`resources`访问纹理数据
  explorer = new Sprite(
    resources["images/treasureHunter.json"].textures["explorer.png"]
  );
  explorer.x = 68;

  //将explorer垂直居中
  explorer.y = stage.height / 2 - explorer.height / 2;
  stage.addChild(explorer);

  //3. 使用`id`指代所有的纹理地图帧数据集合
  id = PIXI.loader.resources["images/treasureHunter.json"].textures; 
  
  //创建宝箱并赋值给变量
  treasure = new Sprite(id["treasure.png"]);
  stage.addChild(treasure);

  //将宝箱放置在画布的右侧边缘
  treasure.x = stage.width - treasure.width - 48;
  treasure.y = stage.height / 2 - treasure.height / 2;
  stage.addChild(treasure);

  //创建出口
  door = new Sprite(id["door.png"]); 
  door.position.set(32, 0);
  stage.addChild(door);

  //创建blobe怪
  var numberOfBlobs = 6,
      spacing = 48,
      xOffset = 150;

  //按照`numberOfBlobs`值创建足够数量的`blob`怪
  for (var i = 0; i < numberOfBlobs; i++) {

    //创建一个blob怪物
    var blob = new Sprite(id["blob.png"]);

    //使用`spacing`的值让每个blob怪水平分布。
    //`xOffset`决定了从画面左侧开始放置怪物时
    //位置的偏移量
    var x = spacing * i + xOffset;

    //给予blob怪一个随机的y位置
    //`randomInt`是一个自定义函数，代码在下面
    var y = randomInt(0, stage.height - blob.height);

    //设置blob怪的位置
    blob.x = x;
    blob.y = y;

    //将blob怪的sprite加入到stage中
    stage.addChild(blob);
  }
 
  //渲染舞台  
  renderer.render(stage);
}

//`randomInt`辅助计算函数
function randomInt(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

</script>
</body>
```

你可以看到，我们在一个循环中创建了所有的blob怪。并且让每个blob沿着x轴平均分布：

```js
var x = spacing * i + xOffset;
blob.x = x;
```

`spacing`的值为48，`xOffset`的值为150。这意味着第一个blob怪将在x值为150的位置，相对于舞台的左边缘偏移了150像素。随后每一个blob怪的x值都会比前一个的x值大48像素。这使得被创建的blob怪可以从左到右的平均分布于`dungeon`地板上。

每一个`blob`还有被设置了随机数值的`y`值，代码是这样的：

```js
var y = randomInt(0, stage.height - blob.height);
blob.y = y;
```

`blob`的`y`值因此是一个处于`0~512`范围中的随机值（512是`stage.height`的值）。随机值产生于自定函数`randomInt`。它可以在2个数字确定的范围中产生一个随机的值。

```js
randomInt(lowestNumber, highestNumber)
```
比如，你想得到一个1-10之间的随机数字，可以这样使用:

```js
var randomNumber = randomInt(1, 10);
```
`randomInt`函数的具体内容是这样的：

```js
function randomInt(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}
```

你可以像我一样将类似`randomInt`这样有用的函数收录到常用代码库中，以便在开发游戏时信手拈来。

