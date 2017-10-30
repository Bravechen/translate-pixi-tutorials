# 加载纹理地图

为了把纹理地图加载入Pixi，我们需要使用Pixi的`loader`。如果`JSON`文件来源于`Texture Packer`，`loader`对象可以自动解析数据，并将瓦片集中的每一帧图像都创建到纹理缓存中。下面的代码展示了如何加载`treasureHunter.json`文件，在文件加载完成后，`setup`函数将被调用。

```js
loader
  .add("images/treasureHunter.json")
  .load(setup);
```

每个瓦片集中的图片现在都独立存储在Pixi的纹理缓存中。你可以随意访问任何一个缓存中的纹理数据，所需要的仅仅是一个名字。一个和你在Texture Packer中设置的相同的名字（例如：`“blob.png”`、`“dungeon.png”`和`“explorer.png”`）。