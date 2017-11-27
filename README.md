# Docker Alpine Hugo Git Bash

### Docker Image

[![](https://images.microbadger.com/badges/image/jermine/hugo.svg)](http://microbadger.com/images/jermine/hugo "Get your own image badge on microbadger.com") [![](https://images.microbadger.com/badges/version/jermine/hugo.svg)](http://microbadger.com/images/jermine/hugo "Get your own version badge on microbadger.com") [![](https://images.microbadger.com/badges/commit/jermine/hugo.svg)](https://microbadger.com/images/jermine/hugo "Get your own commit badge on microbadger.com")

### Build

[![Build Status](https://travis-ci.org/JermineHu/alpine-hugo.svg?branch=master)](https://travis-ci.org/JermineHu/alpine-hugo)

Minimal container with [Hugo](https://gohugo.io), Git & Bash installed. Leverages [alpine](https://hub.docker.com/_/alpine/) base image.


## Directory Tree

```
~/fig/hugo/
|-- docker-compose.yml
`-- www/
    `-- themes/
        `-- hyde/
```

## docker-compose.yml

```
hugo:
  image: jermine/hugo
  command: >
    server --bind=0.0.0.0
           --port=80
           --baseUrl=http://jermine.vdo.pub/
           --theme=hyde
           --watch
  ports:
    - "80:80"
  volumes:
    - ./www:/www
  restart: always
```

> **WARNING:** `hugo server` is for testing purpose.
> The default value of `--baseUrl` option is `http://<bind>:<port>/`.
> I set it to the same value as that in `config.toml` here.
> It maybe not a good practice.

## Up and Running


To make the command a bit more accessible, create an alias. Note that we add the `-p` option here to make hugo's HTTP port 1313 available.

    alias hugo="docker run --rm -ti -v $(pwd):$(pwd) -w $(pwd) -p 1313:1313 jermine/hugo"

Then we can call hugo like this:
 
```
$ cd ~/fig/hugo/www
$ curl -L https://github.com/spf13/hyde/archive/master.tar.gz | tar xz --strip 1 -C ./themes/hyde/

$ hugo new site .
$ vim config.toml

$ hugo new post/hello-world.md
$ vim content/post/hello-world.md

$ docker-compose up -d

$ hugo

```

> The last `hugo` command will use `baseurl` in `config.toml`.

Open <http://jermine.vdo.pub/> in your browser!


## Build your own

To build your own image, do this:

    docker build -t jermine/hugo .
