# nezha
哪吒探针搭建记录
# 一、域名解析需要的操作
建议用两个(子)域名做解析，第一个是面板的域名，套CDN比较方便，第二个仅仅解析到面板服务器的域名，用于客户端连接服务端试用(这个可以没有，但是不建议，如果直接用IP的话，迁移面板后会非常麻烦！)
比如我的tz.haoduck.com作为面板的域名，还有一个tzzzz.haoduck.com是用来记录面板服务器的IP
# 二、GitHub上需要的操作
## 创建一个OAuth Apps
先打开：https://github.com/settings/developers，然后点击New OAuth App按钮
![](https://cdn.jsdelivr.net/gh/aProfessor23/PicBed@main/img/202203122224581.png)

参考下图填写，记得端口也填写上去
![](https://cdn.jsdelivr.net/gh/aProfessor23/PicBed@main/img/202203122233857.png)

OAuth Apps的Client ID和Client secrets
![](https://cdn.jsdelivr.net/gh/aProfessor23/PicBed@main/img/202203122235123.png)
# 三、面板服务器上需要的操作
放行8008、5555两个端口
部署面板
```
国外机器
curl -L https://raw.githubusercontent.com/naiba/nezha/master/script/install.sh  -o nezha.sh && chmod +x nezha.sh
sudo ./nezha.sh
```
选择1，安装面板端，按照提示填入指定内容即可
安装完毕，访问http://tz.haoduck.com:8008
# 反代、SSL、CDN
利用宝塔建站，开启SSL，反代
在宝塔面板中新建站点，我这里是tz.haoduck.com,然后设置反代
```
反代配制文件
location /
{
    proxy_pass http://127.0.0.1:8008;
    proxy_set_header Host $host;
}
location /ws
{
    proxy_pass http://127.0.0.1:8008;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
    proxy_set_header Host $host;
}
```
![](https://cdn.jsdelivr.net/gh/aProfessor23/PicBed@main/img/202203122300236.png)

反代完成，回到GitHub的OAuth Apps设置，按下图
![](https://cdn.jsdelivr.net/gh/aProfessor23/PicBed@main/img/202203122304050.png)
改好之后就可以把8008端口取消放行
# 接下来就是服务端设置了
执行以下代码，选择8,安装监控Agent,按照提示填入信息
```
sudo ./nezha.sh
```
略略略
