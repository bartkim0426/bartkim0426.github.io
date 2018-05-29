---
layout: post
title:  "Docker Compose"
categories: docker
tags: docker container
---

### Docker compose 기초
- 컨테이너들 간의 관계 설정
- easy-to-read 파일로 세팅해서 container를 run하기 위해서
- one-liner startup을 위해서
- 2 가지로 구성
1. `YAML`: container, network, volume에 관련한 옵션
2. CLI tools (`docker-compose`)


### docker-compose.yml
- YAML 포멧 버전: 1, 2, 2.1, 3, 3.1 - first line
- `docker-compose` 코멘드로 쉽게 사용
- 기본 포멧

```
version: '3.1'

service:
	servicename:
		image: # optional
		command: # optional
		environment: # optional
		volumes # optional
	servicename2: # if have second service...
volumes: # optional

network: # optional
```

#### docker-compose.yml 예시 		
```
version: '2'

# same as
# docker run -p 80:4000 -v $(pwd):/site bretfisher/jekyll-serve

services:
	jekyll:
		image: bretfisher/jekyll-serve
		volume: 
			- .:/site
		port:
			- '80:4000'
```

> docker command에서 `-p`, `-v` 등으로 사용한 명령어들은 기본적으로 `-`(dash)를 포함해서 사용
> 만약 key, value 형태로 사용하고 싶으면 dash 없이. (아래 예시)


#### `wordpress` 예시 (wordpress server + db server)
```
version: '2'

services:
	wordpress:
		image: wordpress
		ports:
			- 8000:80
		environment:
			# key: value 형태로
			WORDPRESS_DB_PASSWORD: example
			# - 사용해서 command 형태로 사용
			- WORDPRESS_DB_NAME=example
```


## docker-compose CLI
- Docker for windows/mac, download for Linux
- production level에서 사용하는 tool은 아님 - local dev, test 환경에서
- 가장 많이 사용하는 command
	1. `docker-compose up` # setup volumes/networks, start all containers
	2. `docker-compose down` # stop all containers, remove cont/vol/net
- `.yaml`을 가진 github project를 `clone` 한다음 `docker-dompose up` 해보기 가능

### run sample nginx server

#### `nginx.conf` file
```
server {

	listen 80;

	location / {

		proxy_pass         http://web;
		proxy_redirect     off;
		proxy_set_header   Host $host;
		proxy_set_header   X-Real-IP $remote_addr;
		proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header   X-Forwarded-Host $server_name;

	}
}
```

#### `docker-compose.yml` file
```
version: '3'

services:
  proxy:
    image: nginx:1.13 # this will use the latest version of 1.11.x
    ports:
      - '80:80' # expose 80 on host and sent to 80 in container
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf:ro
  web:
    image: httpd  # this will use httpd:latest
```

#### `docker-compose` CLI

``` 
$docker-compose up
Attaching to composesample2_web_1, composesample2_proxy_1

web_1    | AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.20.0.2. Set the 'ServerName' directive globally to suppress this message
web_1    | AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.20.0.2. Set the 'ServerName' directive globally to suppress this message
web_1    | [Wed Dec 20 02:39:49.356840 2017] [mpm_event:notice] [pid 1:tid 140596108134272] AH00489: Apache/2.4.29 (Unix) configured -- resuming normal operations
web_1    | [Wed Dec 20 02:39:49.359227 2017] [core:notice] [pid 1:tid 140596108134272] AH00094: Command line: 'httpd -D FOREGROUND'
web_1    | 172.20.0.3 - - [20/Dec/2017:02:41:05 +0000] "GET / HTTP/1.0" 200 45
proxy_1  | 172.20.0.1 - - [20/Dec/2017:02:41:05 +0000] "GET / HTTP/1.1" 200 45 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/603.3.8 (KHTML, like Gecko) Version/10.1.2 Safari/603.3.8" "-"
web_1    | 172.20.0.3 - - [20/Dec/2017:02:41:05 +0000] "GET /apple-touch-icon-precomposed.png HTTP/1.0" 404 230
proxy_1  | 172.20.0.1 - - [20/Dec/2017:02:41:05 +0000] "GET /apple-touch-icon-precomposed.png HTTP/1.1" 404 230 "-" "Safari/12603.3.8 CFNetwork/811.5.4 Darwin/16.7.0 (x86_64)" "-"
web_1    | 172.20.0.3 - - [20/Dec/2017:02:41:05 +0000] "GET /apple-touch-icon.png HTTP/1.0" 404 218
```
=> `web_1`, `proxy_1` 이 다른 색상 (랜덤으로 표시)으로 표기되는 것을 볼 수 있다. 개발 중에는 `-d`를 사용하지 않고 이렇게 보면서 사용 가능


