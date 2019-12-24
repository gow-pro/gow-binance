**用户认证RESTful API**


# 接入说明


## 请求头部参数信息

接口请求头部必须包含以下信息

* content-type：application/json
* timestamp：请求时间戳，单位毫秒
* apiKey：API KEY访问密钥
* sign：签名信息


## 签名认证

### 创建API KEY

API Key 包括以下两部分
* API KEY api访问密钥
* Secret Key 签名认证加密所使用的密钥（仅申请时可见）

### 签名步骤

签名使用HmacSHA512算法签名，签名内容将各参数使用字符 “&” 连接

#### 签名内容

**GET请求**:只需要将apiKey和timestamp按照ASCII码的顺序对参数名进行倒序排序，如：
* timestamp=1566322200000&apiKey=4565B83XXXXXXXXXXXXF123

**POST请求**:需将apiKey和timestamp以及对应接口传参按照ASCII码的顺序对参数名进行倒序排序，如撤销订单：
* timestamp=1566322200000&orderId=E12345XXXXXXXX&apiKey=4565B83XXXXXXXXXXXXF123

#### 签名

将生成的签名内容和secretKey使用HmacSHA512算法生成签名

##### java demo 
```java demo 
            String timestamp = "1566322200000";
            String secretKey = "XXXXXXXXXXXXXXXXXXXXXXX";
            Mac hmacSha512 = Mac.getInstance("HmacSHA512");
            SecretKeySpec secKey = new SecretKeySpec(secretKey.getBytes(StandardCharsets.UTF_8), "HmacSHA512");
            hmacSha512.init(secKey);
            String paramStr = "timestamp=1566322200000&orderId=E12345XXXXXXXX&apiKey=4565B83XXXXXXXXXXXXF123";
            byte[] hash = hmacSha512.doFinal(paramStr.getBytes(StandardCharsets.UTF_8));
            String sign = Hex.encodeHexString(hash);

```


##### php demo 
```php demo 
            $timestamp = '1566322200000';
            $secretKey = 'XXXXXXXXXXXXXXXXXXXXXXX';
            $str = 'timestamp=1566322200000&orderId=E12345XXXXXXXX&apiKey=4565B83XXXXXXXXXXXXF123';
            $sign = hash_hmac("sha512", $str, $secretKey,false);

```
## 请求格式

所有的API请求都以GET或者POST形式发出。对于GET请求，所有的参数都在路径参数里；对于POST请求，所有参数则以JSON格式发送在请求主体（body）里，头部信息（header）中
必须传timestamp、apiKey、sign

## 返回内容格式

| 参数名称         | 参数说明                             |    类型 |  schema |
| ------------ | -------------------|-------|----------- |
|code| 响应代码  |string  |  返回000000为调用成功，其他code均为错误代码  |
|data| 响应数据  |Object  |    |
|description| 响应描述  |string  |    |


# 出入金、KYC


## 1、查询用户实名认证状态

**接口地址** `/user/findUserAuthStatus`


**请求方式** `GET`


**consumes**  `["application/json"]`


**produces** `["*/*","application/json"]`


**接口描述** ``

**请求参数**

| 参数名称         | 参数说明     |     请求类型 |  是否必须      |  数据类型   |  schema  |
| ------------ | -------------------------------- |-----------|--------|----|--- |
| account         |      账户（用户邮箱或者第三方唯一标识ID）   |     query        |       true      | string   |      |
| source         |      渠道来源，固定值： binance   |     query        |       true      | string   |      |



**响应状态**

| 状态码         | 说明                             |    schema                         |
| ------------ | -------------------------------- |---------------------- |
| 200         | OK                        |响应结果«用户认证信息»                          |
| 401         | Unauthorized                        |                          |
| 403         | Forbidden                        |                          |
| 404         | Not Found                        |                          |


**响应参数**

| 参数名称         | 参数说明                             |    类型 |  schema |
| ------------ | -------------------|-------|----------- |
| code     |响应代码      |    string   |       |
| data     |响应数据      |    Object   |   用户认证状态信息    |
| description     |响应描述      |    string   |       |
            


**schema属性说明**
  
**用户认证状态信息**

