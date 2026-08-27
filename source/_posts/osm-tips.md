---
title: OpenStreetMap绘画工具技巧
date: 2026-04-24 18:42:12
updated: 2026-08-27 12:42:43
tags:
---

# 实用工具

- 苹果地图海外版（<https://maps.apple.com/frame?map=satellite>）清晰度和时效都相当可观，可以一试
  
- 可以用[PicLayer](https://github.com/JOSM/PicLayer)来将第三方的规划图/卫星图等导入JOSM中作为背景
  
- [UrbanEye3D](https://github.com/Zkir/UrbanEye3D) 可以在JOSM中查看3D建筑
  
- [Better-OSM](https://github.com/deevroman/better-osm-org)是一个Tampermonkey插件，可以直接在网页中突出显示已更改对象。
  
- [BetteriD](https://github.com/koharachan/BetteriD)是国人开发的iD改进版，修复了许多汉化问题，并且添加了实用性功能
  

# 快速参考

本参考仅代表个人用法，可能与社区共识有出入，欢迎各位斧正。

## 要素生命周期

- ```proposed:``` 画大饼
- ```planned:``` 有详细规划，基本确定可以建成
- ```construction:``` 建设中
- ```disused:``` 暂时停止服务
- ```abandoned:``` 弃用，需要大修才能投入使用
- ```demolished:``` 拆毁，移除，用于保存历史痕迹

## 道路分级参考

- 国道/一级公路/城市快速路```highway=trunk```| [trunk](https://wiki.openstreetmap.org/wiki/Tag:highway%3Dtrunk)
  
- 省道/城市主干路/双向六车道及以上```highway=primary```| [primary](https://wiki.openstreetmap.org/wiki/Tag:highway%3Dprimary)
  
- 县道/城市次干路/双向四车道```highway=secondary```| [secondary](https://wiki.openstreetmap.org/wiki/Tag:highway%3Dsecondary)
  
- 乡道/城市支路/通过功能完备的双向两车道```highway=tertiary```|[tertiary](https://wiki.openstreetmap.org/wiki/Tag:highway%3Dtertiary)
  
- 山野中连接聚落/更低等级的交通功能道路-未分级```highway=unclassified```|[unclassified](https://wiki.openstreetmap.org/wiki/Tag:highway%3Dunclassified)
  
- 乡村聚落/县城小路/小区内部路-住宅区```highway=residential```| [residential](https://wiki.openstreetmap.org/wiki/Tag:highway%3Dresidential)
  

## 自然土地覆盖参考

- ```natural=wood```| 自然状态的树林
- ```landuse=forest```| 林场，人工维护痕迹明显的树林
- ```landuse=orchard```| 果园
- 灌木丛细分
- - ```natural=scrub```| 原生高灌木丛，通常过人
- - ```natural=heath```| 原生矮灌木丛，通常不过腿
- - ```natural=shrubbery```| 修剪完备的人工灌木丛，常见于广场
- - ```landuse=shrubs```| 人工种植的灌木丛区域

- ```landuse=grass```| 人工草坪
- ```landuse=greenfield```| 待开发地块，长了杂草但又裸露在外的

## 城市土地覆盖参考
- ```landuse=residential``` | 居民区
- ```landuse=industrial``` | 工业区 
- ```landuse=civic_admin``` | 政府机关