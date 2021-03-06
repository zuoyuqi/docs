# Yunba iOS SDK API 手册 

## setup

### 功能
初始化 YunBa SDK。

### 函数原型

`+ (BOOL)setupWithAppkey:(NSString *)appkey;`

`+ (BOOL)setupWithAppkey:(NSString *)appkey option:(YBSetupOption *)option;`

### 参数说明
名称 | 类型 | 说明
--------- | ------- | -----------
appkey | NSString* | YunBa 中注册的App ID
option | YBSetupOption* | 选项，可包含sub_key(用于获取订阅权限的密钥), pub_key(用于获取发布权限的密钥),            sec_key(用于获取管理权限的密钥，切勿外泄), auth_key(用于access manager模块中权限管理的动态密钥)

### 返回值
* (BOOL) : setup的结果，YES说明setup成功，开始尝试连接，否则说明setup失败，参数错误。

```objective_c

    BOOL succ = [YunBaService setupWithAppkey:appkey option:option];
	if (succ) {
            NSLog(@"setup succ %@:%@", appkey, option);
        } else {
            NSLog(@"setup failed %@:%@", appkey, option);
        }
    }];

```

## subscribe

### 功能
App 可以增加订阅一个Topic, 以便可以接收来自 Topic 的 Message。

### 函数原型

     `+ (void)subscribe:(NSString *)topic resultBlock:(YBResultBlock)resultBlock;`

     `+ (void)subscribe:(NSString *)topic qos:(UInt8)qosLevel resultBlock:(YBResultBlock)resultBlock;`

### 参数说明
名称 | 类型 | 说明
--------- | ------- | ----
topic | NSString* | app 订阅的的主题，topic 只支持英文数字下划线，长度不超过50个字符
qosLevel | NSString* | 服务质量等级，具体参考服务质量等级说明
resultBlock | YBResultBlock | API 回调接口，可通过返回的BOOL succ判断结果的成功与否, NSError *error获取错误原因

### 返回值
None

```objective_c

    [YunBaService subscribe:topic qos:qosLevel resultBlock:^(BOOL succ, NSError *error) {
        if (succ) {
            NSLog(@"subscribe to topic succ: %@", topic);
        } else {
            NSLog(@"subscribe to topic failed due to : %@, recovery suggestion: %@", error, [error localizedRecoverySuggestion]);
        }
    }];

```

## unsubscribe

### 功能
App 可以取消订阅一个 Topic, 以便取消接收来自 Topic 的 Message.

### 函数原型


     `+ (void)unsubscribe:(NSString *)topic resultBlock:(YBResultBlock)resultBlock;`


### 参数说明
名称 | 类型 | 说明
--------- | ------- | ----
topic | NSString* | app 取消订阅的的主题，topic 只支持英文数字下划线，长度不超过50个字符
resultBlock | YBResultBlock | API 回调接口，可通过返回的BOOL succ判断结果的成功与否, NSError *error获取错误原因

### 返回值
None

```objective_c

    [YunBaService unsubscribe:topic resultBlock:^(BOOL succ, NSError *error) {
        if (succ) {
            NSLog(@"unsubscribe to topic succ: %@", topic);
        } else {
            NSLog(@"unsubscribe to topic failed due to: %@, recovery suggestion: %@", error, [error localizedRecoverySuggestion]);
        }
    }];

```


## publish

### 功能
App 可以向 Topic 发送消息, 那么任何订阅此 Topic 的 Client 都会接受到消息。

### 函数原型

     `+ (void)publish:(NSString *)topic data:(NSData *)data resultBlock:(YBResultBlock)resultBlock;`

     `+ (void)publish:(NSString *)topic data:(NSData *)data option:(YBPublishOption *)option resultBlock:(YBResultBlock)resultBlock;`

### 参数说明
名称 | 类型 | 说明
--------- | ------- | ----
topic | NSString* | app 发布消息的主题，只支持英文数字下划线，长度不超过50个字符
data | NSData* | 向对应 topic 的订阅者发布的消息
option | YBPublishOption* | 选项，可包含qos, retained等属性
resultBlock | YBResultBlock | API 回调接口，可通过返回的BOOL succ判断结果的成功与否, NSError *error获取错误原因

### 返回值
None

