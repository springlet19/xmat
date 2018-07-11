------

# x-Mat平台接口设计文档



------

|  版本/状态   |   责任人   |    修改日期    | 版本变更记录                                   |
| :------: | :-----: | :--------: | :--------------------------------------- |
V0.35/草稿 |   梁贝   | 2018/05/16 | 修改检测设备升级的批量接口,update新增字段2:正在升级中 
|V0.34/草稿 |   曹西达   | 2018/05/10 | 修改获取设备激活状态接口
|V0.33/草稿 |   梁贝   | 2018/05/07 | 增加sos录音接口
|V0.32/草稿 |   梁贝   | 2018/04/27 | 增加检测设备升级的批量接口  
| V0.31/草稿 |   梁贝   | 2018/04/27 | 通讯录模块的接口字段增加  
| V0.30/草稿 |   曹西达   | 2018/04/24 | 1.新增查询设备卡信息接口                            |
| V0.29/草稿 |   曹西达   | 2018/04/18 | 1.新增获取设备fota升级进度接口<br />2.新增设备升级时服务器向app推送设备fota升级进度socket传输协议 |
| V0.28/草稿 |   梁贝    | 2018/04/17 | 1.新增静音时段的增产查改的接口                         |
| V0.27/草稿 |   梁贝    | 2018/04/09 | 1.修改立即定位接口                               |
| V0.26/草稿 |   梁贝    | 2018/02/26 | 1.获取当前用户绑定的所有设备列表,轮询接口,激活绑定接口的返回值中增加deviceVoiceType字段,2.修改设备设置项中新增字段deviceVoiceSwitch|
| V0.25/草稿 |   胡才庆   | 2018/01/23 | 1.增加语音对讲模块                               |
| V0.24/草稿 |   梁贝    | 2017/11/17 | 1.增加获取宠物设备灯设置2.修改宠物设备灯设置3.获取当前用户绑定的所有设备列表中增加deviceType字段 |
| V0.23/草稿 |   梁贝    | 2017/10/18 | 1.定位模式( 0:待机模式,4:智能模式, 2:追踪模式)(可选)       |
| V0.22/草稿 |   廖龙辉   | 2017/10/17 | 1.固件升级接口增加size参数;<br>2.告警记录增加电子围栏名称和触发类型; |
| V0.21/草稿 |   廖龙辉   | 2017/09/28 | 1.增加检测固件升级和固件升级接口;<br>2.更正手机卡类型的参数值;<br>3.告警记录和告警信息推送接口增加定位精度和电量信息;<br>4.修改历史轨迹接口，删除time,增加beginTime/endTime,用于计算驻留时间; |
| V0.20/草稿 |   廖龙辉   | 2017/09/21 | 1.修改唤醒设备接口，由单个唤醒改为批量唤醒;                  |
| V0.19/草稿 |   廖龙辉   | 2017/09/19 | 1.增加唤醒设备接口;<br>2.告警记录增加类型:4-SIM卡变更告警;<br>3.废弃"设备更换SIM卡"推送接口, 合并到"告警信息"推送接口; |
| V0.18/草稿 |   廖龙辉   | 2017/09/11 | 1.修改安全区域相关接口中alertCondition的值;           |
| V0.17/草稿 |   廖龙辉   | 2017/09/08 | 1."设备手机号变更推送"更正为"设备更换SIM卡推送";<br>2.设备信息变更推送中增加手机号变更推送信息; |
| V0.16/草稿 |   廖龙辉   | 2017/09/06 | 1. 引入UUID，用以标识客户端的唯一性，受影响的接口包括注册和登录接口;   |
| V0.15/草稿 |   廖龙辉   | 2017/09/04 | 1. 定位模式参数值, 修改设备信息变更推送接口中定位模式的参数名;<br>2. 手机号变更推送删除devicePhoneNo参数 |
| V0.14/草稿 |   廖龙辉   | 2017/08/31 | 1. 修改推送模块协议, 增加title参数;<br>2.对一些历史遗留错误进行修正; |
| V0.13/草稿 |   廖龙辉   | 2017/08/28 | 1. 获取设备详细信息接口增加filter请求参数;<br>2. 获取设备设置接口增加filter请求参数; |
| V0.12/草稿 |   廖龙辉   | 2017/08/24 | 1.拆分设置模块, 独立出闹钟功能模块，其他设置项统一接口并入设备模块 ;<br>2.增加用药提醒模块;<br>3.设备信息中增加设备手机卡类型参数，受影响的接口有:<br>  激活+绑定(2.2)<br>  绑定设备(2.5)<br>  获取已绑定的设备列表(2.8)<br>  获取设备详细信息(2.9)<br>  修改设备信息(2.11) |
| V0.11/草稿 |   廖龙辉   | 2017/08/23 | 1.修改通话记录中通话状态的值:1-呼出 2-未接 3-已接 4-拒接;<br>2.增加设置模块; |
| V0.10/草稿 |   廖龙辉   | 2017/08/21 | 1.增加监听接口;<br>2.增加定位方式说明:1-GPS/2-LBS/3-WIFI/4-BT; |
| v0.9/草稿  |   廖龙辉   | 2017/08/17 | 1.修改记录模块中部分url地址及参数名                     |
| v0.8/草稿  |   廖龙辉   | 2017/08/16 | 1.统一定位方式的参数名为locateType;<br>2.统一验证码的参数名为verifyCode;<br>3.绑定设备接口请求参数增加verifyCode;<br>4.安全区域独立为一个模块; |
| v0.7/草稿  |   廖龙辉   | 2017/08/15 | 1.增加退出登录接口;                              |
| v0.6/草稿  |   廖龙辉   | 2017/08/11 | +1.根据新需求修改记录模块接口:<br>  告警记录增加低电量告警类型, 请求参数增加alertType(可选);<br>  监听记录增加contactName参数;<br>  通话记录增加contactName参数, 修改手机号参数名; |
| v0.5/草稿  |   廖龙辉   | 2017/08/10 | 1.增加RSA加密算法说明及公钥;<br>2.修改推送模块接口, 增加极光推送说明;<br>3. 修改账号头像接口中icon改为userIcon; |
| v0.4/草稿  |   廖龙辉   | 2017/08/09 | 1.修改账号昵称接口改为修改账号信息接口;<br>2.获取设备最新定位信息接口中增加deviceIds参数;<br>3. 修改透传接口响应参数; |
| v0.3/草稿  |   廖龙辉   | 2017/08/08 | 1. 统一access_token为token;<br>2.修改注册推送接口参数; |
| v0.2/草稿  |   廖龙辉   | 2017/08/07 | 1. 修改用户注册接口参数名，删除RegistrationID;<br>2. 修改登录和token接口的响应参数，删除devices;<br>3.修改获取单个设备详情接口URL, 将deviceId修改为请求参数;<br>4. 增加注册推送接口 |
| v0.1/草稿  | 廖龙辉/王荣阳 | 2017/08/04 | 1. 基于原Mat平台接口文档进行修改整合                    |
|          |         |            |                                          |



------

[TOC]

---

## 概述

略

### Header说明

| 参数           | 类型     | 说明                                       |
| ------------ | ------ | ---------------------------------------- |
| Content-Type | String | 请求类型，若无特别，均为“application/json;charset=utf-8” |
| User-Agent   | String | 客户端类型，Android/IOS/WEB                    |

### 响应格式说明

响应格式如下所示:

```json
{
  "code": 0,
  "msg": "success",
  "data": {}
}
```

参数说明:

| 参数   | 类型           | 参数说明            |
| ---- | ------------ | --------------- |
| code | Integer      | 错误码，参考错误码文档     |
| msg  | String       | 错误信息            |
| data | Object/Array | 响应内容，如无响应内容则不返回 |

后续所有响应参数说明不再描述这3个参数。

### 错误码说明

错误码用于定位错误类型，原则上每个错误码都代表唯一的错误类型。

特别地定义了一组通用错误码：

| 错误码    | 错误信息     |
| ------ | -------- |
| 0      | 请求成功     |
| 1      | 请求部分成功   |
| -10000 | 系统错误     |
| -10001 | 请求参数错误   |
| -10002 | token已失效 |

后续所有接口的错误返回均有可能返回通用错误码，所以不再重复列举描述。

---

## 用户模块(黎祖详)

用户账户管理模块。

### 用户注册

> 接口描述

  用户注册。

> 请求方式

  | 请求路径 | http://<HOST>:<PORT>/platform/user/register |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

> 请求参数

  | 参数                | 类型     | 参数说明                                     |
  | ----------------- | ------ | ---------------------------------------- |
  | register          | String | 注册信息经过RSA加密后生成的密文，原文为由以下字段的JSON串:{"userPhoneNo":"","userPassword":"","verifyCode":"","uuid":""} |
  | sign.userPhoneNo  | String | 用户手机号                                    |
  | sign.userPassword | String | 密码（明文）                                   |
  | sign.verifyCode   | String | 验证码                                      |
  | sign.uuid         | String | 标识客户端的唯一性，具体说明参考附件四                      |

> 输出参数

  | 参数           | 类型      | 说明                    |
  | ------------ | ------- | --------------------- |
  | token        | String  | 用户令牌，登录后所有操作均需要持有令牌   |
  | userId       | Long    | 用户唯一ID                |
  | userNickname | String  | 用户昵称                  |
  | userPhoneNo  | String  | 用户手机号                 |
  | userIcon     | String  | 用户头像url               |
  | identityAuth | Integer | 身份认证状态 1. 已认证  0. 未认证 |

  示例:

  ```json
  {
    "code": 0,
    "msg": "success",
    "data":
    {
      "token": "79FC19D3229B3494B6A9AE27DB1A136B0391110D",
      "userId": 19,
      "userNickname": "lalala",
      "userPhoneNo": "13800138000",
      "userIcon": "http://demo.heclouds.com:80/platform/file/download/2017071916510819",
      "identityAuth": 1,
      //"lastLoginTime": "2017-07-20 20:35:22",
      //"status": 1,
      //"appId": 1,
      //"appVersion": "1234",
      //"splashVersion": "123"
    }
  }
  ```

> 错误码

  | 错误码    | 错误说明      |
  | ------ | --------- |
  | -21001 | 手机号已注册    |
  | -21002 | 用户手机号格式错误 |
  | -21003 | 用户密码格式错误  |
  | -10007 | 验证码错误     |

### 获取注册验证码

> 接口描述

  用户注册，发送验证码，校验是否注册，未注册才发送


> 请求方式

  | 请求路径 | http://<HOST>:<PORT>/platform/user/register/verifyCode |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |


> 请求参数

  | 参数          | 类型     | 说明    |
  | ----------- | ------ | ----- |
  | userPhoneNo | String | 用户手机号 |


> 输出参数

  无

  示例:

  ```json
  {
    "code": 0,
    "msg": "success"
  }
  ```


> 错误码

  | 错误码    | 说明        |
  | ------ | --------- |
  | -21001 | 手机号已注册    |
  | -21002 | 用户手机号格式错误 |

###  登录

> 接口描述

  用户用手机号和密码登录


> 请求方式

  | 请求路径 | http://<HOST>:<PORT>/platform/user/login |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |


> 请求参数

  | 参数                 | 类型     | 参数说明                                     |
  | ------------------ | ------ | ---------------------------------------- |
  | login              | String | 注册信息经过RSA加密后生成的密文，原文为由以下字段的JSON串:{"userPhoneNo":"xxxx","userPassword":"xxxx","uuid":"xxx"} |
  | login.userPhoneNo  | String | 用户手机号                                    |
  | login.userPassword | String | 密码（明文）                                   |
  | login.uuid         | String | 标识客户端的唯一性，具体说明参考附件四                      |


> 输出参数

  | 参数           | 类型      | 说明                  |
  | ------------ | ------- | ------------------- |
  | token        | String  | 鉴权Token             |
  | userId       | Long    | 用户唯一ID              |
  | userNickname | String  | 用户昵称                |
  | userPhoneNo  | String  | 注册手机号               |
  | userIcon     | String  | 用户头像url             |
  | identityAuth | Integer | 身份认证状态 1.已认证  0.未认证 |

  示例:

  ```json
  {
    "code": 0,
    "msg": "success",
    "data": {
      "token": "EABB9F5EA4751AE80E9187AE27D92B087163E670",
      "userId": 12,
      "userPhoneNo": "18611169817",
      "userNickname":"pengfeiz",
      "userIcon":"http://172.19.3.157:7090/platform/file/download/2017072020324812",
      "identityAuth": 1,
      //"lastLoginTime": "2017-07-21 18:58:42",
      //"userType": "0",
      //"status": 1,
      //"appId": 1,
      //"appVersion": "1234",
      //"splashVersion": "123"
    }
  }
  ```


> 错误码

  | 错误码    | 说明       |
  | ------ | -------- |
  | -21007 | 账号不存在    |
  | -21008 | 用户名密码不匹配 |



### token登录

> 接口描述

用户用手机号和密码登录

> 请求方式

  | 请求路径 | http://<HOST>:<PORT>/platform/user/loginToken |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |


> Header参数

  | 参数    | 类型     | 说明      |
  | ----- | ------ | ------- |
  | token | String | 鉴权Token |

