---
title: 部署建议
category: deployment
---

# 部署建议

阅读本章前，请先阅读 [TiDB 整体架构](../overview.md#tidb-整体架构)。

## TiDB 集群各个组件的硬件消耗情况及推荐配置

| 组件 | 消耗硬件资源 | 推荐硬件配置 |
| ---- | -------- | -------- |
| TiDB | CPU、内存 |32+ 核/64G+ 内存 |
| TiKV | CPU、内存、磁盘 IO |32+ 核/64G+ 内存/200G+ SSD硬盘|
| PD   | CPU、内存、磁盘 IO |8+ 核/16G+ 内存/200G+ SSD硬盘 |

备注：

* OLTP 线上业务强烈建议使用 SSD 硬盘，否则在一些场景下会有性能问题。
* TiKV 硬盘大小建议不要超过500G（防止硬盘损坏时，数据恢复耗时过长）

## 整体部署方案

| 组件 | 生产环境所需机器情况 | 测试环境所需机器情况 |
| ----- | ------- | ------- |
| TiDB | 至少 2 台，保证高可用，可按所需并发和吞吐，动态增加机器 | 可以 1 台 |
| PD | 必须 3 台，保证高可用 | 可以 1 台 |
| TiKV | 至少 3 台，保证高可用，可按所需计算资源和存储容量，动态增加机器 |至少 3 台 |

备注：

* TiDB 实例可以和任意一台 PD 部署在同一台机器，也可以单独部署。
* PD 和 TiKV 实例，建议每个实例单独部署一个硬盘，避免 IO 冲突，影响性能。
* TiDB 和 TiKV 实例，建议分开部署，以免竞争 CPU 资源，影响性能。

### 举例：生产环境部署（推荐至少 6 台机器）

|机器1|机器2|机器3|机器4|机器5|机器6|
|----|----|----|----|----|----|
|TiKV1|TiKV2|TiKV3|PD1|PD2|PD3|
|-|-|-|TiDB1|TiDB2|-|

其中 TiDB1 和 TiDB2 通过负载均衡组件对外统一提供 SQL 接口。

### 举例：测试验证环境部署（推荐至少 4 台机器）

|机器1|机器2|机器3|机器4|
|----|----|----|----|
|TiKV1|TiKV2|TiKV3|PD1|
|-|-|-|TiDB1|
