# 大小和缩放

我们可以通过设置sprite的`width`和`height`属性来更改sprite的大小。下面的例子展示了将`cat`的宽高设置为80x120：

```js
cat.width = 80;
cat.height = 120;
```
然后，可以把这两行代码加入到`setup`函数中：

```js
function setup() {

  //创建`cat`sprite
  var cat = new Sprite(resources["images/cat.png"].texture);

  //改变sprite的位置
  cat.x = 96;
  cat.y = 96;

  //改变sprite的大小
  cat.width = 80;
  cat.height = 120;

  //将cat添加舞台上
  stage.addChild(cat);
}
```

结果就是这样：
![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/05.png)

你可以看到，`cat`的宽高发生了改变，而位置则没有变。

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/06.png)

sprite的`scale.x`和`scale.y`属性可以成比例的改变宽高，下面的例子展示了使用`scale`缩放一半的效果：

```js
cat.scale.x = 0.5;
cat.scale.y = 0.5;
```

`scale`的值是数字类型的，值域是0-1，表示sprite大小的百分比。1表示100%(正常大小)，而0.5表示50%（一半大小）。你也可以设置sprite为双倍大小，像这样：

```js
cat.scale.x = 2;
cat.scale.y = 2;
```

Pixi库也提供了一个简洁的可选方法`scale.set`：

```js
cat.scale.set(0.5, 0.5);
```

如果符合你需求，尽情使用吧！


