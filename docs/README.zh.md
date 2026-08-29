# MirrorZ 文档

[English](./README.md)

本文档介绍了 MirrorZ 的设计和使用方法。

## 给终端用户

MirrorZ 包含以下服务：

* <https://mirrorz.org/>：动态实时内容的前端
    * <https://mirrors.cernet.edu.cn>：校园网镜像站版 MirrorZ，仅包含教育网镜像
* mirrorz-302：用于将用户请求重定向到他们的「最佳」镜像站点的后端
* mirrorz-monitor：被上述两个服务使用，同时供用户检查镜像站点的状态

## 给镜像站点

注意：除非获得镜像站点同意，否则 MirrorZ 不会将其纳入任何服务中。

MirrorZ 项目存在多项服务，一个镜像站点可以通过有选择地提供以下数据并编辑配置来选择参与哪些服务。

### mirrorz.json

格式定义在 <https://github.com/mirrorz-org/mirrorz#data-format-v17>。每个镜像站点都应该提供这个数据，以便前端、oh-my-mirrorz 和监控器能够使用。

每个镜像站点在
[mirrorz-config/sites](https://github.com/mirrorz-org/mirrorz-config/tree/master/sites)
下拥有一个目录。目录中包含获取或转换该站点数据的 JavaScript，以及 parser
需要的静态元数据。已经提供 mirrorz.json 的站点可以使用一个获取并返回该 JSON
的小型 JavaScript 模块。

在 `parser/parsers.js` 中导出 parser，然后将其名称加入
`mirrorz-config/config` 下相应配置文件的 `mirrors` 数组。前端、
mirrorz-monitor 和 mirrorz-json-legacy 共用这一份列表。如果浏览器需要直接从镜像站
获取数据，数据源必须[允许 mirrorz.org 跨域访问](https://github.com/mirrorz-org/mirrorz/pull/60#issuecomment-884801035)。

### 302 跳转配置

跳转服务的站点与 endpoint 配置定义在 <https://github.com/mirrorz-org/mirrorz-302#site-configuration>，以 `config.json` 的形式存放在 <https://github.com/mirrorz-org/mirrorz-config/tree/master/sites> 下的各站点目录中。仓库列表、仓库路径、状态和同步新鲜度由 mirrorz-monitor 写入 InfluxDB，不在该静态配置中重复维护。

在加入 302 跳转配置后，用户能够使用 mirrorz 提供的重定向功能，在替换软件源配置时只使用 mirrorz 的域名。

目前 <https://mirrors.cernet.edu.cn> 运行此服务，允许重定向到教育网镜像站。

加入该配置表明镜像站点知悉可能会有来自 mirrorz 的流量被重定向到它们。

#### 提供 302 站点信息

新添加的镜像站点需要创建包含 `abbrs` 和 `endpoints` 的 `sites/<site>/config.json`。`abbrs` 中的值必须与 mirrorz-monitor 写入的 `mirror` 标识完全一致；多个监控站点标识可以共用同一组 endpoints。

镜像站点还必须按照上文说明加入 mirrorz-monitor。302 服务会将监控数据中的仓库路径与这里配置的 endpoint 组合成最终跳转地址，不需要运行额外的生成脚本，也不需要维护 `d_parser`。

全部完成之后，可以访问 <https://mirrors.cernet.edu.cn/api/scoring> 确认自己的站点在其中。

```shell
# 例子：如果 filter 限制了 IPv4，那么确认时也需要使用 IPv4 访问
curl -4 https://mirrors.cernet.edu.cn/api/scoring | jq .
```

## 给开发者

[仓库架构](./repo-struct.zh.md)
