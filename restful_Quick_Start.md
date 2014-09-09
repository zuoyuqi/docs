# Yunba RESTful API 快速入门
## 注册开发者账号
打开 <http://yunba.io>, 点击注册创建账号。

![create_accout.jpg](../image/register_account.png)

## 创建应用
注册账号成功跳转到我的应用界面，点击我的应用 --> 创建新应用，输入应用名称和包名

![create_application.jpg](image/create_app.png)

## 方法

### HTTP GET 请求格式如下:

```url
http://rest.yunba.io:8080/<method>/<app-key>/<secret-key>/<topic>/<your_mesage>
```

### HTTP POST 请求 JSON 格式如下:

```json
{'method':<method>, 'appkey':<app-key>, 'seckey':<secret-key>, 'topic':<topic>, 'msg':<message>}
```

在json中可选部分:

```json
"opts":{'time_to_live':<number>,'platform':<number>,'time_delay':<number>,'location':<string>,'qos':<number>,'apn_json':{'alert':<string>,'badge:<number>,'sound':<string>,'priority':<number>,'expiration':<number>','content-available':<number>}}
```

比如:

```
$ curl -l -H "Content-type: application/json" -X POST -d '{"method":"publish", "appkey":"53ea21cd4e9f46851d5a57b5", "seckey":"sec-QMirTLEpuNC6tIUyn40drNfrlWDbgDV64iDnjdni4QFyZaUH", "topic":"rocket", "msg":"just test"}' http://rest.yunba.io:8080
```


其中 app-key, secret-key 从应用详情中页面获得，分别对应于页面中 AppKey， Secret Key。

注意:

* <method\>: 目前只支持"publish", "publish_alias". 如果 <method\> 是"publish_alias", topic用alias填充.

## 发送状态回复

* 发送成功

```json
{'status':0,'msg_id':message-id}
```

* 参数错误

```json
{'status':1}
```

* 内部服务错误

```json
{'status':2}
```

* 没有应用

```json
{'status':3}
```

* 发布超时

```json
{'status':4}
```