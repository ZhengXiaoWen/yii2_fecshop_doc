Api- Paypal Express Start
================

> 在cart页面，点击paypal按钮后执行的api

URL: `/payment/paypal/express/start`

格式：`json`

方式：`post`


一：请求部分
---------

#### 1.Request Header


| 参数名称          | 是否必须    |  类型        |  描述     |
| ------------------| -----:      | :----:       |:----:     |
| access-token      | 必须        |   String     | 从localStorage取出来的值，识别用户登录状态的标示，用户登录成功，服务端返回access-token，VUE保存到localStorage中，在下一次请求从localStorage取出来放到request header中   |
| fecshop-currency  | 必须        |   String     | 从localStorage取出来的值，当前的货币值  |
| fecshop-lang      | 必须        |   String     | 从localStorage取出来的值，当前的语言code  |


#### 2.Request Body Form-Data：


| 参数名称        | 是否必须    |  类型       |  描述     |
| ----------------| -----:      | :----:      |:----:     |
| return_url      | 必须        |   String     | paypal express支付过程中，用户登录paypal账户，然后点击continue跳转回fecshop的url地址    |
| cancel_url      | 必须        |   String     | paypal express支付过程中，用户取消支付，跳转回fecshop的url地址   |

**请求参数示例如下：**

```
{
    return_url:"http://demo.fancyecommerce.com/#/payment/paypal/express/review",
    cancel_url:"http://demo.fancyecommerce.com/#/checkout/cart"
}
```

二：返回部分
----------

#### 1.Reponse Header

| 参数名称          | 是否必须    |  类型        |  描述     |
| ------------------| -----:      | :----:       |:----:     |
| access-token      | 选填        |   String     | 用户登录成功，服务端返回access-token，VUE保存到localStorage中，在下一次请求从localStorage取出来放到request header中   |

#### 2.Reponse Body Form-Data：

格式：`json`

| 参数名称        | 是否必须    |  类型       |  描述        |
| ----------------| -----:      | :----:      |:----:        | 
| code            | 必须        |   Number    | 返回状态码，200 代表完成，完整的返回状态码详细参看:[Api- 状态码](fecshop-server-return-code.md) |
| message         | 必须        |   String    | 返回状态字符串描述  |
| data            | 必须        |   Array     | 返回详细数据        |

#### 3.参数code所有返回状态码：（完整的返回状态码详细参看:[Api- 状态码](fecshop-server-return-code.md) ）

| code Value      |        描述                                        |
| ----------------| --------------------------------------------------:| 
| 200             | 成功状态码                                         |  
| 1500001         | Order: 下订单，产品库存不足                        | 
| 1500002         | Order: 下订单，生成订单失败。                      | 
| 1500003         | Order: 通过paypal express方式支付，获取token失败   | 




#### 4.返回数据举例：



```
{
    "code": 200,
    "message": "process success",
    "data": {
        // 跳转到paypal进行支付的url
        "redirectUrl": "https://www.sandbox.paypal.com/cgi-bin/webscr?cmd=_express-checkout&token=EC-982676192H039871M"  // 跳转到paypal的地址。
    }
}
```