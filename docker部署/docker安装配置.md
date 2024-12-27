## ubuntu版本
```http
https://cloud.tencent.com/document/product/213/46000#1H-kXbk9zoqvzYMVPVsBO
```

### 安装Docker
###### 1、执行以下命令，添加Docker软件源
```bash
sudo apt-get update

sudo apt-get install ca-certificates curl -y

sudo install -m 0755 -d /etc/apt/keyrings

sudo curl -fsSL https://mirrors.cloud.tencent.com/docker-ce/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc

sudo chmod a+r /etc/apt/keyrings/docker.asc

echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://mirrors.cloud.tencent.com/docker-ce/linux/ubuntu/ \

  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" |   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

```
###### 2、执行以下命令，安装 Docker
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

###### 3、执行以下命令，运行 Docker
```bash
systemctl start docker
```
###### 4、执行以下命令，检查安装结果
```bash
docker info
```


### 添加镜像源
###### 1、执行以下命令，打开 `/etc/docker/daemon.json` 配置文件。
```bash
vim /etc/docker/daemon.json
```
###### 2、 按 **i** 切换至编辑模式，添加以下内容，并保存。
```
{

   "registry-mirrors": [

   "https://mirror.ccs.tencentyun.com"

  ]

}
```
###### 3、执行以下命令，重启 Docker 即可。
```
sudo systemctl restart docker
```
###### 4、重启 Docker 后，并运行以下命令来查看当前 Docker 的配置。如镜像源配置成功，则输出的
```
sudo docker info
```
内容中会包含下图所示的部分。
![[../static/Pasted image 20241225175259.png]]