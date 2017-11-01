# 编组sprite

编组可以让我们在创建游戏场景和管理想类似的sprite时，可以像一个独立的集合一样整体操作。Pixi有个一个名为`Container`的对象可以做到这一点。下面就来具体了解下。

想象一下，我们要显示三个sprite:`cat`、`hedgehog`和`tiger`。把他们创建出来——*但是不要把他们添加到舞台上*。

```js

//创建并设置cat
var cat = new Sprite(id["cat.png"]);
cat.position.set(16, 16);

//创建并设置hedgehog
var hedgehog = new Sprite(id["hedgehog.png"]);
hedgehog.position.set(32, 32);

//创建并设置tiger
var tiger = new Sprite(id["tiger.png"]);
tiger.position.set(64, 64);

```