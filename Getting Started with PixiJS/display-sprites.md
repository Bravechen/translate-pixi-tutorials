# 呈现sprites

想要最终将加载的图片和创建好的sprite显示在canvas上，你还有2件事情需要做:

- 1.需要使用Pixi的`stage.addChild`方法将sprite添加到`stage`上，像这样:

```js
stage.addChild(cat);
```
`stage`始终作为主容器承载着你所有的sprite。

- 2.需要调用Pixi的渲染器渲染`stage`。

```js
renderer.render(stage);
```

**在你做完这2个操作之前，任何sprite是不会显示出来的。**

在继续学习之前，让我们来看一个实际的例子，它展示了你之前学到的如何加载一个图片并显示它的知识。
在`examples/images`文件夹中，你可以找到一张64x64尺寸的png格式位图。

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/cat.png)

下面的代码完整展示了从加载图片到创建sprite并显示出来的过程:

```js
var stage = new PIXI.Container(),
    renderer = PIXI.autoDetectRenderer(256, 256);
document.body.appendChild(renderer.view);

//使用Pixi内置的loader对象加载图片
PIXI.loader
  .add("images/cat.png")
  .load(setup);

//在图片加载完毕时，setup函数将会运行。
function setup() {

  //利用纹理创建名为cat的sprite
  var cat = new PIXI.Sprite(
    PIXI.loader.resources["images/cat.png"].texture
  );

  //将cat对象添加到stage上
  stage.addChild(cat);
  
  //让渲染器渲染当前stage   
  renderer.render(stage);
}

```
当代码运行后，将会是这样的:

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/02.png)

有木有点小小的成就感！

如果你想把一个sprite对象从stage上删除，可以使用如下方法:

```js
stage.removeChild(anySprite)
```

不过，让一个sprite对象不可见，更简单并且性能更好的方法是将sprite对象的`visible`属性设置为`false`。

```js
anySprite.visible = false;
```

## 使用别名

或许我们会有减少键入和增加代码可读性的需求，如此可以将频繁使用的Pixi对象和方法起一个别名，省点力气。举例来说，你是否觉得`PIXI.utils.TextureCache`键入一次好麻烦？我也有同感，尤其是在一个大项目中需要频繁输入的时候。所以，我们就可以为它起一个别名，像下面这样:

```js
var TextureCache = PIXI.utils.TextureCache;
```
然后，我们用别名替换之前的代码:

```js
var texture = TextureCache["images/cat.png"];
```

使用别名除了让我们拥有更简洁的代码之外，还有额外的好处：它能稍稍缓和Pixi库API频繁变动时造成的窘境。如果Pixi的API在未来改变，相信这很有可能，我们只需在一个地方(比如程序的开始)更新我们的别名所指代的Pixi对象和方法，这要比到处修改代码要好的多。假如Pixi的开发团队未来决定进行一定程度代码变动，我们也可以很快跟进。

现在来实践一下，我们将之前所写的加载图片并显示它的代码，使用别名形式进行一些修改：

```js
//别名
var Container = PIXI.Container,
    autoDetectRenderer = PIXI.autoDetectRenderer,
    loader = PIXI.loader,
    resources = PIXI.loader.resources,
    Sprite = PIXI.Sprite;

//创建一个Pixi的stage对象和renderer.view，然后放入DOM文档中
var stage = new Container(),
    renderer = autoDetectRenderer(256, 256);
document.body.appendChild(renderer.view);

//加载图片并在完成时运行setup函数
loader
  .add("images/cat.png")
  .load(setup);

function setup() {
  //创建名为cat的sprite实例，并添加到stage上并渲染出来。
  var cat = new Sprite(resources["images/cat.png"].texture);  
  stage.addChild(cat);
  renderer.render(stage);
}
```

在本手册大部分的例子中，我们都会像这样使用别名指代Pixi对象。除了特殊情况，你可以认为所有的例子都将按照这个例子的方式使用别名。

以上就是你需要了解的加载图片以及创建sprite的初步知识。

## 一些有关加载的额外知识

在上文中展示的有关图片加载和显示的例子，可以算作我所推荐的标准使用方法。所以你也可以直接跳到下一节“移动sprite”，而忽略下面所讲的东西。不过，Pixi的`loader`对象本身还是相当复杂的，有些特性我想你也应该了解一下。尽管你可能在常规操作中用不到。下面让我们学习一下其中颇为有用的部分知识。

### 从Image对象(Element Node类型)或者Canvas创建sprite

使用预加载到Pixi纹理缓存中的纹理数据来创建一个sprite，可以说是最佳方式，对于优化和性能来说也好处多多。但是，如果有特殊的原因使我们想使用JS创建的Image对象(`Element Node`)来创建纹理，也是被允许的。这就要用到Pixi库中的`BaseTexture`和`Texture`类了：

```js
var base = new PIXI.BaseTexture(anyImageObject),
    texture = new PIXI.Texture(base),
    sprite = new PIXI.Sprite(texture);
```
我们也可以利用`BaseTexture.fromCanvas`将已有的`canvas`元素对象创建纹理对象:

```js
var base = new PIXI.BaseTexture.fromCanvas(anyCanvasElement),
```

如果想要更换已经呈现的sprite的纹理数据，可以使用`texture`属性。赋予它一个`Texture`类型的对象，如下所示:

```js
anySprite.texture = PIXI.utils.TextureCache["anyTexture.png"];
```

在游戏中如果发生了什么特殊事件，我们可以使用此项技术来动态的改变sprite对象的外观。

### 设定加载文件的名称

或许我们有把资源文件赋予一个独一无二名称的需求。这只需提供一个字符串形式的名称作为`add`方法的第一个参数就可以做到。
例如，下面的例子展示了如何把一个名为`cat`的图片命名为`catImage`。