| 参数名称         | 参数说明                             |    类型 |  schema |
| ------------ | ------------------|--------|----------- |
| cardStatus    |  状态: 0:待提交 1:审核通过 2审核不通过 3待审核 4未认证  |  string   |      |

            


**响应示例**


```json
{
	"code": "",
	"data": {
		"cardStatus": "1"
	},
	"description": ""
}
```





## 2、KYC认证页面

**接口地址** `/user/userKyc.html`


**请求方式** `GET`

**请求参数**

| 参数名称         | 参数说明     |     请求类型 |  是否必须      |  数据类型   |  schema  |
| ------------ | -------------------------------- |-----------|--------|----|--- |
| account  | 账户（用户邮箱或者第三方唯一标识ID） |  query  | true | string   |      |
| source   | 渠道来源，固定值： binance   |  query        | true  | string   |      |


## 3、查询GOW收款信息

**接口地址** `/transaction/getReceiveAccountInfo`


**请求方式** `get`


**consumes** `["application/json"]`


**produces** `["*/*","application/json"]`


**接口描述** ``

**请求参数**

| 参数名称         | 参数说明     |     请求类型 |  是否必须      |  数据类型   |  schema  |
| ------------ | -------------------------------- |-----------|--------|----|--- |
| source   | 渠道来源，固定值： binance   |  query        | true  | string   |      |



**响应状态**

| 状态码         | 说明                             |    schema                         |
| ------------ | -------------------------------- |---------------------- |
| 200         | OK                        |响应结果                          |
| 201         | Created                        |                          |
| 401         | Unauthorized                        |                          |
| 403         | Forbidden                        |                          |
| 404         | Not Found                        |                          |




**响应参数**

| 参数名称         | 参数说明                             |    类型 |  schema |
| ------------ | -------------------|-------|----------- |
| code     |响应代码      |    string   |       |
| data     |响应数据      |    object   |    收款信息   |
| description     |响应描述      |    string   |       |
            
 


**schema属性说明**
  
**用户认证状态信息**

| 参数名称         | 参数说明                             |    类型 |  schema |
| ------------ | ------------------|--------|----------- |
| cardStatus    |  状态: 0:待提交 1:审核通过 2审核不通过 3待审核 4未认证  |  string   |      |
|receiveBankName|收款银行名称| string   |      |
|receiveAccount|收款账号(银行卡账号)| string   |      |
|receiveFirstName|收款人名字| string   |      |
|receiveMiddleName|收款人中间名 |string   |      |
|receiveLastName|收款人姓氏| string   |      |
|receiveCountry	|收款人国家代码| string   |      |
|receiveProvince|收款人省份| string   |      |
|receiveCity|收款人城市| string   |      |
|receiveAddr|收款人地址| string   |      |
|receiveBankAddress|收款银行地址| string   |      |
|bankCountryCode|收款银行的国家代码| string   |      |
|bankProvince|收款银行省份| string   |      |
|bankCity|收款银行城市| string   |      |
|bankSwiftCode|银行的 SwiftCode| string   |      |

**响应示例**


```json
{
	"code":"000000",
	"data":{
		"receiveCountry":"CN",
		"receiveBankName":"UNINE Bank ",
		"bankSwiftCode":”UA125”,
		"bankProvince":"hunan",
		"receiveFirstName":"ETRANSS REMITTANCE INTERNATIONAL CORP.",
		"receiveAddr":"futianqushawei",
		"receiveProvince":"guangdong",
		"bankCity":"shenzhen",
		"receiveMiddleName":"john",
		"receiveBankAddress":"fengtianlu18xiang9hao",
		"bankCountryCode":"PH",
		"receiveCity":"shenzhen",
		"receiveAccount":"888888888",
		"receiveLastName":"Green"
	},
	"description":"成功"
}
```



## 4、推送入金订单

**接口地址** `/transaction/addChangeApplyOrder`


**请求方式** `POST`


**consumes** `["application/json"]`


**produces** `["*/*","application/json"]`


**接口描述** ``

**请求参数**

