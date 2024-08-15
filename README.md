# reskilling_ja
OpenEDXを日本語化する為に必要なファイル、手順を納品して頂く

# 参照イントール方法
https://edly.io/academy/openedx-install/

https://qiita.com/kujiraza/items/a8236f65e2c46735ee91

# 現在の契約筐体
6 CPU
8G Memory
100GB (初期で16GB使用)

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

# 最終稼働状態のdocker stats
```
CONTAINER ID   NAME                          CPU %     MEM USAGE / LIMIT    MEM %     NET I/O           BLOCK I/O         PIDS
fea484292383   tutor_local-cms-worker-1      0.01%     1.204GiB / 7.75GiB   15.54%    360MB / 116MB     97.6MB / 61.7MB   25
a729aead0a55   tutor_local-cms-1             0.01%     538MiB / 7.75GiB     6.78%     140MB / 133MB     59.2MB / 2.59MB   22
2d39ecff4447   tutor_local-lms-worker-1      0.01%     977.7MiB / 7.75GiB   12.32%    138MB / 99.3MB    38MB / 109MB      13
1229fe3eb89a   tutor_local-mfe-1             0.00%     40.43MiB / 7.75GiB   0.51%     1.12MB / 187MB    52.7MB / 53.2kB   12
6617a6597d3d   tutor_local-lms-1             0.01%     544.5MiB / 7.75GiB   6.86%     78.6MB / 140MB    50.6MB / 3.06MB   16
df311a3947c9   tutor_local-forum-1           0.00%     240.1MiB / 7.75GiB   3.03%     29.7MB / 9.74MB   6.37MB / 28.7kB   18
f7a20f812426   tutor_local-redis-1           0.01%     22.13MiB / 7.75GiB   0.28%     216MB / 455MB     70MB / 4.52GB     6
71fa58415fa5   tutor_local-elasticsearch-1   0.01%     1.438GiB / 7.75GiB   18.56%    760kB / 186kB     118MB / 256MB     90
6ddf0f94e393   tutor_local-mongodb-1         0.01%     124.8MiB / 7.75GiB   1.57%     63.3MB / 245MB    61.4MB / 306MB    90
68877d31975b   tutor_local-caddy-1           0.00%     41.02MiB / 7.75GiB   0.52%     389MB / 144MB     10.7MB / 545kB    12
7cd55d86f2c6   tutor_local-mysql-1           0.01%     471.7MiB / 7.75GiB   5.94%     21.8MB / 37MB     164MB / 134MB     45
cbd42b4a07ac   tutor_local-smtp-1            0.00%     8.246MiB / 7.75GiB   0.10%     126kB / 91.1kB    6.62MB / 127kB    2
```

# 最終稼働状態のdocker images
```
REPOSITORY                       TAG             IMAGE ID       CREATED         SIZE
overhangio/openedx               17.0.5-indigo   db73c971e754   2 months ago    3.08GB
overhangio/openedx-permissions   17.0.5          d654358823e0   3 months ago    5.61MB
redis                            7.2.4           9b38108e295d   4 months ago    116MB
overhangio/openedx-mfe           17.0.1-indigo   81c262540304   4 months ago    236MB
overhangio/openedx-forum         17.0.0          b218d339ea25   8 months ago    688MB
mongo                            4.4.25          e772806c1f73   10 months ago   432MB
caddy                            2.7.4           7a7b2e234b15   10 months ago   48.8MB
elasticsearch                    7.17.13         5e5e53187282   11 months ago   623MB
mysql                            8.1.0           ae2502152260   12 months ago   574MB
hello-world                      latest          d2c94e258dcb   15 months ago   13.3kB
devture/exim-relay               4.96-r1-0       ff3cb869215e   20 months ago   9.92MB
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
