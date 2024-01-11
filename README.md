# 1. 服务端搭建
##### 1.1 购买位于海外的服务器，系统选择Linux，并登录 
##### 1.2 安装 v2ray server
```shell
# 下载安装脚本
curl -O https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh
# 执行安装脚本
bash install-release.sh
```
##### 1.3 服务端参数配置
```shell
#创建配置文件
vi /usr/local/etc/v2ray/config.json 
```
删除默认的 "`{}`" , 并新增以下内容：
```json
{
  "inbounds": [{
    "port": 12345,
    "protocol": "vmess",
    "settings": {
      "clients": [
        {
          "id": "456e3ac5-75be-42f0-8431-2d2971b13fff",
          "level": 1,
          "security": "aes-128-gcm",
          "alterId": 64
        }
      ]
    }
  }],
  "outbounds": [{
    "protocol": "freedom",
    "settings": {}
  },{
    "protocol": "blackhole",
    "settings": {},
    "tag": "blocked"
  }],
  "routing": {
    "rules": [
      {
        "type": "field",
        "ip": ["geoip:private"],
        "outboundTag": "blocked"
      }
    ]
  }
}
```
配置说明：

|字段|说明|举例|
|--- | --- | --- |
| port| 指定v2Ray 服务的端口，0~65535之间| 12345|
| protocol|通讯协议，建议使用vmess| vmess
| id|uuid, 类似于token,可以在 [https://www.uuidgenerator.net/](https://www.uuidgenerator.net/) 随机生成一个|--|
| security|加密算法，如aes-128-gcm、aes-128-cfb，也可以不指定|aes-128-cfb|
配置好文件后使用 `wq` 指令保存并退出 vi 编辑器。
##### 1.4 启动v2ray服务端
```shell
 systemctl enable v2ray && systemctl start v2ray
```
##### 1.5 (可选) 查看v2ray 是否已经成功启动
```shell
ps -ef | grep v2ray
```
# 2. 客户端配置
#####  2.1 下载客户端
|系统|名称|类型|下载地址|
|---|---|---|---|
|macOs|v2rayU|图形界面程序|https://github.com/yanue/V2rayU/releases|
|windows|v2rayN|图形界面程序|https://github.com/2dust/v2rayN/releases|
|all|v2ray-core|命令行|https://github.com/v2fly/v2ray-core/releases|

因为v2ray是开源的，因此也有着比较多的第三方客户端，可在此浏览：
https://www.v2fly.org/awesome/tools.html#%E5%9C%A8%E7%BA%BF%E5%B7%A5%E5%85%B7
![第三方客户端](https://upload-images.jianshu.io/upload_images/16901119-526390213db556c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/800)
#####  2.2 下载并安装客户端并启动，在状态栏找到 v2ray 的图标并点击，开始客户端配置(以macOs下v2rayU 为例)，如下：
![开始配置](https://upload-images.jianshu.io/upload_images/16901119-419fd61e6c363b8d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/400)
#####  2.3 下载并安装客户端并启动，在状态栏找到 v2ray 的图标并点击，开始客户端配置(以macOs下v2rayU 为例)，如下：
![配置项](https://upload-images.jianshu.io/upload_images/16901119-0023c5d138a5653c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/800)
> 其中服务端ip、端口、uuid，请确保和 `1.3 服务端参数配置` 中的一致
#####  2.4 本地代理端口配置
![本地代理端口配置](https://upload-images.jianshu.io/upload_images/16901119-b80fb38e2958f312.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/800)
全部配置完成之后点击‘保存’保存配置信息。
#####  2.5 选择服务器并开启代理
![选择服务器](https://upload-images.jianshu.io/upload_images/16901119-02ab76cbc29fc90f.png?imageMogr2/auto-orient/strip|imageView2/2/w/800/format/webp)
![开启代理](https://upload-images.jianshu.io/upload_images/16901119-4abffdc9f5fbac87.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/800)
如果需要在IDEA中使用，请配置IDEA的代理，如下：
![IDEA代理配置](https://upload-images.jianshu.io/upload_images/16901119-a7a2b192f1f018b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/800)
如需在命令行中使用，请使用如下命令(也可以在 v2ray -> 复制终端代理之类获取命令)：
```shell
export http_proxy=http://127.0.0.1:1098;export https_proxy=http://127.0.0.1:1098;export ALL_PROXY=socks5://127.0.0.1:1088
```