| 参数名称         | 参数说明     |     请求类型 |  是否必须      |  数据类型   |  schema  |
| ------------ | -------------------------------- |-----------|--------|----|--- |
| orderNo         |      订单编号（长度40位以内，含40）  |     query        |       true      | string   |      |
| account         |      账户（用户邮箱或者第三方唯一标识ID）   |     query        |       true      | string   |      |
| source         |    渠道来源,固定为： binance  |     query        |       true      | string   |      |
| legalAmount         | 入金数量   |     query        |       true      | string   |      |
| fiatCurrency         | 入金法币类型，固定：php |     query        |       true      | string   |      |
| countryCode         | 汇款人国家代码 |     query        |       true      | string   |      |
| firstName         |汇款人名字  |     query        |       true      | string   |      |
| middleName         |  汇款人中间名  |     query        |       true      | string   |      |
| lastName         | 汇款人姓   |   query   |       true      | string   |      |
| birthDate         | 汇款人生日   |   query    |       true      | string   |      |
| noteCode         | 备注码,汇款人填写汇款时需要用到；后期用于核对到账情况   |   query    |       true      | string   |      |
     



**响应状态**

| 状态码         | 说明                             |    schema                         |
| ------------ | -------------------------------- |---------------------- |
| 200         | OK                        |响应结果                          |
| 201         | Created                        |                          |
| 401         | Unauthorized                        |                          |
| 403         | Forbidden                        |                          |
| 404         | Not Found                        |                          |




**响应参数**

| 参数名称         | 参数说明                             |    类型 |  schema |
| ------------ | -------------------|-------|----------- |
| code     |响应代码      |    string   |       |
| data     |响应数据      |    object   |       |
| description     |响应描述      |    string   |       |
            




**响应示例**


```json
{
	"code": "",
	"data": {},
	"description": ""
}
```



## 5、查询订单详情 - 

**接口地址** `/transaction/getOrderDetail`


**请求方式** `get`


**consumes** `["application/json"]`


**produces** `["*/*","application/json"]`


**接口描述** ``

**请求参数**

| 参数名称         | 参数说明     |     请求类型 |  是否必须      |  数据类型   |  schema  |
| ------------ | -------------------------------- |-----------|--------|----|--- |
| orderNo         |     订单编号  |     query        |       true      | string   |      |
| source   | 渠道来源，固定值： binance   |  query        | true  | string   |      |




**响应状态**

| 状态码         | 说明                             |    schema                         |
| ------------ | -------------------------------- |---------------------- |
| 200         | OK                        |响应结果                          |
| 201         | Created                        |                          |
| 401         | Unauthorized                        |                          |
| 403         | Forbidden                        |                          |
| 404         | Not Found                        |                          |




 
**响应参数**

| 参数名称         | 参数说明                             |    类型 |  schema |
| ------------ | -------------------|-------|----------- |
| code     |响应代码      |    string   |       |
| data     |响应数据      |    object   |    订单信息   |
| description     |响应描述      |    string   |       |
            
 


**schema属性说明**
  
**用户认证状态信息**

| 参数名称         | 参数说明                             |    类型 |  schema |
| ------------ | ------------------|--------|----------- |
| orderNo    |  状态: 0:待提交 1:审核通过 2审核不通过 3待审核 4未认证  |  string   |      |
|account|  账户（用户邮箱或者第三方唯一标识ID）| string   |      |
|legalCurrenyName|法币币种名称| string   |      |
|legalAmount|法币数量| string   |      |
|currencyAmount|稳定币数量 |string   |      |
|fee|手续费（收取稳定币手续费）| string   |      |
|channelFeeRate	|通道费率| string   |      |
|channelFee|收取的通道费| string   |      |
|exchRate|实时汇率| string   |      |
|buyType|入金方式 1微信 2支付宝 3银行卡| string   |      |
|dateCreate|申请时间| long   |      |
|applyStatus|申请状态 1待审核 2审核不通过 3待打款（审核通过） 4待放币 5已放币  | string   |      |
|applyType|兑换类型  1 入金 2出金| string   |      |
|receiveAccount|收款账号(银行卡账号)| string   |      |
|receiveName|收款者姓名| string   |      |
|receiveFirstName|收款人名字| string   |      |
|receiveMiddleName|收款人中间名| string   |      |
|receiveLastName|收款人姓氏| string   |      |
|receiveBirthDate|收款人生日| string   |      |
|receiveCountry|收款人国家代码| string   |      |
|receiveProvince|收款人省份| string   |      |
|receiveCity|收款人城市| string   |      |
|receiveAddr|收款人地址| string   |      |
|receiveBankName|收款银行名称| string   |      |
|receiveBankAddress|收款银行地址| string   |      |
|bankCountryCode|收款银行的国家代码| string   |      |
|bankProvince|收款银行省份| string   |      |
|bankCity|收款银行城市| string   |      |
|branchName|收款银行支行名称| string   |      |
|bankSwiftCode|收款银行的 SwiftCode| string   |      |
|payFirstName|付款人名字| string   |      |
|payMiddleName|付款人中间名| string   |      |
|payLastName|付款人姓氏| string   |      |
|payCountry|付款人国籍| string   |      |
|payDirthDate|付款人生日| string   |      |
|noteCode|外币入金收款备注码| string   |      |
|dateAudit|审核时间| long   |      |
|deadlinePay|打款截止时间| long   |      |
|timePay|打款时间| long   |      |
|timeIssue|放币时间| long   |      |
|loanType|放币类型： 1手动  2自动| string   |      |