```objective_c

    [YunBaService publish:topic data:data option:option resultBlock:^(BOOL succ, NSError *error) {
        if (succ) {
            NSLog(@"publish to topic: %@ data: %@ succ", topic, data);
        } else {
            NSLog(@"publish to topic failed due to: %@, recovery suggestion: %@", error, [error localizedRecoverySuggestion]);
        }
    }];

```


## publish2

### 功能
App 可以向 Topic 发送消息, 那么任何订阅此 Topic 的 Client 都会接受到消息，此API可以带有其他参数，如APN选项等。

### 函数原型

     `+ (void)publish2:(NSString *)topic data:(NSData *)data resultBlock:(YBResultBlock)resultBlock;`

     `+ (void)publish2:(NSString *)topic data:(NSData *)data option:(YBPublish2Option *)option resultBlock:(YBResultBlock)resultBlock;`

### 参数说明
名称 | 类型 | 说明
--------- | ------- | ----
topic | NSString* | app 发布消息的主题，只支持英文数字下划线，长度不超过50个字符
data | NSData* | 向对应 topic 的订阅者发布的消息
option | YBPublish2Option* | 选项，可包含YBApnOption等属性
resultBlock | YBResultBlock | API 回调接口，可通过返回的BOOL succ判断结果的成功与否, NSError *error获取错误原因

### 返回值
None

```objective_c

    [YunBaService publish2:topic data:data option:option resultBlock:^(BOOL succ, NSError *error) {
        if (succ) {
            NSLog(@"publish2 to topic: %@ data: %@ succ", topic, data);
        } else {
            NSLog(@"publish2 to topic failed due to: %@, recovery suggestion: %@", error, [error localizedRecoverySuggestion]);
        }
    }];

```


## publishToAlias

### 功能
App 可以向 alias 发送消息, 那么此别名的 Client 会接受到消息。

### 函数原型

     `+ (void)publishToAlias:(NSString *)alias data:(NSData *)data resultBlock:(YBResultBlock)resultBlock;`

     `+ (void)publishToAlias:(NSString *)alias data:(NSData *)data option:(YBPublishOption *)option resultBlock:(YBResultBlock)resultBlock;`

### 参数说明
名称 | 类型 | 说明
--------- | ------- | ----
alias | NSString* | 目标用户的别名，只支持英文数字下划线，长度不超过50个字符
data | NSData* | 向对应 topic 的订阅者发布的消息
option | YBPublishOption* | 选项，可包含qos, retained等属性
resultBlock | YBResultBlock | API 回调接口，可通过返回的BOOL succ判断结果的成功与否, NSError *error获取错误原因

### 返回值
None

```objective_c

    [YunBaService publishToAlias:alias data:data option:option resultBlock:^(BOOL succ, NSError *error) {
        if (succ) {
            NSLog(@"publish to alias: %@ data: %@ succ", alias, data);
        } else {
            NSLog(@"publish to alias failed due to: %@, recovery suggestion: %@", error, [error localizedRecoverySuggestion]);
        }
    }];

```

## publish2ToAlias

### 功能
App 可以向 alias 发送消息, 那么此别名的 Client 会接受到消息。

### 函数原型

     `+ (void)publish2ToAlias:(NSString *)alias data:(NSData *)data option:(YBPublish2Option *)option resultBlock:(YBResultBlock)resultBlock;`

### 参数说明
名称 | 类型 | 说明
--------- | ------- | ----
alias | NSString* | 目标用户的别名，只支持英文数字下划线，长度不超过50个字符
data | NSData* | 向对应 topic 的订阅者发布的消息
option | YBPublish2Option* | 选项，可包含YBApnOption等属性
resultBlock | YBResultBlock | API 回调接口，可通过返回的BOOL succ判断结果的成功与否, NSError *error获取错误原因

### 返回值
None

```objective_c

    [YunBaService publish2ToAlias:alias data:data option:option resultBlock:^(BOOL succ, NSError *error) {
        if (succ) {
            NSLog(@"publish2 to alias: %@ data: %@ succ", alias, data);
        } else {
            NSLog(@"publish2 to alias failed due to: %@, recovery suggestion: %@", error, [error localizedRecoverySuggestion]);
        }
    }];

```


## subscribePresence

### 功能
App 可以订阅某个频道上的其他用户的上、下线及(取消)订阅频道的事件通知。

