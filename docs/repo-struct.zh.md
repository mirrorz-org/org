# 仓库架构

本文档介绍目前 MirrorZ 项目使用到的仓库与其之间的联系。

```tree
- mirrorz（主站点）
  - static
    - json
      - legacy                             --> mirrorz-json-legacy
  - src
    - config                               --> mirrorz-config
    - i18n
  - legacy
- mirrorz-302（跳转服务）
  - * 从 mirrorz-config/d-extension/sites 加载站点与 endpoint 配置
  - * 从 mirrorz-monitor 写入的 InfluxDB 数据获取仓库路径、状态和同步新鲜度
- mirrorz-help（帮助站点）
  - Requires /static/json/legacy-pack.json --> mirrorz scripts/legacy-pack.js --> mirrorz-json-legacy
- mirrorz-json-legacy（用于非现代浏览器与 mirrorz-help 构建的 JSON 文件）
- mirrorz-monitor（更新 influxdb 数据库）
  - config                                 --> mirrorz-config
- mirrorz-config（mirrorz 站点配置，包含 config、parser 和 d-extension）
```
