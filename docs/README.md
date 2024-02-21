# MirrorZ Documentation

[中文版本](./README.zh.md)

This documentation introduces the design of MirrorZ, and how to use it.

## For end user

MirrorZ consists of the following services:

* <https://mirrorz.org/>: frontend with dynamic, real-time content
    * <https://mirrors.cernet.edu.cn>: MirrorZ hosted by CERNET, with CERNET mirrors only
* <https://mirrorz.org/_/>: frontend with static, not-so-real-time content
* oh-my-mirrorz: Speedtest script
* mirrorz-302: backend for redirecting user requests to their "optimal" mirror site
* mirrorz-search: backend for searching (experimental)
* mirrorz-monitor: used by the above two services, also for users to check the status of mirror sites

## For mirror site

Note: Unless one mirror site gives their consent, MirrorZ would not include them in any of its services.

As there are multiple services, one mirror site can choose which services to participate in by selectively providing the following data and editing the config.

### mirrorz.json

The format is defined at <https://github.com/mirrorz-org/mirrorz#data-format-v17>. For each mirror site, they should provide this data for the frontends, oh-my-mirrorz and monitor to work.

One mirror site has two ways of providing mirrorz.json

#### Provide mirrorz.json on their server

Namely the mirror site exposes one single url for MirrorZ to use.

* To enable dynamic frontend, add the url in [mirrorz-config/config/mirrorz.org.json:upstream_mirrors](https://github.com/mirrorz-org/mirrorz-config) and [enable CORS for mirrorz.org](https://github.com/mirrorz-org/mirrorz/pull/60#issuecomment-884801035)
* To enable static frontend and oh-my-mirrorz.py, add the url in [mirrorz-config/config/mirrorz.org.json:mirrors](https://github.com/mirrorz-org/mirrorz-config)
* To enable monitor, add the url in [mirrorz-config/config/mirrorz.org.json:monitor_mirrors](https://github.com/mirrorz-org/mirrorz-config)

#### Provide a `parser` to MirrorZ

A [parser](https://github.com/mirrorz-org/mirrorz-parser) transforms their data (e.g. tunasync.json) to mirrorz.json.

Once a parser is provided in `mirrorz-parser`:

* To enable dynamic frontend, add the parser in [mirrorz-config/config/mirrorz.org.json:upstream_parser](https://github.com/mirrorz-org/mirrorz-config) and [enable CORS for mirrorz.org](https://github.com/mirrorz-org/mirrorz/pull/60#issuecomment-884801035) on the data the mirror site provides
* To enable static frontend and oh-my-mirrorz.py, add the parser in [mirrorz-config/config/mirrorz.org.json:mirrors_legacy](https://github.com/mirrorz-org/mirrorz-config). MirrorZ would periodically generate a mirrorz.json.
* To enable monitor, add the parser in [mirrorz-config/config/mirrorz.org.json:monitor_parser](https://github.com/mirrorz-org/mirrorz-config)

### mirrorz.d.json

This is an extension to mirrorz.json. It is defined at <https://github.com/mirrorz-org/mirrorz-302#mirrorzdjson> and hosted at <https://github.com/mirrorz-org/mirrorz-d-extension>.

This enables the user to use redirection provided by mirrorz. Namely when they are substituting sources, they can just use the domain name of mirrorz.

Currently there are two instances running this service: <https://mirrors.cernet.edu.cn> (Recommended) and <https://mirrors.mirrorz.org/>

By providing this file, mirror site is aware of potential traffic being redirected to them.

#### Providing mirrorz.d Information

Mirror sites have the option to choose one of the following three methods:

* Add mirrorz.d information to the existing mirrorz.json
* Add a static JSON file containing mirrorz.d information in [mirrorz-d-extension/custom/static/](https://github.com/mirrorz-org/mirrorz-d-extension/tree/master/custom/static)
* Add a dynamic parser in [mirrorz-d-extension/custom](https://github.com/mirrorz-org/mirrorz-d-extension/tree/master/custom) (This is generally suitable for mirror sites that provide mirrorz.d information independently of `mirrorz.json`)

If you opt for the latter two methods, you will also need to add the corresponding entry in [mirrorz-d-extension/custom/index.js](https://github.com/mirrorz-org/mirrorz-d-extension/blob/master/custom/index.js).

Finally, modify [mirrorz-config](https://github.com/mirrorz-org/mirrorz-config/). If using the first method, add the JSON URL in the d_mirrors field; if using the latter two methods, add the parser in the d_parsers field.

## For developers

[Repository Architecture](./repo-struct.md)
