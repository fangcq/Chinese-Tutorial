---
title: 使用Mapbox GL JS制作一张热图
description:添加自定义HTML标记，设置样式，并使用Mapbox GL JS添加工具提示.
thumbnail: makeAHeatmapWithMapboxGlJs
level: 2
topics:
- web apps
- map design
- data
language:
- JavaScript
prereq: 熟悉前端开发概念.
prependJs:
  - "从'../../constants'导入常量;"
  - "从'@mapbox/dr-ui/note'导入注释;"
  - 从'@mapbox/dr-ui/book-image'导入BookImage;"
  - "从'@mapbox/mr-ui/icon'导入图标;"
  - "从'../../components/user-access-token'导入UserAccessToken;"
  - "从'@ mapbox / dr-ui / demo-iframe'导入DemoIframe;"
  - "从'@mapbox/mr-ui/button'导入按钮;"
contentType: 初学者
---

在这篇教程中, 你会学到如何使用[Mapbox GL JS](https://www.mapbox.com/mapbox-gl-js/api/) 来制作一张热图。 热图用于以视觉上吸引人的方式地展示海量的数据信息，并且鼓励你的用户积极探索你所开发的地图。

{{
  <DemoIframe src="/help/demos/heatmap/index.html" />
}}

## 准备开始

- **一个 Mapbox 账号以及相应的 access token**. 在 mapbox.com/signup 注册一个账号，你可以在你的账号界面[Account page](https://www.mapbox.com/account)找到你拥有的access tokens。
- [**Mapbox GL JS**](https://www.mapbox.com/mapbox-gl-js). 包含Mapbox 中用于制作web地图的JavaScript API接口.
- **Data**. 在这篇教程中, 你将使用一个包含匹斯堡市街道树木信息 [Western Pennsylvania Regional Data Center](http://www.wprdc.org/)的GeoJSON 文件.

{{
<Button href="/help/demos/heatmap/trees.geojson" passthroughProps={{ download: "trees.geojson" }} >
    <Icon name='arrow-down' inline={true} /> 下载GeoJSON
</Button>
}}

## 热图的主要目的是什么?

"热图"这个术语可以指许多不同种类的制图可视化，你可能看到他用于总统选举支持率图，人口密度图，甚至是气象图。

在你网络上可以找到的地图中，最常见的有两种热图：第一种是鼓励用户取探索密集点数据的热图，第二种是连续曲面上离散插值，并且在这些点直接创建平滑渐变的热图。后者比前者用的更少一些，它通常被用于科学出版物或者当一个现象以一种可预测的方式分布在一个区域时。例如，你的城市可能只有很少的几个气象预测站，但是你最喜欢的天气应用程序会显示整个城市区域的平滑温度梯度。对于你的城镇本地天气服务，我们可以合理地假设：如果两个相邻的气象站点报告了不同的温度，那么他们之间的温度将会平滑地从一个站点变化到下一个站点。

这篇教程的目的在于, 我们指出一种不同类型的可视化，它能更好地展示区域上点的密集度。这种类型的热图并不是通过以[choropleth](/help/tutorials/choropleth-studio-gl-pt-1/)映射的方式来聚合一组边界内的特征点来显示密度, 而是在两个点之间连续地渐变来展示密度。 

热图不仅用于可视化地展示密度，它还有助于可视化地展示这些点之间的差异。你可以根据每个点在数据集中的特征值来为每个点分配更高或者更低的权重。在这篇教程中，你将会你数据集中的`DBH`属性进行加权. `DBH` 代表 "胸部直径" ，并且这是在离地4.5英尺测量树木直径的标准方法。通常而言，可以安全地假设拥有更高DBH的树更加年老且体积更大。当你给这些比较大的树一些相比较小树苗更高的权重时，你的可视化可以是区域森林覆盖率的有效近似。 

## 热图绘制属性
<!-- copyeditor ignore represents -->
添加一个热图图层 [heatmap layer](https://www.mapbox.com/mapbox-gl-js/style-spec/#layers-heatmap) 到你的地图, 你需要配置一些属性。理解这些属性含义是创建出准确表示你的数据地图的关键，并且在太多细节和简单通用之间找到正确的平衡。
- **heatmap-weight热图权重**: 用于衡量每个点各自为你的热图表现起多大的作用。热图图层的权重默认是1.0，这意味着所有点的权重都是均等的。把 `heatmap-weight` 属性增加到5.0 和把五个点放在同一个位置具有同样的效果。你可以使用一个暂停的功能来为每个点设置基于指定属性的权重。
- **heatmap-intensity热图强度**: 一个在 `heatmap-weight`之上的乘数， 主要用于方便地根据缩放级别来调整热图外观。
- **heatmap-color热图颜色**: 定义了热图的颜色渐变，从最小值到最大值。颜色取决于每个像素点的热图密度 [heatmap-density](https://www.mapbox.com/mapbox-gl-js/style-spec#expressions-heatmap-density) (范围从`0.0` 到 `1.0`)，这个属性值是 [expression](https://www.mapbox.com/mapbox-gl-js/style-spec/#expressions) 它使用热图密度作为输入。有关热图颜色的选择灵感，请查看 [Color Brewer](http://colorbrewer2.org/#type=sequential&scheme=BuGn&n=3).
- **heatmap-radius热图半径**: 为每个点设置以像素为单位的半径，半径越大，热图越平滑，而细节也越少。
- **heatmap-opacity热图不透明度**: 控制热图图层的全局不透明度.

## 使用 Mapbox GL JS 创建你的地图

现在你理解了热图的目的和你可能用到的绘制属性，是时候来建立你的地图了。在这个示例中，你将会使用 Mapbox Dark 主题 [template style](https://www.mapbox.com/studio-manual/reference/styles/#mapbox-template-styles)。 你可以在 [our API documentation](https://docs.mapbox.com/api/maps/#styles)中找到每种模板样式的 [Style URLs](/help/glossary/style-url)。

在你的文本编辑器中，创建一个新的 `index.html` 文件，接下来复制并粘贴下面的代码，确保你把 `{{ <UserAccessToken /> }}`替换为你个人账号中的 [access token](/help/glossary/access-token/)。把代码添加完毕并保存到 `index.html` 文件中之后，你就可以在浏览器中浏览并确保你可以正确查看到你的地图。

{{
  <Note imageComponent={<BookImage />}>
    <p>请务必把 GeoJSON 文件保存并放到和你项目相同的目录中，你需要在本地服务器运行这个应用，否则你会收到 <a href='http://en.wikipedia.org/wiki/Cross-origin_resource_sharing'>跨资源共享(CORS)</a> 的错误提示。 <a href='http://www.2ality.com/2014/06/simple-http-server.html'>Python的SimpleHTTPServer</a> 在许多计算机中都已经包含，并且如果这是你第一次运行本地服务器，这将是一个很好的选择。</p>
  </Note>
}}

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset='utf-8' />
    <title></title>
    <meta name='viewport' content='initial-scale=1,maximum-scale=1,user-scalable=no' />
    <script src='https://api.tiles.mapbox.com/mapbox-gl-js/{{constants.VERSION_MAPBOXGLJS}}/mapbox-gl.js'></script>
    <link href='https://api.tiles.mapbox.com/mapbox-gl-js/{{constants.VERSION_MAPBOXGLJS}}/mapbox-gl.css' rel='stylesheet' />
    <style>
      body {
        margin: 0;
        padding: 0;
      }

      #map {
        position: absolute;
        top: 0;
        bottom: 0;
        width: 100%;
      }
    </style>
</head>
<body>
  <div id='map'></div>
  <script>
    mapboxgl.accessToken = '{{ <UserAccessToken /> }}';
    var map = new mapboxgl.Map({
      container: 'map',
      style: 'mapbox://styles/mapbox/dark-v{{constants.VERSION_DARK_STYLE}}',
      center: [-79.999732, 40.4374],
      zoom: 11
    });

   // we will add more code here in the next steps

  </script>
</body>
</html>

```
### 添加你的数据

你需要首先把本教程开头要求下载的 GeoJSON 文件添加为热图的来源。 你可以通过使用 [`addSource`](https://www.mapbox.com/mapbox-gl-js/api/#map#addsource) 方法来实现这个操作。这个来源文件不仅仅用于制作热图图层，还可以制作圆图层。热图图层将会在圆图层淡入的时候淡出，以便在更高缩放级别显示各个数据点。在你完成前一步的初始化操作后，添加以下代码。
```js
map.on('load', function() {

  map.addSource('trees', {
    type: 'geojson',
    data: './trees.geojson'
  });
  // add heatmap layer here
  // add circle layer here
});
```

### 添加热图图层

下一步，使用 [`addLayer`](https://www.mapbox.com/mapbox-gl-js/api/#map#addlayer) 方法来为你的热图创建一个新的图层。创建完图层之后，你将利用前面所提到的热图属性来微调热图的外观。

对于 `heatmap-weight`，指定一个反映你数据的范围 (`dbh` 属性的范围是 GeoJSON源中的 1-62)。因为较大的树有较高的 `dbh`，所以通过创建一个暂停函数来给这些树一些更加高的权重，当 `dbh` 增加时，`heatmap-weight`也会增加。

因为 `heatmap-intensity` 是 `heatmap-weight`之上的乘数，`heatmap-intensity` 可以随着地图缩放等级而增大，以在整个变焦范围内保持类似的外观。下面的图片显示了 `heatmap-intensity` 对你的地图外观影响，左边的图展示了`heatmap-intensity`随缩放等级而增大的地图外观，而右边图片显示了`heatmap-intensity`使用默认值 `1`时的地图外观.

{{
<div className='grid'>
  <img className='col' alt='demonstrates a heatmap-intensity that increases with zoom level' style={{ width: "50%", paddingRight: "10px", height: "50%" }} src='/help/img/gl-js/heatmap-intensity-two.png' />
  <img className='col' alt='demonstrates heatmap intensity default of one' style={{ width: "50%", paddingLeft: "10px", height: "50%" }} src='/help/img/gl-js/heatmap-intensity-one.png' />
</div>
}}

对于 `heatmap-color`，添加一个 [interpolate](https://www.mapbox.com/mapbox-gl-js/style-spec/#expressions-interpolate) 表达式，用一组输入-输出的集合来定义一个热图密度和热图颜色之间的线性关系。如果你对学习更多的Mapbox GL JS 表达式有兴趣，阅读以下两个文档：[Get Started with Mapbox GL JS expressions guide](/help/tutorials/mapbox-gl-js-expressions) 和 [Mapbox GL JS documentation](https://www.mapbox.com/mapbox-gl-js/style-spec/#expressions)。

通过设置`heatmap-radius` 和`heatmap-opacity`的值来完成热图图层的配置。`heatmap-radius` 应该随着缩放等级的增高而增大，以保持热图的平滑度，因为点变得更加分散。`heatmap-opacity` 应该在缩放级别介于14和15之间时，刚好平滑地从 `1` 减少到 `0`，因为你的圆图层在这时淡入并代替热图图层。在`addSource` 方法之后的load事件中添加以下代码。

```js
map.addLayer({
  id: 'trees-heat',
  type: 'heatmap',
  source: 'trees',
  maxzoom: 15,
  paint: {
    // increase weight as diameter breast height increases
    'heatmap-weight': {
      property: 'dbh',
      type: 'exponential',
      stops: [
        [1, 0],
        [62, 1]
      ]
    },
    // increase intensity as zoom level increases
    'heatmap-intensity': {
      stops: [
        [11, 1],
        [15, 3]
      ]
    },
    // assign color values be applied to points depending on their density
    'heatmap-color': [
      'interpolate',
      ['linear'],
      ['heatmap-density'],
      0, 'rgba(236,222,239,0)',
      0.2, 'rgb(208,209,230)',
      0.4, 'rgb(166,189,219)',
      0.6, 'rgb(103,169,207)',
      0.8, 'rgb(28,144,153)'
    ],
    // increase radius as zoom increases
    'heatmap-radius': {
      stops: [
        [11, 15],
        [15, 20]
      ]
    },
    // decrease opacity to transition into the circle layer
    'heatmap-opacity': {
      default: 1,
      stops: [
        [14, 1],
        [15, 0]
      ]
    },
  }
}, 'waterway-label');
```

### 添加圆图层
下一步，添加一个圆图层。当你放大你的热图时，这些点在视觉上会停止重叠，并不再需要显示它们的分布和密度。此时，你可以把点本身显示出来并且允许查看者以交互的方式浏览数据。

记住你在上一步是如何在缩放等级介于14到15之间时把热图图层淡出的吗？你需要把图层替换为圆图层，通过让圆图层淡入来实现，使用一个缩放函数function来增加缩放等级14和15之间圆图层的不透明度 `circle-opacity`。对于圆图层半径 `circle-radius`，使用一个缩放和属性的函数function来增加缩放级别和属性半径(如下所示)。添加以下代码到上一步热图层之后。

```js
map.addLayer({
  id: 'trees-point',
  type: 'circle',
  source: 'trees',
  minzoom: 14,
  paint: {
    // increase the radius of the circle as the zoom level and dbh value increases
    'circle-radius': {
      property: 'dbh',
      type: 'exponential',
      stops: [
        [{ zoom: 15, value: 1 }, 5],
        [{ zoom: 15, value: 62 }, 10],
        [{ zoom: 22, value: 1 }, 20],
        [{ zoom: 22, value: 62 }, 50],
      ]
    },
    'circle-color': {
      property: 'dbh',
      type: 'exponential',
      stops: [
        [0, 'rgba(236,222,239,0)'],
        [10, 'rgb(236,222,239)'],
        [20, 'rgb(208,209,230)'],
        [30, 'rgb(166,189,219)'],
        [40, 'rgb(103,169,207)'],
        [50, 'rgb(28,144,153)'],
        [60, 'rgb(1,108,89)']
      ]
    },
    'circle-stroke-color': 'white',
    'circle-stroke-width': 1,
    'circle-opacity': {
      stops: [
        [14, 0],
        [15, 1]
      ]
    }
  }
}, 'waterway-label');
```

## 添加一些额外的交互
以下的代码通过允许观看者点击你的圆图层来查看包含树木`DBH`值的弹出窗口，从而为你的地图添加了交互性，圆图层代码之后添加以下代码即可实现。

{{<img src='/help/img/gl-js/heatmap-popup.gif' alt='demonstrates how to add a popup to the circle layer of a heatmap' className="w-full" />}}

```js
map.on('click', 'trees-point', function(e) {
  new mapboxgl.Popup()
    .setLngLat(e.features[0].geometry.coordinates)
    .setHTML('<b>DBH:</b> ' + e.features[0].properties.dbh)
    .addTo(map);
});

```

## 完成你的作品

恭喜! 你已经使用我们的 Mapbox GL JS制作完成了属于你的第一张热图!♥

{{
  <DemoIframe src="/help/demos/heatmap/index.html" />
}}
