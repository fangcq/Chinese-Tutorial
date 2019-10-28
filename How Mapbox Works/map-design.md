---
标题: 地图设计
描述: 学习Mapbox样式的工作原理的以及从哪里学到更多信息并开始设计你的地图.
图像: /img/narrative/design.svg
主题:
  - 地图设计
prependJs:
  - "import DemoIframe from '@mapbox/dr-ui/demo-iframe';"
内容类型: 教程
---

你能用Mapbox控制地图设计过程中的几乎每个方面，包括上传自定义数据，调整地图配色方案，添加字体，新建数据驱动的可视化等等。地图设计过程的核心是 **样式**: 用一个准确定义如何绘制地图的JSON文件。因为所有的Mapbox样式符合开放资源[Mapbox 样式规范](https://www.mapbox.com/mapbox-gl-js/style-spec/), 你的地图能在多个平台上一致地呈现，包括在网页版Mapbox GL JS，Mapbox静态图像API，在移动设备上适用于Android和iOS的Mapbox地图SDK，以及任何能读取Mapbox样式的第三方库。这个教程介绍了Mapbox样式的工作原理以及你可以在哪里了解更多信息并开始设计你的地图.

{{
  <DemoIframe src="/help/demos/how-mapbox-works/how-styles-work.html" />
}}

样式文档或样式JSON，包含地图配色者使用的样式规则，用来显示浏览器或设备中的地图。 在这个示例中，你可以看到样式的数据和图像，以及呈现在左侧的**样式 JSON** 和右侧的**显示实时地图** 中如何显示它们的说明. 在Mapbox GL使用样式JSON及其相应的瓦片集绘制最终地图后，地图将显示在浏览器中.

## 地图样式是如何工作的

你必须严格按照规范编写JSON样式，以便渲染器去解释它并在浏览器中显示地图.

### Mapbox 样式规范

[Mapbox 样式规范](https://www.mapbox.com/mapbox-gl-style-spec/) 定义样式文档中应包含的信息，供渲染器显示地图，包括标题，初始 [相机](/help/glossary/camera/) 位置的值, 样式中使用的源和其他资源，以及地图图层的样式规则。 完整的要求都列在Mapbox样式规范中，其中一些需要理解的关键概念有:

你在浏览器或设备上看到的地图是将 **样式规则** (样式JSON)应用于 **数据源** (通常是地图图块或GeoJSON) 以呈现完整地图的结果。 在Mapbox样式规范的语言中，数据源称为 **源** ，数据的样式规则被组织为 **图层**. 如果不指定源和图层，则无法创建地图.

- [**Sources**](https://www.mapbox.com/mapbox-gl-js/style-spec/#sources). 数据源告诉渲染器你想要包含哪种数据以及在哪里找到它.
- [**Layers**](https://www.mapbox.com/mapbox-gl-js/style-spec/#layers). 一个图层是代表数据源中数据的样式。 它包含地图上图层显示方式的信息，包括颜色，透明度，字体等.

如果你要在地图中使用图标，图像或字体，那你的样式将需要包含一个 `sprite` 或`glyphs` 属性.

- [**Sprite**](https://www.mapbox.com/mapbox-gl-js/style-spec/#sprite). 样式中的所有图标或图像都需要存储在sprite. 获取更多 [sprites 是怎样工作的](/help/glossary/sprite).
- [**Glyphs**](https://www.mapbox.com/mapbox-gl-js/style-spec/#glyphs). 字形用于显示样式中的子图。样式中字形的属性提供一个URL模板，用以`PBF` 格式加载符号-距离-字段的字形集.

### 使用样式

地图样式适用于 [Mapbox GL JS](https://docs.mapbox.com/mapbox-gl-js/), Mapbox地图SDKs 的[Android](https://docs.mapbox.com/android/maps/overview/) 和[iOS](https://docs.mapbox.com/ios/maps/overview/), 以及任何可以读取mapbox样式的第三方软件。样式规则和瓦片集会组合并完全呈现在被请求的计算机或移动设备上。 由于样式设计为与基于GL的渲染器一起使用，因此可以在加载地图后以编程方式更改样式.

### 数据驱动的样式

数据驱动的样式允许你根据图层源中的属性更改图层的样式。 例如，创建一个数据驱动的样式规则，该规则根据每个州的人口设置美国的州的颜色.

<div class='grid grid--gut6'>
  <div class='col col--6 pt12 pr12'>
    <strong>Mapbox GL JS 表达式入门</strong>: 在<a href='/help/tutorials/mapbox-gl-js-expressions/'>这个教程中</a>, 每个圆圈代表一个历史性地表. 圆的大小是地标的年龄，缩放插值用于创建流畅的用户体验.
  </div>
  <div class='col col--6'>
    <img class='mt0' src='/help/img/gl-js/mapbox-gl-js-expressions.png' alt='不同大小的圆圈渐变构成圆圈图'>
  </div>
  <div class='col col--6 pt12 pr12'>
    <strong>等值区域图</strong>: 在等值区域图中， '填充' 图层会根据数据属性更改颜色.  在<a href='/help/tutorials/choropleth-studio-gl-pt-1/'>这个教程中</a>, 一个州的颜色是根据人口数据的变化而变化的.
  </div>
  <div class='col col--6'>
    <img class='mt0' class='fr' src='/help/img/screenshots/choropleth-160809.png' alt='根据数据属性对美国各州的等值线图进行着色'>
  </div>
  <div class='col col--6 pt12 pr12'>
    <strong>彩线地图</strong>: 在彩线图中，线图层会根据数据属性更改颜色。 在此示例中，飞行路径的颜色根据航班起点和目的地的当地时间之间的差异而变化.
  </div>
  <div class='col col--6'>
    <img class='mt0' class='fr' src='/help/img/screenshots/timezone-flights.png' alt='彩线图显示飞行路径'>
  </div>
  <div class='col col--6 pt12 pr12'>
    <strong>3D 建图</strong>: 使用填充-拉伸新建图层, 基于数据属性在3D中新建建筑物. 在 <a href='https://www.mapbox.com/mapbox-gl-js/example/3d-buildings/'>这个例子中</a>, 建筑物的拉伸高度根据建筑物多边形的“高度”和“最低高度”而变化.
  </div>
  <div class='col col--6'>
    <img class='mt0' class='fr' src='/help/img/screenshots/3D_buildings_example_dds.png' alt='显示根据数据属性分配高度的3D建筑物的地图'>
  </div>
</div>

#### 数据驱动样式所需的样式表对象

样式表中任何布局属性，绘制属性或过滤器的值都可以指定为表达式. 使用 [表达式](https://www.mapbox.com/mapbox-gl-js/style-spec#expressions), 可以同时为具有多个要素属性的数据设置样式，应用条件逻辑，并使用算术和字符串操作数据以获得数据与样式之间更复杂的关系. 下面是使用[匹配表达式](https://www.mapbox.com/mapbox-gl-js/style-spec#expressions-match) 显示不同图标图像的布局属性值的示例，具体取决于数据中`marker-number` 属性的值 -

```
"layout": {
  "icon-image": [
      "match", ["string", ["get", "marker-number"]],
        "one",
        "pin-red",
        "two",
        "pin-blue",
        "three",
        "pin-green",
        "pin-white"
      ],
  "icon-size": 1
}
```

可以在[Mapbox 样式规范](https://www.mapbox.com/mapbox-gl-js/style-spec) 和[表达式入门](/help/tutorials/mapbox-gl-js-expressions/) 以及 [用Mapbox GL JS制作热力图](/help/tutorials/make-a-heatmap-with-mapbox-gl-js/)的示例中进一步探索多种类型的表达式.

## 创建地图样式

你可以使用Mapbox Studio样式编辑器生成样式JSON，直接编写它，或使用Mapbox样式API来读取和更改地图样式，字体和图像.

### Mapbox Studio

Mapbox Studio 样式编辑器是一个可视化界面，用于根据Mapbox样式规范创建和编辑样式. 你可以在[Mapbox Studio 手册](https://www.mapbox.com/studio-manual/reference/styles/) 的样式部分了解有关如何创建和编辑样式的更多信息.

### Cartogram

用拖放工具 [Cartogram](https://apps.mapbox.com/cartogram/)可在几秒钟内创建自定义地图。 上传图片，选择你想要使用的颜色，并创建适合你品牌的地图样式. 新地图样式可以在网站或移动应用程序中使用，也可以在Mapbox Studio样式编辑器中打开它以继续自定义样式或添加自定义数据.

### Mapbox 地图 API

Mapbox样式API允许您阅读和更改地图样式，字体和图标。 API是Mapbox Studio的基础。 如果你使用Mapbox Studio，Mapbox GL JS或Mapbox Mobile SDK，那么你已经在使用Styles API。 可以在[API 文档](https://docs.mapbox.com/api/maps/#styles)中了解有关样式 API的更多信息.

### Mapbox 样式规范

你可以使用文本编辑器从头开始创建样式并遵循 [Mapbox 样式规范](https://www.mapbox.com/mapbox-gl-js/style-spec).
