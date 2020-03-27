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

执行命令：`phantomjs /data/nas.js`

返回`初始化完毕`说明运行成功。
### 捷径安装
----
#### 扫描二维码在Safari浏览器中打开；
![](http://qr.topscan.com/api.php?text=https://www.icloud.com/shortcuts/c1dc6ce07bf947178916668f0c895215)
### 捷径配置
----
![](https://www.wan7.xin/wp-content/uploads/2020/03/nasjiejing-8.png)
### 进阶指导
----
Ios捷径已经模块化，按照nas.js模块方法可自行制作所需功能。
```
"电影": {
	"Select": "Video", //首次获取信息列表函数名。
	"Down": "VideoDown", //接收ios捷径选择的信息，并对信息处理，返回结果。
	"Dir": "\/video", //Aria2下载地址
	"Msg": "请输入影视名称 数据格式：影视名" //提示信息
}
```
`Select`为首次调用方法，对应`Video`函数。
```
'Video': function(request, response) {
    var url = 'https://www.tlyy.cc/search.asp?searchword=' + urlEncode(JSON.parse(request.post)['url']); //生成链接urlEncode对字符串进行urlGb2312编码
    console.log(url); //打印url到控制台
    page.open(url, function() { //打开Url并获取信息
      page.includeJs("https://cdn.bootcss.com/jquery/3.4.1/jquery.js", function() { //引入jquery方便对元素操作
        var list = page.evaluate(function() { //在网页内运行脚本
          var VideoInfoList = {}; //定义一个存储信息对象
          $(".watch li").each(function(index) { //遍历clss为watch的li标签
            var item = { //将获取到的标题与url保存到变量
              name: $(this).children("h4").children("a")[0].text,
              url: $(this).children("h4").children("a")[0].href
            }
            VideoInfoList[item.name] = item; //将获取得到的信息存入对象
          });
          console.log(JSON.stringify(VideoInfoList)); //将对象转换为json字符串并打印到控制台
          return JSON.stringify(VideoInfoList); //返回信息
        });
        response.statusCode = 200; //回应给捷径请求无措
        response.write(list); //回应给捷径获取到的信息
        response.close(); //关闭与捷径的通讯
      });
    });
    return
  }
```
### 问题解答
----
[找不到「允许不受信任的快捷指令」怎么办？](https://jiejingku.net/2276.html)
[ios13无法安装第三方捷径怎么办？不允许不受信任的快捷指令解决方法](https://jiejingku.net/2162.html)
[ios13如何在捷径库中添加「不受信任的快捷指令」？](https://jiejingku.net/2151.html)
