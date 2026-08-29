# MirrorZ Documentation

[中文版本](./README.zh.md)

This documentation introduces the design of MirrorZ, and how to use it.

## For end user

MirrorZ consists of the following services:

* <https://mirrorz.org/>: frontend with dynamic, real-time content
    * <https://mirrors.cernet.edu.cn>: MirrorZ hosted by CERNET, with CERNET mirrors only
* mirrorz-302: backend for redirecting user requests to their "optimal" mirror site
* mirrorz-monitor: used by the above two services, also for users to check the status of mirror sites

## For mirror site

Note: Unless one mirror site gives their consent, MirrorZ would not include them in any of its services.

As there are multiple services, one mirror site can choose which services to participate in by selectively providing the following data and editing the config.

### mirrorz.json

The format is defined at <https://github.com/mirrorz-org/mirrorz#data-format-v17>. For each mirror site, they should provide this data for the frontends, oh-my-mirrorz and monitor to work.

Each mirror site has a directory under
[mirrorz-config/sites](https://github.com/mirrorz-org/mirrorz-config/tree/master/sites).
The directory contains the JavaScript used to fetch or transform the site's
data and the static metadata needed by that parser. A site that already serves
mirrorz.json can use a small JavaScript module that fetches and returns it.

Export the parser from `parser/parsers.js`, then add its name to the `mirrors`
array in the applicable file under `mirrorz-config/config`. This single list is
used by the frontend, mirrorz-monitor, and mirrorz-json-legacy. If the browser
fetches data from the mirror site, the source must
[allow CORS for mirrorz.org](https://github.com/mirrorz-org/mirrorz/pull/60#issuecomment-884801035).

### 302 Redirect Configuration

The site and endpoint configuration for the redirect service is defined at <https://github.com/mirrorz-org/mirrorz-302#site-configuration> and stored as `config.json` in each directory under <https://github.com/mirrorz-org/mirrorz-config/tree/master/sites>. Repository lists, paths, status, and freshness are written to InfluxDB by mirrorz-monitor and are not duplicated in this static configuration.

This enables the user to use redirection provided by mirrorz. Namely when they are substituting sources, they can just use the domain name of mirrorz.

Currently <https://mirrors.cernet.edu.cn> provides redirecting service to CERNET mirrors.

Joining this configuration indicates that the mirror site is aware of potential traffic being redirected to it.

#### Providing 302 Site Information

Mirror sites shall add `sites/<site>/config.json` containing `abbrs` and `endpoints`. Every value in `abbrs` must exactly match the `mirror` identifier written by mirrorz-monitor; multiple monitored site identifiers may share one endpoint configuration.

The mirror site must also be added to mirrorz-monitor as described above. The 302 service combines repository paths from monitor data with the configured endpoints, so no separate generation script or `d_parser` entry is needed.

After everything is done, check <https://mirrors.cernet.edu.cn/api/scoring> and you would see your site is listed:

```shell
# example: You need to access with IPv4 if your endpoint filter limits IPv4 only
curl -4 https://mirrors.cernet.edu.cn/api/scoring | jq .
```

## For developers

[Repository Architecture](./repo-struct.md)
