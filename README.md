# laravel-wechat

> 注意：此版本为 2.x 版本，不兼容 1.x，已经移除外观，与 [overtrue/wechat 2.0](https://github.com/overtrue/wechat) 同步

微信 SDK for Laravel 5， 基于 [overtrue/wechat](https://github.com/overtrue/wechat)

本项目只适用于，只有一个固定的账号，如果是开发微信公众号管理系统就不要使用了，直接用 [overtrue/wechat](https://github.com/overtrue/wechat) 更方便些。

## 安装

1. 安装包文件
  ```shell
  composer require "overtrue/laravel-wechat:2.0.*"
  ```

2. 添加 `ServiceProvider` 到您项目 `config/app.php` 中的 `providers` 部分:

  ```php
  'Overtrue\LaravelWechat\ServiceProvider',
  ```

3. 请修改 `config/wechat.php` 中对应的项即可。


## 使用

> 注意：

> 1. Laravel 5 默认启用了 CRSF 中间件，因为微信的消息是 POST 过来，所以会触发 CRSF 检查导致无法正确响应消息，所以请去除默认的 CRSF 中间件，改成路由中间件。[默认启用的代码位置](https://github.com/laravel/laravel/blob/master/app/Http/Kernel.php#L18)

所有的Wechat对象都已经放到了容器中，直接从容器中取就好。

别名对应关系如下：

  'wechat.server'    => 'Overtrue\\Wechat\\Server',
  'wechat.user'      => 'Overtrue\\Wechat\\User',
  'wechat.group'     => 'Overtrue\\Wechat\\Group',
  'wechat.auth'      => 'Overtrue\\Wechat\\Auth',
  'wechat.menu'      => 'Overtrue\\Wechat\\Menu',
  'wechat.menu.item' => 'Overtrue\\Wechat\\MenuItem',
  'wechat.js'        => 'Overtrue\\Wechat\\Js',
  'wechat.staff'     => 'Overtrue\\Wechat\\Staff',
  'wechat.store'     => 'Overtrue\\Wechat\\Store',
  'wechat.card'      => 'Overtrue\\Wechat\\Card',
  'wechat.qrcode'    => 'Overtrue\\Wechat\\QRCode',
  'wechat.url'       => 'Overtrue\\Wechat\\Url',
  'wechat.media'     => 'Overtrue\\Wechat\\Media',
  'wechat.image'     => 'Overtrue\\Wechat\\Image',

下面以接收普通消息为例写一个例子：

> 假设您的域名为 `overtrue.me` 那么请登录微信公众平台 “开发者中心” 修改 “URL（服务器配置）” 为： `http://overtrue.me/wechat`。

路由：

```php
Route::any('/wechat', 'WechatController@serve');
```

> 注意：一定是 `Route::any`, 因为微信服务端认证的时候是 `GET`, 接收用户消息时是 `POST` ！

然后创建控制器 `WechatController`：

```php
<?php namespace App\Http\Controllers;

use Overtrue\Wechat\Server;
use Log;

class WechatController extends Controller {

    /**
     * 处理微信的请求消息
     *
     * @param Overtrue\Wechat\Server $server
     *
     * @return string
     */
    public function serve(Server $server)
    {
        $server->on('message', function($message){
            return "欢迎关注 overtrue！";
        });

        return $server->serve(); // 或者 return $server;
    }
}
```

> 注意：不要忘记在头部 `use` 哦，或者你就得用 `\Overtrue\Wechat\Server` 全称咯。:smile:

### 从容器获取对应实例

```php
  $wechatServer = App::make('wechat.server'); // 服务端
  $wechatUser = App::make('wechat.user'); // 用户服务
  // ... 其它同理
```

更多使用请参考：https://github.com/overtrue/wechat/wiki/

## License

MIT
