FecShop 缓存
===========

> fecshop 有整页缓存和局部缓存

Fecshop-2.x更新说明
-------------

2版本更新后台配置


![xx](images/af3.png)

![xx](images/af4.png)

Fecshop-1.x
-------------


### 缓存刷新

进入appadmin后台,刷新缓存，具体如图：

![appadmin](images/b111.png)

因为各个入口的redisCache都是独立配置的database，因此，后台刷新缓存，需要加载redis cache的配置，
因此，需要在配置中指定具体的配置文件

打开: @appadmin/config/fecshop_local_services/Fecadmin.php 可以看到配置：

```
'fecadmin' => [
        'params' => [
            /**
             * Fecshop缓存是基于redis，下面是各个入口redis配置所在的文件路径
             * 1.`commonConfig`是公用部分
             * 2.app开头的key，指的是各个入口的redis所在的配置文件
             * 这个配置的作用，是为了在后台清空各个入口的全部缓存，因此需要加载相应的redis的配置
             */
            'cacheRedisConfigFile' => [
                'commonConfig'      => '@common/config/main-local.php',
                'appAdmin'           => '@appadmin/config/main-local.php',
                'appApi'            => '@appapi/config/main-local.php',
                'appFront'          => '@appfront/config/main-local.php',
                'appHtml5'          => '@apphtml5/config/main-local.php',
                'appServer'         => '@appserver/config/main-local.php',
                
            ],
        ],
    ],

```


`cacheRedisConfigFile`: 数组的作用是，指定各个入口的redisCache配置所在文件，其中，
`commonConfig` 是公用redisCache配置。

关于这个功能的具体实现，您可以参看文件：`@fecshop\app\appadmin\modules\Fecadmin\block\cache\Index.php`

### Appfront  AppHtml5 入口使用缓存

实现原理为yii2的full page cache



配置：

@appfront/config/fecshop_local_services/Cache.php

具体如下：

```
return [
	'cache' => [
		/**
		 * cache 总开关，设置false后，无论cacheConfig中的enable
		 * 是否为true，都会全部关闭掉cache。
		 */
		'enable'=> true, 	
		/**
		 * 各个页面cache的配置
		 */
		'cacheConfig'	=> [
			# 分类页面
			'category'  => [
				'enable' 		=> true, 	# 是否开启分类页面的缓存
				'timeout'		=> 3600, 	# 则设置缓存的过期时间，这里设置为秒
				'disableUrlParam' => 'fecshop', # 如果开启缓存，在url加入什么参数后，系统不读取缓存，这个选项是为了方便在不刷新缓存的情况下，查看无缓存的页面是什么样子。
				# url出现的这些参数的值，将参与cache唯一key的生成。
				'cacheUrlParam'	=> [
					# 分页，排序，等参数 
					'p','dir','sort','numPerPage',
					# 侧栏属性过滤等参数
					'price','size','color',
					'style','dresses-length','pattern-type',
					'collar','xinghao','cpu'
				],
			],
			# 产品页面
			'product'  => [
				'enable' 		=> true, 	# 是否开启产品页面的缓存
				'timeout'		=> 3600, 	#则设置缓存的过期时间，这里设置为秒
				'disableUrlParam' => 'fecshop', # 如果开启缓存，在url加入什么参数后，系统不读取缓存，这个选项是为了方便在不刷新缓存的情况下，查看无缓存的页面是什么样子。
			],
			
			# 首页页面
			'home'  => [
				'enable' 		=> true, 	# 是否开启首页页面的缓存
				'timeout'		=> 3600, 	# 则设置缓存的过期时间，这里设置为秒
				'disableUrlParam' => 'fecshop', # 如果开启缓存，在url加入什么参数后，系统不读取缓存，这个选项是为了方便在不刷新缓存的情况下，查看无缓存的页面是什么样子。
			],
			
			# Article（page）页面
			'article'  => [
				'enable' 		=> true, 	# 是否开启Article页面的缓存
				'timeout'		=> 3600, 	# 则设置缓存的过期时间，这里设置为秒
				'disableUrlParam' => 'fecshop',# 如果开启缓存，在url加入什么参数后，系统不读取缓存，这个选项是为了方便在不刷新缓存的情况下，查看无缓存的页面是什么样子。
			],
		],
	],
];
```

详细看里面的注释代表的具体含义。

开启缓存后，页面的数据将从cache中读取，页面的动态数据部分，会
通过ajax的方式把页面的动态数据加载过来。
通过整页缓存，页面的加载就会非常的快。


