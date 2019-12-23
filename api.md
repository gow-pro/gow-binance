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


# 用户认证


## 查询用户认证信息

**接口地址** `/openapi/v1/verified/queryUserVerified.do`


**请求方式** `GET`


**consumes** ``


**produces** `["*/*","application/json"]`


**接口描述** ``

**请求参数**

| 参数名称         | 参数说明     |     请求类型 |  是否必须      |  数据类型   |  schema  |
| ------------ | -------------------------------- |-----------|--------|----|--- |






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
| data     |响应数据      |    用户认证信息   |   用户认证信息    |
| description     |响应描述      |    string   |       |
            



**schema属性说明**
  
**用户认证信息**

| 参数名称         | 参数说明                             |    类型 |  schema |
| ------------ | ------------------|--------|----------- |
| birthAddr         |     出生地      |  string   |      |
| birthDate         |     出生日期(yyyy-MM-dd)      |  string   |      |
| cardFrontServerUrl         |     证件正面照服务器URL      |  string   |      |
| cardFrontUrl         |     证件正面照URL      |  string   |      |
| cardNo         |     证件号码      |  string   |      |
| cardStatus         |     状态: 1.未认证; 2.待审核; 3.审核不通过; 4.已认证; 5.复审;      |  string   |      |
| cardType         |     证件类型 1护照 2身份证  3Employee ID 4 SSS  5 GSIS 6 TIN  7PRC Reg.No. 8 NBI Clearance 9 Senior Citizen ID 10 Student ID 11 Integrated Bar of the Philippines ID(菲律宾籍） 12 OWWA ID 13 OFW ID 14 SEAMAN'S BOOK 15 POSTAL ID  16 DIGITIZED VOTER'S ID 17 LICENSE TO CARRY FIREARMS 18 PHILIPPINE RETIREMENT AUTHORITY ID 19 驾驶证 (菲律宾籍） 20外国人登记证明书 (菲律宾籍）      |  string   |      |
| cardValidityPeriod         |     证件有效期(yyyy-MM-dd)      |  string   |      |
| companyName         |     公司名称      |  string   |      |
| countryCode         |     国家代码      |  string   |      |
| currentStep         |     当前步骤: 1.第一步; 2.第二步; 3.第三步; 4.第四步;      |  string   |      |
| employmentStatus         |     就业状态: 1.在职; 2.个体经营; 3.退休; 4.无业; 5.学生;      |  string   |      |
| faceAuthUrl         |     人脸识别图片url      |  string   |      |
| firstName         |     名字      |  string   |      |
| fundSource         |     资金来源: 1.工资; 2.储蓄; 3.礼物/遗产; 4.营运资金; 5.出售物业; 6.汇款; 7.养老金; 8.补贴; 9.彩票; 10.捐款;      |  string   |      |
| handSignServerUrl         |     手持签名照服务器URL      |  string   |      |
| handSignUrl         |     手持签名照URL      |  string   |      |
| id         |     主键ID      |  string   |      |
| income         |     年收入(单位: 美元)      |  string   |      |
| incomeSource         |     收入来源: 1.薪水; 2.家庭成员或海外汇款; 3.个人储蓄; 4.其他;      |  string   |      |
| incomeSourceOther         |     其他收入来源      |  string   |      |
| isIdentical         |     永久居住地址是否与现居住地址一致: 1. 是; 2. 否;      |  string   |      |
| lastName         |     姓氏      |  string   |      |
| natureBusiness         |     行业: 1.专业; 2.家务; 3.私企雇员; 4.政府雇员; 5.创业/企业家; 6.医生; 7.律师; 8.牙医; 9.工程师; 10.建筑师; 11.会计; 12.银行家; 13.运动员; 14.家庭主妇; 15.海外劳工; 99.其他职业;      |  string   |      |
| permanentAddr         |     永久居住详细地址      |  string   |      |
| permanentCity         |     永久居住城市      |  string   |      |
| permanentCountry         |     永久居住国家      |  string   |      |
| permanentPostcode         |     永久居住邮政编码      |  string   |      |
| permanentProvince         |     永久居住省份      |  string   |      |
| position         |     职位      |  string   |      |
| residenceAddr         |     现居住详细地址      |  string   |      |
| residenceCity         |     现居住城市      |  string   |      |
| residenceCountry         |     现居住国家      |  string   |      |
| residencePostcode         |     现居住邮政编码      |  string   |      |
| residenceProvince         |     现居住省份      |  string   |      |
| sex         |     性别      |  string   |      |
| taxNo         |     税号      |  string   |      |
| taxType         |     税号类型: 1.TIN; 2.SSS; 3.GSIS      |  string   |      |
| userId         |     用户ID      |  string   |      |
| verifiedDesc         |     审核描述--中文      |  string   |      |
| verifiedEnDesc         |     审核描述--英文      |  string   |      |
            




**响应示例**


```json
{
	"code": "",
	"data": {
		"birthAddr": "",
		"birthDate": "",
		"cardFrontServerUrl": "",
		"cardFrontUrl": "",
		"cardNo": "",
		"cardStatus": "",
		"cardType": "",
		"cardValidityPeriod": "",
		"companyName": "",
		"countryCode": "",
		"currentStep": "",
		"employmentStatus": "",
		"faceAuthUrl": "",
		"firstName": "",
		"fundSource": "",
		"handSignServerUrl": "",
		"handSignUrl": "",
		"id": "",
		"income": "",
		"incomeSource": "",
		"incomeSourceOther": "",
		"isIdentical": "",
		"lastName": "",
		"natureBusiness": "",
		"permanentAddr": "",
		"permanentCity": "",
		"permanentCountry": "",
		"permanentPostcode": "",
		"permanentProvince": "",
		"position": "",
		"residenceAddr": "",
		"residenceCity": "",
		"residenceCountry": "",
		"residencePostcode": "",
		"residenceProvince": "",
		"sex": "",
		"taxNo": "",
		"taxType": "",
		"userId": "",
		"verifiedDesc": "",
		"verifiedEnDesc": ""
	},
	"description": ""
}
```


## 认证第一步 - 个人信息

**接口地址** `/openapi/v1/verified/userVerifiedStepOne.do`


**请求方式** `POST`


**consumes** `["application/json"]`


**produces** `["*/*","application/json"]`


**接口描述** ``

**请求参数**

| 参数名称         | 参数说明     |     请求类型 |  是否必须      |  数据类型   |  schema  |
| ------------ | -------------------------------- |-----------|--------|----|--- |
| birthAddr         |      出生地   |     query        |       true      | string   |      |
| birthDate         |      出生日期   |     query        |       true      | string   |      |
| countryCode         |      国家代码   |     query        |       true      | string   |      |
| firstName         |      名字   |     query        |       true      | string   |      |
| lastName         |      姓氏   |     query        |       true      | string   |      |
| locale         |      语言   |     query        |       false      | string   |      |
| sex         |      性别: 0.保密; 1.男; 2.女;   |     query        |       true      | string   |      |
            


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



## 认证第二步 - 地址信息

**接口地址** `/openapi/v1/verified/userVerifiedStepTwo.do`


**请求方式** `POST`


**consumes** `["application/json"]`


**produces** `["*/*","application/json"]`


**接口描述** ``

**请求参数**

| 参数名称         | 参数说明     |     请求类型 |  是否必须      |  数据类型   |  schema  |
| ------------ | -------------------------------- |-----------|--------|----|--- |
| isIdentical         |      永久居住地址是否与现居住地址一致: 1.是; 2.否;   |     query        |       true      | string   |      |
| locale         |      语言   |     query        |       false      | string   |      |
| permanentAddr         |      永久居住详细地址   |     query        |       false      | string   |      |
| permanentCity         |      永久居住城市   |     query        |       false      | string   |      |
| permanentCountry         |      永久居住国家   |     query        |       false      | string   |      |
| permanentPostcode         |      永久居住邮政编码   |     query        |       false      | string   |      |
| permanentProvince         |      永久居住省份   |     query        |       false      | string   |      |
| residenceAddr         |      现居住详细地址   |     query        |       true      | string   |      |
| residenceCity         |      现居住城市   |     query        |       true      | string   |      |
| residenceCountry         |      现居住国家   |     query        |       true      | string   |      |
| residencePostcode         |      现居住邮政编码   |     query        |       true      | string   |      |
| residenceProvince         |      现居住省份   |     query        |       true      | string   |      |



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



## 认证第三步 - 收入信息

**接口地址** `/openapi/v1/verified/userVerifiedStepThree.do`


**请求方式** `POST`


**consumes** `["application/json"]`


**produces** `["*/*","application/json"]`


**接口描述** ``

**请求参数**

| 参数名称         | 参数说明     |     请求类型 |  是否必须      |  数据类型   |  schema  |
| ------------ | -------------------------------- |-----------|--------|----|--- |
| companyName         |      公司名称   |     query        |       false      | string   |      |
| employmentStatus         |      就业状态: 1.在职; 2.个体经营; 3.退休; 4.无业; 5.学生;   |     query        |       true      | string   |      |
| fundSource         |      资金来源: 1.工资; 2.储蓄; 3.礼物/遗产; 4.营运资金; 5.出售物业; 6.汇款; 7.养老金; 8.补贴; 9.彩票; 10.捐款;   |     query        |       false      | string   |      |
| income         |      年收入(单位: 美元)   |     query        |       true      | string   |      |
| incomeSource         |      收入来源: 1.薪水; 2.家庭成员或海外汇款; 3.个人储蓄; 4.其他;   |     query        |       true      | string   |      |
| incomeSourceOther         |      其他收入来源   |     query        |       false      | string   |      |
| locale         |      语言   |     query        |       false      | string   |      |
| natureBusiness         |      行业: 1.专业; 2.家务; 3.私企雇员; 4.政府雇员; 5.创业/企业家; 6.医生; 7.律师; 8.牙医; 9.工程师; 10.建筑师; 11.会计; 12.银行家; 13.运动员; 14.家庭主妇; 15.海外劳工; 99.其他职业;   |     query        |       false      | string   |      |
| position         |      职位   |     query        |       false      | string   |      |
    |
| taxNo         |      税号   |     query        |       false      | string   |      |
| taxType         |      税号类型: 1.TIN; 2.SSS; 3.GSIS   |     query        |       false      | string   |      |           



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



## 认证第四步 - 身份验证

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



