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
  - * 配置需要填写 mirrorz-d-extension 仓库中 dist 目录的路径
- mirrorz-help（帮助站点）
  - Requires /static/json/legacy-pack.json --> mirrorz scripts/legacy-pack.js --> mirrorz-json-legacy
- mirrorz-json-legacy（用于非现代浏览器与 mirrorz-help 构建的 JSON 文件）
- mirrorz-monitor（更新 influxdb 数据库）
  - config                                 --> mirrorz-config
  - parser                                 --> mirrorz-parser
- mirrorz-config（mirrorz 站点配置，包含 config, parser, json-site 和 d-extension）
```
