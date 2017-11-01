# 键盘操控

要想使用键盘控制sprite，我们需要多做点工作以建立一个配套的小系统。简单起见，我们将使用一个名为`keyboard`的自定义函数监听和捕获键盘事件。

```js
function keyboard(keyCode) {
  var key = {};
  key.code = keyCode;
  key.isDown = false;
  key.isUp = true;
  key.press = undefined;
  key.release = undefined;

  //`downHandler`监听向下的键盘事件
  key.downHandler = function(event) {
    if (event.keyCode === key.code) {
      if (key.isUp && key.press) key.press();
      key.isDown = true;
      key.isUp = false;
    }
    event.preventDefault();
  };

  //`downHandler`监听向上的键盘事件
  key.upHandler = function(event) {
    if (event.keyCode === key.code) {
      if (key.isDown && key.release) key.release();
      key.isDown = false;
      key.isUp = true;
    }
    event.preventDefault();
  };

  //添加事件侦听
  window.addEventListener(
    "keydown", key.downHandler.bind(key), false
  );
  window.addEventListener(
    "keyup", key.upHandler.bind(key), false
  );
  return key;
}
```

使用`keyboard`函数很简单。创建一个`keyboard`对象即可：

```js
var keyObject = keyboard(asciiKeyCodeNumber);
```

唯一的参数是一个ASCII码，指代需要监听的键盘按键的键控代码。[这里有一份ASCII键控代码的列表](http://help.adobe.com/en_US/AS2LCR/Flash_10.0/help.html?content=00000520.html)。

接下来，为`keyboard`对象增加`press`和`release`方法：

```js
keyObject.press = function() {
  //处理键盘按下
};
keyObject.release = function() {
  //处理键盘释放
};
```

`keyboard`对象还含有另外两个布尔值类型的属性`isDown`和`isUp`，用来检测那些我们监听的按键的状态。

你可以在`examples`目录下的`keyboardMovement.html`文件中看到我们是如何通过方向小键盘来控制一个`sprite`的。尝试运行一下，使用方向按键上下左右的动一动，让`cat`在舞台上跑一跑吧，have fun!

![image](https://raw.githubusercontent.com/kittykatattack/learningPixi/master/examples/images/screenshots/17.png)

下面的代码展示了全部内容:

```js
function setup() {

  //创建`cat`
  cat = new Sprite("images/cat.png");
  cat.y = 96;
  cat.vx = 0;
  cat.vy = 0;
  stage.addChild(cat);

  //监听键盘方向键
  var left = keyboard(37),
      up = keyboard(38),
      right = keyboard(39),
      down = keyboard(40);

  //左方向键`press`方法
  left.press = function() {

    //改变`cat`的速度值
    cat.vx = -5;
    cat.vy = 0;
  };

  //左方向键`release`方法
  left.release = function() {

    //如果左方向键被释放，并且右方向键没有被按下
    //那么cat将没有移动速度，从而暂停
    if (!right.isDown && cat.vy === 0) {
      cat.vx = 0;
    }
  };

  //向上
  up.press = function() {
    cat.vy = -5;
    cat.vx = 0;
  };
  up.release = function() {
    if (!down.isDown && cat.vx === 0) {
      cat.vy = 0;
    }
  };

  //向右
  right.press = function() {
    cat.vx = 5;
    cat.vy = 0;
  };
  right.release = function() {
    if (!left.isDown && cat.vy === 0) {
      cat.vx = 0;
    }
  };

  //向下
  down.press = function() {
    cat.vy = 5;
    cat.vx = 0;
  };
  down.release = function() {
    if (!up.isDown && cat.vx === 0) {
      cat.vy = 0;
    }
  };

  //设置游戏状态
  state = play;

  //启动游戏循环
  gameLoop();
}

function gameLoop() {
  requestAnimationFrame(gameLoop);
  state();
  renderer.render(stage);
}

function play() {

  //使用cat的速度值使其运动
  cat.x += cat.vx;
  cat.y += cat.vy
}
```