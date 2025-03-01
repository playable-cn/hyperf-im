<h1 align="center"> 腾讯云 IM 服务端 SDK for Hyperf </h1>

<p align="center">腾讯云 IM.</p>

> 普通 PHP 请使用 [hedeqiang/ten-im](https://github.com/hedeqiang/IM) 扩展
## 前提
使用本扩展前需要登录 [即时通信 IM 控制台](https://console.cloud.tencent.com/avc) 创建应用，配置管理员、获取 app_id、Key 等关键信息

更多请查看并熟读 [即时通信 IM 服务端API](https://cloud.tencent.com/document/product/269/32688)

[REST API 接口列表](https://cloud.tencent.com/document/product/269/1520)
## 安装

```shell
$ composer require playable-cn/im -vvv
```

## 发布配置
```php
php bin/hyperf.php vendor:publish playable-cn/im
```

编辑 `.env` 文件
```php
SDK_APP_ID=
IDENTIFIER=
SECRET_KEY=
REGION=
// REGION 默认zh，还可以配置：sgp、kr、ger、ind、usa
```

## 使用
### 获取用户在线状态
```php
<?php

declare(strict_types=1);

namespace App\Controller;

use Hedeqiang\IM\Exceptions\Exception;
use Hedeqiang\IM\Exceptions\HttpException;
use Hedeqiang\IM\IM;
use Hyperf\Config\Annotation\Value;

class IndexController extends AbstractController
{
    /**
     * @Value(key="im")
     */
    protected $config;

    public function index()
    {
        $config = $this->config;
        $im = new IM($config);
        $params = [
            'To_Account' => ['hedeqiang']
        ];
        
        print_r($im->send('openim','querystate',$params));
    }
}

```
返回示例
```php
{
	"ActionStatus": "OK",
	"ErrorInfo": "",
	"ErrorCode": 0,
	"QueryResult": [{
		"To_Account": "hedeqiang",
		"State": "Offline"
	}]
}
```
### 设置资料
```php
$params = [
    'From_Account' => 'hedeqiang',
        'ProfileItem' => [
            ['Tag' => 'Tag_Profile_IM_Nick', 'Value' => 'hedeqiang'],
            ['Tag' => 'Tag_Profile_IM_Gender', 'Value' => 'Gender_Type_Male'],
            ['Tag' => 'Tag_Profile_IM_BirthDay', 'Value' => 19940410],
            ['Tag' => 'Tag_Profile_IM_SelfSignature', 'Value' => '程序人生的寂静欢喜'],
            ['Tag' => 'Tag_Profile_IM_Image', 'Value' => 'https://upyun.laravelcode.cn/upload/avatar/1524205770e4fbfbff-86ae-3bf9-b7b8-e0e70ce14553.png'],
        ],
];

print_r($im->send('profile','portrait_set',$params));
```

返回示例：
```php
{
	"ActionStatus": "OK",
	"ErrorCode": 0,
	"ErrorInfo": "",
	"ErrorDisplay": ""
}
```

### 单发单聊消息
```php
$params = [
    'SyncOtherMachine' => 1, // 消息不同步至发送方
    'From_Account' => 'hedeqiang',
    'To_Account' => 'zhangsan',
    'MsgRandom' => 1287657,
    'MsgTimeStamp' => 1557387418,
    'MsgBody' => [
        [
            'MsgType' => 'TIMTextElem',
            'MsgContent' => [
                'Text' => '晚上去撸串啊'
            ]
        ]
    ]
];

print_r($im->send('openim','sendmsg',$params));
```

返回示例：
```php
{
    "ActionStatus":"OK",
    "ErrorInfo":"",
    "ErrorCode":0,
    "MsgTime":1573179125,
    "MsgKey":"748144182_1287657_1573179125"
}
```

> 其中 `send` 方法接收三个参数。第一个参数 $servicename : 内部服务名，不同的 servicename 对应不同的服务类型；第二个参数 `$command`：命令字，与 servicename 组合用来标识具体的业务功能；第三个参数为请求包主体 

> 示例：`v4/im_open_login_svc/account_import`，其中 `im_open_login_svc` 为 `servicename`； `account_import` 为 `command`

请求包示例：
```php
{
    "From_Account":"id",
    "ProfileItem":
    [
        {
            "Tag":"Tag_Profile_IM_Nick",
            "Value":"MyNickName"
        }
    ]
}
```

更多用法请参考 [REST API 接口列表](https://cloud.tencent.com/document/product/269/1520)

TODO

## Contributing

You can contribute in one of three ways:

1. File bug reports using the [issue tracker](https://github.com/hedeqiang/hyperf-im/issues).
2. Answer questions or fix bugs on the [issue tracker](https://github.com/hedeqiang/hyperf-im/issues).
3. Contribute new features or update the wiki.

_The code contribution process is not very formal. You just need to make sure that you follow the PSR-0, PSR-1, and PSR-2 coding guidelines. Any new code contributions must be accompanied by unit tests where applicable._

## License

MIT