### 函数原型

     `+ (void)subscribePresence:(NSString *)topic resultBlock:(YBResultBlock)resultBlock;`

### 参数说明
名称 | 类型 | 说明
--------- | ------- | ----
topic | NSString* | app 订阅的的目标用户所在频道主题，topic 只支持英文数字下划线，长度不超过50个字符
resultBlock | YBResultBlock | API 回调接口，可通过返回的BOOL succ判断结果的成功与否, NSError *error获取错误原因

### 返回值
None

```objective_c

    [YunBaService subscribePresence:topic resultBlock:^(BOOL succ, NSError *error) {
        if (succ) {
            NSLog(@"subscribe presence to topic:%@ succ", topic);
        } else {
            NSLog(@"subscribe presence failed due to : %@, recovery suggestion: %@", error, [error localizedRecoverySuggestion]);
        }
    }];

```


## unsubscribePresence

### 功能
App 可以取消订阅某个频道上的其他用户的上、下线及(取消)订阅频道的事件通知。

### 函数原型

     `+ (void)unsubscribePresence:(NSString *)topic resultBlock:(YBResultBlock)resultBlock;`

### 参数说明
名称 | 类型 | 说明
--------- | ------- | ----
topic | NSString* | app 订阅的的目标用户所在频道主题，topic 只支持英文数字下划线，长度不超过50个字符
resultBlock | YBResultBlock | API 回调接口，可通过返回的BOOL succ判断结果的成功与否, NSError *error获取错误原因

### 返回值
None

```objective_c

    [YunBaService unsubscribePresenceToTopic:topic resultBlock:^(BOOL succ, NSError *error) {
        if (succ) {
            NSLog(@"unsubscribe presence to topic:%@ succ", topic);
        } else {
            NSLog(@"unsubscribe presence failed due to : %@, recovery suggestion: %@", error, [error localizedRecoverySuggestion]);
        }
    }];

```


## getAliasList

### 功能
App 可以查询订阅某个频道的所有用户别名个数、列表及状态。

### 函数原型

     `+ (void)getAliasList:(NSString *)topic resultBlock:(YBArrayCountResultBlock)arrayCountResultBlock;`

     `+ (void)getAliasList:(NSString *)topic disableState:(BOOL)disableState disableAlias:(BOOL)disableAlias resultBlock:(YBArrayCountResultBlock)arrayCountResultBlock;`

### 参数说明
名称 | 类型 | 说明
--------- | ------- | ----
topic | NSString* | 目标频道
disableState | BOOL | 结果是否排除别名状态信息
disableAlias | BOOL | 结果是否排除别名列表。
arrayCountResultBlock | YBArrayCountResultBlock | API 回调接口，可通过返回的error.code判断结果的成功与否，NSError *error获取错误原因，NSArray *resArray获取别名及状态列表，size_t resCount获取别名数量。

### 返回值
None

```objective_c

    [YunBaService getAliasList:testTopic disableState:NO disableAlias:NO resultBlock:^(NSArray *resArray, size_t resCount, NSError *error) {
        if (error.code == kYBErrorNoError) {
                NSLog(@"get alias list[%@] of topic[%@] succ", res, testTopic);
            } else {
                NSLog(@"get alias list failed due to: %@, recovery suggestion: %@", error, [error localizedRecoverySuggestion]);
            }
    }];

```


## getTopicList

### 功能
App 可以查询用户订阅的频道列表。

### 函数原型

     `+ (void)getTopicList:(YBArrayResultBlock)stringResultBlock;`

     `+ (void)getTopicList:(NSString *)alias resultBlock:(YBArrayResultBlock)arrayResultBlock;`

### 参数说明
名称 | 类型 | 说明
--------- | ------- | ----
alias | NSString* | 目标用户别名
arrayResultBlock | YBArrayResultBlock | API 回调接口，可通过返回的error.code判断结果的成功与否，NSError *error获取错误原因，NSArray *res获取频道列表

### 返回值
None

```objective_c

    [YunBaService getTopicList:alias resultBlock:^(NSArray *res, NSError *error) {
            if (error.code == kYBErrorNoError) {
                NSLog(@"get topic list[%@] of alias[%@] succ", res, alias);
            } else {
                NSLog(@"get topic list failed due to: %@, recovery suggestion: %@", error, [error localizedRecoverySuggestion]);
            }
    }];

```

