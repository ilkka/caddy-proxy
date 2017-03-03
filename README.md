# caddy-proxy
Automated [caddy](https://github.com/mholt/caddy) proxy for Docker containers using docker-gen, based on [BlackGlory's version](https://github.com/BlackGlory/caddy-proxy).

### Usage

Start any containers you want proxied with an env var `VIRTUAL_HOST=subdomain.youdomain.com` just like [nginx-proxy](https://github.com/jwilder/nginx-proxy):
```sh
$ docker run -e VIRTUAL_HOST=foo.bar.com  ...
```

Traffic is proxied from HTTP and HTTPS ports to exported ports on the containers. If the containers export more than one port, the proxied port should be set by setting `VIRTUAL_PORT` in the environment of the container.

If you want the container protected by HTTP Basic Authentication add a `BASIC_AUTH` env var with the path to protect (i.e. `/`), username, and password:
```sh
$ docker run -e VIRTUAL_HOST=foo.bar.com -e BASIC_AUTH="/ myname mysecrect" ...
```

Then to run it:
```sh
$ docker run -v /var/run/docker.sock:/tmp/docker.sock:ro -v /data/.caddy:/root/.caddy --name caddy-proxy -p 80:80 -p 443:443 -e CADDY_OPTIONS="--email youremail@example.com" -d blackglory/caddy-proxy:0.2.1
```

When you launch new (or stop) containers caddy-proxy will reload its configuration to make the new containers available.
