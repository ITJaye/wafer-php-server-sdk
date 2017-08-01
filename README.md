# Wafer 服务端 SDK - PHP  支持微信小程序登陆及授权获取用户信息的新要求



## 使用

### 加载 SDK

```php
// 方法：
use \QCloud_WeApp_SDK\Auth\LoginService as LoginService;

### 使用会话服务

#### 处理用户登录请求

业务服务器提供一个路由（如 `/login`）处理客户端的登录请求，登录成功后，可以获取用户信息。

```php
use \QCloud_WeApp_SDK\Auth\LoginService;

$result = LoginService::login();

if ($result['code'] === 0) {
    // 微信用户信息：`$result['data']['userInfo']`
} else {
    // 登录失败原因：`$result['message']`
}
```

#### 解密用户信息获取nickName和avatarUrl等信息并更新数据库

业务服务器提供一个路由（如 `/decrypt`）处理客户端的登录请求，解密成功后，可以获取用户解密信息,这里可以更新数据库操作。

```php
use \QCloud_WeApp_SDK\Auth\LoginService;

$result = LoginService::decryptUserInfo();
// check failed
if ($result['code'] !== 0) {
    return;
}
 ...更新用户表


```


#### 检查请求登录态 判断登陆是否过期

客户端交给业务服务器的请求，业务服务器可以通过 SDK 的LoginService::check() 方法来检查该请求是否包含合法的会话。如果包含，则会返回会话对应的用户信息。

```php
use \QCloud_WeApp_SDK\Auth\LoginService;

$result = LoginService::check();

if ($result['code'] !== 0) {
    // 登录态失败原因：`$result['message']`
    return;
}
$userInfo = $result['data']['userInfo'];


// 使用微信用户信息（`$result['data']['userInfo']`）处理其它业务逻辑
// ...
```

原项目参考 [Wafer 服务端 SDK - PHP](https://github.com/tencentyun/wafer-php-server-sdk)
新登陆授权要求 [小程序授权登陆新要求](https://developers.weixin.qq.com/blogdetail?action=get_post_info&lang=zh_CN&token=&docid=c45683ebfa39ce8fe71def0631fad26b)
原始项目 [Wafer](https://github.com/tencentyun/wafer)