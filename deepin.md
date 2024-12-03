# 一些关于日常使用deepin的东西
版本： 20.9  体验了下deepin v23 感觉没有20.9体验好。当然这个也是个人感受。。。

## 安装docker 
配置docker源
```shell
curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/debian/gpg | sudo apt-key add -
```
安装docker 
```shell
sudo apt update
sudo apt install docker-ce 
```

## 安装nodejs
下载
```shell
 wget https://cdn.npmmirror.com/binaries/node/v20.18.0/node-v20.18.0-linux-x64.tar.xz
```

解压创建软连接
```shel
 sudo ln -s /usr/local/node/bin/node /usr/bin/
sudo ln -s /usr/local/node/bin/npm /usr/bin/
```

## 安装青龙
```shell
 sudo docker run -dit   -v /home/huihui/ql/data:/ql/data   -p 5700:5700   --name qinglong   --hostname qinglong   --restart unless-stopped    whyour/qinglong:latest
```


## 安装kvm
```shell

```












