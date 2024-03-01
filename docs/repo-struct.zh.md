# 仓库架构

本文档介绍目前 MirrorZ 项目使用到的仓库与其之间的联系。

```tree
- mirrorz（主站点）
  - static
    - json
      - legacy                             --> mirrorz-json-legacy
      - site                               --> mirrorz-json-site
  - src
    - parser                               --> mirrorz-parser
    - config                               --> mirrorz-config
    - i18n                                 --> mirrorz-i18n
  - scripts
    - oh-my-mirrorz                        --> oh-my-mirrorz
  - legacy                                 --> mirrorz-legacy
- mirrorz-302（跳转服务）
  - * 配置需要填写 mirrorz-d-extension 仓库中 dist 目录的路径
- mirrorz-d-extension（处理 mirrorz.d 与 mirrorz.json 融合后的数据）
  - config                                 --> mirrorz-config
  - parser                                 --> mirrorz-parser
- mirrorz-help（帮助站点）
  - Requires /static/json/legacy-pack.json --> mirrorz scripts/legacy-pack.js --> mirrorz-json-legacy
- mirrorz-json-legacy（用于非现代浏览器构建的 JSON 文件）
- mirrorz-json-site（？）
- mirrorz-monitor（更新 influxdb 数据库）
  - config                                 --> mirrorz-config
  - parser                                 --> mirrorz-parser
- mirrorz-parser（从镜像站点获取数据并转换为 mirrorz.json）
  - config.json                            --> mirrorz-config/config.json
- mirrorz-i18n（国际化）
- oh-my-mirrorz（测速脚本）
- mirrorz-legacy（非现代浏览器站点）
- mirrorz-config（mirrorz 站点配置）
```
