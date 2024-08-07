# MirrorZ 文档

[English](./README.md)

本文档介绍了 MirrorZ 的设计和使用方法。

## 给终端用户

MirrorZ 包含以下服务：

* <https://mirrorz.org/>：动态实时内容的前端
    * <https://mirrors.cernet.edu.cn>：校园网镜像站版 MirrorZ，仅包含教育网镜像
* <https://mirrorz.org/_/>：静态、非实时内容的前端
* oh-my-mirrorz：速度测试脚本
* mirrorz-302：用于将用户请求重定向到他们的「最佳」镜像站点的后端
* mirrorz-search：用于搜索的后端（实验性的）
* mirrorz-monitor：被上述两个服务使用，同时供用户检查镜像站点的状态

## 给镜像站点

注意：除非获得镜像站点同意，否则 MirrorZ 不会将其纳入任何服务中。

MirrorZ 项目存在多项服务，一个镜像站点可以通过有选择地提供以下数据并编辑配置来选择参与哪些服务。

### mirrorz.json

格式定义在 <https://github.com/mirrorz-org/mirrorz#data-format-v17>。每个镜像站点都应该提供这个数据，以便前端、oh-my-mirrorz 和监控器能够使用。

一个镜像站点有两种提供 mirrorz.json 的方式：

#### 在镜像站服务器上提供 mirrorz.json

即镜像站点公开一个单一的 url 供 MirrorZ 使用。

* 要启用动态前端，在 [mirrorz-config/config/mirrorz.org.json:upstream_mirrors](https://github.com/mirrorz-org/mirrorz-config) 中添加 url，并为 mirrorz.org [启用 CORS](https://github.com/mirrorz-org/mirrorz/pull/60#issuecomment-884801035)
* 要启用静态前端和 oh-my-mirrorz.py，在 [mirrorz-config/config/mirrorz.org.json:mirrors](https://github.com/mirrorz-org/mirrorz-config) 中添加 url
* 要启用监控，在 [mirrorz-config/config/mirrorz.org.json:monitor_mirrors](https://github.com/mirrorz-org/mirrorz-config) 中添加 url

#### 提供一个 `parser` 给 MirrorZ

[parser](https://github.com/mirrorz-org/mirrorz-parser) 转换他们的数据（例如 tunasync.json）为 mirrorz.json。

一旦在 `mirrorz-parser` 中提供了一个 parser：

* 要启用动态前端，在 [mirrorz-config/config/mirrorz.org.json:upstream_parser](https://github.com/mirrorz-org/mirrorz-config) 中添加 parser，并在镜像站点提供的数据上为 mirrorz.org [启用 CORS](https://github.com/mirrorz-org/mirrorz/pull/60#issuecomment-884801035)
* 要启用静态前端和 oh-my-mirrorz.py，在 [mirrorz-config/config/mirrorz.org.json:mirrors_legacy](https://github.com/mirrorz-org/mirrorz-config) 中添加 parser。MirrorZ 会定期生成一个 mirrorz.json。
* 要启用监控，在 [mirrorz-config/config/mirrorz.org.json:monitor_parser](https://github.com/mirrorz-org/mirrorz-config) 中添加 parser

### mirrorz.d.json

这是对 mirrorz.json 的扩展。其定义在 <https://github.com/mirrorz-org/mirrorz-302#mirrorzdjson>，托管在 <https://github.com/mirrorz-org/mirrorz-d-extension>。

在加入 mirrorz.d.json 后，用户能够使用 mirrorz 提供的重定向功能，在替换配置时可以只使用 mirrorz 的域名。

目前有两个实例运行此服务：<https://mirrors.cernet.edu.cn>（推荐）和 <https://mirrors.mirrorz.org/>

提供这个文件表明镜像站点知悉可能会有来自 mirrorz 的流量被重定向到它们。

#### 提供 mirrorz.d 信息

镜像站点可以选择以下三种方式中的一种：

* 在已有的 mirrorz.json 中添加 mirrorz.d 信息
* 在 [mirrorz-d-extension/custom/static/](https://github.com/mirrorz-org/mirrorz-d-extension/tree/master/custom/static) 中添加静态的、包含 mirrorz.d 信息的 JSON 文件
* 在 [mirrorz-d-extension/custom](https://github.com/mirrorz-org/mirrorz-d-extension/tree/master/custom) 中添加动态的解析器（这一般适用于镜像站点独立于 `mirrorz.json`，单独提供了 mirrorz.d 信息的场合）

如果选择后两种方式，还需要在 [mirrorz-d-extension/custom/index.js](https://github.com/mirrorz-org/mirrorz-d-extension/blob/master/custom/index.js) 添加对应的内容。

最后修改 [mirrorz-config](https://github.com/mirrorz-org/mirrorz-config/)。如果使用第一种方式，需要在 `d_mirrors` 字段中添加 JSON URL；如果使用后两种方式，需要在 `d_parsers` 字段中添加解析器。

全部完成之后，可以访问 <https://mirrors.cernet.edu.cn/api/scoring> 确认自己的站点在其中。

```shell
# 例子：如果 filter 限制了 IPv4，那么确认时也需要使用 IPv4 访问
curl -4 https://mirrors.cernet.edu.cn/api/scoring | jq .
```

## 给开发者

[仓库架构](./repo-struct.zh.md)