> 请求参数

  无


> 输出参数

  | 参数           | 类型      | 说明                      |
  | ------------ | ------- | ----------------------- |
  | token        | String  | 鉴权Token                 |
  | userId       | Long    | 用户唯一ID                  |
  | userNickname | String  | 用户昵称                    |
  | userPhoneNo  | String  | 注册手机号                   |
  | userIcon     | String  | 用户头像url                 |
  | identityAuth | Integer | 用户身份认证状态: 1. 已认证  0.未认证 |

  示例:

  ```json
  {
    "code": 0,
    "msg": "success",
    "data": {
      "token": "EABB9F5EA4751AE80E9187AE27D92B087163E670",
      "userId": 12,
      "userPhoneNo": "18611169817",
      "userNickname":"",
      "userIcon":"http://172.19.3.157:7090/platform/file/download/2017072020324812",
      "identityAuth": 1,
      //"lastLoginTime": "2017-07-21 18:58:42",
      //"userType": "0",
      //"status": 1,
      //"appId": 1,
      //"appVersion": "1234",
      //"splashVersion": "123"
    }
  }
  ```

> 错误码

  | 错误码    | 说明       |
  | ------ | -------- |
  | -10002 | token已失效 |

###  发送重置密码验证码

> 接口描述

  用户忘记密码，发送重置密码验证码


> 请求方式

  | 请求路径 | http://<HOST>:<PORT>/platform/user/password/verifyCode |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |

> 请求参数

  | 参数          | 类型     | 说明    |
  | ----------- | ------ | ----- |
  | userPhoneNo | String | 用户手机号 |

> 输出参数

  无

  示例:

  ```json
  {
    "code": 0,
    "msg": "success"
  }
  ```

> 错误码

  | 错误码    | 说明        |
  | ------ | --------- |
  | -21002 | 用户手机号格式错误 |
  | -21007 | 账号不存在     |

### 重置密码

> 接口描述


用户不记得旧密码通过验证码来重置密码

> 请求方式

  | 请求路径 | http://<HOST>:<PORT>/platform/user/password/reset |
  | ---- | ---------------------------------------- |
  | 请求方法 | PATCH                                    |


> 请求参数

  | 参数                    | 类型     | 参数说明                                     |
  | --------------------- | ------ | ---------------------------------------- |
  | reset                 | String | 注册信息经过RSA加密后生成的密文，原文为由以下字段的JSON串:{"userPhoneNo":"xxx","userNewPassword":"xxx","verifyCode":""} |
  | reset.userPhoneNo     | String | 用户手机号                                    |
  | reset.userNewPassword | String | 新密码（明文）                                  |
  | reset.verifyCode      | String | 验证码                                      |


> 输出参数

  无

  示例:

  ```json
  {
    "code": 0,
    "msg": "success"
  }
  ```

> 错误码

  | 错误码    | 说明        |
  | ------ | --------- |
  | -21007 | 账号不存在     |
  | -21002 | 用户手机号格式错误 |
  | -21003 | 用户密码格式错误  |
  | -10007 | 验证码错误     |

###  修改密码

> 接口描述

  用户记得旧密码，通过旧密码修改密码

  > 请求方式

  | 请求路径 | http://<HOST>:<PORT>/platform/user/password/modification |
  | ---- | ---------------------------------------- |
  | 请求方法 | PATCH                                    |

> Header参数

  | 参数    | 类型     | 说明      |
  | ----- | ------ | ------- |
  | token | String | 鉴权Token |

> 请求参数

  | 参数                    | 类型     | 参数说明                                     |
  | --------------------- | ------ | ---------------------------------------- |
  | modification          | String | 注册信息经过RSA加密后生成的密文，原文为由以下字段的JSON串:{"userOldPassword":"a132456","userNewPassword":"abcdefg1"} |
  | reset.userOldPassword | String | 旧密码（明文）                                  |
  | reset.userNewPassword | String | 新密码（明文）                                  |

> 输出参数

  无

  示例:

  ```json
  {  
    "code": 0,
    "msg": "success"
  }
  ```

> 错误码
  | 错误码    | 说明       |
  | ------ | -------- |
  | -21005 | 原密码格式错误  |
  | -21006 | 新密码格式错误  |
  | -21008 | 用户名密码不匹配 |



###  获取身份认证信息

> 接口描述

  获取用户的身份认证信息

> 请求方式

  | 请求路径 | http://<HOST>:<PORT>/platform/user/identityauthinfo |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |

> Header参数

  | 参数    | 类型     | 说明      |
  | ----- | ------ | ------- |
  | token | String | 鉴权Token |

> 请求参数

  无

> 输出参数

  | 参数       | 类型     | 说明   |
  | -------- | ------ | ---- |
  | cardName | String | 名字   |
  | cardNo   | String | 身份证号 |

  示例:

  ```json
  {
    "code": 0,
    "msg": "success",
    "data": {
      "cardName": "张三", 
      "cardNo": "440306200001010001"
    }
  }
  ```


> 错误码

  | 错误码    | 说明      |
  | ------ | ------- |
  | -21009 | 未进行身份认证 |

### 身份认证

> 接口描述


身份认证

> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/user/identityauthinfo |
| ---- | ---------------------------------------- |
| 请求方法 | PATCH                                    |

> Header参数


| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权Token |

> 请求参数


| 参数       | 类型     | 说明    |
| -------- | ------ | ----- |
| cardNo   | String | 身份证号  |
| cardName | String | 身份证名称 |

> 输出参数

无

示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

- ### 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -21010 | 已进行身份认证  |
| -21011 | 身份认证失败   |
| -21012 | 身份证号格式错误 |

 

###  修改账户头像

> 接口描述


上传修改app账户的头像

> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/user/icon |
| ---- | --------------------------------------- |
| 请求方法 | POST                                    |

> Header参数


| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权Token |

> 请求参数


| 参数       | 类型   | 说明                                       |
| -------- | ---- | ---------------------------------------- |
| userIcon | File | 头像文件（大小不超过100M，格式要求jpg,gif,png,ico,bmp,jpeg几种图片格式） |

> 输出参数

| 参数       | 类型     | 说明      |
| -------- | ------ | ------- |
| userIcon | String | 头像URL地址 |

示例:

```json
{
  "code": 0,
  "msg": "success",
  "data":{
    "userIcon":"http://xxxx.xx/xxx.jpg"
  }
}
```

>  错误码

| 错误码    | 说明      |
| ------ | ------- |
| -21013 | 图片格式不支持 |
| -21014 | 图片过大    |

 

###  修改账号信息

> 接口描述


修改昵称

> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/user/info |
| ---- | --------------------------------------- |
| 请求方法 | PATCH                                   |

> Header参数


| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权Token |

> 请求参数


| 参数           | 类型     | 说明           |
| ------------ | ------ | ------------ |
| userNickname | String | 昵称（可选）       |
| ...          |        | 其他可修改的信息（可选） |

> 输出参数

无

示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -21004 | 用户昵称格式错误 |

 

### 退出登录

> 接口描述

退出登录

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/user/logout |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权Token |

> 请求参数

无

> 输出参数

无

示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

> 错误码

无

### app版本更新

> 接口描述

通过系统类型和版本信息判断app是否需要升级

> 请求方式

| 请求路径     | http://<HOST>:<PORT>/platform/app/versionUpdate |
| -------- | ---------------------------------------- |
| **请求方法** | POST                                     |

> 请求参数

| 参数      | 类型      | 说明               |
| ------- | ------- | ---------------- |
| OS      | Integer | 系统类型  #0.安卓 1.苹果 |
| version | String  | 当前版本             |

> 输出参数

| 参数          | 类型      | 说明                 |
| ----------- | ------- | ------------------ |
| update      | Integer | 是否可以升级  #0.否 1.是   |
| forceUpdate | Integer | 是否需要强制升级  #0.否 1.是 |
| version     | String  | 版本号                |
| updateUrl   | String  | 文件路径               |
| updateLog   | String  | 更新日志               |

 示例:

```json
    安卓系统建议升级:
  {
    "msg": "请求成功",
    "code": 0,
    "data": {
      "update": 1,
      "forceUpdate": 0,
      "version": "1.0.1",
      "updateUrl": "www.android.app",
      "updateLog": "优化了Android系统兼容性"
    },
    "operateSuccess": 1
  }
  苹果系统强制升级:
  {
    "msg": "请求成功",
    "code": 0,
    "data": {
      "update": 1,
      "forceUpdate": 1,
      "version": "1.0.1",
      "updateUrl": "www.ios.app",
      "updateLog": "解决了IOS系统的延迟收讯问题"
    },
    "operateSuccess": 1
  }
```



## 设备模块

### 获取激活状态

> 接口描述


通过设备序列号判断设备是否激活

> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/device/activate |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数


| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权Token |

> 请求参数


| 参数       | 类型     | 说明    |
| -------- | ------ | ----- |
| deviceSn | String | 设备序列号(二选一) |
| deviceOnlyId | String | 设备OnlyID(二选一) |
```json
**说明**
	 设备sn号为1.0版本固有,现版本与sn有关流程维持原有不变.
	 设备OnlyId为2.0版本后新增属性,用户可对2.0版本的设备通过其DeviceOnlyId在设备离线的情况下进行激活绑定、解绑、同步通讯录等原有一切操作.
	 设备OnlyId为json格式的字符串,如下方所示,用户可通过扫描设备包装盒上的二维码获取．
	 {"deviceOnlyId":"101GEWJK7BC8BFC0"}
```

> 输出参数

| 参数            | 类型      | 说明                       |
| ------------- | ------- | ------------------------ |
| bind          | Integer | 设备绑定状态1.已绑定  0.未绑定       |
| deviceId      | String  | 设备ID                     |
| devicePhoneNo | String  | 设备手机号码                   |
| adminPhoneNo  | String  | 管理员手机号                   |
| deviceType    | Integer | 设备类型:0普通设备,1:行业设备,2:宠物设备 |

 示例:

```json
已激活:(服务器同时自动向管理员注册手机发送验证码)
{
  "code": 0,
  "msg": "success",
  "data": {
    "activate":1,
    "deviceId": "sfdsdw845sf",
    "adminPhoneNo":"1365498789"
  }
}

未激活:
{
  "code": 0,
  "msg": "success",
  "data": {
    //"devicePhoneNo": "13652467852",
    "deviceId": "sfdsdw845sf",
    "activate":0,
  }
}
```

> 错误码


| 错误码    | 说明                                       |
| ------ | ---------------------------------------- |
| -10003 | 设备不存在                                    |
| -10004 | 设备已离线                                    |
| -23001 | 设备已绑定                                    |
| -23015 | 设备未激活，请联系行业平台管理员激活                       |
| -23011 | 正在与设备同步，可重启设备后保持设备开机并位于网络较好的环境中，此过程可能需要几分钟。 |
| -23023 | 设备版本不支持当前操作 |
 

### 激活+绑定

> 接口描述


设备为未激活状态时, app调用此接口发送设备穿戴者信息和设备管理员通信信息来激活&绑定设备.

> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/device/activate |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数


| 参数           | 类型     | 说明                       |
| ------------ | ------ | ------------------------ |
| Content-Type | String | 请求类型，multipart/form-data |
| token        | String | 鉴权Token                  |

> 请求参数


| 参数              | 类型      | 说明                  |
| --------------- | ------- | ------------------- |
| deviceName      | String  | 设备名                 |
| deviceId        | String  | 设备信息表id             |
| deviceIcon      | File    | 设备头像(可选)            |
| devicePhoneNo   | String  | 手机号                 |
| devicePhoneType | Integer | 设备电话卡类型:0.普通卡 1.物联卡 |
| gender          | Integer | 性别：男:1,女:2(可选)      |
| birth           | String  | 生日(可选)              |
| height          | Integer | 身高, 单位cm(可选)        |
| weight          | Integer | 体重, 单位kg(可选)        |
| description     | String  | 详细描述(可选)            |
| contactIconNo   | Integer | 联系人头像序号             |
| contactName     | String  | 联系人姓名               |
| contactPhoneNo  | String  | 联系人手机号              |

> 输出参数

