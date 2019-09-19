Api- Cart 更新购物车信息
================

> vue Cart页面，更新购物车的产品的个数(购物车产品个数`加一`和`减一`)，以及`删除`购物车产品等操作

URL: `/checkout/cart/updateinfo`

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


| 参数名称        | 是否必须    |  类型        |  描述     |
| ----------------| -----:      | :----:       |:----:     |
| up_type         | 必须        |   String     | 更新的类型，分别为：`less_one`（Cart产品个数减一）, `add_one`（Cart产品个数加一）, `remove`（删除Cart产品）    |
| item_id         | 必须        |   String     | 购物车item的id，也就是 sales_flat_cart_item表的id   |

**请求参数示例如下：**

```
{
    up_type:"add_one",
    item_id:411
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
| 1400003         | Cart：更改cart中product的个数失败                  | 


#### 4.返回数据举例：

```
{
    "code": 200,
    "message": "process success",
    "data": []
}
```