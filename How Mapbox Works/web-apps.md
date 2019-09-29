---
标题: Web applications
description: Learn how Mapbox’s JavaScript libraries work and how to integrate them into your web applications.
描述:学习Mapbox的JavaScript库如何工作,以及如何将它们应用到您的web应用中;
image: /img/narrative/web-develop.svg
topics:
主题:
- web apps
prependJs:
- "import as constants from '../../constants';"
- "import Note from '@mapbox/dr-ui/note';"
- "import BookImage from '@mapbox/dr-ui/book-image';"
- "import DemoIframe from '@mapbox/dr-ui/demo-iframe';"
- "import LegacyNote from '../../components/legacy-note';"
contentType: guide
内容类型:指南
---

Mapbox 提供很多工具来在您的网站或者web基础的应用中构建地图。 Mapbox GL JS(https://www.mapbox.com/mapbox-gl-js)和Mapbox.js(https://www.mapbox.com/mapbox.js) 都是您可以用来展示您的Mapbox地图、添加交互性、以及在您的应用中自定义地图体验的JavaScript开源库。我们还提供很多插件,通过与绘制工具或和Mapbox Web服务API(如Mapbox 地理编码 API或Mapbox 导航 API)接口交互来丰富您的web地图功能。使用我们的JavaScript库来构建web页面或者web应用程序是需要写代码的，但是本指南旨在为您提供入门的信息。
<div class='bg-white border-b'>
<div class='txt-m txt-bold'>Web app using Mapbox GL JS</div>
<div class='txt-xs pb6'>Combine our JavaScript library with HTML, CSS, and JavaScript</div>
</div>

{
<DemoIframe src="/help/demos/gl-store-locator/step-five.html" />
}

{<div className='caption'>}

这个web应用综合使用了HTML, CSS, JavaScript和我们GL-based的JavaScript库 Mapbox GL JS。如果您有兴趣想学习更多关于如何构建类似应用的知识，请阅读详细教程Build a store locator(/help/tutorials/building-a-store-locator)。
{</div>}


## web apps如何工作
一个web地图绘制库允许您添加地图到web页面并定义其包含的数据，其显示和各种功能。可以把它想象成一个有很多地图制作工具的工具箱，这些工具可以一起用来构建美观和自定义的交互体验。

### Mapbox GL JS

Mapbox GL JS(https://www.mapbox.com/mapbox-gl-js)是一个使用我们现代的地图绘制技术构建web应用的JavaScript库。本指南将介绍Mapbox GL JS的一些基本功能和常见模式，重点介绍将Mapbox GL JS与其他地图库区分开来的一些核心概念。对于那些有使用Leaflet(/help/glossary/leaflet/), Mapbox.js(/help/glossary/mapbox-js/), 或OpenLayers经验的人，接下来的几节将介绍Mapbox GL JS的一些不同之处以及一些看起来应该很熟悉的功能。
#### 客户端渲染

Mapbox GL JS 的核心就是客户端渲染。在web应用中使用Mapbox GL JS，地图将通过使用JavaScript 和WebGL将vector tiles(/help/glossary/vector-tiles/) 与 style rules(https://www.mapbox.com/mapbox-gl-style-spec/) 结合动态渲染。在浏览器中而不是在服务器上渲染地图可以更方便的改变地图的样式、动态显示的数据、和用户交互的响应。
#### 相机
相机是地图的视野。在像Leaflet或Mapbox.js的系统中，视点是由地图的中心点和缩放级别决定的，Mapbox GL JS也包含一些用于调整地图视角的，像俯仰角和方位角这样的参数。

中心：经度纬度顺序组成的坐标
缩放：在缩放范围内的任何数字，包括小数。例如，1.5或者6.2也是有效的缩放级别。
方位角：一个介于0-360度范围内用来确定地图的方位或旋转的值。
俯仰角：一个介于0-60度范围内用来确定地图的斜度或者俯仰角的值。

这里有一个用到了方位角和缩放的例子：
{
<DemoIframe src="/help/demos/gl-js-fundamentals/bearingandzoom.html" />
}

#### 图层

传统的JavaScript地图库通常会有两个不同的类，即 "layers(/help/glossary/layer/)": baselayers(/help/glossary/baselayer/)，或提供地图基础的影像切片，和overlays，它们通常是像GeoJSON 这样可以显示在baselayers上面的矢量数据，有时会模糊像标签这样的详细信息。
Mapbox GL JS 在baselayers和overlay layers之间并没有区分 。这意味着地图的详情像标签和图标还有像街道和建筑这样的元素是可以通过JavaScript来修改的，就像在早期的地图绘制库中的overlays。每个图层提供在浏览器中渲染器应该如何绘制某些数据的规则，然后渲染器使用这些图层在屏幕上绘制地图。
#### Mapbox GL JS and Mapbox GL Native

Mapbox GL JS和Mapbox GL Native
Mapbox GL JS(https://www.mapbox.com/mapbox-gl-js) 和 Mapbox GL Native(https://github.com/mapbox/mapbox-gl-native) 是通过构建Mapbox样式标准(https://www.mapbox.com/mapbox-gl-style-spec)来渲染地图的两个不同项目.他们是相似的，但是用于不同的目的并没有100%的相似。
-Mapbox GL JS是一个为web端制作地图的JavaScript库。它可以读取Mapbox样式标准并使用WebGL在现代浏览器中渲染地图。
-Mapbox GL Native是 Mapbox Maps SDKs为支持iOS(https://www.mapbox.com/ios-sdk)和Android(https://www.mapbox.com/android-sdk)打造的核心基础.它支持 iOS和Android的手持平台使用OpenGL ES。
### Mapbox.js

{
<LegacyNote
legacyProduct='Mapbox.js'
alternativeResource={{
title: 'the Mapbox GL JS documentation',
link: 'https://docs.mapbox.com/mapbox-gl-js/overview/'
}
/>
}}

Mapbox.js 是拓展自流行的Leaflet.js(/help/glossary/leaflet)的一个web地图绘制库 ，但是同样也与Mapbox技术栈深度融合。您可以在它的API文档(https://www.mapbox.com/mapbox.js/api/)中学习更多关于 Mapbox.js 的知识。
## 创建一个web应用
创建web地图应用，您需要对HTML、CSS和JavaScript有一定的熟悉度。如果您是刚刚接触web地图应用，可以通过浏览我们的教程(/help/tutorials/)来帮助您开始。
### Mapbox GL JS

Before getting started coding up your Mapbox GL JS map, you'll need to include the relevant JavaScript and CSS files in your webpage. Mapbox provides hosted versions of each that you can include in the <head> of your HTML file:
在开始编码您的Mapbox GL JS地图之前，您需要在您的web页面中引用相关的JavaScript和 CSS文件。Mapbox提供了每一个版本的托管，您可以使用<head>标签来引用到您的HTML文件中。
html
<script src='https://api.mapbox.com/mapbox-gl-js/{{constants.VERSION_MAPBOXGLJS}}/mapbox-gl.js'></script>
<link href='https://api.mapbox.com/mapbox-gl-js/{{constants.VERSION_MAPBOXGLJS}}/mapbox-gl.css' rel='stylesheet' />


The basis of every Mapbox GL JS project is the mapboxgl.Map class. The example code in this section demonstrates the minimum you need to add a map to your page.
每一个Mapbox GL JS项目的基础是mapboxgl.Map类。本节的示例代码示范了添加一个地图到您的页面中所必须的内容。
<div class='fr pl12' style='width:50%'>
<pre>
var map = new mapboxgl.Map(
container: 'map',
style: 'mapbox://styles/mapbox/streets-v{{constants.VERSIONSTREETSSTYLE}',
center: -74.50, 40,
zoom: 9
});
</pre>
</div>

容器: 容器就是您放置地图的HTML元素。在上面的例子中，他是id="map"这个元素。

- 样式: 地图通过URLmapbox://styles/mapbox/streets-v{{constants.VERSION_STREETS_STYLE}}来加载样式。这是一个远程文件的URL，因此地图将下载该文件来决定它包含的 tilesets(/help/glossary/tileset/)以及它们对最终用户的展示方式。Mapbox GL JS 允许在许多地方（包括数据源）使用URL来代替文字数据。考虑使用 style-optimized vector tiles(/help/glossary/style-optimized-vector-tiles/)样式来获得更高性能的地图。

- 中心: Mapbox GL JS将坐标作为数组处理(如 [-74.50, 40])，它假定坐标是以经度、纬度(/help/glossary/lat-lon/) 的顺序组织的(相对于Leaflet和Mapbox.js中的纬度、经度顺序)。这个顺序和geojson还有其他地理空间格式中的坐标顺序，以及数学中的x，y顺序是一致的。
- 缩放: 应初始化地图的缩放级别。使用Mapbox GL JS，可以是十进制的值。
#### 添加图层到地图
您可以使用 addLayer() 方法来将图层添加到地图上。addLayer(https://www.mapbox.com/mapbox-gl-js/api/#Map.addLayer)方法只有一个必需参数：一个Mapbox样式图层对象。它也接收一个可选的before参数，这个参数是在插入新图层之前已经存在的图层的ID。如果您没有传这个before参数，渲染器会将图层绘制在地图的最上方。下面的章节描述了Mapbox样式图层对象的元素。
##### 异步
<div class='fr pl12' style='width:50%'>
<pre>
map.on('load', function() 
map.addLayer({
id: 'terrain-data',
type: 'line',
source: {
type: 'vector',
url: 'mapbox://mapbox.mapbox-terrain-v2'
,
'source-layer': 'contour'
});
});
</pre>
</div>

因为这些资源是 远程的，所以他们是 异步的。因此连接Mapbox GL JS 的代码经常使用事件绑定来在正确的时间改变地图。例如：
上面的代码使用map.on('load', function() {使得在完成地图资源，包括样式加载之后来调用map.addLayer方法。如果立即运行map.addLayer 方法，因为您想要添加的图层的样式还不存在将会出错。
##### 指定资源
当您添加一个新图层的时候您需要定义一个源。源接收type和url 参数（如果是GeoJSON的源将不需要url）。这里有五种类型的源，以及他们每一个相对应的属性：
- vector tiles(https://www.mapbox.com/mapbox-gl-style-spec/#sources-vector)
- raster tiles(https://www.mapbox.com/mapbox-gl-style-spec/#sources-raster)
- GeoJSON(https://www.mapbox.com/mapbox-gl-style-spec/#sources-geojson)
- image(https://www.mapbox.com/mapbox-gl-style-spec/#sources-image)
- video(https://www.mapbox.com/mapbox-gl-style-spec/#sources-video)

Tilesets可以包含多个称为source layers (/help/glossary/source-layer/)的数据子集 （ Mapbox的街道tileset就包含了像道路，公园等这样的源图层）。为了确保您的图层引用了正确的源图层，您的图层对象也需要包含source-layer（通常是原始文件的名称）。来看下面这个例子。
{
<Note title='addSource()' imageComponent={<BookImage />>
<p>您也可用使用 Mapbox GL JS的 <code>addSource()</code>方法来添加源。当使用这个替代方法的时候在地图展示方面并没有什么不同，但是有些时候在保持代码的可读性方面会更好一些。可以在 <a href="https://www.mapbox.com/mapbox-gl-js/api/#Map#addSource">Mapbox GL JS documentation</a>阅读更多关于这个方法的介绍。 </p>
</Note>
}}

js
map.on('load', function() {
map.addLayer({
id: 'rpd_parks',
type: 'fill',
source: {
type: 'vector',
url: 'mapbox://mapbox.3o7ubwm8'
},
'source-layer': 'RPD_Parks'
});
});


对于更多关于每种源文件类型的信息，您可以浏览Sources章节Mapbox Style Specification(https://www.mapbox.com/mapbox-gl-style-spec#sources)。
##### 指定排版和绘制属性
图层要素有两个特殊的属性可以启用数据样式: paint(https://www.mapbox.com/mapbox-gl-style-spec/#paint)和layout(https://www.mapbox.com/mapbox-gl-style-spec/#layout)。它们被用来定义数据在地图上面的呈现方式。layout 属性指的是放置位置和可见性，以及其他高级首选项，并且用在渲染进程的早期。paint 属性是一个更加细颗粒度的样式属性像不透明，颜色和转变。它们处理密集程度比较低，并且在之后渲染。

下面的代码添加一个图层到地图，并用绿色填充来设置公园数据的样式。
{
<Note title='addLayer()' imageComponent={<BookImage />>
<p>If you added your source using the alternative <code>addSource()</code> method, you will need to include the source id as the <code>source</code> in <code>addLayer()</code>. Read more about this in the <a href="https://www.mapbox.com/mapbox-gl-js/api/#Map#addLayer">Mapbox GL JS API documentation.</a></p>

<p>如果您使用替代方法 <code>addSource()</code> 方法来添加您的源，您需要在 <code>addLayer()</code> 方法中包含源的id作为 <code>source</code>。如果您想阅读更多关于这个主题的内容请在Mapbox GL JS API 文档中参考 <a href="https://www.mapbox.com/mapbox-gl-js/api/#Map#addLayer"> </p>
</Note>
}}

js
map.on('load', function() {
map.addLayer({
id: 'rpd_parks',
type: 'fill',
source: {
type: 'vector',
url: 'mapbox://mapbox.3o7ubwm8'
},
'source-layer': 'RPD_Parks',
layout: {
visibility: 'visible'
},
paint: {
'fill-color': 'rgba(61,153,80,0.55)'
}
});
});


最终产品：地图缩放到旧金山，公园图层用绿色填充。这个图层是基于城市公园土地数据的矢量源。
可以查看教程章节(/help/tutorials/)来获取更多关于 Mapbox GL JS 的信息。
### Mapbox.js

{
<LegacyNote
legacyProduct='Mapbox.js'
alternativeResource={{
title: 'the Mapbox GL JS documentation',
link: 'https://docs.mapbox.com/mapbox-gl-js/overview/'
}
/>
}}


#### 将地图添加到页面
See our example to add a map with Mapbox.js(https://www.mapbox.com/mapbox.js/example/v1.0.0/).
Mapbox.js 的核心功能是添加一个地图到您的HTML页面。只需使用一行JS代码您就可以在您的web页面添加一个可以平移和缩放的地图，并为其设定特定的位置和缩放级别(https://www.mapbox.com/mapbox.js/api/{constants.VERSIONMAPBOXJS}/l-map-class/#map-options)。


可以查看我们通过Mapbox.js 添加地图的例子(https://www.mapbox.com/mapbox.js/example/v1.0.0/)。
#### 添加自定义数据并设置其样式
如果您想添加您自己的数据到您的 Mapbox.js地图，当然可以！Mapbox.js支持几种不同的格式，包括GeoJSON(/help/glossary/geojson)。然后您可以通过在simplestyle specification(/help/glossary/simplestyle/)中将样式属性添加到GeoJSON或者使用内置的 setStyle方法来设置GeoJSON数据的样式。

可以参考如何添加GeoJSON格式的数据到地图并使用simplestyle specification来对它进行样式设置的例子(https://www.mapbox.com/mapbox.js/example/v1.0.0/single-marker/)。
## 使用插件来扩展您的web应用程序
Mapbox GL JS和Mapbox.js都支持丰富的插件生态系统，可用来扩展web地图的功能。这些插件可以用于添加交互式绘图工具、添加嵌入式地图、与Mapbox地理编码API和Mapbox导航API集成等等！可以浏览 Mapbox GL JS plugins page(https://www.mapbox.com/mapbox-gl-js/plugins) 和 Mapbox.js plugins page(https://www.mapbox.com/mapbox.js/plugins/)来获取更多这方面的信息。

## 将React与Mapbox GL JS一起使用
Mapbox GL JS可用与各种JavaScript框架一起使用，包括React(https://facebook.github.io/react/)。想学习更多关于 Mapbox GL JS与React一起使用的知识，可以查阅我们在GitHub上面关于Mapbox react的例子(https://github.com/mapbox/mapbox-react-examples)。
