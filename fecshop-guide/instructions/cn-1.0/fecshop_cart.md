Fecshop 购物车
=============

> 在产品页面中通过按钮 加入购物车可以将产品加入购物车中，fecshop的购物车数据
> 是存储到数据库中的，session保存cart表的id，因此，当用户登录账号后，将产品加入购物车，换了其他的浏览器，
> 然后重新登录 
> 账号后，购物车信息还是存在的。


Fecshop-2.x更新说明
-------------

2版本更新为后台配置


![xx](images/as10.png)

Fecshop-1.x
-------------


购物车的配置信息：

@fecshop/config/services/Cart.php

```
<?php
return [
	'cart' => [
		'class' => 'fecshop\services\Cart',
		
		# 子服务
		'childService' => [
			'quote' => [
				'class' => 'fecshop\services\cart\Quote',
			],
			'quoteItem' => [
				'class' => 'fecshop\services\cart\QuoteItem',
			],
			
			'info' => [
				'class' => 'fecshop\services\cart\Info',
				/**
				 * 单个sku加入购物车的最大个数。
				 */
				'maxCountAddToCart' => 100,
				# 上架状态产品加入购物车时，
				# 如果addToCartCheckSkuQty设置为true，则需要检查产品qty是否>购买qty，
				# 如果设置为false，则不需要，也就是说产品库存qty小于购买qty，也是可以加入购物车的。
				'addToCartCheckSkuQty' => false,
			],
			'coupon' => [
				'class' => 'fecshop\services\cart\Coupon',
			],
		],
	],
];

```

单个sku加入购物车的最大个数：通过配置`maxCountAddToCart`

加入购物车是否检查库存：通过配置`addToCartCheckSkuQty`,一般是设置成false，
一般是在生成订单的时候进行检查，因为有一些人的订单可能未付款，超过一段时间
未付款的订单，会由后台定时脚本释放库存，因此，加入购物车的时候没有库存，可能待会就有了。

购物车数据是放到mysql的，因为涉及到多表事务。


### 购物车点击 Proceed to Pay 的设置

游客和登录用户都可以把产品加入购物车，但是，如果用户想进一步下单，我想强制
让用户先注册账户才能下单，也就是说，支持游客加入购物车，但是不允许游客下单，您可以
在文件：
`@fecshop/app/appfront(apphtml5 | appserver)/config/modules/Checkout.php`


```
return [
    /**
     * checkout 模块的配置，您可以在@apphtml5/config/fecshop_local_modules/Checkout.php 
     * 中进行配置，二开，或者重写该模块（在上面路径中如果文件不存在，自行新建配置文件。）
     */
    'checkout' => [
        'class' => '\fecshop\app\apphtml5\modules\Checkout\Module',
        /**
         * 模块内部的params配置。
         */
        'params'=> [
            'guestOrder' => true, // 是否支持游客下单
        ],
    ],
];
```

将`guestOrder` 设置成 `false`后，在购物车点击`proceed to Pay` ,就会跳转到账户登录页面

这里的设置，在入口 `appfront` , `apphtml5`, `appserver` 都有效


### 可勾选产品的购物车

在购物车页面，如果某些产品想放到购物车中，
但是用户想对部分产品下单，可以通过勾选的
方式勾选想下单的产品（类似淘宝的方式），然后点击下单按钮即可。

产品加入购物车，默认是勾选状态

下单成功后，购物车中只清除勾选下单的产品，未勾选下单的产品在购物车中依旧保留。


![fecshop  cart](images/777.png)



















