FecMall Fecyo 中文单商户B2C商城安装和配置
===============

> FecMall Fecyo 中文单商户B2C商城, 进行安装和配置





准备工作READY
--------

1.安装fecmall开源商城，安装地址参看：[Fecmall开源商城安装](http://www.fecmall.com/doc/fecshop-guide/develop/cn-2.0/guide-fecshop-2-graphical-install.html)

2.应用市场注册账户，加入购物车，免费下单，应用市场地址：http://addons.fecmall.com/27944278

*注意*: 因为是在线下载安装，Fecyo的压缩包大致未4MB，php默认的执行时间为30秒，如果在此时间内下载超时，将会安装报错，
因此您需要设置php的最大超时时间，详细参看帖子：http://www.fecmall.com/topic/2474



应用市场在线安装Fecyo
-----------

1.下单完成后，fecmall后台在线安装fecyo应用系统，关于应用市场的使用，参看：[Fecmall应用市场-安装应用扩展文档](http://www.fecmall.com/doc/fecshop-guide/addons/cn-2.0/guide-fecmall-addons-install.html)

2.安装完成后，就进入配置阶段


Fecyo配置
-----------

1.Store设置（配置第三方模板路径）

1.1网站配置 --> Appfront配置  -->  Store配置

编辑，弹出框，第三方模板路径： `@fecyo/app/appfront/theme/fecyo`

1.2网站配置 --> Apphtml5配置  -->  Store配置

编辑，弹出框，第三方模板路径： `@fecyo/app/apphtml5/theme/fecyo`


2.首页Banner以及底部静态块配置

详细参看：[FecMall Fecyo 首页Banner以及静态块配置](fecmall-fecyo-banner-config.md)

3.手机帐号登陆，阿里短信服务，微信PC扫码关注公众号登陆，微信H5静默登陆，微信分享等

需要对阿里大鱼短信（阿里大鱼改名阿里云短信服务），微信等进行配置

详细的配置文档参看：[Fecmall Fecyo 微信登陆,微信分享,手机帐号](fecmall-fecyo-phone-weixin-account.md)









