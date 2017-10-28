# 使用纹理地图集

如果你正在开发一款大型、复杂的游戏，使用快速而高效的方式从瓦片集创建`sprite`一定是不二之选。纹理地图集的有用之处正在于此。
它是一个`json`文件，记录了瓦片集中子图片的位置、大小等信息。使用纹理地图集，你只需要知道子图片的名字就能在文件中获取它的信息。
按照一定的顺序排列好瓦片集，这个json文件就可以将大小和位置信息记录好。当需要在游戏程序中使用瓦片集图片的大小和位置信息时，纹理地图集可以很方便的满足需求，而不必硬编码到程序中。一旦你需要改变瓦片集，比如增加图片、改变大小或者移动位置，仅仅需要重新发布一个`JSON`文件，游戏程序就可以使用最新的数据正确的显示图片。你甚至都不用修改任何一处代码。

Pixi能够使用流行软件[Texture Packer](https://www.codeandweb.com/texturepacker)输出的纹理地图`JSON`文件。而且Texture Packer软件本身也有免费许可版。下面我们就来学习一下如何使用它制作纹理地图，以及如何载入Pixi。(如果你不想使用Texture Packer，也没有问题。类似的工具还有诸如[Shoebox](http://renderhjs.net/shoebox/)或者[spritesheet.js](https://github.com/krzysztof-o/spritesheet.js/)，同样可以输出Pixi可用的`PNG`图片格式和标准`JSON`格式文件。）

首先，让我们先收集一下需要在游戏中使用到的图片。

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/11.png)

本节所有的图片都来源于`Lanea Zimmerman`的作品。你可以从[这里](http://opengameart.org/users/sharm)了解到更多她的工作。谢谢，`Lanea`!

下一步，打开Texture Packer并且选择JSON Hash。将我们需要的图片拖到Texture Packer的工作区中。（你也可以指定包含图片的目录让Texture Packer把他们加载进来。）软件会自动将图片排列在一张瓦片集图片上，之后再赋予这些图片可以匹配的名字。

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/12.png)

（如果你使用的是免费版的Texture Packer，设置`Algorithm`为`Basic`，设置`Trim mode`为`None`，设置`Extrude`为`0`，设置`Size constraints`为`Any size`，然后始终保持`PNG Opt Level`处于0。这些基本设置可以让你在使用免费版的Texture Packer创建文件时不会报错或者收到警告。）

当你完成时，按下`Publish`按钮。填上文件的名称并选择保存地址，就可以保存文件了。最后我们会得到2个文件：一张`PNG`的图和一个`JSON`文件。
在我们的例子里，使用的文件被命名为`treasureHunter.json`和`treasureHunter.png`。为了避免不必要的麻烦，我们就保持这两个文件在同一个图片目录中就好了（可以把`JSON`文件想象成一种图片文件的元数据，所以将他们放在同一个目录里还是很有用的）。`JSON`文件中的内容描述了瓦片集子图片的名称、大小和位置。下面的片段展示了一段描述`blob`怪图形的信息。

```json
"blob.png":
{
	"frame": {"x":55,"y":2,"w":32,"h":24},
	"rotated": false,
	"trimmed": false,
	"spriteSourceSize": {"x":0,"y":0,"w":32,"h":24},
	"sourceSize": {"w":32,"h":24},
	"pivot": {"x":0.5,"y":0.5}
},
```

类似的，`treasureHunter.json`文件还包含了`“dungeon.png”`、`“door.png”`、`"exit.png"`和`"explorer.png"`属性，对应各自的数据。
这里，我们把每一个子图片称为帧(`frame`)。这些数据相当有用，我们只需要记得将来每个sprite的frame的id，就可以知道瓦片集中对应图片的大小、位置等信息。而frame的id完全等同于原文件的名称，就像`"blob.png"`和`"explorer.png"`一样。

在众多有用的功能中，还有一项可以使我们轻松的为图片之间增加2像素的间隔（`Texture Packer`的默认设置。）这对于防止纹理出血很有用。
纹理出血现象通常出现在与毗邻图片相接的边缘位置。当电脑的GPU（图形处理器）处理含有小数的像素值时，它会如何取整呢？向上取整或者是向下取整对于GPU都具有不同的意义。因此，在瓦片集的每个图片周围预留1或者2像素的空白间隔可以让每个图片都有完整的显示。

（注意：如果你设置了2像素的图形间隔，仍然需要在Pixi渲染它时留意怪异的`"少了一像素"`(`"off by one pixel"`)问题。此时可以试试改变纹理的缩放模式的算法。像这样做：`texture.baseTexture.scaleMode = PIXI.SCALE_MODES.NEAREST;`。究其原因，是因为GPU的浮点舍入误差造成了偶然出现这样的问题。）

现在你知道了如何创建纹理地图集，那就让我们来看看如何在游戏代码中加载并使用它吧。