参考2.8 获取已绑定的设备列表接口中devices相关参数。

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "deviceId": "sdfas12314",
    "deviceName": "找TA二代2号",
    //"onenetId": "6876676",
    //"deviceKey": "HM9fbweWQ00NzRUsWDdgrPHJnB8=",
    "deviceSn": "002",
    "online": 1,
    //"contactPhoneNo": "18611169817",
    "devicePhoneNo": "13922893814",
    "devicePhoneType": 1,
    "deviceIcon": "http://172.19.3.157:7090/platform/file/download/2017072118594634",
    "deviceStep": 0,
    "userType": 0,
    "deviceVoiceType":0
  }
}
```

> 错误码


| 错误码    | 说明         |
| ------ | ---------- |
| -10003 | 设备不存在      |
| -10004 | 设备已离线      |
| -23002 | 设备已激活      |
| -23003 | 设备名称格式错误   |
| -23004 | 设备手机号格式错误  |
| -23005 | 联系人名称格式错误  |
| -23006 | 联系人手机号格式错误 |

 

### 发送绑定设备验证码

> 接口描述


发送绑定验证码到管理员手机号

- ### 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/device/activate/verifyCode |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> 请求参数


| 参数       | 类型     | 说明   |
| -------- | ------ | ---- |
| deviceId | String | 设备ID |

> 输出参数

无

示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

> 错误码


| 错误码    | 说明    |
| ------ | ----- |
| -10003 | 设备不存在 |

 

### 绑定验证

> 接口描述


通过验证码、设备id进行绑定验证

> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/device/activate/validation |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数


| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权Token |

> 请求参数


| 参数         | 类型     | 说明      |
| ---------- | ------ | ------- |
| verifyCode | String | 验证码     |
| deviceId   | String | 设备信息表id |

> 输出参数


无

示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

> 错误码


| 错误码    | 说明      |
| ------ | ------- |
| -10003 | 设备不存在   |
| -10004 | 设备已离线   |
| -10007 | 验证码错误   |
| -23001 | 已绑定过该设备 |

 

### 绑定设备

> 接口描述


设备为已激活状态时, app调用此接口发送用户通信信息来绑定设备.

> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/device/bind |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数


| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权Token |

> 请求参数


| 参数             | 类型      | 说明      |
| -------------- | ------- | ------- |
| deviceId       | String  | 设备id    |
| contactIconNo  | Integer | 联系人头像序号 |
| contactName    | String  | 联系人姓名   |
| contactPhoneNo | String  | 联系人手机号  |
| verifyCode     | String  | 验证码     |

> 输出参数


参考2.8 获取已绑定的设备列表接口中devices相关参数。

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "deviceId": "aiwnoahfoqw123",
    "deviceName": "找TA二代2号",
    //"onenetId": "6876676",
    //"deviceKey": "HM9fbweWQ00NzRUsWDdgrPHJnB8=",
    "deviceSn": "002",
    "online": 1,
    //"contactPhoneNo": "18611169817",
    "devicePhoneNo": "13922893814",
    "devicePhoneType":1,
    "deviceIcon": "http://172.19.3.157:7090/platform/file/download/2017072118594634",
    "deviceStep": 0,
    "userType": 0
  }
}
```

> 错误码


| 错误码    | 说明         |
| ------ | ---------- |
| -10003 | 设备不存在      |
| -10004 | 设备已离线      |
| -10007 | 验证码错误      |
| -23001 | 已绑定过该设备    |
| -23005 | 联系人名称格式错误  |
| -23006 | 联系人手机号格式错误 |

 

### 解绑设备

> 接口描述


删除设备通讯人与设备的绑定关系。

管理员点击解除绑定: 设备恢复出厂状态, 服务器推送消息给所有绑定的APP账户。

普通用户点击解除绑定: 用户从设备通讯中移除。

管理员删除其他用户, 需要指定联系人ID,并推送给对应的App账户,对应的APP账户和设备解除绑定关系(推送消息给APP账户, 让APP账户和设备解绑)。

> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/device/unbind |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权Token |

> 请求参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权Token |

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权Token |

| 参数        | 类型     | 说明        |
| --------- | ------ | --------- |
| deviceId  | String | 设备id      |
| contactId | Long   | 联系人ID(可选) |

> 输出参数


无

示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

> 错误码


| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备已离线    |
| -10006 | 设备权限验证失败 |
| -23008 | 联系人不存在   |

  

### 恢复出厂设置

> 接口描述


只有管理员才能进行操作。

若进行此项操作，清空设备的所有数据，解除和该设备的所有绑定，推送给所有与该设备绑定的用户。

设备恢复出厂设置，当再次绑定的第一个用户将成为管理员。

> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/device/reset |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数


| 参数    | 类型     | 说明   |
| ----- | ------ | ---- |
| token | String |      |

> 请求参数


| 参数       | 类型     | 说明   |
| -------- | ------ | ---- |
| deviceId | String | 设备ID |

> 输出参数


无

示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备已离线    |
| -10006 | 设备权限验证失败 |

 

###  获取已绑定的设备列表

> 接口描述


获取当前用户绑定的所有设备列表

> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/device |
| ---- | ------------------------------------ |
| 请求方法 | GET                                  |

> Header参数


| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权Token |

> 输出参数


| 参数                      | 类型      | 说明                                       |
| ----------------------- | ------- | ---------------------------------------- |
| devices                 | Array   | 用户账号绑定的设备信息                              |
| devices.deviceId        | String  | 设备ID                                     |
| devices.deviceName      | String  | 设备名称                                     |
| devices.deviceKey       | String  | oneNET设备APIKEY                           |
| devices.deviceSn        | String  | 设备SN                                     |
| devices.online          | Integer | 设备在线状态 1. 在线  0.离线                       |
| devices.contactPhoneNo  | String  | 设备管理员的通讯录手机号(可以不要)                       |
| devices.devicePhoneNo   | String  | 设备手机号                                    |
| devices.deviceCardType | Integer | 设备电话卡类型:0.普通卡 1.物联卡                      |
| devices.deviceIcon      | String  | 设备头像URL                                  |
| devices.deviceStep      | Integer | 设备当前步数                                   |
| devices.userType        | Integer | 用户类型:0-管理员   1-普通用户(有APP账号)    2-普通用户(无账号) |
| userId                  | integer | 用户ID                                     |
| deviceType              | Integer | 设备类型:0普通设备,1:行业设备,2:宠物设备                 |
| deviceVoiceType         | Integer | 设备语音类型(打电话:0  语音对讲:1)                     |

示例:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    {
    "deviceId": "sfosodjfow231",
    "deviceName": "找TA二代2号",
    //"onenetId": "6876676",
    //"deviceKey": "HM9fbweWQ00NzRUsWDdgrPHJnB8=",
    "deviceSn": "002",
    "online": 1,
    //"contactPhoneNo": "18611169817",
    "devicePhoneNo": "13922893814",
    "deviceCardType":1,
    "deviceIcon": "http://172.19.3.157:7090/platform/file/download/2017072118594634",
    "deviceStep": 0,
    "userType": 0
  }   
}
}
```

> 错误码


无

 

###  获取设备详细信息(可用于轮询)

> 接口描述


获取当前用户绑定的某一个设备详情信息

> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/device/detail |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权Token |

> 请求参数

| 参数       | 类型           | 说明                                       |
| -------- | ------------ | ---------------------------------------- |
| deviceId | String       | 设备ID                                     |
| filter   | String Array | 指定要获取的设备信息(可选)<br>目前定义的设备信息包括:<br>1.basic:设备基本信息，包括设备id和设备用户信息等<br>2. locations:位置信息<br>3.battery:电量信息<br>例：<br>只获取基本信息: "filter":["basic"]<br>获取基本信息和位置信息:"filter":["basic","locations"]<br>特别地，如果参数为空或者不携带本参数，则表示获取所有信息 |

> 输出参数

| 参数                   | 类型      | 说明                |
| -------------------- | ------- | ----------------- |
| gender               | Integer | 性别：男:1,女:2(可选)    |
| birth                | String  | 生日(可选)            |
| height               | Integer | 身高(可选)            |
| weight               | Integer | 体重(可选)            |
| deviceVoiceSwitch             | Integer | 语音开关            |
| description          | String  | 详细描述(可选)          |
| locations            | Object  | 当前位置信息            |
| locations.time       | Long    | 时间, 单位ms          |
| locations.lon        | Double  | 经度                |
| locations.lat        | Double  | 纬度                |
| locations.precision  | Integer | 定位精度,单位米          |
| locations.address    | String  | 定位地址              |
| locations.locateType | Integer | 定位方式(0,1,2 待定)    |
| battery              | Object  | 当前电池信息            |
| battery.status       | Integer | 电池状态 (0正常 1正在充电 ) |
| battery.energy       | Integer | 电池电量 0-100        |

其他参数2.8 获取已绑定的设备列表接口中devices相关参数。

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "deviceId": "sfaoiwfo9013",
    "deviceName": "找TA二代2号",
    //"onenetId": "6876676",
    //"deviceKey": "HM9fbweWQ00NzRUsWDdgrPHJnB8=",
    "deviceSn": "002",
    "online": 1,
    //"contactPhoneNo": "18611169817",
    "devicePhoneNo": "13922893814",
    "devicePhoneType": 1,
    "deviceIcon": "http://172.19.3.157:7090/platform/file/download/2017072118594634",
    "deviceStep": 0,
    "userType": 0,
    "gender": 1,
    "birth": "2003-03-26",
    "height": 30,
    "weight": 140,
    "description": "xxxxx",
    "locations": {
      "time": 35141148945,
      "lon": 29.55,
      "lat": 29.57,
      "precision": 100,
      "address": "广东省",
      "locateType": 0
    },
    "battery": {
      "status": 1,
      "energy": 90
    }
  }
}
```

> 错误码


| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备已离线    |
| -10006 | 设备权限验证失败 |

 

### 修改设备头像

> 接口描述


上传修改设备持有人的头像

> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/device/icon |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数


| 参数           | 类型     | 说明                       |
| ------------ | ------ | ------------------------ |
| Content-Type | String | 请求类型，multipart/form-data |
| token        | String | 鉴权Token                  |

> 请求参数


| 参数       | 类型     | 说明                                       |
| -------- | ------ | ---------------------------------------- |
| icon     | File   | 头像文件（大小不超过100M，格式要求jpg,gif,png,ico,bmp,jpeg几种图片格式） |
| deviceId | String | 所修改设备的id                                 |

> 输出参数


无

示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

> 错误码


| 错误码    | 说明       |
| ------ | -------- |
| -21013 | 图片格式不支持  |
| -21014 | 图片过大     |
| -10003 | 设备不存在    |
| -10006 | 设备权限验证失败 |

 

###  修改设备信息(不包含设备头像)

> 接口描述


修改设备持有人的信息（非头像）

> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/device |
| ---- | ------------------------------------ |
| 请求方法 | PATCH                                |

> Header参数


| 参数           | 类型     | 说明                         |
| ------------ | ------ | -------------------------- |
| Content-Type | String | 请求类型，目前只支持application/json |
| token        | String | 鉴权Token                    |

> 请求参数


| 参数            | 类型      | 说明    |
| ------------- | ------- | ----- |
| deviceName    | String  | 设备名称  |
| devicePhoneNo | String  | 设备手机号 |
| gender        | Integer | 性别    |
| birth         | String  | 生日    |
| height        | Integer | 身高    |
| weight        | Integer | 体重    |
| deviceId      | String  | 设备id  |
| description   | String  | 详情    |
|               |         |       |

> 输出参数


| 参数              | 类型      | 说明                                       |
| --------------- | ------- | ---------------------------------------- |
| deviceCardType | Integer | 设备电话卡类型:0.普通卡 1.物联卡(可选,如果修改了手机号，则须返回此参数) |
| deviceVoiceType | Integer | 设备语音类型(不保留:0  保留:1)                      |

示例:

```json
{
  "code": 0,
  "msg": "success",
  "data": 
  {
    "devicePhoneType":1
  }
}
```

> 错误码

| 错误码    | 说明        |
| ------ | --------- |
| -10003 | 设备不存在     |
| -10006 | 设备权限验证失败  |
| -23003 | 设备名称格式错误  |
| -23004 | 设备手机号格式错误 |

 

### 行业平台设备通讯录审批

> 接口描述

  通讯录审批。

> 请求方式

  | 请求路径 | http://<HOST>:<PORT>/portal/indDevBing/approval |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

> Header参数

  | 参数    | 类型     | 参数说明    |
  | ----- | ------ | ------- |
  | token | String | 鉴权token |

> 请求参数

  | 参数             | 类型      | 参数说明  |
  | -------------- | ------- | ----- |
  | deviceId       | String  | 设备编号  |
  | contactName    | String  | 联系人名称 |
  | contactPhoneNo | String  | 联系人电话 |
  | contactIconNo  | Integer | 用户的头像 |

> 输出参数

  无

  示例:

  ```json
  {
    "code": 0,
    "msg": "success"
  }
  ```

> 错误码

  | 错误码    | 说明            |
  | ------ | ------------- |
  | -23007 | 该手机号码已绑定通讯录   |
  | -21020 | 审批记录已存在,请耐心等待 |

###  获取设备通讯录

> 接口描述


获取设备通讯录中所有成员信息。

(普通用户界面: 当前账号的在第一条,管理员在第二条)

(管理员用户: 管理员在第一条)

> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/device/contacts |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数


| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数


| 参数       | 类型     | 说明   |
| -------- | ------ | ---- |
| deviceId | String | 设备ID |

> 输出参数

| 参数                      | 类型      | 说明      |
| ----------------------- | ------- | ------- |
| contacts                | Array   | 联系人列表   |
| contacts.contactId      | Long    | 联系人ID   |
| contacts.userType       | Integer | 用户类型    |
| contacts.contactIconNo  | Integer | 联系人头像序号 |
| contacts.contactName    | String  | 联系人姓名   |
| contacts.contactPhoneNo | String  | 联系人手机号  |
| contacts.userId         | Long    | 用户id    |
| contacts.telState         | Integer    | 通讯录与设备同步状态(1：新增设备联系人2：通知设备有新增联系人3：设备已尝试下载语音文件4：语音文件下载成功)|

 示例:

