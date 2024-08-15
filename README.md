# reskilling_ja
OpenEDXを日本語化する為に必要なファイル、手順を納品して頂く

# 参照イントール方法
https://edly.io/academy/openedx-install/

https://qiita.com/kujiraza/items/a8236f65e2c46735ee91

# 先にDNSレコードを用意しておく
reskilling.dxup.jp

apps.reskilling.dxup.jp

studio.reskilling.dxup.jp

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
