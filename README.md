# How to use
## Nginxの設定
`.docker/volumes/nginx/conf.d/default.conf`

```
server {
  listen 80;
  server_name {ここは任意のドメインに変更};
  location / {
    proxy_pass http://jenkins:8080;
  }
}

server {
  listen 80;
  server_name {ここは任意のドメインに変更};
  location / {
    proxy_pass http://grafana:3000;
  }
}

server {
  listen 80;
  server_name {ここは任意のドメインに変更};
  location / {
    proxy_pass http://influxdb:8083;
  }
}

server {
  listen 80;
  server_name {ここは任意のドメインに変更};
  location / {
    proxy_pass http://prometheus:9090;
  }
}
```

## Prometheusの設定
`prometheus.yml`

```
global:
  scrape_interval: 15s
  evaluation_interval: 15s
  external_labels:
    monitor: 'monitor'

# 監視対象の設定
scrape_configs:
  - job_name: 'prometheus'
    static_configs:
    - targets:
      - 'prometheus:9090'
  - job_name: 'node'
    static_configs:
    - targets:
      - 'localhost:9100' # localhostだと拾えない？
      labels:
        group: 'docker-host'
  - job_name: 'docker'
    static_configs:
    - targets:
      - 'cadvisor:8080'
      labels:
        group: 'docker-container'
```

## Grafanaの環境変数設定
`grafana.env`

```
# 管理者ユーザー
GF_SECURITY_ADMIN_USER=admin
# 管理者パスワード
GF_SECURITY_ADMIN_PASSWORD=password
# サインアップの無効化
GF_USERS_ALLOW_SIGN_UP=false
# 管理者以外のorgnization作成の無効化
GF_USERS_ALLOW_ORG_CREATE=false
```
