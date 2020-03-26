### 介绍
----
疫情期间兴趣使然写了一个ios捷径；为了方便下载影音/文件到Nas;
### Nas安装
----
在Docker中安装oldiy-aria2-ui-ng，wernight-phantomjs两个容器。

aria2的安装请遵循这篇文章[群晖Docker安装Aria2+WebUI+AriaNG+FilesWeb 115插件-百度插件 全速下载 中文版教程](https://odcn.top/2019/01/20/2144/%E7%BE%A4%E6%99%96docker%E5%AE%89%E8%A3%85aria2webuiariangfilesweb115%E6%8F%92%E4%BB%B6%E5%85%A8%E9%80%9F%E4%B8%8B%E8%BD%BD-%E4%B8%AD%E6%96%87%E7%89%88%E6%95%99%E7%A8%8B/)

wernight-phantomjs容器名称定义为phantomjs，安装设置网络为"使用与 Docker Host 相同的网络"，创建共享目录Docker>phantomjs，将phantomjs目录挂在到/data；

两个容器安装完毕后，下载nas.js到phantomjs目录；
![](https://www.wan7.xin/wp-content/uploads/2020/03/nasjiejing-5.png)
### Nas配置
----
编辑phantomjs目录下nas.js，修改以下四处信息为自己的；

注意SecretKey为与ios捷径通讯密钥，在捷径中需要配置为相同密钥；

注意Jsonrpc地址推荐使用内网地址，Jsonrpc并非Aria2web地址，为Aria2NG RPC中的地址；

Aria2jsonrpc地址`Jsonrpc = 'http://10.10.10.10:6800/jsonrpc'; //Aria2jsonrpc地址`

Aria2jsonrpc密钥`JsonrpcKey = 'rrsyycm'; //Aria2jsonrpc密钥`

与捷径的通讯密钥`SecretKey = '此处填写通讯密钥'; //与捷径的通讯密钥`

Phantomjs与捷径的通讯端口`PhantomjsPort = 4363; //Phantomjs与捷径的通讯端口`
### Nas运行
----
使用ssh链接群晖

执行命令：`sudo -i`

输入密码回车。

执行命令：`docker exec -it phantomjs /bin/bash`，此处phantomjs为容器名称；

执行命令：`phantomjs -v`

返回版号说明环境完好。

执行命令：`phantomjs /data/nasjiejing.js`

返回`初始化完毕`说明运行成功。
### 捷径安装
----
#### 扫描二维码在Safari浏览器中打开；
![](https://www.wan7.xin/wp-content/uploads/2020/03/qrcode.png)
### 捷径配置
----
![](https://www.wan7.xin/wp-content/uploads/2020/03/nasjiejing-8.png)
