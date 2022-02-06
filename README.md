# apt-cacher-ng

docker image for apt-cacher-ng

Forked from: [https://github.com/mbentley/docker-apt-cacher-ng](https://github.com/mbentley/docker-apt-cacher-ng)

## Requirements

* Docker
* docker-compose

## Building

```shell
docker compose build --pull
```

## Running

```shell
docker compose up -d
```

## Notes

Caches Ubuntu packages for x64, Arm, etc.

Packages are saved to: `/srv/docker/apt-cacher-ng`

To add the cacher, edit `/etc/apt/apt.conf.d/01proxy`

```shell
Acquire::HTTP::Proxy "http://192.168.1.226:3142";
Acquire::HTTPS::Proxy "false";
```

Visit the web-ui: `http://192.168.1.226:3142/acng-report.html`

This image runs `apt-cacher-ng`, `cron`, and `rsyslogd` to ensure that apt-cacher-ng functions properly with scheduled jobs and appropriate logging.

In order to configure a host to make use of apt-cacher-ng on a box, you should create a file on the host `/etc/apt/apt.conf` with the following lines:

```shell
Acquire::http::Proxy "http://<docker-host>:3142";
```

You can also bypass the apt caching server on a per client basis by using the following syntax in your `/etc/apt/apt.conf` file:

```shell
Acquire::HTTP::Proxy::<repo-url> "DIRECT";
```

For example:

```shell
Acquire::HTTP::Proxy::get.docker.com "DIRECT";
Acquire::HTTP::Proxy::download.virtualbox.org "DIRECT";
```

Note:  The above assumes that you are mapping port 3142 on the docker host and 3142 is accessible from all machines.

You can also update the /etc/apt-cacher-ng/acng.conf and add one or more `PassThroughPattern` lines to force clients to bypass a repository:

```shell
PassThroughPattern: get\.docker\.com
PassThroughPattern: download\.virtualbox\.org
```
