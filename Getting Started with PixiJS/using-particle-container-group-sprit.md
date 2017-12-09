## 使用`ParticleContainer`

除了之前提到的组合`sprite`方法之外，Pixi还提供了一种可选的、高性能的方式称为`ParticleContainer`（`PIXI.particles.ParticleContainer`）。它渲染的速度要比普通容器快2到5倍。非常有利于提升游戏的性能。

可以这样创建一个`ParticleContainer`:

```js
let superFastSprites = new PIXI.particles.ParticleContainer();
```

然后使用`addChild`来添加`sprite`，就像我们之前使用普通容器时的方式一样。

当然啦，有得必有失，我们也不得不在使用`ParticleContainer`时妥协一些事情。
`ParticleContainer`中的`sprite`只能使用一些有限并且基础的属性，差不多只有`x,y,width,height,scale,pivot,alpha,visible`这些了。
并且，这些`sprite`还不能有自己的子级显示对象。
再者，也不允许`ParticleContainer`使用滤镜和混合模式(blend mode)等Pixi的高级显示效果。
纹理多了也不行，`ParticleContainer`只允许使用一个纹理（所以如果想要每个sprite都有不同的显示，只好使用一张`spritesheet`）。
咋一看限制还挺多，但是对比于我们获得的性能提升，还是值得的。况且我们还可以在同一个项目中混合使用`Containers`和`ParticleContainers`，这对于我们进行优化微调还是相当有利的。

很好奇为什么`ParticleContainer`能渲染这么快？因为他们都是直接使用GPU进行处理的。Pixi开发团队致力于将尽可能多的`sprite`通过GPU进行处理。因此，当你使用最新版本的Pixi.js时，`ParticleContainer`可能已经有了更为丰富的特性。我建议你查看一下[文档](http://pixijs.download/release/docs/PIXI.particles.ParticleContainer.html)，以补本文介绍之不足。

创建`ParticleContainer`时，还可以附加4个可选的参数：`maxSize,properties,batchSize,autoResize`。

```js
let superFastSprites = new ParticleContainer(maxSize, properties, batchSize, autoResize);
```

`maxSize`的默认值是`15,000`。如果需要容纳更多`sprite`，可以设置更高的数值。
`properties`参数接受一个对象，其中包含五个值为布尔值(`Boolean`)的属性：`scale,position,rotation,uvs,alphaAndTint`。
除了`position`的默认值是`true`，其它4个属性值均为`false`。当需要修改`ParticleContainer`中sprite的`rotation`、`scale`、`tine`(译注：这里作者似乎写的有误，因该是`tint`（着色）更对?)和`uvs`时，我们可以像下面这样修改属性为`true`：

```js
let superFastSprites = new ParticleContainer(
  size,
  {
    rotation: true,
    alphaAndtint: true,
    scale: true,
    uvs: true
  }
);
```

不过，如果确定不需要用到这些属性，那么保持他们为`false`，有利于产生最好的性能输出。

`uvs`属性在此的作用是什么呢？在某些项目中，我们需要在动画的过程中改变粒子的纹理，这时就可以将这个属性设置为`true`。（切记，这些`sprite`所用的纹理要来自于同一张瓦片集图片）。

（注意：`UV`是一种用于3D图形展示的技术，主要用来将纹理(图片)的`x`和`y`坐标映射到3d表面以显示纹理图形。`U`指代的是`x`轴，`V`则是`y`轴。我们知道在`WebGL`中通常使用`x,y,z`来表示3D空间的位置，因此，`U`和`V`就被用来解决如何在三维空间描绘二维图形纹理的问题。）

（对于最后两个没介绍的属性`batchSize`和`autoResize`，其实我也不太确定他们的功能，所以，如果你知道话别忘了告诉我！）