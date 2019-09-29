---
title: Unity applications
description: Learn how the Mapbox Maps SDK for Unity works, how to use it, and how to get started building applications.
image: /img/narrative/game-develop.svg
topics:
  - unity
contentType: guide
---

[Mapbox Maps SDK for Unity](https://www.mapbox.com/unity-sdk/) 是一组用真实地理数据来构建[Unity](https://unity3d.com/)应用的工具集。 这套工具集由性能稳健的API和基于Unity平台搭建的图形化用户界面构成，该API用于和[Mapbox web services](https://docs.mapbox.com/api/)交互，把地图资源转化成游戏对象。这份指南综述了[Mapbox Maps SDK for Unity](/help/glossary/mapbox-maps-sdk-for-unity/) 是如何工作的, 要怎样使用它,以及如何开始着手构建应用。

<img src="/help/img/unity/unity-blocks.png" alt="unity示例项目的截图">

## Mapbox Maps SDK for Unity是如何工作的

[Mapbox Maps SDK for Unity](https://www.mapbox.com/unity-sdk/) 通过提供一个关于 Mapbox web 服务 API的，可简单编程的图形化接口，帮助 Unity 开发者把动态地图数据添加到他们的游戏和应用当中。 部分Mapbox web 服务 API 如下：
- [Vector Tiles API](https://docs.mapbox.com/api/maps/#vector-tiles)
- [Raster Tiles API](https://docs.mapbox.com/api/maps/#raster-tiles)
- [Static Images API](https://docs.mapbox.com/api/maps/#static-images)
- [Geocoding API](https://docs.mapbox.com/api/search/#geocoding)
- [Directions API](https://docs.mapbox.com/api/navigation/#directions)
- [Map Matching API](https://docs.mapbox.com/api/navigation/#map-matching)

### 动态数据

部分以地图为主的 Unity 插件被设计用来帮助开发者使用地图数据去构建静态的游戏环境，Mapbox Maps 的 Unity 软件开发工具包在设计上可以在运行时请求并渲染地图数据。这意味着用 Maps SDK for Unity 构建的应用将只展示最近的空间数据。这也意味着游戏和应用只会去请求符合用户所浏览区域这个条件的数据集，这样可以使游戏和应用保持轻量级。

### 片元生成

Mapbox Maps SDK for Unity 可以使用卫星影像、自定义地图样式、地形数据和 POI 数据生成构建 2D 或者3D 地图的片元数据。详情请参见 [mesh generation tutorial](/help/tutorials/unity-mesh-pt-1/)。

### 增强现实支持

The Mapbox Maps SDK for Unity 对增强现实提供支持, 无论是开发桌面端的 AR 应用还是类似PokemonGo的游戏应用

## 使用 Mapbox Maps SDK for Unity

Mapbox Maps SDK for Unity ，和 [Unity](https://unity3d.com/) 该桌面程序协作使用。

### 安装 Mapbox Maps SDK for Unity

Mapbox Maps SDK for Unity 可以 [via direct download](https://www.mapbox.com/unity/). 想了解完整的 Mapbox Maps SDK for Unity 文档，请看 [installation instructions](https://www.mapbox.com/unity-sdk/#install)。

### 创建游戏对象

Mapbox Maps SDK for Unity 可以用大量不同类型的数据来构建丰富的渲染环境，包括地形数据、栅格地图瓦片和矢量地图瓦片等数据。

#### 地形

Mapbox [terrain-rgb](https://www.mapbox.com/blog/terrain-rgb/) 瓦片集是被设计用于高分辨率的高程可视化，它尤其适合用于 Unity 构建 3D 面片。更多示例请见 [Mesh generation basics](https://www.mapbox.com/mapbox-unity-sdk/docs/03-examples.html#mesh-generation-basics)。

<img src="/help/img/unity/unity-terrain.png" alt=" unity 地形数据的截图">

#### 矢量切片特色功能

无论是使用 [Mapbox tileset](https://www.mapbox.com/vector-tiles/)还是使用通过 [Mapbox Uploads API](https://docs.mapbox.com/api/maps/#uploads)自定义的瓦片集， Mapbox Maps SDK for Unity 都能请求到矢量切片，并将它们转换成游戏对象或者面片数据，与其他游戏数据一起渲染。更多示例请见 [Slippy map](https://www.mapbox.com/mapbox-unity-sdk/docs/03-examples.html#slippy-vector-terrain)。

<img src="/help/img/unity/unity-city.png" alt="unity城市的截图">

### 获取 Mapbox web services

The Mapbox Maps SDK for Unity 可以将你的游戏或应用与大部分的 Mapbox 网络服务接口连接起来，包括 Mapbox [Vector Tiles API](https://docs.mapbox.com/api/maps/#vector-tiles) [Raster Tiles API](https://docs.mapbox.com/api/maps/#raster-tiles), [Static Images API](https://docs.mapbox.com/api/maps/#static-images), [Geocoding API](https://docs.mapbox.com/api/search/#geocoding), [Directions API](https://docs.mapbox.com/api/navigation/#directions), and [Map Matching API](https://docs.mapbox.com/api/navigation/#map-matching). 更多如何调用 Mapbox API 的示例请见 [Playground](https://www.mapbox.com/mapbox-unity-sdk/docs/03-examples.html#playground)。

### 发布

任何 Unity 可以工作的地方，Mapbox Maps SDK for Unity同样可以，包括桌面端、移动端以及即将上线的Web端。

### 费用

使用 Maps SDK for Unity 构建的移动端应用的用量按照此标准 [monthly active users](/help/glossary/monthly-active-users/) (MAU)。这意味着在一个月内，每个调用了你的应用的设备被视为一个 MAU。其他用 Unity 构建的应用将按照瓦片数据请求量来收费。所有类型的应用都将按照 Mapbox APIs 实际调用量来收费，比如地理编码和导航功能，当调用量超出了免费的范围时就会收费。
