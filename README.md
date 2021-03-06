Linux Package Proxy Cache
=========================

Proxy server intended for caching _.rpm_ and _.deb_ packages from a Linux
repository mirror.

Usage
=====

Run proxy container
-------------------

```text
$ docker run \
	--name="packages" \
	--rm=true \
	--volume=repository-package-cache:/var/cache/nginx:Z \
	docker.io/aesiniath/proxy:latest
```

runs in foreground and logs when requests hit its cache. In this repo this
command is conveniently contained in _./run.sh_ so:

```text
$ ./run.sh
```

Change configuration of new images
----------------------------------

Change the repository configurations in your base image to point to the
internal package mirror:

### Fedora

In _/etc/yum.repos.d/_ adjust _fedora.repo_ to have:

```text
baseurl=http://127.0.0.1:1999/fedora/linux/releases/$releasever/Everything/$basearch/os/
```
and in _fedora-updates.repo_ to have:

```text
baseurl=http://127.0.0.1:1999/fedora/linux/updates/$releasever/$basearch/
```

### Debian

Adjust _/etc/apt/sources.list_ to have:

```text
deb http://127.0.0.1:1999/debian/ stretch main
deb http://127.0.0.1:1999/debian/ stretch-updates main
```

Use normally
------------

Build on a [Fedora](https://github.com/aesiniath/docker-fedora) base image:

```text
$ docker run -i -t --rm --network="proxy" localhost/afcowie/fedora:27 bash
ab317b9920d3 / # dnf install -y findutils
...
```

or building on a [Debian](https://github.com/afcowie/docker-debian) base image:

```text
$ docker run -i -t --rm docker.io/aesiniath/debian:stretch bash
d874ac8dd5d4 / # apt-get install findutils
...
```

Enjoy!

Bugs
====

See [TODO](TODO.markdown).

Acknowledgements
================

This container was set up based on incredibly helpful posts from:

 - Michał Czeraszkiewicz, <http://czerasz.com/2015/03/30/nginx-caching-tutorial/>
 - James Nzomo, <http://tdt.rocks/repo_cache_ft_nginx.html>
