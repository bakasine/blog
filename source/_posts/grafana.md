---
title: Prometheus + Grafana 搭建服务器监控系统
date: 2024-12-14 19:19:39
tags:
	- prometheus
    - grafana
    - docker
categories: 
    - tool
---

[安装 Docker](#docker)
[安装 Grafana](#grafana)
[安装 Prometheus](#prometheus)
[安装 Node Exporter](#node_exporter)
[添加 Node](#node)

# <h2 id="docker">安装 Docker</h2>

`curl -fsSL https://get.docker.com |bash`

# <h2 id="grafana">安装 Grafana</h2>

`docker run -d --name=grafana -p 3000:3000 grafana/grafana`

* 默认账号密码均为 admin

# <h2 id="Prometheus">安装 prometheus</h2>

`docker run -d -p 9090:9090 -v /root/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus`

* 如果报错需要手动创建/root/prometheus/prometheus.yml

```yaml
global:
  scrape_interval:     15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: prometheus
    static_configs:
      - targets: ['127.0.0.1:9090']
        labels:
          instance: prometheus
```

# <h2 id="node_exporter">安装 Node Exporter</h2>

* 一键脚本 `bash -c "$(curl -sL https://raw.githubusercontent.com/uerax/script/master/node-exporter.sh)" @`

[下载对应版本](https://github.com/prometheus/node_exporter/releases/tag/v1.8.2)

# <h2 id="node">添加 Node</h2>

* 要注意由于 Grafana 和 Prometheus 都是安装在 Docker 里, 无法用 127.0.0.1 进行互相访问, 需要用 `ip address` 查询 Docker 对应的 IP 段才可以访问

```yaml
scrape_configs:
  - job_name: 'linux'
    static_configs:
      - targets: ['127.0.0.1:9100']
        labels:
          server_name: 'server1'
      - targets: ['127.0.0.1:9100']
        labels:
          server_name: 'server2'
```


