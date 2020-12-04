# [hyperf]
```
docker run -v /d/www/hyperf-service:/hyperf-skeleton -p 9501:9501 -it --entrypoint /bin/sh hyperf/hyperf:latest
```
## [制作合适的hyperf镜像]
```
docker tag 741351a4b5fa registry.cn-hangzhou.aliyuncs.com/packages1/hyperf:1
```

# [showdoc]
```
docker run -d --name showdoc --user=root --privileged=true -p 4999:80 -v /d/showdoc_data/html:/var/www/html/ star7th/showdoc
```

# [运行 gitlab]
## [文档]
[url]https://docs.gitlab.com/omnibus/docker/README.html
## [拉取镜像]
```
docker pull gitlab/gitlab-ce:latest
```
## [linux]
```
docker run -d -p 8443:443 -p 8180:80 --name gitlab --restart unless-stopped -v /d/docker/gitlab/config:/etc/gitlab -v /d/docker/gitlab/logs:/var/log/gitlab -v /d/docker/gitlab/data:/var/opt/gitlab gitlab/gitlab-ce:latest
```
## [windows]
```
docker run --detach --hostname 192.168.10.46 --publish 8929:443 --publish 8980:80 --publish 8922:22 --restart always --volume /srv/gitlab/config:/etc/gitlab --volume /srv/gitlab/logs:/var/log/gitlab --volume /srv/gitlab/data:/var/opt/gitlab --name gitlab gitlab/gitlab-ce:latest
```


# [运行 drone]
## [文档]
[url]https://docs.drone.io
## [拉取镜像]
docker pull drone/drone:latest
## [运行镜像]
```
docker run -v /d/docker/drone/data:/data -e DRONE_AGENTS_ENABLED=true -e DRONE_GITLAB_SERVER=http://192.168.10.46:8980 -e DRONE_GITLAB_CLIENT_ID=af36422e5da0ce72b5fda038c01382b04b7a46fee27137d68e72a5f32109bf72 -e DRONE_GITLAB_CLIENT_SECRET=df497a5c4793c779f541fbcaea9d1efd87bb153cc3fed8b002c30a41e485d451 -e DRONE_RPC_SECRET=2eab2105565f2719a3ae6e9d1d0961a7 -e DRONE_SERVER_HOST=192.168.10.46:8981 -e DRONE_SERVER_PROTO=http -e DRONE_TLS_AUTOCERT=false -e DRONE_LOGS_DEBUG=true -e DRONE_USER_CREATE=username:lixs,admin:true --publish=8981:80 --publish=8930:443 --restart=always --detach=true --name=drone drone/drone:latest
```
## [运行 drone-runner-docker]
## [拉取镜像]
docker pull drone/drone-runner-docker:1
## [运行镜像]
docker run -d -v /var/run/docker.sock:/var/run/docker.sock -e DRONE_RPC_PROTO=http -e DRONE_RPC_HOST=192.168.10.46:8981 -e DRONE_RPC_SECRET=2eab2105565f2719a3ae6e9d1d0961a7 -e DRONE_RUNNER_CAPACITY=2 -e DRONE_RUNNER_NAME=192.168.10.46 -p 3000:3000 --restart always --name runner drone/agent:1

# [运行 php(swoole reids : laravels-demo) docker image]
## [运行镜像]
## [拉取镜像]
```
docker pull registry.cn-hangzhou.aliyuncs.com/packages1/php:7.2.4-cli-alpine3.7-swoole4.5.5-redis4.2.0
```
```
--- windows ---
docker run -d -p 9601:9601 -v /d/www/test/laravels:/opt/www -it --name laravels-demo registry.cn-hangzhou.aliyuncs.com/packages1/php:7.2.4-cli-alpine3.7-swoole4.5.5-redis4.2.0
--- linux ---
docker run -d -p 9601:9601 -v /mnt/www/test/laravels:/opt/www -it --name laravels-demo registry.cn-hangzhou.aliyuncs.com/packages1/php:7.2.4-cli-alpine3.7-swoole4.5.5-redis4.2.0
docker exec -it laravels-demo sh
cd /opt/www
composer create-project --prefer-dist laravel/laravel laravels
cd laravels
vi composer.json
add
---
"config": {
	"platform": {
		"ext-pcntl": "7.2",
		"ext-posix": "7.2"
	}
}
---
composer require "hhxsv5/laravel-s:~3.7.0" -vvv
php artisan laravels publish
vi .env
add
---
LARAVELS_LISTEN_IP=0.0.0.0
LARAVELS_LISTEN_PORT=9601
---
php bin/laravels start -d
```

# [运行 k8s]
