# Repository Architecture

This document introduces the repositories used by the MirrorZ project and their interconnections.

```tree
- mirrorz (main site)
  - static
    - json
      - legacy                             --> mirrorz-json-legacy
  - src
    - config                               --> mirrorz-config
    - i18n
  - legacy
- mirrorz-302 (redirect service)
  - * Loads site and endpoint configuration from mirrorz-config/d-extension/sites
  - * Gets repository paths, status, and freshness from InfluxDB data written by mirrorz-monitor
- mirrorz-help (help site)
  - Requires /static/json/legacy-pack.json --> mirrorz scripts/legacy-pack.js --> mirrorz-json-legacy
- mirrorz-json-legacy (JSON files built for non-modern browsers and mirrorz-help)
- mirrorz-monitor (updates the influxdb database)
  - config                                 --> mirrorz-config
- mirrorz-config (configuration for mirrorz sites, including config, parser, and d-extension)
```
