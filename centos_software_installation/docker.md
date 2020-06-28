# docker

## mysql

1.Get the mysql image

```shell
docker pull mysql:5.7.30
```

2.Create local folders and configuration files

```shell
mkdir -p /opt/docker/mysql/conf
mkdir -p /opt/docker/mysql/data
tee /opt/docker/mysql/conf/my.cnf <<-'EOF'
[mysqld]
user=mysql
character-set-server=utf8
default_authentication_plugin=mysql_native_password

[client]
default-character-set=utf8

[mysql]
default-character-set=utf8
EOF
```

3.Run the container

```shell
docker run -itd -p 3306:3306 --restart always --privileged=true --name mysql -e MYSQL_USER="username" -e MYSQL_PASSWORD="password" -e MYSQL_ROOT_PASSWORD="root_password" -v=/opt/docker/mysql/conf:/etc/mysql/conf.d -v=/opt/docker/mysql/data:/var/lib/mysql mysql:5.7.30
```

## redis

1.Get the redis image

```shell
docker pull redis
```

2.Create local folder and configuration files

```shell
mkdir -p /opt/docker/redis/conf
mkdir -p /opt/docker/redis/conf
```

Modify configuration file `redis.conf`

```shell
# bind 127.0.0.1
requirepass password
```

3.Run the container

```shell
docker run -itd --restart always --privileged=true -p 6379:6379 -v /opt/docker/redis/conf/redis.conf:/etc/redis/redis.conf -v /opt/docker/redis/data:/data --name redis redis:latest redis-server /etc/redis/redis.conf --appendonly yes
```
