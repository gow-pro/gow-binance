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





## 2、跳入GOW的KYC界面

**接口地址** `/user/userKyc`


**请求方式** `POST`


**consumes** `["application/json"]`


**produces** `["*/*","application/json"]`


**接口描述** ``

**请求参数**

| 参数名称         | 参数说明     |     请求类型 |  是否必须      |  数据类型   |  schema  |
| ------------ | -------------------------------- |-----------|--------|----|--- |
| account  | 账户（用户邮箱或者第三方唯一标识ID） |  query  | true | string   |      |
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
|bankCountryCode|收款银行的国家| string   |      |
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



## 查询订单 - 身份验证

**接口地址** `/openapi/v1/verified/userVerifiedStepFour.do`


**请求方式** `POST`


**consumes** `["multipart/form-data"]`


**produces** `["*/*","application/json"]`


**接口描述** ``

**请求参数**

| 参数名称         | 参数说明     |     请求类型 |  是否必须      |  数据类型   |  schema  |
| ------------ | -------------------------------- |-----------|--------|----|--- |
| cardFrontFile         |      证件正面照   |     formData        |       false      | file   |      |
| cardNo         |      证件号码   |     query        |       true      | string   |      |
| cardType         |      证件类型 1护照 2身份证  3Employee ID 4 SSS  5 GSIS 6 TIN  7PRC Reg.No. 8 NBI Clearance 9 Senior Citizen ID 10 Student ID 11 Integrated Bar of the Philippines ID(菲律宾籍） 12 OWWA ID 13 OFW ID 14 SEAMAN'S BOOK 15 POSTAL ID  16 DIGITIZED VOTER'S ID 17 LICENSE TO CARRY FIREARMS 18 PHILIPPINE RETIREMENT AUTHORITY ID 19 驾驶证 (菲律宾籍） 20外国人登记证明书 (菲律宾籍）   |     query        |       true      | string   |      |
| cardValidityPeriod         |      证件有效期   |     query        |       true      | string   |      |
| handSignFile         |      手持签名照   |     formData        |       false      | file   |      |



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



## 认证第五步 - 人脸识别

**接口地址** `/openapi/v1/verified/userVerifiedStepFive.do`


**请求方式** `POST`


**consumes** `["multipart/form-data"]`


**produces** `["*/*","application/json"]`


**接口描述** ``

**请求参数**

| 参数名称         | 参数说明     |     请求类型 |  是否必须      |  数据类型   |  schema  |
| ------------ | -------------------------------- |-----------|--------|----|--- |
| faceAuthFile         |      人脸识别图片   |     formData        |       false      | file   |      |




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



