# Repository Architecture

This document introduces the repositories used by the MirrorZ project and their interconnections.

```tree
- mirrorz (main site)
  - static
    - json
      - json-legacy     --> mirrorz-json-legacy
      - site            --> mirrorz-json-site
  - src
    - parser            --> mirrorz-parser
    - config            --> mirrorz-config
    - i18n              --> mirrorz-i18n
  - scripts
    - oh-my-mirrorz     --> oh-my-mirrorz
  - legacy              --> mirrorz-legacy
- mirrorz-302 (redirect service)
  - * Configuration needs to fill in the path of the dist directory in the mirrorz-d-extension repository
- mirrorz-d-extension (handles data merged from mirrorz.d and mirrorz.json)
  - config              --> mirrorz-config
  - parser              --> mirrorz-parser
- mirrorz-help (help site)
- mirrorz-json-legacy (JSON files built for non-modern browsers)
- mirrorz-json-site (?)
- mirrorz-monitor (updates the influxdb database)
  - config              --> mirrorz-config
  - parser              --> mirrorz-parser
- mirrorz-parser (fetches data from mirror sites and converts it into mirrorz.json)
  - config.json         --> mirrorz-config/config.json
- mirrorz-i18n (internationalization)
- oh-my-mirrorz (speed test script)
- mirrorz-legacy (site for non-modern browsers)
- mirrorz-config (configuration for mirrorz sites)
```