### Appfront  AppHtml5 入口使用局部缓存

局部缓存，相当于一个区块，
譬如fecshop pc端web的menu部分，头部header部分，尾部footer部分等，都可以
通过局部缓存的方式，将其缓存起来，
在购物车，下单，账户中心等内容全部是动态内容，不能做全页缓存的页面，
可以通过局部缓存的方式缓存某些非动态内容的区块，
fecshop是通过 page - widget 服务实现的。

首先参看配置文件：

@appfront/config/fecshop_local_services/Page.php , 代码如下：

```
'widget' => [
	'widgetConfig' => [
		'head' => [
			# 动态数据提供部分
			'class' => 'fecshop\app\appfront\widgets\Head',
			# 根据多模板的优先级，依次去模板找查找该文件，直到找到这个文件。
			'view'  => 'widgets/head.php',
			# 缓存
			'cache' => [
				'enable'	=> false, # 是否开启
				'timeout' 	=> 4500,  # 缓存过期时间
			],
		],
		'header' => [
			'class' => 'fecshop\app\appfront\widgets\Headers',
			# 根据多模板的优先级，依次去模板找查找该文件，直到找到这个文件。
			'view'  => 'widgets/header.php',
			'cache' => [
				'enable'	=> false,
				'timeout' 	=> 4500,
			],
		],
		'topsearch' => [
			'view'  => 'widgets/topsearch.php',
		],
		'menu' => [
			'class' => 'fecshop\app\appfront\widgets\Menu',
			# 根据多模板的优先级，依次去模板找查找该文件，直到找到这个文件。
			'view'  => 'widgets/menu.php',
			'cache' => [
				'enable'	=> false,
				//'timeout' 	=> 4500,
			],
		],
		'footer' => [
			'class' => 'fecshop\app\appfront\widgets\Footer',
			# 根据多模板的优先级，依次去模板找查找该文件，直到找到这个文件。
			'view'  => 'widgets/footer.php',
			'cache' => [
				'enable'	=> false,
				//'timeout' 	=> 4500,
			],
		],
		'scroll' => [
			#'class' => 'fecshop\app\appfront\modules\Cms\block\widgets\Scroll',
			# 根据多模板的优先级，依次去模板找查找该文件，直到找到这个文件。
			'view'  => 'widgets/scroll.php',
		],
		'breadcrumbs' => [
			'view'  => 'widgets/breadcrumbs.php',
		],
		'flashmessage' => [
			'view'  => 'widgets/flashmessage.php',
		],
	]
],
```

上面是各个区块的配置，
各个区块的调用方式为：

```
<?= Yii::$service->page->widget->render('menu',$this); ?>
```

如果上面的配置设置了cache 为开启，并且class对应的动态数据提供对象，
存在函数 getCacheKey ,则可以使用缓存

譬如：head的配置如下，下面定义了cache是否开启，如果设置为true，则
代表开启缓存， timeout代表缓存的过期时间。

```
'head' => [
			# 动态数据提供部分
			'class' => 'fecshop\app\appfront\widgets\Head',
			# 根据多模板的优先级，依次去模板找查找该文件，直到找到这个文件。
			'view'  => 'widgets/head.php',
			# 缓存
			'cache' => [
				'enable'	=> false, # 是否开启
				'timeout' 	=> 4500,  # 缓存过期时间
			],
		],
```

上面配置中的class对应的Head.php文件内容为：

```
<?php
namespace fecshop\app\appfront\widgets;
use Yii;
use fecshop\interfaces\block\BlockCache;
class Head implements BlockCache
{
    public function getLastData()
    {
		return [
			
		];
	}
	
	public function getCacheKey(){
		$store = Yii::$service->store->currentStore;
		$moduleId = Yii::$app->controller->module->id;
		$controllerId = Yii::$app->controller->id;
		$actionId = Yii::$app->controller->action->id;
		$urlPathKey = $moduleId.'_'.$controllerId.'_'.$actionId;
		return self::BLOCK_CACHE_PREFIX.'_'.$store.'_'.$urlPathKey;
	
	}
}
```

`getCacheKey()` 返回的是cache的唯一key。


通过上面的配置可以看出，`head`， `header`， `footer`， `menu` 
都是可以设置局部缓存的。

### 关于Yii2 cache

更多的参看：[Yii2 cache](http://www.yiichina.com/doc/guide/2.0/caching-overview)