**响应示例**


```json
{
	"code":"000000",
	"data":{
                     ...
	},
	"description":"成功"
}
```

 
 


## 6、推送出金订单

**接口地址** `/transaction/addChangeApplySale`


**请求方式** `POST`


**consumes** `["application/json"]`


**produces** `["*/*","application/json"]`


**接口描述** ``

**请求参数**

| 参数名称         | 参数说明     |     请求类型 |  是否必须      |  数据类型   |  schema  |
| ------------ | -------------------------------- |-----------|--------|----|--- |
| orderNo         |      订单编号（长度40位以内，含40）  |     query        |       true      | string   |      |
| account         |      账户（用户邮箱或者第三方唯一标识ID）   |     query        |       true      | string   |      |
| source         |    渠道来源,固定为： binance  |     query        |       true      | string   |      |
| currencyAmount         | 出金数量   |     query        |       true      | string   |      |
| fiatCurrency         | 出金得到的法币类型，固定值：php |     query        |       true      | string   |      |
| receiveAccount         |收款账号(银行卡账号)|     query        |       true      | string   |      |
| receiveName         | 收款者姓名 |     query        |       true      | string   |      |
| receiveFirstName         |收款人名字  |     query        |       true      | string   |      |
| receiveMiddleName         |  收款人中间名  |     query        |       true      | string   |      |
| receiveLastName         | 收款人姓氏   |   query   |       true      | string   |      |
| receiveBirthDate         | 收款人生日   |   query    |       true      | string   |      |
| receiveCountry         | 收款人国家代码   |   query    |       true      | string   |      |
| receiveProvince         | 收款人省份   |   query    |       false      | string   |      |
| receiveCity         | 收款人城市   |   query    |       false      | string   |      |
| receiveAddr         | 收款人地址   |   query    |       true      | string   |      |
| receiveBankName         | 收款银行名称   |   query    |       true      | string   |      |
| receiveBankAddress         | 收款银行地址   |   query    |       true      | string   |      |
| bankCountryCode         | 收款银行的国家代码   |   query    |       true      | string   |      |
| bankProvince         | 收款银行省份   |   query    |       true      | string   |      |
| bankCity         | 收款银行城市   |   query    |       true      | string   |      |
| branchName         | 收款银行支行名称   |   query    |       true      | string   |      |
| bankSwiftCode         | 收款银行的 SwiftCode   |   query    |       true      | string   |      |
     



**响应状态**

| 状态码         | 说明                             |    schema                         |
| ------------ | -------------------------------- |---------------------- |
| 200         | OK                        |响应结果                          |
| 201         | Created                        |                          |
| 401         | Unauthorized                        |                          |
| 403         | Forbidden                        |                          |
| 404         | Not Found                        |                          |




**响应参数**

| 参数名称         | 参数说明                             |    类型 |  schema |
| ------------ | -------------------|-------|----------- |
| code     |响应代码      |    string   |       |
| data     |响应数据      |    object   |       |
| description     |响应描述      |    string   |       |
            




**响应示例**


```json
{
	"code": "",
	"data": {},
	"description": ""
}
```


