WIO的SSO机制
=============
SSO英文全称Single Sign On，单点登录。SSO是在多个应用系统中，用户只需要登录一次就可以访问所有相互信任的应用系统。它包括可以将这次主要的登录映射到其他应用中用于同一个用户的登录的机制。它是目前比较流行的企业业务整合的解决方案之一。

设想一下，你想要开发一款IOT应用，它有自己的产品描述，产品功能定义，有一套完整API。你当然希望用户登陆你的应用后，用户就自动登陆了WIO系统。如果不这样做，用户就需要再次登陆WIO系统，不易于使用。
<!-- ![SSO框图]() -->

建立SSO账户系统
--------------
市场上有一些服务可以帮助我们来管理用户，包括SSO支持。比如Firebase和Stormpath，这里用Firebase作为例子，教你快速实现WIO的SSO登陆。
- 登陆https://console.firebase.google.com/ 创建一个项目
- 记下找到项目ID
- 在Authentication->登陆方法，启用“电子邮件地址／密码”
- 在WIO项目目录找到./server/handler/logic/sso.py文件，修改FirebaseSso的类变量PROJECT_ID为项目ID
- 下载https://github.com/firebase/quickstart-js/blob/master/auth/email.html，并在TODO部分按提示增加app配置信息，用浏览器打开。
- 用邮件和密码注册一个用户，页面会返回数据，找到access_token
- 测试SSO自动登陆，调用curl "http://wio_domain/user?access_token=your_acccess_token",
- WIO会自动校验token，如果正确保存用户ID，TOKEN和TOKEN的过期时间，在过期时间内，WIO不会再次校验。

把WIO系统集成到你的用户系统中
-----------------
如果你已经有了自己的用户系统，并希望给WIO做SSO支持。修改以下代码
- 在WIO项目目录找到./server/handler/logic/sso.py文件，创建一个类继承SSO类，并实现auth_token方法。
- auth_token可以在线方式验证token或者支持JWT token的本地验证
- 最后返回user_id, token, expire即可。