## report

### 功能
App  可以调用此函数来上报客户端的行为，如打开通知栏次数，按钮点击次数，资源下载成功等等行为。

### 函数原型

     `+ (void)report:(NSString *)action withData:(NSData *)data;`

### 参数说明
名称 | 类型 | 说明
--------- | ------- | ----
action | NSString* | app 需要统计的行为，如打开通知栏，下载资源成功等等
data | NSData * | 想对应 action 的附加数据，以满足统计相关的其他业务需求

### 返回值
None

```objective_c

    [YunBaService report:action withData:data];

```


## getState

### 功能
App 可以查询用户的在线状态。

### 函数原型

     `+ (void)getState:(NSString *)alias resultBlock:(YBStringResultBlock)stringResultBlock;`

### 参数说明
名称 | 类型 | 说明
--------- | ------- | ----
alias | NSString* | 目标用户别名
stringResultBlock | YBStringResultBlock | API 回调接口，可通过返回的error.code判断结果的成功与否, NSError *error获取错误原因

### 返回值
None

```objective_c

    [YunBaService getState:alias resultBlock:^(NSString *res, NSError *error) {
        if (error.code == kYBErrorNoError) {
            NSLog(@"get state[%@] of alias[%@] succ", res, alias);
        } else {
            NSLog(@"get state of alias failed due to: %@, recovery suggestion: %@", error, [error localizedRecoverySuggestion]);
        }
	}];

```


## setAlias

### 功能
App 可以调用此函数来绑定账号，用户名，每个用户只能指定一个别名。

### 函数原型

     `+ (void)setAlias:(NSString *)alias resultBlock:(YBResultBlock)resultBlock;`

### 参数说明
名称 | 类型 | 说明
--------- | ------- | ----
alias | NSString* | 用户设置的别名信息，只支持英文数字下划线，长度不超过50个字符
resultBlock | YBResultBlock | API 回调接口，可通过返回的BOOL succ判断结果的成功与否, NSError *error获取错误原因

### 返回值
None

```objective_c

    [YunBaService setAlias:alias resultBlock:^(BOOL succ, NSError *error) {
        if (succ) {
            NSLog(@"set alias[%@] succ", alias);
        } else {
            NSLog(@"set alias failed due to : %@, recovery suggestion: %@", error, [error localizedRecoverySuggestion]);
        }
    }];

```


## getAlias

### 功能
App 可以调用此函数来获取当前用户的别名。

### 函数原型

     `+ (void)getAlias:(YBStringResultBlock)stringResultBlock;`

### 参数说明
名称 | 类型 | 说明
--------- | ------- | ----
stringResultBlock | YBStringResultBlock | API 回调接口，可通过返回的 NSError *error获取错误代码(kYBErrorNoError表示成功)及原因

### 返回值
None

```objective_c

    [YunBaService getAlias:^(NSString *res, NSError *error) {
        if (error.code == kYBErrorNoError) {
            NSLog(@"get alias succ:%@", res);
        } else {
            NSLog(@"get alias failed due to : %@, recovery suggestion: %@", error, [error localizedRecoverySuggestion]);
        }
    }];

```


## storeDeviceToken

### 功能
App 将DeviceToken 存储在YunBa的云端，那么可以通过YunBa发送APNs通知。

### 函数原型

     `+ (void)storeDeviceToken:(NSData *)token resultBlock:(YBResultBlock)resultBlock;`

### 参数说明
名称 | 类型 | 说明
--------- | ------- | ----
token | NSData* | 通过注册APNs而获取到的对应设备的device token
resultBlock | YBResultBlock | API 回调接口，可通过返回的BOOL succ判断结果的成功与否, NSError *error获取错误原因

### 返回值
None

> 注册APNs，申请获取device token:

```objective_c
    [[UIApplication sharedApplication] registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
```

> 上传device token:

```objective_c
    - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
        NSLog(@"get Device Token: %@", [NSString stringWithFormat:@"Device Token: %@", deviceToken]);
        [YunBaService storeDeviceToken:deviceToken resultBlock:^(BOOL succ, NSError *error) {
            if (succ) {
                NSLog(@"store device token to YunBa succ");
            } else {
                NSLog(@"store device token to YunBa failed due to : %@", error);
            }
        }];
    }
```