```json
{
  "code": 200,
  "msg": "",
  "data": {
    "contacts": [
      {
        "contactId": 1,
        "userType": 1,
        "contactPhoneNo": "110",
        "userId": 1,
        "contactName": "小明",
        "contactIconNo": 1,
        "telState" :1,
        //"contactRelation": "",
        //"createTime": "",
        //"updateTime": "",
        //"isReg": "",
        //"deviceId": 1
      },
      {
        "contactId": 2,
        "userType": 1,
        "contactPhoneNo": "112",
        "userId": 2,
        "telState" :1,
        "contactName": "小wang",
        "contactIconNo": 1,
        //"contactRelation": "",
        //"createTime": "",
        //"updateTime": "",
        //"isReg": "",
        //"deviceId": 1
      }
    ]
  }
}
```

>  错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备已离线    |
| -10006 | 设备权限验证失败 |

 

###  修改通讯录中用户信息

> 接口描述


管理员可以修改所有成员在通讯中的信息。

普通用户只能修改自己的信息。

> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/device/contacts |
| ---- | ---------------------------------------- |
| 请求方法 | PATCH                                    |

> Header参数


| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数


| 参数             | 类型      | 说明      |
| -------------- | ------- | ------- |
| contactIconNo  | Integer | 联系人头像序号 |
| contactName    | String  | 联系人姓名   |
| contactPhoneNo | String  | 联系人手机号  |
| deviceId       | String  | 设备ID    |
| contactId      | Long    | 账号ID    |

> 输出参数


无

示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

> 错误码

| 错误码    | 说明         |
| ------ | ---------- |
| -10003 | 设备不存在      |
| -10004 | 设备已离线      |
| -10006 | 设备权限验证失败   |
| -23005 | 联系人名称格式错误  |
| -23006 | 联系人手机号格式错误 |
| -23007 | 联系人手机号重复   |

 

### 添加通讯录成员

> 接口描述


只有管理员才能主动添加新通讯录成员。

当有用户为未在app端注册，但是又想加入手表通讯录与定位器设备互相打电话，则可以通过这种方式。

> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/device/contacts |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数


| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数


| 参数             | 类型     | 说明   |
| -------------- | ------ | ---- |
| contactName    | String | 姓名   |
| contactPhoneNo | String | 手机   |
| deviceId       | String | 设备ID |

> 输出参数


