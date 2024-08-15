# reskilling_ja
OpenEDXを日本語化する為に必要なファイル、手順を納品して頂く

# 参照イントール方法
https://edly.io/academy/openedx-install/

https://qiita.com/kujiraza/items/a8236f65e2c46735ee91

# 現在の契約筐体
6 CPU
8G Memory

# OS
Ubuntu 22.04.4 LTS

# システム的な課題
Dockerで1台で簡単に動かしてしまっているが、冗長性を確保する必要がある

# 最終稼働状態のdocker ps
```
CONTAINER ID   IMAGE                                  COMMAND                  CREATED        STATUS        PORTS      NAMES
fea484292383   overhangio/openedx:17.0.5-indigo       "celery --app=cms.ce…"   2 months ago   Up 26 hours   8000/tcp   tutor_local-cms-worker-1
a729aead0a55   overhangio/openedx:17.0.5-indigo       "/bin/sh -c 'uwsgi u…"   2 months ago   Up 26 hours   8000/tcp   tutor_local-cms-1
2d39ecff4447   overhangio/openedx:17.0.5-indigo       "celery --app=lms.ce…"   2 months ago   Up 26 hours   8000/tcp   tutor_local-lms-worker-1
1229fe3eb89a   overhangio/openedx-mfe:17.0.1-indigo   "caddy run --config …"   2 months ago   Up 26 hours   80/tcp, 443/tcp, 2019/tcp, 443/udp    tutor_local-mfe-1
6617a6597d3d   overhangio/openedx:17.0.5-indigo       "/bin/sh -c 'uwsgi u…"   2 months ago   Up 26 hours   8000/tcp   tutor_local-lms-1
df311a3947c9   overhangio/openedx-forum:17.0.0        "/app/bin/docker-ent…"   2 months ago   Up 26 hours   4567/tcp   tutor_local-forum-1
f7a20f812426   redis:7.2.4                            "docker-entrypoint.s…"   2 months ago   Up 26 hours   6379/tcp   tutor_local-redis-1
71fa58415fa5   elasticsearch:7.17.13                  "/bin/tini -- /usr/l…"   2 months ago   Up 26 hours   9200/tcp, 9300/tcp     tutor_local-elasticsearch-1
6ddf0f94e393   mongo:4.4.25                           "docker-entrypoint.s…"   2 months ago   Up 26 hours   27017/tcp   tutor_local-mongodb-1
68877d31975b   caddy:2.7.4                            "caddy run --config …"   2 months ago   Up 26 hours   0.0.0.0:80->80/tcp, :::80->80/tcp, 0.0.0.0:443->443/tcp, :::443->443/tcp, 0.0.0.0:443->443/udp, :::443->443/udp, 2019/tcp   tutor_local-caddy-1
7cd55d86f2c6   mysql:8.1.0                            "docker-entrypoint.s…"   2 months ago   Up 26 hours   3306/tcp, 33060/tcp    tutor_local-mysql-1
cbd42b4a07ac   devture/exim-relay:4.96-r1-0           "/sbin/tini -- exim …"   2 months ago   Up 26 hours   8025/tcp      tutor_local-smtp-1
```

# 先にDNSレコードを用意しておく
reskilling.dxup.jp

apps.reskilling.dxup.jp

studio.reskilling.dxup.jp

preview.reskilling.dxup.jp

# sshログイン
ssh -l www prod-rk-web1

## 全て0に
vi /etc/apt/apt.conf.d/20auto-upgrades

sudo apt update

sudo apt install \

 ca-certificates curl gnupg lsb-release

# Dockerの公式GPGキーを追加
sudo mkdir -p /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg \

  | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Dockerリポジトリ登録
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  
  https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" \
  
  | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Docker Engineのインストール
sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io

systemctl start docker

systemctl enable docker

vi =~/.bash_profile

export PATH=/usr/libexec/docker/cli-plugins:$PATH

apt-get install -y pip;

pip install "tutor[full]";

useradd openedx;
# add openedx to docker group

sudo su - openedx;

tutor local launch

Are you configuring a production platform? Type 'n' if you are just testing Tutor on your local computer [Y/n] y

Your website domain name for students (LMS) [www.myopenedx.com] reskilling.dxup.jp

Your website domain name for teachers (CMS) [studio.reskilling.dxup.jp]

Your platform name/title [My Open edX] DXUP: Reskilling

Your public contact email address [contact@reskilling.dxup.jp] $EMAIL

The default language code for the platform [en] ja

The default language code for the platform [en] ja-jp

tutor local do createuser --staff --superuser $ADMINUSER $ADMINUSEREMAIL
Pass: $ADMINPASSWORD

tutor local do importdemocourse

tutor plugins enable forum

tutor local launch
