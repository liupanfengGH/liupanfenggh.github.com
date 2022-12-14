---
title: NGUI中使用2D多边形Collider
date: 2022-12-14 20:04:08
tags: Unity
---

​		在项目中由于地图板块界面 中使用了很多 3D Box Collider的组件，看起来特别恶心，而且性能也很低，点击反馈也不准，因为 地图板块都是多边形的。故优化了一下这块业务。

1.调整 界面中 摄像机 UICamera 脚本 EventType参数为2DUI

2.调整地图板块的GameObject组件为2DPolygonCollider

3.编辑PolygonCollider 的点的范围，挂载NGUI能响应的事件脚本（UIButton)

4.运行项目，打开相应界面，即可测试 响应效果。

​	通过以上步骤，运行游戏即可使用2D触发方式进行 交互响应了，在低版本的NGUI中没有Camera参数选择只能修改相应的源码进行 射线检测。