| 参数        | 类型   | 说明    |
| --------- | ---- | ----- |
| contactId | Long | 联系人ID |
| telState | Integer | 通讯录与设备同步状态(1：新增设备联系人2：通知设备有新增联系人3：设备已尝试下载语音文件4：语音文件下载成功) |
示例:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "contactId": 655,
    "telState":1
  }
}
```

> 错误码

| 错误码    | 说明         |
| ------ | ---------- |
| -10003 | 设备不存在      |
| -10004 | 设备已离线      |
| -10006 | 设备权限验证失败   |
| -23005 | 联系人名称格式错误  |
| -23006 | 联系人手机号格式错误 |
| -23007 | 联系人手机号重复   |



### 获取设备设置

> 接口描述

获取设备设置。

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/settings |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

说明:

settingsName为要获取的设置项名称，如果不传settingsName表示获取所有设置项。

设置项名称参考输出参数。

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数       | 类型           | 说明                                       |
| -------- | ------------ | ---------------------------------------- |
| deviceId | String       | 设备ID                                     |
| filter   | String Array | 指定要获取的设备设置项(可选)<br>目前定义的设备信息包括:<br>locateMode:定位模式设置<br>autoAnswer:自动应答设置<br>whiteList:白名单状态设置<br>deviceVoiceSwitch:语音开关设置(0-关闭  1-开启)置<br>autoSleep:自动休眠设置<br>例：<br>只获取定位模式设置: "filter":["locateMode"]<br>获取自动应答设置和白名单状态设置:"filter":["autoAnswer","whiteList"]<br>特别地，如果参数为空或者不携带本参数，则表示获取所有设置项 |

> 输出参数

| 参数                | 类型      | 说明                                |
| ----------------- | ------- | --------------------------------- |
| locateMode        | Integer | 定位模式( 0:待机模式,4:智能模式, 2:追踪模式)(可选)  |
| autoAnswer        | Integer | 自动应答设置(0-关闭  1-开启)(可选)            |
| whiteList         | Integer | 白名单状态设置(0-关闭  1-开启)(可选)           |
| autoSleep         | Object  | 自动休眠设置(可选)                        |
| autoSleep.enabled | Integer | 开启状态(0-关闭  1-开启)                  |
| autoSleep.start   | Integer | 休眠开始时间(距离当天00:00的秒数, 如3600表示1:00) |
| autoSleep.end     | Integer | 休眠结束时间(距离当天00:00的秒数, 如3600表示1:00) |
| deviceVoiceSwitch | Integer | 语音开关设置(0-关闭  1-开启)                |

 示例:

```json
{
  "code": 0,
  "msg": "success",
  "data":
  {
    "locateMode":3,
    "autoAnswer":1,
    "whiteList":0,
    "autoSleep":
    {
      "enabled":1,
      "start":3600,
      "end":7200
    }
    "deviceVoiceSwitch":0
  }
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10006 | 设备权限验证失败 |

###  修改设备设置项

> 接口描述

修改设备设置项

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/settings |
| ---- | ---------------------------------------- |
| 请求方法 | PATCH                                    |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数                | 类型      | 说明                                       |
| ----------------- | ------- | ---------------------------------------- |
| deviceId          | String  | 设备ID                                     |
| locateMode        | Integer | 定位模式(-1:关闭追踪模式 0:待机模式,4:智能模式, 2:追踪模式)(可选) |
| autoAnswer        | Integer | 自动应答设置(0-关闭  1-开启)(可选)                   |
| whiteList         | Integer | 白名单状态设置(0-关闭  1-开启)(可选)                  |
| autoSleep         | Object  | 自动休眠设置(可选)                               |
| autoSleep.enabled | Integer | 开启状态(0-关闭  1-开启)                         |
| autoSleep.start   | Integer | 休眠开始时间(距离当天00:00的秒数, 如3600表示1:00)        |
| autoSleep.end     | Integer | 休眠结束时间(距离当天00:00的秒数, 如3600表示1:00)        |
| deviceVoiceSwitch | Integer | 语音开关设置(0-关闭  1-开启)                       |

> 该接口推荐一次只设置一个设置项，如果设置多个设置项，请求时间会很长。

> 输出参数

无

 示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备已离线    |
| -10006 | 设备权限验证失败 |

### 唤醒设备

> 接口描述

尝试唤醒设备，不保证设备能唤醒成功。

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/wakeup |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数        | 类型          | 说明                                       |
| --------- | ----------- | ---------------------------------------- |
| deviceIds | StringArray | 设备ID数组，如果为空则表示唤醒当前账号绑定的所有设备。<br>例如:唤醒一台设备["xxxx"], 唤醒多台设备[“xxx1”,"xxx2"], 唤醒所有设备[]。 |

> 输出参数

无

 示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10006 | 设备权限验证失败 |



### 获取设备升级信息

> 接口描述

获取设备升级信息。

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/update |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数       | 类型     | 说明   |
| -------- | ------ | ---- |
| deviceId | String | 设备ID |

> 输出参数

| 参数     | 类型      | 说明                        |
| ------ | ------- | ------------------------- |
| update | Integer | 是否有新版本 1. 有更新  0.当前已是最新版本 |
| swVer  | String  | 最新软件版本（可选, 有更新时返回）        |
| hwVer  | String  | 硬件版本（可选, 有更新时返回）          |
| url    | String  | 固件下载地址（可选, 有更新时返回）        |
| log    | String  | 固件升级信息（可选, 有更新时返回）        |
| size   | Integer | 固件大小（可选, 有更新时返回）          |

 示例:

```json
{
  "code": 0,
  "msg": "success",
  "data":
  {
    "update":1,
    "swVer":"CMCC-DST1A_V0.7_20170925_A",
    "hwVer":"xxxxx",
    "url":"http://csdfslkdfsfd.com/xxxx/xxx",
    "log":"1.修复了xxxx",
    "size":1354656
  }
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10006 | 设备权限验证失败 |
| -23023        | 设备版本不支持当前操作        |
### 获取设备升级信息_批量获取

> 接口描述

获取设备升级信息。

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/updates |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数       | 类型     | 说明   |
| -------- | ------ | ---- |
| deviceIds | String[] | 设备ID数组 |

> 输出参数

| 参数     | 类型      | 说明                        |
| ------ | ------- | ------------------------- |
| update | Integer | 是否有新版本 2.正在升级中 1. 有更新  0.当前已是最新版本 |
| deviceId | String | 设备id |
| swVer  | String  | 最新软件版本（可选, 有更新时返回）        |
| hwVer  | String  | 硬件版本（可选, 有更新时返回）          |
| url    | String  | 固件下载地址（可选, 有更新时返回）        |
| log    | String  | 固件升级信息（可选, 有更新时返回）        |
| size   | Integer | 固件大小（可选, 有更新时返回）          |

 示例:

```json
{
  "code": 0,
  "msg": "success",
  "data":
  
[
  {
    "deviceId":"123456",
    "update":1,
    "swVer":"CMCC-DST1A_V0.7_20170925_A",
    "hwVer":"xxxxx",
    "url":"http://csdfslkdfsfd.com/xxxx/xxx",
    "log":"1.修复了xxxx",
    "size":1354656
  },
  {
    "deviceId":"123456",
    "update":1,
    "swVer":"CMCC-DST1A_V0.7_20170925_A",
    "hwVer":"xxxxx",
    "url":"http://csdfslkdfsfd.com/xxxx/xxx",
    "log":"1.修复了xxxx",
    "size":1354656
  }
  ]
  
}
```
### 固件升级

> 接口描述

通知设备升级固件

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/update |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数       | 类型     | 说明   |
| -------- | ------ | ---- |
| deviceId | String | 设备ID |

> 输出参数

无

 示例:

```json
{
  "code": 0,
  "msg": "success",
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备已离线    |
| -10006 | 设备权限验证失败 |
| -23016 | 电量不足     |



### 获取设备升级进度

> 接口描述

获取设备fota升级进度

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/updateRate |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数       | 类型     | 说明   |
| -------- | ------ | ---- |
| deviceId | String | 设备ID |

> 输出参数

| 参数          | 类型      | 说明      |
| ----------- | ------- | ------- |
| deviceId    | String  | 设备ID    |
| fotaPercent | Integer | 升级进度(%) |

示例:

```json
{
    "msg": "请求成功",
    "code": 0,
    "data": {
      "deviceId": 123456789,
      "fotaPercent": 20
    },
    "operateSuccess": 1
  }
```



### 获取宠物设备灯设置

> 接口描述

获取宠物设备灯设置

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/pet/light |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数       | 类型     | 说明   |
| -------- | ------ | ---- |
| deviceId | String | 设备ID |

> 输出参数

| 参数         | 类型      | 说明                  |
| ---------- | ------- | ------------------- |
| lightState | Integer | 灯光开关状态:0关闭,1开启      |
| lightType  | Integer | 灯光类型:0:闪烁,1.长亮      |
| lightColor | Integer | 0-红，1-绿，2-蓝，3-白，4-彩 |
| deviceId   | String  | 设备id                |

 示例:

```json
{
  "code": 0,
  "msg": "success",
  "data":
  {
    "lightState":3,
    "lightType":1,
    "lightColor":0
    
  }
}
```



错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备已离线    |
| -10006 | 设备权限验证失败 |
| -23011 | 电量不足     |

### 修改宠物设备灯设置
> 接口描述

修改宠物设备灯设置

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/pet/light |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数         | 类型      | 说明                  |
| ---------- | ------- | ------------------- |
| deviceId   | String  | 设备ID                |
| lightState | Integer | 灯光开关状态:0关闭,1开启      |
| lightType  | Integer | 灯光类型:0:闪烁,1.长亮      |
| lightColor | Integer | 0-红，1-绿，2-蓝，3-白，4-彩 |

> 输出参数

 示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备已离线    |
| -10006 | 设备权限验证失败 |
| -23011 | 电量不足     |



### 获取设备卡信息

> 接口描述

获取设备卡余额,当月GPRS使用量等

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/simCard/querySimCardInfo |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数        | 类型      | 说明                     |
| --------- | ------- | ---------------------- |
| card      | String  | 卡号                     |
| queryCode | Integer | 查询参数(1:余额 2:GPRS 3:全部) |



> 输出参数

| 参数             | 类型    | 说明               |
| -------------- | ----- | ---------------- |
| balance        | Float | 余额               |
| gprsTotal      | Float | GPRS套餐总流量(单位:KB) |
| inGprsFlow     | Float | GPRS套餐内流量(单位:KB) |
| exceedGprsFlow | Float | GPRS套餐外流量(单位:KB) |
| gprsLeft       | Float | GPRS套餐内剩余(单位:KB) |

示例:

```json
{
    "msg": "请求成功",
    "code": 0,
    "data": {
        "balance": -6,
        "exceedGprsFlow": 0,
        "gprsTotal": 102400,
        "gprsLeft": 102331,
        "inGprsFlow": 69
    },
    "operateSuccess": 1
}
```



错误码

| 错误码    | 说明           |
| ------ | ------------ |
| -23019 | sim卡同步失败     |
| -23020 | sim卡查询余额失败   |
| -23021 | sim卡查询GPRS失败 |

### 获取设备计步信息
> 接口描述


设备计步统计

> 请求方式


| 请求路径 |http://<HOST>:<PORT>/platform/device/stepCount |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数


| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| companyToken或token| String | 鉴权token |

> 请求参数


| 参数       | 类型     | 说明   |
| -------- | ------ | ---- |
| deviceId | String | 设备ID |
| year | String | 查询的年份(必传) |
| month | String | 查询的月份(查询天的步数必须传月份) |
| day | String | 查询的天(查询小时的步数必须传月份,天份) |

> 输出参数

无

 示例:

```json
{
    "data": [
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        18,
        7
    ],
    "code": 0,
    "msg": "请求成功",
    "operateSuccess": true
}
或
{
    "data": [
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        11,
        7,
        0,
        0,
        0,
        0
    ],
    "code": 0,
    "msg": "请求成功",
    "operateSuccess": true
}
```

>  错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备已离线    |
| -10006 | 设备权限验证失败 |

 ### 获取设备SOS告警录音

> 接口描述

获取设备SOS告警录音

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/SOSsoundRecord |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数       | 类型     | 说明   |
| -------- | ------ | ---- |
| deviceId | String | 设备ID |
| alarmId | Integer | 设备告警id |

> 输出参数

| 参数         | 类型      | 说明                  |
| ---------- | ------- | ------------------- |
| deviceId | String | 设备ID |
| voiceUrl  | String | 录音下载地址     |


 示例:

```json
{
  "code": 0,
  "msg": "success",
  "data":
  {
    "deviceId":"123456",
    "voiceUrl":"",
    
    
  }
}
```
###  获取socket长连接状态
> 接口描述

获取app和设备socket长连接状态，如果设备socket连接断开，则发送命令通知设备重新连接socket

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/keepDeviceVoice |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                      |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数       | 类型     | 说明   |
| -------- | ------ | ---- |
| deviceId | String | 设备ID |

 示例:

```json
{
  "code": 0,
  "msg": "success",
   "data":
   {
     "appSocketStatus":1, #0:app长连接断开 1：app长连接在线 
     "deviceSocketStatus":1 #0:设备长连接断开 1:设备长连接在线
  }
}
```
---

## 设备透传

### 透传接口

> 接口描述

透传接口, 服务器直接转发参数中data中携带的数据到设备，并将设备的响应返回。

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/passthrough |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数       | 类型      | 说明                           |
| -------- | ------- | ---------------------------- |
| deviceId | String  | 设备ID                         |
| qos      | Integer | 是否需要设备响应(1.需要 0不需要, 可选,默认为1) |
| timeout  | Integer | 设备响应超时时间, ms(可选, 默认2000ms)   |
| data     | Object  | 透传数据                         |

示例:

```json
{
  "deviceId": 1231, 
  "qos":0, 
  "timeout": 1000, 
  "data":
  {
    "status":1 , 
    "startTime":0, 
    "endTime":3600
  }
}
```

> 输出参数

| 参数       | 类型     | 说明      |
| -------- | ------ | ------- |
| response | Object | 设备响应的数据 |

示例:

```json
{
  "code": 0, 
  "msg":"success",
  "data":
  {
    "status":1 , 
    "startTime":0, 
    "endTime":3600
  }
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备已离线    |
| -10005 | 设备请求超时   |
| -10006 | 设备权限验证失败 |

 

---

## 定位模块

### 获取设备最新定位信息

> 接口描述

查询用户绑定的所有设备的定位信息。

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/locations |
| :--- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数

| 参数           | 类型     | 说明                                       |
| ------------ | ------ | ---------------------------------------- |
| token        | String | 鉴权token                                  |
| interVersion | String | app版本号(可选),不传默认1.0版本.提示:**如果传入此参数,必须传入deviceIds** |

> 请求参数

| 参数        | 类型    | 说明                                       |
| --------- | ----- | ---------------------------------------- |
| deviceIds | Array | ["deviceId1","deviceId2"], 要获取的设备信息, 可选,默认返回该用户已绑定的所有设备定位信息。**此响应需要时间较长,建议一次传入一个deviceId** |

> 输出参数

| 参数                   | 类型      | 说明                            |
| -------------------- | ------- | ----------------------------- |
| deviceId             | String  | 设备ID                          |
| locations            | Object  | 设备位置信息                        |
| locations.time       | Long    | 时间戳，单位ms                      |
| locations.lon        | Double  | 经度                            |
| locations.lat        | Double  | 纬度                            |
| locations.precision  | Integer | 定位精度,单位米                      |
| locations.address    | String  | 定位地址                          |
| locations.locateType | Integer | 定位方式(1-GPS 2-LBS 3-WIFI 4-BT) |

```json
{
  "code": 0,
  "msg": "success",
  "data": [
    {
      "deviceId": "ssss12",
      "locations": {
        "time": 35124562456,
        "lon": 29.55,
        "lat": 29.57,
        "precision": 100,
        "address": "广东省",
        "locateType": 0
      }
    },
    {
      "deviceId": "ssss13",
      "locations": {
        "time": 35123579564,
        "lon": 29.55,
        "lat": 29.57,
        "precision": 100,
        "address": "广东省",
        "locateType": 0
      }
    }
  ]
}
```

> 错误码

无



### 获取设备历史定位轨迹

> 接口描述

获取设备指间段的历史轨迹。

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/locations/history |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数       | 类型     | 说明                 |
| -------- | ------ | ------------------ |
| deviceId | String | 设备id               |
| date     | String | 查询日期，格式：yyyy-MM-dd |

> 输出参数

| 参数                   | 类型      | 说明                            |
| -------------------- | ------- | ----------------------------- |
| locations            | Array   | 设备位置信息数组                      |
| locations.beginTime  | Long    | 定位起始时间，单位ms                   |
| locations.endTime    | Long    | 定位结束时间，单位ms                   |
| locations.lon        | Double  | 经度                            |
| locations.lat        | Double  | 纬度                            |
| locations.precision  | Integer | 定位精度,单位米                      |
| locations.address    | String  | 定位地址                          |
| locations.locateType | Integer | 定位方式(1-GPS 2-LBS 3-WIFI 4-BT) |

示例:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "locations":[ 
      {
        "beginTime": 3516461846,
        "endTime": 3516481846,
        "deviceId": 100,
        "lon": 29.55,
        "lat": 29.57,
        "precision": 100,
        "address": "广东省",
        "locateType": 0
      },
      {
        "beginTime": 3516461846,
        "endTime": 3516481846,
        "lon": 29.55,
        "lat": 29.57,
        "precision": 100,
        "address": "广东省",
        "locateType": 0
      }
    ]
  }
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10006 | 设备权限验证失败 |



##  安全区域模块

### 新增安全区域

> 接口描述

对设备增加安全区域或修改已有的安全区域。

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/safeRegion |
| ---- | ---------------------------------------- |
| 请求方法 | PUT                                      |

> Header参数

| 参数    | 类型     | 说明   |
| ----- | ------ | ---- |
| token | String | 鉴权   |

> 请求参数

| 参数             | 类型       | 说明                    |
| -------------- | -------- | --------------------- |
| deviceId       | String   | 设备ID                  |
| lon            | Double   | 经度                    |
| lat            | Double   | 纬度                    |
| radius         | Interger | 半径, 单位米               |
| name           | String   | 安全区域名称                |
| address        | String   | 安全区域地址                |
| alertCondition | Interger | 告警条件 1.进入2.离开 3.进入或离开 |
| icon           | Interger | 图标ID                  |

> 输出参数

| 参数   | 类型   | 说明     |
| ---- | ---- | ------ |
| id   | Long | 安全区域id |

```json
{
  "code": 0,
  "msg": "success",
  "data":
  {
    "id":100
  }
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10006 | 设备权限验证失败 |

 

###  删除安全区域

> 接口描述

对设备增加安全区域或修改已有的安全区域。

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/safeRegion |
| ---- | ---------------------------------------- |
| 请求方法 | DELETE                                   |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数       | 类型     | 说明     |
| -------- | ------ | ------ |
| id       | Long   | 安全区域id |
| deviceId | String | 设备id   |

> 输出参数

无

示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10006 | 设备权限验证失败 |

 

### 修改安全区域

> 接口描述

对设备修改已有的安全区域。

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/safeRegion |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数             | 类型      | 说明                    |
| -------------- | ------- | --------------------- |
| deviceId       | String  | 设备ID                  |
| id             | Long    | 安全区域ID                |
| lon            | Double  | 经度                    |
| lat            | Double  | 纬度                    |
| radius         | Integer | 半径, 单位米               |
| name           | String  | 安全区域名称                |
| address        | String  | 安全区域地址                |
| alertCondition | Integer | 告警条件 1.进入2.离开 3.进入或离开 |
| icon           | Integer | 图标ID                  |

> 输出参数

无

示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10006 | 设备权限验证失败 |
| -31001 | 安全区域不存在  |

 

### 查询安全区域

> 接口描述

查询指定设备的安全区域列表。

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/safeRegion |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数       | 类型     | 说明   |
| -------- | ------ | ---- |
| deviceId | String | 设备id |

> 输出参数

输出参数说明

| 参数             | 类型      | 说明                    |
| -------------- | ------- | --------------------- |
| id             | Long    | 安全区域id                |
| lon            | Double  | 经度                    |
| lat            | Double  | 纬度                    |
| radius         | Integer | 半径, 单位米               |
| name           | String  | 安全区域名称                |
| address        | String  | 安全区域地址                |
| alertCondition | Integer | 告警条件 1.进入2.离开 3.进入或离开 |
| icon           | Integer | 图标ID                  |
| description    | String  | 安全描述(待定)              |

 示例:

```json
{
  "code": 0,
  "msg": "success",
  "data":
  {
    "safeRegion": 
    [
      {
        "id":1,
        "lon":123.05,
        "lat":123.05,
        "radius":50.2,
        "name":"name1",
        "address":
        "address1",
        "alertCondition":1,
        "icon":1,
        "description":"xxxxx"
      },
      {
        "id":2,
        "lon":123.05,
        "lat":123.05,
        "radius":50.2,
        "name":"name1",
        "address":
        "address1",
        "alertCondition":1,
        "icon":1,
        "description":"xxxxx"
      }
    ]
  }
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10006 | 设备权限验证失败 |



---

## 记录模块

### 告警记录列表

> 接口描述

定位器一键sos和定位器离开电子围栏发出告警，客户端会接收到推送，服务器记录sos消息和记录离开电子围栏告警信息，客户端再从服务器获取告警记录.

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/records/alarms |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数         | 类型      | 说明                                      |
| ---------- | ------- | --------------------------------------- |
| startId    | Long    | 起始记录id(可选, 默认为0,表示从最新或最老的数据开始)          |
| count      | Integer | 最大查询数量(可选,默认为20)                        |
| direction  | Integer | 查询方向(0倒序, 1正序 )(可选,默认为0倒序)              |
| deviceId   | String  | 设备id                                    |
| alarmTypes | Array   | 要获取的告警类型,例: alarmTypes=1,2 (可选，默认为获取所有) |

> 输出参数

输出参数说明

| 参数             | 类型      | 说明                                       |
| -------------- | ------- | ---------------------------------------- |
| records        | Array   | 告警记录数组                                   |
| id             | Long    | 记录id                                     |
| alarmType      | Integer | 告警类型:1-sos告警  2-电子围栏告警 3-低电量告警 4-SIM卡变更告警（只有管理员可以获取到SIM卡变更告警记录）5-关机告警 |
| deviceId       | String  | 设备ID                                     |
| time           | Long    | 时间戳，单位ms                                 |
| address        | String  | 详细地址(sos告警和电子围栏告警)                       |
| lon            | Double  | 经度(sos告警和电子围栏告警)                         |
| lat            | Double  | 纬度(sos告警和电子围栏告警)                         |
| locateType     | Integer | 定位方式(sos告警和电子围栏告警)(1-GPS 2-LBS 3-WIFI 4-BT) |
| precision      | Integer | 定位精度,单位米(sos告警和电子围栏告警)                   |
| energy         | Integer | 电池电量 0-100(低电量告警)                        |
| regionName     | String  | 电子围栏名称(电子围栏告警)                           |
| alertCondition | Integer | 电子围栏告警类型:1.进入 2.离开(电子围栏告警)               |

示例:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "records": [
      {
        "id": 3,
        "deviceId": "sdsfsdf1",
        "time": 2346516546,
        "alarmType": 1,
        "lon":33.16,
        "lat":66.31,
        "locateType": 1,
        "precision": 100,
        "address":"广东省xxx"
      },
      {
        "id": 2,
        "deviceId": "sdsfsdf1",
        "time": 2346516546,
        "alarmType": 2,
        "lon":33.16,
        "lat":66.31,
        "locateType": 1,
        "precision": 100,
        "address":"广东省xxx",
        "regionName":"学校",
        "alertCondition":2
      },
      {
        "id": 1,
        "deviceId": "sdsfsdf1",
        "time": 2346516546,
        "alarmType": 3,
        "lon":33.16,
        "lat":66.31,
        "locateType": 1,
        "precision": 100,
        "address":"广东省xxx",
        "energy":15
        }
      }
    ]
  }
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10006 | 设备权限验证失败 |

### 监听记录列表

> 接口描述

当前用户监听设备的所有记录。

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/records/monitors |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数        | 类型      | 说明                             |
| --------- | ------- | ------------------------------ |
| startId   | Long    | 起始记录id(可选, 默认为0,表示从最新或最老的数据开始) |
| count     | Integer | 最大查询数量(可选,默认为20)               |
| direction | Integer | 查询方向(0倒序, 1正序 )(可选,默认为0倒序)     |
| deviceId  | String  | 设备id                           |

> 输出参数

| 参数          | 类型     | 说明          |
| ----------- | ------ | ----------- |
| id          | Long   | 记录id        |
| deviceId    | String | 设备ID        |
| time        | Long   | 时间戳，单位ms    |
| duration    | Long   | 监听时长，单位s    |
| contactName | String | 监听者在通讯录中的名称 |

示例:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "records": [
      {
        "id": 2,
        "deviceId": "sdflksadf1",
        "time": 32451234561,
        "duration": 60,
        "contactName": "张三"
      },
      {
        "id": 1,
        "deviceId": "sdflksadf1",
        "time": 31461564587,
        "duration": 90,
        "contactName": "李四"
      }
    ]
  }
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10006 | 设备权限验证失败 |

### 通话记录列表

> 接口描述

当前用户与设备所有通话记录。

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/records/calls |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数        | 类型      | 说明                             |
| --------- | ------- | ------------------------------ |
| startId   | Long    | 起始记录id(可选, 默认为0,表示从最新或最老的数据开始) |
| count     | Integer | 最大查询数量(可选,默认为20)               |
| direction | Integer | 查询方向(0倒序, 1正序 )(可选,默认为0倒序)     |
| deviceId  | String  | 设备id                           |

> 输出参数

输出参数说明

| 参数             | 类型      | 说明                        |
| -------------- | ------- | ------------------------- |
| id             | Long    | 记录id                      |
| deviceId       | String  | 设备ID                      |
| time           | Long    | 时间戳，单位ms                  |
| callStatus     | String  | 通话状态(1-呼出 2-未接 3-已接 4-拒接) |
| duration       | Integer | 通话时长，单位s                  |
| contactPhoneNo | String  | 通话联系人电话号                  |
| contactName    | String  | 通话联系人名称                   |

示例:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "records": [
      {
        "id": 2,
        "deviceId": "sdflksadf1",
        "time": 3512456421,
        "callStatus": "1",
        "duration": 20,
        "contactPhoneNo": "13998653471",
        "contactName": "张三"
      },
      {
        "id": 1,
        "deviceId": "sdflksadf1",
        "time": 35113245618,
        "callStatus": "1",
        "duration": 20,
        "contactPhoneNo": "13998653471",
        "contactName": "李四"
      }
    ]
  }
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10006 | 设备权限验证失败 |



---

## 监听

### 监听

> 接口描述

向设备发起监听请求。

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/monitor |
| ---- | ------------------------------------- |
| 请求方法 | POST                                  |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数       | 类型     | 说明   |
| -------- | ------ | ---- |
| deviceId | String | 设备ID |

> 输出参数

无

 示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备已离线    |
| -10006 | 设备权限验证失败 |

 

---

## 闹钟

### 获取闹钟列表

> 接口描述

获取闹钟列表

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/alarmClock |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数       | 类型     | 说明   |
| -------- | ------ | ---- |
| deviceId | String | 设备ID |

> 输出参数

| 参数                      | 类型      | 说明                                       |
| ----------------------- | ------- | ---------------------------------------- |
| alarmClocks             | Array   | 闹钟数组                                     |
| alarmClocks.id          | Integer | 闹钟id                                     |
| alarmClocks.time        | Integer | 闹钟时间(距离当天00:00的秒数, 如3600表示1:00)          |
| alarmClocks.ring        | Integer | 铃声id                                     |
| alarmClocks.repeat      | Integer | 重复信息，7bit整型数据，bit7:周日开关，bit6:周一开关，bit5:周二开关，bit4:周三开关，bit3:周四开关，bit2:周五开关，bit1:周六开关 |
| alarmClocks.enabled     | Integer | 子开关, 是否开启: 0-关闭 1-开启                     |
| alarmClocks.createTime  | Long    | 创建时间，Ms                                  |
| alarmClocks.modifyTime  | Long    | 修改时间，Ms                                  |
| alarmClocks.description | String  | 描述                                       |

 示例:

```json
{
  "code": 0,
  "msg": "success",
  "data":
  {
    "alarmClocks":
    [
      {
        "id":1,
        "time":3600,
        "ring":1,
        "repeat":65,
        "enabled":1,
        "createTime":15064235646556,
        "modifyTime":15065625464846,
        "description":"起床"
      },
      {
        "id":2,
        "time":7200,
        "ring":1,
        "repeat":7,
        "enabled":1,
        "createTime":15064235646556,
        "modifyTime":15065625464846,
        "description":"睡觉"
      }
    ]
  }
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10006 | 设备权限验证失败 |

### 添加闹钟

> 接口描述

添加闹钟

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/alarmClock |
| ---- | ---------------------------------------- |
| 请求方法 | PUT                                      |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数          | 类型      | 说明                                       |
| ----------- | ------- | ---------------------------------------- |
| deviceId    | String  | 设备ID                                     |
| time        | Integer | 闹钟时间(距离当天00:00的秒数, 如3600表示1:00)          |
| ring        | Integer | 铃声id                                     |
| repeat      | Integer | 重复信息，7bit整型数据，bit7:周日开关，bit6:周一开关，bit5:周二开关，bit4:周三开关，bit3:周四开关，bit2:周五开关，bit1:周六开关 |
| enabled     | Integer | 子开关, 是否开启: 0-关闭 1-开启                     |
| description | String  | 描述                                       |

> 输出参数

| 参数   | 类型      | 说明   |
| ---- | ------- | ---- |
| id   | Integer | 闹钟id |

 示例:

```json
{
  "code": 0,
  "msg": "success",
  "data":
  {
    "id":1
  }
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备不在线    |
| -10006 | 设备权限验证失败 |

### 修改闹钟

> 接口描述

修改闹钟

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/alarmClock |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数          | 类型      | 说明                                       |
| ----------- | ------- | ---------------------------------------- |
| deviceId    | String  | 设备ID                                     |
| id          | Integer | 闹钟id                                     |
| time        | Integer | 闹钟时间(距离当天00:00的秒数, 如3600表示1:00)          |
| ring        | Integer | 铃声id                                     |
| repeat      | Integer | 重复信息，7bit整型数据，bit7:周日开关，bit6:周一开关，bit5:周二开关，bit4:周三开关，bit3:周四开关，bit2:周五开关，bit1:周六开关 |
| enabled     | Integer | 子开关, 是否开启: 0-关闭 1-开启                     |
| description | String  | 描述                                       |

> 输出参数

无

 示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备不在线    |
| -10006 | 设备权限验证失败 |
| -31101 | 闹钟不存在    |

### 删除闹钟

> 接口描述

删除闹钟

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/alarmClock |
| ---- | ---------------------------------------- |
| 请求方法 | DELETE                                   |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数       | 类型      | 说明   |
| -------- | ------- | ---- |
| deviceId | String  | 设备ID |
| id       | Integer | 闹钟id |

> 输出参数

无

 示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备不在线    |
| -10006 | 设备权限验证失败 |
| -31101 | 闹钟不存在    |

## 用药提醒

### 获取用药提醒列表

> 接口描述

获取用药提醒列表

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/medicineReminder |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数       | 类型     | 说明   |
| -------- | ------ | ---- |
| deviceId | String | 设备ID |

> 输出参数

| 参数                            | 类型      | 说明                                       |
| ----------------------------- | ------- | ---------------------------------------- |
| medicineReminders             | Array   | 用药提醒数组                                   |
| medicineReminders.id          | Integer | 用药提醒id                                   |
| medicineReminders.time        | Integer | 用药提醒时间(距离当天00:00的秒数, 如3600表示1:00)        |
| medicineReminders.repeat      | Integer | 重复信息，7bit整型数据，bit7:周日开关，bit6:周一开关，bit5:周二开关，bit4:周三开关，bit3:周四开关，bit2:周五开关，bit1:周六开关 |
| medicineReminders.enabled     | Integer | 子开关, 是否开启: 0-关闭 1-开启                     |
| medicineReminders.createTime  | Long    | 创建时间，Ms                                  |
| medicineReminders.modifyTime  | Long    | 修改时间，Ms                                  |
| medicineReminders.description | String  | 描述                                       |

 示例:

```json
{
  "code": 0,
  "msg": "success",
  "data":
  {
    "medicineReminders":
    [
      {
        "id":1,
        "time":3600,
        "repeat":65,
        "enabled":1,
        "createTime":15064235646556,
        "modifyTime":15065625464846,
        "description":"感冒药"
      },
      {
        "id":2,
        "time":7200,
        "repeat":7,
        "enabled":1,
        "createTime":15064235646556,
        "modifyTime":15065625464846,
        "description":"毒药"
      }
    ]
  }
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10006 | 设备权限验证失败 |

### 添加用药提醒

> 接口描述

添加用药提醒

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/medicineReminder |
| ---- | ---------------------------------------- |
| 请求方法 | PUT                                      |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数          | 类型      | 说明                                       |
| ----------- | ------- | ---------------------------------------- |
| deviceId    | String  | 设备ID                                     |
| time        | Integer | 用药提醒时间(距离当天00:00的秒数, 如3600表示1:00)        |
| repeat      | Integer | 重复信息，7bit整型数据，bit7:周日开关，bit6:周一开关，bit5:周二开关，bit4:周三开关，bit3:周四开关，bit2:周五开关，bit1:周六开关 |
| enabled     | Integer | 子开关, 是否开启: 0-关闭 1-开启                     |
| description | String  | 描述                                       |

> 输出参数

| 参数   | 类型      | 说明   |
| ---- | ------- | ---- |
| id   | Integer | 闹钟id |

 示例:

```json
{
  "code": 0,
  "msg": "success",
  "data":
  {
    "id":1
  }
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备不在线    |
| -10006 | 设备权限验证失败 |

### 修改用药提醒

> 接口描述

修改用药提醒

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/medicineReminder |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数          | 类型      | 说明                                       |
| ----------- | ------- | ---------------------------------------- |
| deviceId    | String  | 设备ID                                     |
| id          | Integer | 用药提醒id                                   |
| time        | Integer | 用药提醒时间(距离当天00:00的秒数, 如3600表示1:00)        |
| repeat      | Integer | 重复信息，7bit整型数据，bit7:周日开关，bit6:周一开关，bit5:周二开关，bit4:周三开关，bit3:周四开关，bit2:周五开关，bit1:周六开关 |
| enabled     | Integer | 子开关, 是否开启: 0-关闭 1-开启                     |
| description | String  | 描述                                       |

> 输出参数

无

 示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备不在线    |
| -10006 | 设备权限验证失败 |
| -31201 | 用药提醒不存在  |

### 删除用药提醒

> 接口描述

删除用药提醒

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/medicineReminder |
| ---- | ---------------------------------------- |
| 请求方法 | DELETE                                   |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数       | 类型      | 说明     |
| -------- | ------- | ------ |
| deviceId | String  | 设备ID   |
| id       | Integer | 用药提醒id |

> 输出参数

无

 示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备不在线    |
| -10006 | 设备权限验证失败 |
| -31201 | 用药提醒不存在  |

## 静音时段

### 获取静音时段列表

> 接口描述

获取静音时段列表

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/mutePeriod |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数       | 类型     | 说明   |
| -------- | ------ | ---- |
| deviceId | String | 设备ID |

> 输出参数

| 参数                     | 类型      | 说明                                       |
| ---------------------- | ------- | ---------------------------------------- |
| mutePeriods            | Array   | 静音时段数组                                   |
| mutePeriods.id         | Integer | 静音时段id                                   |
| mutePeriods.startTime  | Integer | 静音时段开始时间(距离当天00:00的秒数, 如3600表示1:00)      |
| mutePeriods.endTime    | Integer | 静音时段结束时间(距离当天00:00的秒数, 如3600表示1:00)      |
| mutePeriods.repeat     | Integer | 重复信息，7bit整型数据，bit7:周日开关，bit6:周一开关，bit5:周二开关，bit4:周三开关，bit3:周四开关，bit2:周五开关，bit1:周六开关 |
| mutePeriods.enabled    | Integer | 子开关, 是否开启: 0-关闭 1-开启                     |
| mutePeriods.createTime | Long    | 创建时间，Ms                                  |
| mutePeriods.modifyTime | Long    | 修改时间，Ms                                  |

 示例:

```json
{
  "code": 0,
  "msg": "success",
  "data":
  {
    "mutePeriods":
    [
      {
        "id":1,
        "startTime":3600,
        "endTime":10000,
        "repeat":65,
        "enabled":1,
        "createTime":15064235646556,
        "modifyTime":15065625464846,
      
      },
      {
        "id":2,
        "startTime":3600,
        "endTime":10000,
        "repeat":7,
        "enabled":1,
        "createTime":15064235646556,
        "modifyTime":15065625464846,
       
      }
    ]
  }
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10006 | 设备权限验证失败 |

### 添加静音时段

> 接口描述

添加静音时段

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/mutePeriod |
| ---- | ---------------------------------------- |
| 请求方法 | PUT                                      |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数        | 类型      | 说明                                       |
| --------- | ------- | ---------------------------------------- |
| deviceId  | String  | 设备ID                                     |
| startTime | Integer | 静音时段开始时间(距离当天00:00的秒数, 如3600表示1:00)      |
| endTime   | Integer | 静音时段结束时间(距离当天00:00的秒数, 如3600表示1:00)      |
| repeat    | Integer | 重复信息，7bit整型数据，bit7:周日开关，bit6:周一开关，bit5:周二开关，bit4:周三开关，bit3:周四开关，bit2:周五开关，bit1:周六开关 |
| enabled   | Integer | 子开关, 是否开启: 0-关闭 1-开启                     |

> 输出参数

| 参数   | 类型      | 说明     |
| ---- | ------- | ------ |
| id   | Integer | 静音时段id |

 示例:

```json
{
  "code": 0,
  "msg": "success",
  "data":
  {
    "id":1
  }
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备不在线    |
| -10006 | 设备权限验证失败 |
| -31506 | 静音时段已存在  |

### 修改静音时段

> 接口描述

修改静音时段

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/mutePeriod |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数        | 类型      | 说明                                       |
| --------- | ------- | ---------------------------------------- |
| deviceId  | String  | 设备ID                                     |
| id        | Integer | 静音时段id                                   |
| startTime | Integer | 静音时段开始时间(距离当天00:00的秒数, 如3600表示1:00)      |
| endTime   | Integer | 静音时段结束时间(距离当天00:00的秒数, 如3600表示1:00)      |
| repeat    | Integer | 重复信息，7bit整型数据，bit7:周日开关，bit6:周一开关，bit5:周二开关，bit4:周三开关，bit3:周四开关，bit2:周五开关，bit1:周六开关 |
| enabled   | Integer | 子开关, 是否开启: 0-关闭 1-开启                     |

> 输出参数

无

 示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备不在线    |
| -10006 | 设备权限验证失败 |
| -31501 | 静音时段不存在  |
| -31506 | 静音时段已存在  |

### 删除静音时段

> 接口描述

删除静音时段

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/mutePeriod |
| ---- | ---------------------------------------- |
| 请求方法 | DELETE                                   |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数       | 类型      | 说明     |
| -------- | ------- | ------ |
| deviceId | String  | 设备ID   |
| id       | Integer | 静音时段id |

> 输出参数

无

 示例:

```json
{
  "code": 0,
  "msg": "success"
}
```

> 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备不在线    |
| -10006 | 设备权限验证失败 |
| -31101 | 静音时段不存在  |

## 语音对讲

### APP_soket传输消息定义

备注:

msgType: 消息传输方式

```
0——命令下达：服务器->客户端；   1——命令应答：客户端->服务器
2——主动上送：客户端->服务器；   3——上送应答：服务器->客户端
```

cmdType: 消息类型

```
1——首次发送消息
2——心跳包
3——下载消息
4——推送绑定设备在线状态
5——推送已读状态
6——上送文件下载成功
7——app向服务器请求设备在线状态
8——向app推送设备fota升级进度
9——向IOSapp推送语音离线消息
```

#### 首次连接socket

app首次连接携带消息

```json
{
  "data": {
    "token": "e3835af894d448da819a9c32a43e4b28"#用户token
    },
  "msgType":2,
  "cmdType":1
}
```

socket服务器响应消息 

```json
{
  "msgType": 3,
  "cmdType": 1,
  "msgMap": {
    "msg": "请求成功",
    "code": 0,
    "operateSuccess": 1,
    "data": {
        "pushTime": 1514534547000,
        "deviceStatus": {
          "socketOnLine":  ["13723376","23840582"],#socket在线的设备Id
            "socketOffLine": ["23840577","23840588"] #socket离线的设备Id
          }
      }
    }
}
```

#### 心跳包信息

服务器发送给app

```json
{
  "msgType":0,
  "cmdType":2
}
```

app响应服务器

```json
{
  "msgType":1,
  "cmdType":2
}
```

#### 发送下载消息文件通知信息

服务器推送app下载语音文件

```json
{
  "data": {
    "deviceId": "11393803",
      "recordId": 123444,#语音记录主键ID
      "ip": "183.230.102.49",
      "port": 1107,
      "fileUrl": "group1/M00/00/18/rBMPA1o8cyaAAP26AAAB3FbMmJA353.amr",
    "createTime":1514534547000
  },
  "msgType":0,
  "cmdType":3
}
```

app回应服务器

```json
{
  "msgMap": {
      "code":0,
    "msg":"请求成功",
    "data":{
        "recordId": 123444
    }
  },
  "msgType":1,
  "cmdType":3
}
```

#### 向app推送绑定设备在线状态

 当设备socket连接或断开向app推送

```json
{
  "cmdType": 4,
  "msgType": 0,
  "data": {
      "pushTime": 1514534547000,
      "deviceStatus": {
    "socketUpdateStatus" : {
      "deviceId": "23840588","deviceStatus": "1"  # 0为离线,1为在线
      }
    }
}
```

app响应服务器信息

```json
{
  "cmdType": 4,
  "msgType": 1
}
```
#### 服务器向app发送设备已读消息

```json
{
  "cmdType": 5,
  "msgType": 0,
  "data": {
    "recordId":123444
    }
}
```

app响应服务器信息

```json
{
  "cmdType": 5,
  "msgType": 1
}
```

#### app向服务器上送文件下载成功

```json
{
  "cmdType": 6,
  "msgType": 2,
  "data": {
  "recordId":1234455
    }
}
```
服务器响应app信息

```json
{
  "cmdType": 6,
  "msgType": 3
}
```



#### app向服务器请求设备在线状态

```json
{
  "cmdType": 7,
  "msgType": 2,
  "data": {
    "deviceId":"23840591"
  }
}
```

服务器响应app信息

```json
{
  "cmdType": 7,
  "msgType": 3,
  "msgMap": {
    "msg": "success",
    "code": 0,
    "operateSuccess": 1,  # 0为失败,1为成功
    "data": {
        "pushTime": 1514534547000,
      "socketUpdateStatus" : {
        "deviceId": "23840588","deviceStatus": 1  # 0为离线,1为在线
          } 
      }
    }
}
```

#### 服务器向IOSapp推送语音离线消息
```json
{
    "data": [
        {
            "deviceId": "11393803",
            "recordId": 123444,
            "ip": "183.230.102.49",
            "port": 1107,
            "fileUrl": "group1/M00/00/18/rBMPA1o8cyaAAP26AAAB3FbMmJA353.amr",
            "createTime": 1514534547000
        },
        {
            "deviceId": "11393803",
            "recordId": 123445,
            "ip": "183.230.102.49",
            "port": 1107,
            "fileUrl": "group1/M00/00/18/rBMPA1o8cyaAAP26AAAB3FbMmJA3sd3.amr",
            "createTime": 1514534588000
        }
    ],
    "msgType": 0,
    "cmdType": 9
}
```
 app响应服务器
```json
{
      "msgMap": {
        "code":0,
        "msg":"请求成功",
        "data":{
            "recordId": [123444,123445]
        }
      },
      "msgType":1,
      "cmdType":9
}
```

### APP上传聊天语音文件

> 接口描述

上传聊天的语音文件到服务器

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/voiceChat/appUploadVoiceFile |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header 参数

| 参数    | 类型     | 说明       |
| ----- | ------ | -------- |
| token | String | apptoken |

> 请求参数

| 参数        | 类型      | 说明           |
| --------- | ------- | ------------ |
| deviceId  | String  | 设备id         |
| sendType  | Integer | 2  :APP发送到设备 |
| voiceFile | 文件      | 语音文件参数       |

> 输出参数

| 参数       | 类型   | 说明       |
| -------- | ---- | -------- |
| recordId | Long | 该条消息主键ID |

> 错误码


| 错误码    | 说明      |
| ------ | ------- |
| -10003 | 设备不存在   |
| -10005 | 设备请求超时 |
|-23022  |设备语音功能为开启|
|-23025  |设备长连接不在线|


 示例:

```json
{
  "code": 0,
  "msg": "success",
  "operateSuccess": true
  "data": {
    "recordId":121215454212 
  }
}
```



## 推送模块

### 概述

推送模块用于平台向APP推送信息, 用于告警提示和数据实时刷新等。

APP若要使用推送功能，首先需要进行推送注册。

### 注册推送

> 接口描述

APP向平台注册推送信息。

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/push/register |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| token | String | 鉴权token |

> 请求参数

| 参数             | 类型       | 说明                                      |
| -------------- | -------- | --------------------------------------- |
| registrationId | String   | 推送注册ID                                  |
| solution       | Interger | 推送方案: 1.极光推送（目前使用极光推送, 后续有更多需求可以按顺序添加）  |
| platform       | Interger | 客户端类型: 1.ANDROID 2.IOS 3.WINPHONE 4.WIN |

> 输出参数

无

```json
{
  "code": 0,
  "msg": "success",
}
```

> 错误码

无

### 协议格式

```JSON
{
  "msgId": "xxxx",
  "msgType": 1,
  "time": 3516648465,
  "deviceId": "sdflksadf1",
  "notifyType": 0,
  "title":"这是消息标题",
  "content":"这是一条消息",
  "data": {}
}
```

参数说明:

| 参数         | 类型      | 说明                            |
| ---------- | ------- | ----------------------------- |
| msgId      | String  | 消息ID, 唯一                      |
| msgType    | Integer | 消息类型, 参考消息类型定义表               |
| time       | Long    | 消息产生时间戳, 单位ms                 |
| notifyType | Integer | 消息展示类型 0. 不展示 1. Notification |
| deviceId   | String  | 设备ID(如果推送内容和设备无关，可以为空)        |
| title      | String  | 消息标题，用于消息展示，可以为空              |
| content    | String  | 消息内容，用于消息展示，可以为空              |
| data       | Objcect | 消息内容                          |

后续推送接口中仅描述data中的参数。



#### *设备更换SIM卡推送（废弃，合并到告警信息推送）

> 描述

设备更换SIM卡时，向用户推送。

> msgType : 1
>
> notifyType: 1

> 参数

无

示例:

```json
{
  "msgId": "xxxx",
  "msgType": 1,
  "time": 6516648465,
  "deviceId": "sdflksadf1",
  "notifyType": 0,
  "title":"sim卡变更",
  "content":"\"宝贝1\"更换了sim卡，请及时更新sim卡号，以保证设备正常使用。",
  "data": 
  {
  }
}
```



#### 与设备绑定关系变更

> 描述

当前账号与设备发生绑定/解绑关系时，推送给APP。

目前仅推送设备解绑。

> msgType : 2
>
> notifyType: 1

> 参数

| 参数     | 类型      | 说明                                      |
| ------ | ------- | --------------------------------------- |
| bind   | Integer | 绑定操作 1绑定  0解绑                           |
| type   | Integer | 类型 1 被管理员解绑  2 设备被管理员恢复出厂(可选，bind为0时需要) |
| userId | Long    | 绑定该设备的用户id(可选，bind为1时需要)                |



示例:

```json
{
  "msgId": "xxxx",
  "msgType": 2,
  "time": 6516648465,
  "deviceId": "sdflksadf1",
  "notifyType": 0,
  "title":"",
  "content":"",
  "data": 
  {
    "bind":0,
    "type":1
  }
}
```



#### token失效

> 描述

当前账号登录的token失效时，推送给APP。

> msgType : 3
>
> notifyType: 1

> 参数

| 参数   | 类型      | 说明                         |
| ---- | ------- | -------------------------- |
| type | Integer | 类型 1 账号在其他客户端登录  2 token过期 |



示例:

```json
{
  "msgId": "xxxx",
  "msgType": 3,
  "time": 6516648465,
  //"deviceId": "sdflksadf1",
  "notifyType": 1,
  "title":"",
  "content":"",
  "data":
  {
    "type":1
  }
}
```



#### 告警信息

> 描述

当设备发生告警信息时，推送给APP。

> msgType : 4
>
> notifyType: 1

> 参数

| 参数                   | 类型      | 说明                                       |
| -------------------- | ------- | ---------------------------------------- |
| type                 | Integer | 类型 1-sos告警  2-电子围栏告警 3-低电量告警 4-SIM卡变更告警（SIM卡变更告警只推送设备管理员） |
| id                   | Long    | 告警记录ID                                   |
| deviceName           | String  | 设备名称                                     |
| locations            | Object  | 设备当前位置信息（可选，SIM卡变更无位置信息）                 |
| locations.lon        | Double  | 经度                                       |
| locations.lat        | Double  | 纬度                                       |
| locations.locateType | Integer | 定位                                       |
| locations.precision  | Integer | 定位精度,单位米                                 |
| locations.address    | String  | 地址                                       |
| battery              | Object  | 设备当前电量信息（可选，仅低电量告警包含）                    |
| battery.status       | Integer | 电池状态 (0正常 1正在充电 )                        |
| battery.energy       | Integer | 电池电量 0-100                               |



示例:

```json
{
  "msgId": "xxxx",
  "msgType": 4,
  "time": 6516648465,
  "deviceId": "sdflksadf1",
  "notifyType": 0,
  "title":"",
  "content":"",
  "data":
  {
    "type":1,
    "id":100,
    "deviceName":"宝宝",
    "locations":
    {
      "lon":33.16,
      "lat":66.31,
      "locateType": 1,
      "precision": 100,
      "address":"广东省xxx"
    },
    "battery":
    {
      "status":0,
      "energy":15
    }
  }
}
```



#### 设备开关机推送

> 描述

设备开关机时，推送给APP。

> msgType : 5
>
> notifyType: 1

> 参数

| 参数    | 类型      | 说明             |
| ----- | ------- | -------------- |
| power | Integer | 类型 1 on  0 off |



示例:

```json
{
  "msgId": "xxxx",
  "msgType": 5,
  "time": 6516648465,
  //"deviceId": "sdflksadf1",
  "notifyType": 0,
  "title":"",
  "content":"",
  "data":
  {
    "power":0
  }
}
```

#### 设备信息变更推送

> 描述

设备信息发生变更时，向用户推送变更后的信息。

目前推送的设备信息包括:模式切换。

> msgType : 6
>
> notifyType: 0

> 参数

| 参数                    | 类型      | 说明                                       |
| :-------------------- | ------- | ---------------------------------------- |
| locateMode            | Integer | 0:省电模式,2:普通模式, 4:追踪模式(可选)                |
| devicePhoneNo         | String  | 设备手机号, 管理员修改设备手机号时向所有用户推送(可选)            |
| devicePhoneType       | Integer | 设备电话卡类型:1.普通卡 2.物联卡(可选,与devicePhoneNo一起返回) |
| updateFwVersionResult | Integer | 设备固件升级:1.更新成功2更新失
```
sequenceDiagram
A->>B: How are you?
B->>A: Great!
```

```
sequenceDiagram
A->>B: How are you?
B->>A: Great!
```
败(可选)                   |
| deviceVoiceType         | Integer | 设备语音类型(打电话:0  语音对讲:1)                     |

示例:

```json
{
  "msgId": "xxxx",
  "msgType": 1,
  "time": 6516648465,
  "deviceId": "sdflksadf1",
  "notifyType": 0,
  "title":"",
  "content":"",
  "data": 
  {
    "locateMode":2,
    "devicePhoneNo":"13800000000",
    "devicePhoneType":1
    "updateFwVersionResult":1
  }
}
```

#### 行业平台审核通过推送 

> 描述

行业平台审核通过时，推送给APP。

> msgType : 7
>
> notifyType: 0

> 参数



示例:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "dealReslut":1
    "deviceId": "aiwnoahfoqw123",
    "deviceName": "找TA二代2号",
    //"onenetId": "6876676",
    //"deviceKey": "HM9fbweWQ00NzRUsWDdgrPHJnB8=",
    "deviceSn": "002",
    "online": 1,
    //"contactPhoneNo": "18611169817",
    "devicePhoneNo": "13922893814",
    "devicePhoneType":1,
    "deviceIcon": "http://172.19.3.157:7090/platform/file/download/2017072118594634",
    "deviceStep": 0,
   
  }
}
```
#### 语音消息推送

> 描述

设备发送语音消息，app socket长连接不在线时，发送极光推送给APP。

> msgType : 8

> notifyType: 1

> 参数

| 参数    | 类型      | 说明             |
| ----- | ------- | -------------- |     
|   deviceId  | 	String  | 	设备id |    
| recordId  | 	Long    | 	语音记录id | 
|   ip  |       	String   | 	ip         | 
|   port   |    	Ineteger | 	端口     
|   fileUrl  |  	String  | 	语音文件地址 | 
|   createTime | 	Long    | 	消息创建的时间 | 

示例:
```json
    {
    	"msgId": "xxxx",
    	"content": "【语音通知】{deviceName}发送了一条语音消息",
    	"time": 3516648465,
    	"deviceId": "11393803",
    	"msgType": 8,
    	"notifyType": 1,
    	"data": {
        	"deviceId": "11393803",
        	"recordId": 123444,
        	"ip": "183.230.102.49",
        	"port": 1107,
        	"fileUrl": "group1/M00/00/18/rBMPA1o8cyaAAP26AAAB3FbMmJA353.amr",
        	"createTime": 1514534547000
    	}
    }
```



### 极光推送

目前平台使用的推送方案为极光推送。

> 极光推送协议:https://docs.jiguang.cn/jpush/server/push/rest_api_v3_push/

#### 示例与说明

```json
{
  "cid": "8103a4c628a0b98974ec1949-711261d4-5f17-4d2f-a855-5e5a8909b26e",
  "platform": 
  [
    "android",
    "ios"
  ] ,
  "audience": {
    "registration_id" : 
    [ 
      "4312kjklfds2", 
      "8914afd2", 
      "45fdsa31" 
    ]
  },
  "notification": {
    "android": {
      "alert": "模式变更",
      "title": "Send to Android",
      "builder_id": 1,
      "extras": {
        "pushInfo": "{\"msgId\": \"xxxx\",\"msgType\": 1,\"time\": 6516648465,\"deviceId\": \"sdflksadf1\",\"notifyType\": 0,\"content\":\"模式变更\",\"data\": {\"deviceMode\":3}}"
      }
    },
    "ios": {
      "alert": "模式变更",
      "sound": "default",
      "badge": "+1",
      "extras": {
        "pushInfo": "{\"msgId\": \"xxxx\",\"msgType\": 1,\"time\": 6516648465,\"deviceId\": \"sdflksadf1\",\"notifyType\": 0,\"content\":\"模式变更\",\"data\": {\"deviceMode\":3}}"
      }
    }
  },
  "message": {
    "msg_content": "模式变更",
    "content_type": "text",
    "title": "msg",
    "extras": {
      "pushInfo": "{\"msgId\": \"xxxx\",\"msgType\": 1,\"time\": 6516648465,\"deviceId\": \"sdflksadf1\",\"notifyType\": 0,\"content\":\"模式变更\",\"data\": {\"deviceMode\":3}}"
    }
  },
  "options": {
    "time_to_live": 60,
  }
}
```

| 参数                          | 类型             | 说明                                       |
| --------------------------- | -------------- | ---------------------------------------- |
| cid                         | String         | cid 是用于防止 api 调用端重试造成服务端的重复推送而定义的一个推送参数。<br>用户使用一个 cid 推送后，再次使用相同的 cid 进行推送，则不会再次进行推送。<br>使用极光推送时，推送协议中的msgId使用cid填充。 |
| platform                    | String \|Array | JPush 当前支持 Android, iOS, Windows Phone 三个平台的推送。<br>关键字分别为："android", "ios", "winphone"。 |
| audience                    | Object         | 推送设备对象。<br>表示一条推送可以被推送到哪些设备列表,目前主要使用registration_id的方式进行定向推送。 |
| notification                | Object         | 通知类型的推送，即App收到推送后以notification的形式显示推送内容，与message参数不能同时存在。<br>推送协议中notifyType为1时使用此推送类型进行推送。 |
| notification.android.alert  | String         | 通知类型推送的内容，推送协议中的content将填充到这里。           |
| notification.android.extras | Object         | 通知类型推送的业务信息，这部分内容将直接转发给app。<br>extras中的key和value只能是字符串，所以我们使用固定pushInfo作为key, 将推送协议中完整的数据以***`JSON字符串`***的形式放到extras中。 |
| message                     | Object         | 消息类型的推送，即App收到推送后以不会显示推送内容，由app自行处理, 与notification参数不能同时存在。<br>推送协议中notifyType为0时使用此推送类型进行推送。 |
| message.alert               | String         | 推送内容，推送协议中的content将填充到这里。                |
| message.extras              | Object         | 同notification.android.extras。            |
| options                     | Object         | 一些推送参数。                                  |
| options.time_to_live        | Object         | 推送当前用户不在线时，为该用户保留多长时间的离线消息，以便其上线时再次推送。<br>默认 86400 （1 天），最长 10 天,设置为 0 表示不保留离线消息，只有推送当前在线的用户可以收到。 |

### socket长连接推送

>备注:

>msgType: 消息传输方式
```

    0——命令下达：服务器->客户端；   1——命令应答：客户端->服务器
    2——主动上送：客户端->服务器；   3——上送应答：服务器->客户端
```
#### 服务器推送设备fota升级进度

> 描述

设备发送语音消息，app socket长连接不在线时，发送极光推送给APP。

>msgType : 0

>cmdType: 8

>参数

  参数         	类型     	说明   
  deviceId   	String 	设备id 
  fotaPercent	Integer	升级百分比

>示例:
```json
    {
      "msgType":0,
      "cmdType":8,
      "msgMap":{
        "data":{
        	"deviceId": "123456",
         	"fotaPercent":20
        }
      }
    }
```
app响应
```json
    {
      "msgType": 1,
      "cmdType": 8
    }
```


#### 服务器推送联系人语音下载成功

>描述

设备发送语音消息，app socket长连接不在线时，发送极光推送给APP。

>msgType : 0

>cmdType: 10

参数

  参数       	类型     	说明      
  deviceId 	String 	设备id    
  userId   	Integer	用户ID    
  contactId	Long   	条目id唯一主键

>示例:
```json
    {
      "msgType":0,
      "cmdType":10,
       "data":{
    		"deviceId": "123456", 
    		"userId":4,   
            "contactId":100, 
    		}
      
    }
```
app响应
```json
    {
      "msgType": 1,
      "cmdType": 10
    }
```
---

 



---

# 附件一: RSA加密

App必须使用RSA加密算法对关键信息进行加密，比如账号密码。

RSA公钥:

MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCTYed2VG1Q1RZSmR4mzFAh+GgQm4CJCKjN5ww8eNwDVznNEtFO+5SFWLAZTNxM4LOaD1OMQ3a7eiwAzeFdlG/OQFNARCHXxo72ataNHoPmvzX9ibHYKUyZMvhk5OnXlSph3+Np6DBCDgE+HchJ6io21EC+m8uJm4x2aK2AvLAMTwIDAQAB














---

# 附件二: HASH算法

暂定采用MD5算法。



---

# 附件三: 字符合法性

|       | 长度               | 合法字符集                  |
| ----- | ---------------- | ---------------------- |
| 用户密码  | 6~20             | 英文,数字,可打印字符, 必须包含英文和数字 |
| 用户昵称  | 1~20(一个中文按2个字符算) | 任意字符                   |
| 设备昵称  | 1~12(一个中文按2个字符算) | 任意字符                   |
| 通讯录昵称 | 1~12(一个中文按2个字符算) | 中文,英文,数字               |

 

---

# 附件四: UUID

UUID含义是通用唯一识别码 (Universally Unique Identifier)，这是一个软件建构的标准，也是被开源软件基金会 (Open Software Foundation, OSF) 的组织在分布式计算环境 (Distributed Computing Environment, DCE) 领域的一部份。

在本文档中，UUID用于标识客户端的唯一性，所有登录xMat平台的客户端都必须具有唯一且固定的UUID。

UUID应在应用首次运行时生成并固化存储到文件中，之后则直接从文件中读取UUID。

## Android 生成UUID

```java
// 生成UUID并固化到SharedPreferences
public String getIdentity() {
  SharedPreferences preference = PreferenceManager.getDefaultSharedPreferences(context);
  String identity = preference.getString("identity", null);
  if (identity == null) {
    identity = java.util.UUID.randomUUID().toString();
    preference.edit().putString("identity", identity);
  }
  return identity;
}
```

## IOS生成UUID

```objective-c
// 仅包含生成UUID的功能，固化存储的功能自己实现
- (NSString *)createCUID:(NSString *)prefix
{
  NSString *  result;
  CFUUIDRef  uuid;
  CFStringRef uuidStr;
  uuid = CFUUIDCreate(NULL);
  uuidStr = CFUUIDCreateString(NULL, uuid);
  result =[NSString stringWithFormat:@"%@-%@", prefix,uuidStr];
  CFRelease(uuidStr);
  CFRelease(uuid);
  return result;
}
```
# 附件五: 字符合法性

|       | 长度               | 合法字符集                  |
| ----- | ---------------- | ---------------------- |
| 用户密码  | 6~20             | 英文,数字,可打印字符, 必须包含英文和数字 |
| 用户昵称  | 1~20(一个中文按2个字符算) | 任意字符                   |
| 设备昵称  | 1~12(一个中文按2个字符算) | 任意字符                   |
| 通讯录昵称 | 1~12(一个中文按2个字符算) | 中文,英文,数字               |