```js
PIXI.loader
  .add("catImage", "images/cat.png")
  .load(setup);
```

上面的代码会在`loader.resources`中创建一个名为`catImage`的对象。也意味着你可以通过引用这个`catImage`对象来创建一个sprite:

```js
var cat = new PIXI.Sprite(PIXI.loader.resources.catImage.texture);
```

然而，我并不推荐你这样做。这是因为你不得不好好记住这些命名过的资源文件，而且也增大了因为同名造成问题的概率。像我们之前的例子中那样，使用文件地址作为名称，既简单又不容易出错。

### 监控加载进度

Pixi库的`loader`对象可以在每次加载文件时派发一个`progress`事件，并调用一个自定义的回调函数（事件处理器）。`progress`事件可以在`loader`对象的`on`方法中进行设置：

```js
PIXI.loader.on("progress", loadProgressHandler);
```

下面的例子展示了如何在加载链中包含`on`方法，从而在每次加载文件时调用一个名为`loadProgressHandler`的自定义回调函数：

```js
PIXI.loader
  .add([
    "images/one.png",
    "images/two.png",
    "images/three.png"
  ])
  .on("progress", loadProgressHandler)
  .load(setup);

function loadProgressHandler() {
  console.log("loading"); 
}

function setup() {
  console.log("setup");
}
```

当每个文件被加载时，`progress`事件都会调用`loadProgressHandler`函数，使用`console`输出`"loading"`。当所有三个文件都被加载完成，`setup`函数将会运行。这里展示了以上代码运行时的所有输出：

```
loading
loading
loading
setup
```

结果挺好，但是可以更妙。我们还可以精确的获知，哪个文件正在加载并且加载完成了多少。通过为`loadProgressHandler`函数增加2个可选参数
`loader`和`resource`就可以:

```js
function loadProgressHandler(loader, resource) { //...
```

利用`resource.url`属性，我们可以知道哪个文件正在被加载（也可以使用`resource.name`属性利用你之前为文件设定的可选名称，回忆一下我们曾传入到`add`方法的第一个参数的例子）。

使用`loader.progress`属性，我们可以获得当前加载文件已被下载数据的百分比。下面的例子展示了具体的详情：

```js
PIXI.loader
  .add([
    "images/one.png",
    "images/two.png",
    "images/three.png"
  ])
  .on("progress", loadProgressHandler)
  .load(setup);

function loadProgressHandler(loader, resource) {

  //输出当前被加载文件的url
  console.log("loading: " + resource.url); 

  //输出被加载的文件完成的百分比
  console.log("progress: " + loader.progress + "%"); 

  //如果你在`add`方法中，将设定的名称作为第一个参数，
  //那么你可以在这样使用它：
  //console.log("loading: " + resource.name);
}

function setup() {
  console.log("All files loaded");
}
```
运行之后`console`会输出如下内容:

```
loading: images/one.png
progress: 33.333333333333336%
loading: images/two.png
progress: 66.66666666666667%
loading: images/three.png
progress: 100%
All files loaded
```

酷，这下我们可以精心的设计一个进度条啦。

>注意：`resource`对象还有一些有用的属性。`resource.error`可以获取到加载文件时发生的错误信息。`resource.data`则可以让我们获取文件的二进制形式数据。

### 更多关于Pixi库中loader对象的内容

Pixi的`loader`对象具有令人感叹的丰富性和可配置性。让我们快速的鸟瞰一下。

`loader`链式方法`add`其实有4个基本的参数:

```js
add(name, url, optionObject, callbackFunction)
```
下面是源码中关于`loader`对象这些参数的解释:

- `name` (string): 被加载文件的名称。如果没有设置，则使用`url`作为名称。
- `url` (string): 资源的地址，相对于`loader`的`baseUrl`。
- `options` (object literal): 加载操作的选项。
- `options.crossOrigin` (Boolean): 请求是否跨域？默认会进行自动判定。
- `options.loadType`: 资源将会以何种方式加载？默认值是`Resource.LOAD_TYPE.XHR`。
- `options.xhrType`: 如果使用XHR方式，那么将如何解析加载到的数据？默认值为`Resource.XHR_RESPONSE_TYPE.DEFAULT`。
- `callbackFunction`: 在特定资源下载完成时调用的回调函数。

如此众多的参数中其实只有url是必须的(即我们加载的文件地址)。

接下来将会展示若干种你可以使用`add`方法的方式。第一个是文档中称作“一般语法”(`"normal syntax"`)的方式：

```js
.add('key', 'http://...', function () {})
.add('http://...', function () {})
.add('http://...')
```

然后再来一些“对象语法”(`"object syntax"`)的例子：

```js
.add({
  name: 'key2',
  url: 'http://...'
}, function () {})

.add({
  url: 'http://...'
}, function () {})

.add({
  name: 'key3',
  url: 'http://...'
  onComplete: function () {}
})

.add({
  url: 'https://...',
  onComplete: function () {},
  crossOrigin: true
})
```

顺便了解下传入一个对象数组，或者url数组，甚或二者皆有的例子:

```js
.add([
  {name: 'key4', url: 'http://...', onComplete: function () {} },
  {url: 'http://...', onComplete: function () {} },
  'http://...'
]);
```

>注意：如果你想重置`loader`对象，重新加载一批新的文件，可以使用`loader`对象的`reset`方法:`PIXI.loader.reset();`

Pixi的`loader`对象有很多高级的特性，甚至包括使你可以加载和解析各种类型二进制文件的配置选项。这对于我们需要了解的日常开发知识点和基本原理来说并不是那么必要，同时也超出了本手册内容的范围。[如果你喜欢更深入的探究，不妨到`loader`的GitHub项目中了解更多信息](https://github.com/englercj/resource-loader)。