# nats 在线实验环境

## 软件简介

软件基本信息，License，所属的类别，作用，特点等。
nats 是NATS消息系统的高性能服务器，是一种开放源码的高性能云本地消息系统

所属类别是服务器软件

License:The MIT License (MIT)

特点：

小巧轻便，性能高

## 软件官网

http://nats.io/

### Dockerfile 使用方法
```
# Run a NATS server
# Each server exposes multiple ports
# 4222 is for clients.
# 8222 is an HTTP management port for information reporting.
# 6222 is a routing port for clustering.
# use -p or -P as needed.

$ docker run -d --name nats-main nats
[INF] Starting nats-server version 0.9.6
[INF] Starting http monitor on 0.0.0.0:8222
[INF] Listening for client connections on 0.0.0.0:4222
[INF] Server is ready
[INF] Listening for route connections on 0.0.0.0:6222

...

# To run a second server and cluster them together..
$ docker run -d --name=nats-2 --link nats-main nats --routes=nats-route://ruser:T0pS3cr3t@nats-main:6222

# If you want to verify the routes are connected, try
$ docker run -d --name=nats-2 --link nats-main nats --routes=nats-route://ruser:T0pS3cr3t@nats-main:6222 -DV
[INF] Starting nats-server version 0.9.6
[DBG] Go build version go1.6.3
[INF] Starting http monitor on 0.0.0.0:8222
[INF] Listening for client connections on 0.0.0.0:4222
[DBG] Server id is HQLO4vvXi434nlqWjbTbec
[INF] Server is ready
[INF] Listening for route connections on 0.0.0.0:6222
[DBG] Trying to connect to route on nats-main:6222
[DBG] 172.17.0.2:6222 - rid:1 - Route connection created
[DBG] 172.17.0.2:6222 - rid:1 - Route connect msg sent
[DBG] 172.17.0.2:6222 - rid:1 - Registering remote route "PRJPXBdDnqK5ATfyQ7Jjlv"
[DBG] 172.17.0.2:6222 - rid:1 - Route sent local subscriptions
```
服务器将加载下面的配置文件。任何命令行标志都可以覆盖这些值。

### 默认配置文件
```
# Client port of 4222 on all interfaces
port: 4222

# HTTP monitoring port
monitor_port: 8222

# This is for clustering multiple servers together.
cluster {

  # Route connections to be received on any interface on port 6222
  port: 6222

  # Routes are protected, so need to use them with --routes flag
  # e.g. --routes=nats-route://ruser:T0pS3cr3t@otherdockerhost:6222
  authorization {
    user: ruser
    password: T0pS3cr3t
    timeout: 0.75
  }

  # Routes are actively solicited and connected to from this server.
  # This Docker image has none by default, but you can pass a
  # flag to the gnatsd docker image to create one to an existing server.
  routes = []
}
```
### 命令行选项
```
Server Options:
    -a, --addr <host>                Bind to host address (default: 0.0.0.0)
    -p, --port <port>                Use port for clients (default: 4222)
    -P, --pid <file>                 File to store PID
    -m, --http_port <port>           Use port for http monitoring
    -ms,--https_port <port>          Use port for https monitoring
    -c, --config <file>              Configuration file

Logging Options:
    -l, --log <file>                 File to redirect log output
    -T, --logtime                    Timestamp log entries (default: true)
    -s, --syslog                     Enable syslog as log method
    -r, --remote_syslog <addr>       Syslog server addr (udp://localhost:514)
    -D, --debug                      Enable debugging output
    -V, --trace                      Trace the raw protocol
    -DV                              Debug and trace

Authorization Options:
        --user <user>                User required for connections
        --pass <password>            Password required for connections
        --auth <token>               Authorization token required for connections

TLS Options:
        --tls                        Enable TLS, do not verify clients (default: false)
        --tlscert <file>             Server certificate file
        --tlskey <file>              Private key for server certificate
        --tlsverify                  Enable TLS, verify client certificates
        --tlscacert <file>           Client certificate CA for verification

Cluster Options:
        --routes <rurl-1, rurl-2>    Routes to solicit and connect
        --cluster <cluster-url>      Cluster URL for solicited routes
        --no_advertise <bool>        Advertise known cluster IPs to clients


Common Options:
    -h, --help                       Show this message
    -v, --version                    Show version
        --help_tls                   TLS help
```
## 资源链接

- http://nats.io/ 
