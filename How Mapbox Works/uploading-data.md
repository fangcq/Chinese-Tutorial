---
title: Upload data
description: Learn the difference between tilesets and datasets, how the upload process works, and techniques for uploading.
image: /img/narrative/data-upload.svg
topics:
  - uploads
contentType: guide
---

Mapbox 允许用户上传自定义数据并转换成**tileset**或**dataset**格式。这篇指南概述了瓦片集和数据集之间的区别、数据上传程序是如何工作的、上传的技巧以及入门所需的资源。

<!-- <div style='width:100%;height:300px;background-color:#eee' class='mb24'></div> -->

## 上传进程如何工作

Mapbox将用户的数据以一种优化的格式存储，以便在交互式的web地图中流畅显示。根据上传的数据类型和所需的实例，用户的数据将被保存为原生的 [GeoJSON](/help/glossary/geojson/) 或者处理成栅格和矢量的 [tileset](/help/glossary/tileset/)。

有两种方法可以将数据上传到用户的Mapbox帐户，每种方法都有相应的注意事项：

- [Mapbox Uploads API](https://docs.mapbox.com/api/maps/#uploads) 在设计上可以接受大部分常见的地理空间数据类型的文件，当它们上传时, 会被自动处理为 [vector tiles](/help/glossary/vector-tiles/)。 瓦片集可以通过 Mapbox Studio 的瓦片集页面上传，也可以通过 Mapbox Uploads API 程序化地上传。瓦片集数据可以从上传的数据中产生，也可以从已有的[dataset](/help/glossary/dataset)中产生。
- [Mapbox Datasets API](https://docs.mapbox.com/api/maps/#datasets) 设计将原生的[GeoJSON](/help/glossary/geojson/) 数据储存为数据集。数据集可以通过 Mapbox Studio 的数据集页面上传，也可以通过 Mapbox Datasets API 程序化地上传。

接下来的章节将提供 Mapbox Uploads API 和 Datasets APIs 之间的更详细区别。

### Datasets vs. tilesets

[**Datasets**](/help/glossary/dataset) 和 [**tilesets**](/help/glossary/tileset) 是两种不同的文件类型，用户可以在上传数据到自己的 Mapbox 账户时创建。数据集是可编辑的，瓦片集是样式可变的。

**Datasets** 提供对地理要素（点、线、多边形）和其属性的访问，并且都能在 Mapbox Studio [dataset editor](https://www.mapbox.com/studio/datasets/) 或者通过 Mapbox [Datasets API](https://docs.mapbox.com/api/maps/#datasets)进行 **编辑**。

如果你正在处理需要经常更新且不太复杂的数据，请考虑使用dataset。需要注意的是，一旦创建了数据集，就需要将其发布成瓦片集，并在 Mapbox Studio 的样式编辑器中给瓦片集添加样式。通过数据集，用户可以继续修改数据，将每次的更新数据发布到相关联的瓦片集，这样就能看到该瓦片集在任何样式模式同步改变了。

![在Mapbox Studio 数据集编辑器中编辑数据集的动画GIF](/help/img/studio/dataset-modify-feature.gif)

**Tilesets**是矢量数据的轻量级集合，针对渲染效果进行了优化且不可*编辑*，但可以在 Mapbox Studio 样式编辑器中设置
*样式*。如果你的数据量很大且不需要经常更新，可以考虑将数据作为tileset上传。

![在Mapbox Studio 样式编辑器中显示瓦片集的GIF动画](/help/img/studio/tileset-upload.gif)

### 数据如何变成瓦片集

当你把数据以瓦片集的格式上传或者以瓦片集的形式导出时，Mapbox 会在预设的缩放级别将它分割成统一的正方形平铺网格。这些数据想要变的轻量级且能快速加载，必须经过许多处理步骤。[Mapbox Streets](https://www.mapbox.com/vector-tiles/) 的数据在变成你可能熟悉的全球 Mapbox 街道瓦片集之前，也经理了相同的过程。当数据变成瓦片集，两件事将会发生：

**1. 简化**

Mapbox Uploads API 会分析数据的复杂度，并根据缩放层级以一定的比例自动删除多余的节点。这个过程叫做 *简化*，这能帮助你的地图更快地加载，尤其在小比例尺的级别下额外的复杂数据根本不会展示。

**2. 确定缩放范围**

Mapbox Uploads API 通过检查数据的范围(数据所覆盖的地理区域)和复杂性来保持瓦片集的轻量级，并且只为可能需要的缩放级别创建对应的矢量切片 &mdash;[zoom extent](/help/glossary/zoom-extent/) 是已经确定的。对于有着复杂的要素或有小比例尺缩放级别的数据，通常意味着小比例尺级别（缩小）的数据会被丢弃。对于结构比较简单的大面积要素，这通常意味着大比例尺级别（放大）的数据会被丢弃。

如果需要将瓦片集在不同的缩放级别都能展示，你可以[手动调节](/help/troubleshooting/adjust-tileset-zoom-extent/), 但是请注意下方的信息：

- 最小缩放级别可以增加，但不能少于已经指定的最小缩放级别。
- 最大缩放级别可以减少也可以增加，因为数据可以 [overzoomed](/help/glossary/overzoom/) 并且到22级一直可见。

### 栅格和矢量瓦片集

用户可以使用栅格或者矢量数据来创建瓦片集。

如果你上传了栅格数据（比如 GeoTIFF ），生成的栅格瓦片集将只包含栅格数据，这是一种 [基于像素](https://en.wikipedia.org/wiki/Raster_graphics)的、符合所有数码照片标准的数据格式。

如果你上传了矢量数据，将会生成包含矢量数据的矢量瓦片集，其中包含以 x/y 点串形式储存的点、线、多边形。 [Mapbox GL](/help/glossary/mapbox-gl/) 可以将矢量数据直接在浏览器进行计算和渲染，所以对处理矢量瓦片集很有优势。

### 你可以用瓦片集做什么

如果通过上传带有属性的地理空间数据来创建矢量瓦片集，则生成的矢量瓦片集将继承原始数据源中包含的属性。用矢量瓦片集中的要素属性可以做些什么，这里有一部分示例：

- 使用 [Mapbox Studio](/help/tutorials/choropleth-studio-gl-pt-1/) 根据人口密度来定义一个州显示什么颜色。
- 使用 [Mapbox Maps SDK for iOS](https://www.mapbox.com/ios-sdk/examples/dds-circle-layer/)根据 `年龄` 属性动态地改变树的样式 。
- 使用 [Mapbox GL JS](https://www.mapbox.com/mapbox-gl-js/example/query-similar-features/) 高亮有着相同名称的郡县数据。

## 上传数据

这里几种方法让用户能够上传自定义数据，来生成瓦片集或者数据集。

### Mapbox Studio

用户可以将瓦片集和数据集上传到 Mapbox Studio。如果数据以瓦片集形式上传请使用 Mapbox Studio 中的 [Tilesets page](https://www.mapbox.com/studio/tilesets/)。若用户将自己的数据以数据集的形式上传请使用 Mapbox Studio 中的 [Dataset page](https://www.mapbox.com/studio/datasets)。你还可以创建一个空白数据集，并使用Mapbox Studio 数据集编辑器在浏览器中直接绘制数据。更多信息有关如何在 Mapbox Studio 中从零开始创建数据，请参见 [创建新数据](/help/how-mapbox-works/creating-data/)。

想要了解更多信息关于可接受文件类型、传输限制以及如何管理数据，请见[Mapbox Studio 手动上传章节](https://www.mapbox.com/studio-manual/overview/geospatial-data/)。

### Mapbox Uploads API

用户可以使用 Mapbox Uploads API 直接以编程的方式创建栅格或矢量瓦片集。关于如何上传自定义数据的详细信息，请参阅 [Mapbox Uploads API](https://docs.mapbox.com/api/maps/#uploads) 文档。

### Mapbox Datasets API

用户可以使用 Mapbox Datasets API 直接以编程的方式来创建、编辑和管理数据集。关于如何创建和处理数据集的详细信息，请参阅 [Mapbox Datasets API](https://docs.mapbox.com/api/maps/#datasets) 文档。