### `docker-compose` CLI commands
```
# help 명령어 - 사용 가능한 docker-compose의 모든 명령어
$docker-compose --help

# background에서 실행하기
$docker-compose up -d

# logs 확인
$docker-compose logs

# ps 확인
$docker-compose ps
         Name                  Command          State         Ports
--------------------------------------------------------------------------
composesample2_proxy_1   nginx -g daemon off;   Up      0.0.0.0:80->80/tcp
composesample2_web_1     httpd-foreground       Up      80/tcp

$docker-compose top

composesample2_proxy_1
PID    USER   TIME                    COMMAND
---------------------------------------------------------------
2797   0      0:00   nginx: master process nginx -g daemon off;
3071   101    0:00   nginx: worker process

composesample2_web_1
PID    USER   TIME        COMMAND
---------------------------------------
2748   0      0:00   httpd -DFOREGROUND
2969   1      0:00   httpd -DFOREGROUND
2970   1      0:00   httpd -DFOREGROUND
2971   1      0:00   httpd -DFOREGROUND

# stop commands
$docker-compose down
```

### Assignment: writing compose file
- build docker-compose file: `Drupal` content management system website
- use `drupal` image along with `postgres` image
- use `ports` expose Drupal `8080`
- set `POSTGRES_PASSWORD`
- using volumes to store Drupal unique data



## Using compose to build
- compose can build custom images
- will build wtih `docker-compose up`: only build if not found in cache
- rebuild with `docker-compose build`
- Great for complex builds (lots of variable)


#### `docker-compose.yml` 파일
```
version: '2'

services:
	proxy:
		build:
			context: .
			# choose dockerfile you want to build
			dockerfile: nginx.Dockerfile
		# local cache에서 해당 image를 찾아서 없으면 위 dockerfile 이용해서 build
		# image를 적어 놓으면 만들어진 이미지가 해당 이름을 가짐
		image: nginx-custom
		ports:
			- '80:80'
	web:
		image: httpd
		volumes:
		  - ./html:/usr/local/apache2/htdocs/
```

#### `nginx.Dockerfile`
```
FROM nginx:1.13

COPY nginx.conf /etc/nginx/conf.d/default.conf
```

이제 `docker-compose up`을 하면 cache에서 해당 이미지를 못찾고 build한 후 run을 실행함

```
$docker-compose up

# stop runuing with delete every image
$docker-compose down --rmi local
```


## Assignment: Build and run compose
- build custom drupal image for local setting


#### `Dockerfile`
```
FROM drupal:8.2

# install -y : 자동으로 yes
RUN apt-get update && apt-get install -y git \
	&& rm -rf /var/lib/apt/lists/*

WORKDIR /var/www/html/themes

# only clone specific branch: save time, space
RUN git clone --branch 8.x-3.x --single-branch --depth 1 https://git.drupal.org/project/bootstrap.git \
# docker only work with root
# so have to change permission
	&& chown -R www-data:www-data bootstrap

WORKDIR /var/www/html
```

#### `docker-compose.yml`
```
version: '2'

services:
    drupal:
        image: custom-drupal
		# shortcut to build from current dir
		build: .
        ports:
            - 8080:80
        volumes:
          - drupal-modules:/var/www/html/modules
          - drupal-profiles:/var/www/html/profiles
          - drupal-sites:/var/www/html/themes
          - drupal-themes:/var/www/html/sites
        restart: always
    postgres:
        image: postgres:9.6
        environment:
            # - POSTGRES_PASSWORD=tmfcks478
            POSTGRES_PASSWORD: tmfcks478
        restart: always
		volumes:
			- drupal-data:/var/lib/postgresql/data
volumes:
	drupal-data:
	drupal-modules:
	drupal-profiles:
	drupal-themes:
	drupal-sites:
```

#### use `docker-compose` CLI
```
$docker-compose up

# 이후 drupal 기본 세팅을 한 뒤
# docker-compose up을 다시 실행해도 volumes의 설정들, db는 그대로 남아있는 것을 볼 수 있다.
$docker-compose down
$docker-compose up
```
