------

# 对外-xMat平台第三方接口



------

|  版本/状态   |   责任人   |    修改日期    | 版本变更记录                                   |
| :------: | :-----: | :--------: | :--------------------------------------- |
| V0.2/草稿 |   胡才庆   | 2018/06/19 | 1.添加对接步骤                            |
| V0.1/草稿 |   胡才庆   | 2018/04/26 | 1.设备模块相关的对外接口                            |




------

[TOC]

---

## 概述

所有接口需要填写Header里的companyToken参数，companyToken需要调用接口自行获取。

### Header说明

| 参数           | 类型     | 说明                                       |
| ------------ | ------ | ---------------------------------------- |
| Content-Type | String | 请求类型，若无特别，均为“application/json;charset=utf-8” |

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

## 对接流程
对于一个新的设备，通过执行以下三步后，激活设备并获取设备位置信息

1. 根据创建公司时的手机号和密码通过[查询行业用户companyToken](#queryCompanyToken)
2. 调用[设备信息查询(批量)](#queryDevices)根据设备sn以及companyToken可获取设备的deviceId/IMEI号码，以便后续接口
3. 使用[创建紧急联系人](#createEmergencyContact)添加设备紧急联系人并激活
4. 调用[获取设备最新定位信息](#nowLocation)获取实时位置

服务器ip以及port：

> 测试环境
>> HTTP接口   ip:183.230.102.49  port:1002
<br> rabbitmq对列  ip:183.230.102.49   port:1257

## RabbitMQ连接
RabbitMQ java 连接版
```text
import java.io.IOException;
import java.util.concurrent.TimeoutException;
import com.rabbitmq.client.AMQP;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;
import com.rabbitmq.client.DefaultConsumer;
import com.rabbitmq.client.Envelope;

public class Customer {
	private final static String QUEUE_NAME = "queueName";
	private final static String USERNAME = "username";
	private final static String PASSWORD = "password";
	public static void main(String[] args) throws IOException, TimeoutException {
		// 创建连接工厂
		ConnectionFactory factory = new ConnectionFactory();
		// 设置RabbitMQ地址
		factory.setHost("183.230.102.49");
		factory.setUsername(USERNAME);
		factory.setPassword(PASSWORD);
		factory.setPort(1257);
		// 创建一个新的连接
		Connection connection = factory.newConnection();
		// 创建一个通道
		Channel channel = connection.createChannel();
		
		System.out.println("Customer Waiting Received messages");
		// DefaultConsumer类实现了Consumer接口，通过传入一个频道，
		// 告诉服务器我们需要那个频道的消息，如果频道中有消息，就会执行回调函数handleDelivery
		// 手动回复队列应答 -- RabbitMQ中的消息确认机制
		channel.basicConsume(QUEUE_NAME, false, new DefaultConsumer(channel) {
			@Override
			public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties,
					byte[] body) throws IOException {
				try {
				    String message = new String(body, "UTF-8");
					System.out.println("Customer Received '" + message + "'");
					channel.basicAck(envelope.getDeliveryTag(), false);
				} catch (Exception e) {
					channel.basicNack(envelope.getDeliveryTag(), false, false);
				}
				
			}
		});
	}
}
```

---
# 基础功能模块 

## 行业平台信息

<div id="queryCompanyToken" />

### 查询companyToken 

> 接口描述

查询行业用户companyToken,以及rabbitMq队列的账号名、密码 、对列名字等
1. companyToken: 所有HTTP接口都需要在Header携带companyToken
2. rabbitMq: 平台推送的各种消息通过队列获取得到

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.basic.CompanyInterface.queryCompanyInfo(CompanyDTO)
2. 测试例子位置：com.cmiot.test.CompanyAndDeviceTest.testGetCompanyToken()


> 请求方式

| 请求路径 | http://<HOST>:<PORT>/portal/industry/user/selectCompanyToken |
| :--- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数

无

> 请求参数

| 参数        | 类型      | 说明                                       |
| --------- | -----   |  ---------------------------------------- |
|userPhoneNo| String  | 用户登陆手机号                                  |
| companyAdminPassword | String |用户密码                               |

> 输出参数

|参数    |类型  |  说明|
| --------- | -----   |  ---------------------------------------- |
| companyToken | String |鉴权companyToken                           |
| mqUsername | String |公司mq用户名                           |
| mqPassword | String |公司mq用户密码                           |
| mqQueuename | String |公司mq消息队列名称                           |

响应:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
      "companyToken":"971f886fa25b42889f238161b2c6095b",
      "mqUsername":"15038666473",
      "mqPassword":"201805236473",
      "mqQueuename":"xMat.toOtherIndu.15038666473"
  }
}
```

> 错误码

| 错误码    | 错误信息     |
| ------ | -------- |
| -21008      | 用户名密码不匹配     |

## 设备设置

包含了对设备设置项的修改接口、例如设备关机，重启，响铃等.

<div id="queryDevices" />

### 设备信息查询（批量）

> 接口描述


根据设备的SN 或IMEI 号 查询公司设备的sn、deviceId、IMEI等信息 。deviceSns和deviceImeis参数只能2选其一,  单次个数不大于20。  

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.basic.DeviceInterface.queryDeviceInfo(DeviceDTO)
2. 测试例子位置：com.cmiot.test.CompanyAndDeviceTest.testQueryDeviceInfo()


> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/queryIndustryDevice |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数

| 参数           | 类型     | 说明      |
| ------------ | ------ | ------- |
| companyToken | String | 鉴权token |

>请求参数

| 参数          | 类型    | 说明            |
| ----------- | ----- | ------------- |
| deviceSns   | Array | 设备deviceSn号集合 |
| deviceImeis | Array | 设备IMEI号集合     |

> 

> 输出参数

| 参数                       | 类型     | 说明            |
| ------------------------ | ------ | ------------- |
| deviceinfos              | Array  | 根据条件查询到得设备对集合 |
| deviceinfos.id           | Long   | 编号            |
| deviceinfos.deviceSn     | String | 设备SN号         |
| deviceinfos.deviceId     | String | 设备deviceId号   |
| deviceinfos.deviceImei   | String | 设备IMEI号       |
| deviceinfos.deviceOnlyId | String |               |
| errerDevices             | Array  | 查询失败的设备       |



 示例:

```json
{
    "msg": "请求成功",
    "code": 0,
    "data": {
        "deviceinfos": [
            {
                "id": 681,
                "deviceSn": "102000218290000123",
                "deviceId": "23840602",
                "deviceImei": "866545030010141",
                "deviceOnlyId": "101REMB56316RV3A"
            }
           {
                "id": 682,
                "deviceSn": "102000218290000122",
                "deviceId": "23840601",
                "deviceImei": "866545030010140",
                "deviceOnlyId": "101REMB56316RV3b"
            }
          ...
        ],
        "errerDevices": [
            "102000218290000124",
            "102000218290000125",
           ...
          
        ]
    },
    "operateSuccess": 1
}
```

### 激活接口
只激活设备，让设备开始上传位置信息，暂无

<div id="createEmergencyContact" />

### 创建紧急联系人
> 接口描述


设备为未激活状态时, app调用此接口发送设备穿戴者信息和设备管理员通信信息来激活&绑定设备.

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.basic.DeviceInterface.createEmergencyContact(DeviceDTO, File)
2. 测试例子位置：com.cmiot.test.CompanyAndDeviceTest.testCreateEmergencyContact()


> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/device/activate |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数


| 参数           | 类型     | 说明                       |
| ------------ | ------ | ------------------------ |
| Content-Type | String | 请求类型，multipart/form-data |
| companyToken        | String | 鉴权Token                  |

> 请求参数


| 参数              | 类型      | 说明                  |
| --------------- | ------- | ------------------- |
| deviceName      | String  | 设备名                 |
| deviceId        | String  | 设备信息表id             |
| deviceIcon      | File    | 设备头像(可选)            |
| devicePhoneNo   | String  | 设备插入卡的手机号                |
| gender          | Integer | 性别：男:1,女:2(可选)      |
| birth           | String  | 生日(可选)              |
| height          | Integer | 身高, 单位cm(可选)        |
| weight          | Integer | 体重, 单位kg(可选)        |
| description     | String  | 详细描述(可选)            |
| contactName     | String  | 联系人姓名               |
| contactPhoneNo  | String  | 联系人手机号              |

> 输出参数



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
### 修改紧急联系人
> 接口描述

修改紧急联系人

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.basic.DeviceInterface.updateEmergencyContact(DeviceDTO)
2. 测试例子位置：com.cmiot.test.CompanyAndDeviceTest.tesetupdateEnmergencyContact()

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/industry/updateEmergencytel |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数

| 参数           | 类型     | 说明      |
| ------------ | ------ | ------- |
| companyToken | String | 鉴权token |

>请求参数

| 参数             | 类型      | 说明                   |
| -------------- | ------- | -------------------- |
| deviceId       | String  | 设备ID                 |
| contactName    | String  | 联系人姓名                |
| contactPhoneNo | String  | 联系人手机号               |
| holdUser       | Integer | 是否保留以前的紧急联系人在通讯录中0:保留1不保留 |
请求示例:

  ```json
  {
	"deviceId": "23840590",
	"contactName":"Stirng",
	"contactPhoneNo":"17673452110",
	"holdUser":"1"
}
  ```
> 


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
###  获取设备详细信息(可用于轮询)

> 接口描述


获取当前用户绑定的某一个设备详情信息

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.basic.DeviceInterface.queryDeviceDetail(DeviceDTO)
2. 测试例子位置：com.cmiot.test.CompanyAndDeviceTest.queryDeviceDetail()


> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/industry/detail |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数

| 参数           | 类型     | 说明      |
| ------------ | ------ | ------- |
| companyToken | String | 鉴权Token |

> 请求参数

| 参数        | 类型          | 说明                                       |
| --------- | ----------- | ---------------------------------------- |
| deviceIds | StringArray | <br>例如获取一台设备["xxxx"], 获取多台设备[“xxx1”,"xxx2"], |

请求示例：
 ```json
  {
		"deviceIds":["23840590","2384059",...]
}
  ```
> 输出参数

| 参数                   | 类型      | 说明                |
| -------------------- | ------- | ----------------- |
| gender               | Integer | 性别：男:1,女:2(可选)    |
| birth                | String  | 生日(可选)            |
| height               | Integer | 身高(可选)            |
| weight               | Integer | 体重(可选)            |
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
| devices                 | Array   | 用户账号绑定的设备信息                              |
| devices.deviceId        | String  | 设备ID                                     |
| devices.deviceName      | String  | 设备名称                                     |
| devices.deviceKey       | String  | oneNET设备APIKEY                           |
| devices.deviceSn        | String  | 设备SN                                     |
| devices.online          | Integer | 设备在线状态 1. 在线  0.离线                       |
| devices.contactPhoneNo  | String  | 设备管理员的通讯录手机号(可以不要)                       |
| devices.devicePhoneNo   | String  | 设备手机号                                    |
| devices.devicePhoneType | Integer | 设备电话卡类型:0.普通卡 1.物联卡                      |
| devices.deviceIcon      | String  | 设备头像URL                                  |
| devices.deviceStep      | Integer | 设备当前步数                                   |
| devices.userType        | Integer | 用户类型:0-管理员   1-普通用户(有APP账号)    2-普通用户(无账号) |
| userId                  | integer | 用户ID                                     |
| deviceType              | Integer | 设备类型:0普通设备,1:行业设备,2:宠物设备                 |
| deviceVoiceType         | Integer | 设备语音类型(不保留:0  保留:1)                      |


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

 ###  修改设备相关信息(不包含设备头像)

> 接口描述


修改设备持有人的信息（非头像）

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.basic.DeviceInterface.updateDeviceInfo(DeviceDTO)
2. 测试例子位置：com.cmiot.test.CompanyAndDeviceTest.updateDeviceDetail()


> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/device |
| ---- | ------------------------------------ |
| 请求方法 | PATCH                                |

> Header参数


| 参数           | 类型     | 说明                         |
| ------------ | ------ | -------------------------- |
| Content-Type | String | 请求类型，目前只支持application/json |
| companyToken        | String | 鉴权Token                    |

> 请求参数


| 参数            | 类型      | 说明    |
| ------------- | ------- | ----- |
| deviceName    | String  | 设备名称  |
| devicePhoneNo | String  | 设备手机号 |
| gender        | Integer | 性别   男:1,女:2 |
| birth         | String  | 生日 格式"2017-09-16"   |
| height        | Integer | 身高  单位cm  |
| weight        | Integer | 体重   单位kg  |
| deviceId      | String  | 设备id  |
| description   | String  | 详情    |
|               |         |       |
请求示例：

```json
{
  "deviceName":"yufan",
"devicePhoneNo":"1064716342130",
"gender":1,
"birth":"2017-09-16",
"height":120,
"weight":40,
"deviceId":"23840590",
"description":"鸿行学院..."
}
```
> 输出参数


| 参数              | 类型      | 说明                                       |
| --------------- | ------- | ---------------------------------------- |
| devicePhoneType | Integer | 设备电话卡类型:0.普通卡 1.物联卡(可选,如果修改了手机号，则须返回此参数) |
| deviceVoiceType | Integer | 设备语音类型(不保留:0  保留:1)                      |

示例:

```json
{
  "code": 0,
  "msg": "success",
  "data": 
  {
    "devicePhoneType":1
    "deviceVoiceType":0
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

### 获取设备设置

> 接口描述

获取设备设置。

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.basic.DeviceInterface.queryDeviceSetings(DeviceDTO)
2. 测试例子位置：com.cmiot.test.CompanyAndDeviceTest.queryDeviceSetings()

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
| companyToken | String | 鉴权token |

> 请求参数

| 参数       | 类型           | 说明                                       |
| -------- | ------------ | ---------------------------------------- |
| deviceId | String       | 设备ID                                     |
| filter   | String Array | 指定要获取的设备设置项(可选)<br>目前定义的设备信息包括:<br>locateMode:定位模式设置<br>autoAnswer:自动应答设置<br>whiteList:白名单状态设置<br>autoSleep:自动休眠设置<br>例：<br>只获取定位模式设置: "filter":["locateMode"]<br>获取自动应答设置和白名单状态设置:"filter":["autoAnswer","whiteList"]<br>特别地，如果参数为空或者不携带本参数，则表示获取所有设置项 |

> 输出参数

| 参数                | 类型      | 说明                                |
| ----------------- | ------- | --------------------------------- |
| locateMode        | Integer | 定位模式( 0:待机模式,4:智能模式, 2:追踪模式)(可选)  <br>追踪模式：最快可1分钟上报一次位置信息<br>智能模式：3-5分钟上报一次位置信息|
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

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.basic.DeviceInterface.updateDeviceSetings(DeviceDTO)
2. 测试例子位置：com.cmiot.test.CompanyAndDeviceTest.updateDeviceSetings()

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/settings |
| ---- | ---------------------------------------- |
| 请求方法 | PATCH                                    |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| companyToken | String | 鉴权token |

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
### 透传命令
> 接口描述

设备控制: 激活命令,远程响铃,远程重启,远程关机

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.basic.DeviceInterface.commandDevice(DeviceDTO)
2. 测试例子位置：com.cmiot.test.CompanyAndDeviceTest.commandDevice()

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/deviceCommand |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                    |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| companyToken | String | 鉴权token |

> 请求参数

| 参数                | 类型      | 说明                                       |
| ----------------- | ------- | ---------------------------------------- |
| sn         | String  | 设备SN号                                    |
| msgType        | Integer | 0 (默认,不可修改) |
| cmdType        | Integer | 3/4/5     (远程关机,远程重启,远程响铃)            |
请求示例：
```json
{
	"sn":"102000218290000117",
	"msgType":0,
    "cmdType":5
	
}
```

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

## 定位模块

提供位置信息的服务，能操作设备立即定位，查询设备历史轨迹

<div id="nowLocation" />

### 获取设备最新定位信息

> 接口描述

查询用户绑定的所有设备的定位信息。

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.basic.LocationInterface.queryDeviceLocations(LocationsDTO)
2. 测试例子位置：com.cmiot.test.LocationTest.testQueryDeviceLocations()

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/locations |
| :--- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数

| 参数           | 类型     | 说明                                       |
| ------------ | ------ | ---------------------------------------- |
| companyToken  | String | 鉴权companyToken                                  |

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

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.basic.LocationInterface.queryDeviceLoctionsHistory(LocationsDTO)
2. 测试例子位置：com.cmiot.test.LocationTest.testQueryDeviceLoctionsHistory()

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/locations/history |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数

| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| companyToken | String | 鉴权companyToken |

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



# 额外功能模块

## 电子围栏 

提供对设备位置的监控服务，能够自定义围栏形状，以及精准设置围栏告警时段及规则


### 获取电子围栏详情

> 接口描述

根据围栏编号获取围栏详情

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.extra.impl.ElectronicFenceInterfaceImpl.queryFenceDetail(ElectronicFenceDTO)
2. 测试例子位置：com.cmiot.test.ElectronicFenceTest.testQueryFenceDetail()

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/eleclosure/getFenceDetail |
| ---- | ------------------------------------ |
| 请求方法 | POST                                 |

> Header参数

| 参数           | 类型     | 说明             |
| ------------ | ------ | -------------- |
| companyToken | String | 鉴权companyToken |

> 请求参数

| 参数        | 类型   | 说明              |
| --------- | ---- | --------------- |
| id        | Long | /**围栏编号**/ [必填] |
| companyId | Long | /**公司编号**/ [必填]   |
请求示例：
~~~~json
{
   "id":102,
   "companyId":585
}
~~~~

> 输出参数

| 参数            | 类型      | 说明                                       |
| ------------- | ------- | ---------------------------------------- |
| enclosuRadius | Integer | /**围栏半径**/                               |
| enclosuId     | Long    | /**围栏编号**/                               |
| weekTime      | String  | /**星期几开启围栏 “0111111”   0是关闭  1是开启**/     |
| enclosuName   | String  | /**围栏的名称**/                              |
| enclosuType   | String  | /**围栏触发类型  [1是进入时告警 2是离开是告警 3是进入离开都告警]**/ |
| areaName      | String  | /**围栏中心地址**/                             |
| startTime     | String  | /**一天中围栏监控开始时间**/                        |
| endTime       | String  | /**一天中围栏监控结束时间 **/                       |
| deviceIds     | Array   | /**围栏绑定的设备**/                            |
| lon           | Double  |                                          |
| lat           | Double  |                                          |
| state         | String  | /**围栏开关状态**/                             |

示例:

~~~~json
{
    "msg": "请求成功",
    "code": 0,
    "data": {
        "enclosuType": 1,
        "enclosuRadius": 907,
        "areaName": "一本大厦-D座 (广东省深圳市南山区茶光路一本电子商务产业园一本大厦)",
        "weekTime": "0001001",
        "deviceIds": [
            "23840590",
          ...
        ],
        "startTime": "02:00",
        "endTime": "23:38",
        "state": "0",
        "enclosuId": 292,
        "enclosuName": "dddd",
        "lon": 114.05634,
        "lat": 22.57294
    },
    "operateSuccess": 1
}
~~~~

> 错误码

| 错误码  | 说明   |
| ---- | ---- |
|      |      |



### 获取围栏集合

> 接口描述

 获取多个围栏信息 
 
> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.extra.impl.ElectronicFenceInterfaceImpl.queryIndustryFences(ElectronicFenceDTO)
2. 测试例子位置：com.cmiot.test.ElectronicFenceTest.testQueryIndustryFences()

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/eleclosure/getIndustryFences |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数

| 参数           | 类型     | 说明             |
| ------------ | ------ | -------------- |
| companyToken | String | 鉴权companyToken |

> 请求参数

| 参数              | 类型     | 说明                |
| --------------- | ------ | ----------------- |
| eleclosures     | Array  | /**围栏编号集合**/[必填]  |
| eleclosuresName | String | /**围栏名称 支持模糊查询**/ |
| companyId       | Long   | /**公司编号**/[必填]    |
请求示例：
~~~~json
{
   "eleclosures":[123,456,321],
   "eleclosuresName":"后海"
   "companyId":44
}
~~~~
> 输出参数

| 参数            | 类型      | 说明                                       |
| ------------- | ------- | ---------------------------------------- |
| enclosuRadius | Integer | /**围栏半径**/                               |
| id            | Long    | /**围栏编号**/                               |
| weekTime      | String  | /**星期几开启围栏 “0111111”   0是关闭  1是开启**/     |
| enclosuName   | String  | /**围栏的名称**/                              |
| enclosuType   | String  | /**围栏触发类型  [1是进入时告警 2是离开是告警 3是进入离开都告警]**/ |
| areaName      | String  | /**围栏中心地址**/                             |
| startTime     | String  | /**一天中围栏监控开始时间**/                        |
| endTime       | String  | /**一天中围栏监控结束时间 **/                       |
| state         | String  | /**围栏状态**/                               |
| lon           | Double  | /**围栏中心点 经度**/                           |
| lat           | Double  | /**围栏中心点纬度**/                            |
| createTime    | Long    | /**创建时间**/                               |
~~~~json
{
    "msg": "请求成功",
    "code": 0,
    "data": [
        {
            "id": 282,
            "enclosuName": "cds",
            "enclosuRadius": 500,
            "areaName": "J D (深圳市福田区深南大道6019号丰盛町商业步行街C区B1层)",
            "weekTime": "0100000",
            "startTime": "00:00",
            "endTime": "23:59",
            "state": "0",
            "createTime": 1523268050750,
            "enclosuType": 1,
            "companyId": 5,
            "lon": 114.01935,
            "lat": 22.53831
        },
        {
            "id": 285,
            "enclosuName": "cdsswdq",
            "enclosuRadius": 500,
            "areaName": "Eni:d (深圳市福田区深南大道1095号新城市广场1层)",
            "weekTime": "0000000",
            "startTime": "00:00:00",
            "endTime": "23:59:00",
            "state": "0",
            "createTime": 1523154529704,
            "enclosuType": 1,
            "companyId": 5,
            "lon": 114.09351,
            "lat": 22.54205
        },
        ...
    ],
    "operateSuccess": 1
}
~~~~



### 添加电子围栏
> 接口描述

 添加围栏信息

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.extra.impl.ElectronicFenceInterfaceImpl.addIndustryFences(ElectronicFenceDTO)
2. 测试例子位置：com.cmiot.test.ElectronicFenceTest.testAddElectronicFence()

> 请求方式

| 请求路径 | http://<HOST>:<PORT> /platform/eleclosure/setElectronicFence |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数

| 参数           | 类型     | 说明             |
| ------------ | ------ | -------------- |
| companyToken | String | 鉴权companyToken |

> 请求参数

| 参数            | 类型      | 说明                                      |
| ------------- | ------- | --------------------------------------- |
| companyId     | Long    | /**公司编号**/                              |
| enclosuType   | Long    | /**围栏告警类型  1.进圈  2.出圈  3是进入离开都告警**/     |
| enclosuRadius | Integer | /**围栏半径**/                              |
| areaName      | String  | /**围栏区域名称: 如 重庆正大软件专修学院(南泉镇白鹤村16号附近)**/ |
| weekTime      | String  | /**告警周期 7位二进制，0不告警，1告警，比如0001100**/     |
| deviceIds     | Array   | /**围栏绑定的设备**/                           |
| startTime     | String  | /**一天中围栏监控开始时间**/                       |
| endTime       | String  | /**一天中围栏监控结束时间 **/                      |
| lon           | Double  |                                         |
| lat           | Double  |                                         |
| enclosuName   | String  | /**围栏名称**/                              |

请求示例:
~~~~json
 {

 "companyId":5,
 "enclosuType":3,
 "enclosuRadius":520,
 "areaName":"重庆正大软件专修学院(南泉镇白鹤村16号附近)",
 "weekTime":"0001100",
 "deviceIds":["23840602","23840590"],
 "startTime":"00:00",
 "endTime":"11:00",
 "enclosuName":"黎祖详测试",
 "lon":114.05634,
 "lat":22.57294

}
~~~~

> 输出参数

| 参数        | 类型   | 说明         |
| --------- | ---- | ---------- |
| enclosuId | Long | /**围栏编号**/ |

示例:

~~~~json
{
    "msg": "请求成功",
    "code": 0,
    "data": {
        "enclosuId": 303
    },
    "operateSuccess": 1
}
~~~~

> 错误码

| 错误码    | 说明     |
| ------ | ------ |
| -10001 | 请求参数错误 |

### 更新电子围栏
> 接口描述

编辑围栏基本信息

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.extra.ElectronicFenceInterface.updateFencesEdit(ElectronicFenceDTO)
2. 测试例子位置：com.cmiot.test.ElectronicFenceTest.testUpdateElectronicFence()

> 请求方式

| 请求路径 | http://<HOST>:<PORT>platform/eleclosure/setEleclosureEdit |
| ---- | ------------------------------------ |
| 请求方法 | POST                                 |

> Header参数

| 参数           | 类型     | 说明             |
| ------------ | ------ | -------------- |
| companyToken | String | 鉴权companyToken |

> 请求参数

| 参数            | 类型      | 说明                                       |
| ------------- | ------- | ---------------------------------------- |
| companyId     | Long    | /**公司编号**/                               |
| enclosuRadius | Integer | /**围栏半径**/                               |
| enclosuId     | Long    | /**围栏编号**/                               |
| weekTime      | String  | /**星期几开启围栏 “0111111”   0是关闭  1是开启**/     |
| enclosuName   | String  | /**围栏的名称**/                              |
| enclosuType   | String  | /**围栏触发类型  [1是进入时告警 2是离开是告警 3是进入离开都告警]**/ |
| areaName      | String  | /**围栏中心地址**/                             |
| startTime     | String  | /**一天中围栏监控开始时间**/                        |
| endTime       | String  | /**一天中围栏监控结束时间 **/                       |
| deviceIds     | Array   | /**围栏绑定的设备**/                            |
| lon           | Double  |                                          |
| lat           | Double  |                                          |
|               |         |                                          |

> 输出参数

| 参数   | 类型   | 说明   |
| ---- | ---- | ---- |
|      |      |      |

示例:

~~~~json
{
    "msg": "请求成功",
    "code": 0,
    "operateSuccess": 1
}
~~~~

> 错误码

| 错误码    | 说明        |
| ------ | --------- |
| -31001 | 安全区域id不存在 |
### 更改围栏开关状态
> 接口描述

设置围栏临时关闭或开启

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.extra.impl.ElectronicFenceInterfaceImpl.updateFencesSwitch(ElectronicFenceDTO)
2. 测试例子位置：com.cmiot.test.ElectronicFenceTest.testUpdateSwitch()


> 请求方式

| 请求路径 | http://<HOST>:<PORT>platform/eleclosure/setOpenClosure |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数

| 参数           | 类型     | 说明             |
| ------------ | ------ | -------------- |
| companyToken | String | 鉴权companyToken |

> 请求参数

| 参数        | 类型     | 说明                        |
| --------- | ------ | ------------------------- |
| enclosuId | Long   | /**围栏编号**/ [必填]           |
| companyId | Long   | /**公司编号**/                |
| state     | String | /*** 围栏开关状态 0表示开，1表示关***/ |

> 输出参数

无

示例:

~~~~json
{
    "msg": "请求成功",
    "code": 0,
    "operateSuccess": 1
}
~~~~

> 错误码

| 错误码    | 说明        |
| ------ | --------- |
| -10001 | 请求参数错误    |
| -31001 | 安全区域id不存在 |

### 删除电子围栏
> 接口描述

 删除指定围栏
 
> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.extra.impl.ElectronicFenceInterfaceImpl.deleteFences(ElectronicFenceDTO)
2. 测试例子位置：com.cmiot.test.ElectronicFenceTest.testDeleteSwitch()

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/ platform/eleclosure/delectEleclosure |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数

| 参数           | 类型     | 说明             |
| ------------ | ------ | -------------- |
| companyToken | String | 鉴权companyToken |

> 请求参数

| 参数        | 类型   | 说明              |
| --------- | ---- | --------------- |
| enclosuId | Long | /**围栏编号**/ [必填] |
| companyId | Long | /**公司编号**/ [必填] |

> 输出参数

无

示例:

~~~~json
{
    "msg": "请求成功",
    "code": 0,
    "operateSuccess": 1
}
~~~~

> 错误码

| 错误码    | 说明     |
| ------ | ------ |
| -10001 | 请求参数错误 |


## 通讯录

为设备添加可拨号的通讯录，目前上限10人

### 获取通讯录
> 接口描述


获取设备通讯录中所有成员信息。

(普通用户界面: 当前账号的在第一条,管理员在第二条)

(管理员用户: 管理员在第一条)

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.extra.DeviceTelInerface.queryDeviceTel(DeviceTelDTO)
2. 测试例子位置：com.cmiot.test.DeviceTelTest.testqueryDeviceTel()

> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/device/contacts |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |

> Header参数


| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| companyToken | String | 鉴权token |

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
        "deviceId": 1
      },
      {
        "contactId": 2,
        "userType": 1,
        "contactPhoneNo": "112",
        "userId": 2,
        "telState" :1,
        "contactName": "小wang",
        "contactIconNo": 1,
        "deviceId": 1
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

### 添加联系人
> 接口描述


只有管理员才能主动添加新通讯录成员。

当有用户为未在app端注册，但是又想加入通讯录与定位器设备互相打电话，则可以通过这种方式。

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.extra.DeviceTelInerface.addDeviceTel(DeviceTelDTO)
2. 测试例子位置：com.cmiot.test.DeviceTelTest.testAddtDeviceTel()

> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/device/contacts |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> Header参数


| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| companyToken | String | 鉴权token |

> 请求参数


| 参数             | 类型     | 说明   |
| -------------- | ------ | ---- |
| contactName    | String | 姓名   |
| contactPhoneNo | String | 手机   |
| deviceId       | String | 设备ID |
请求示例：
~~~~json
{
   "contactName":"妈妈",
   "contactPhoneNo":"17673452111"
   "deviceId":"4560214"
}
~~~~
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

### 更新联系人
> 接口描述


管理员可以修改所有成员在通讯中的信息。

普通用户只能修改自己的信息。

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.extra.DeviceTelInerface.updateDeviceTel(DeviceTelDTO)
2. 测试例子位置：com.cmiot.test.DeviceTelTest.testUpdateDeviceTel()

> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/device/contacts |
| ---- | ---------------------------------------- |
| 请求方法 | PATCH                                    |

> Header参数


| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| companyToken | String | 鉴权token |

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
### 删除联系人
> 接口描述


管理员可以删除(除管路员外)所有成员在通讯中的信息。
管理员删除自己的通讯录调用恢复出厂设置的接口

普通用户1只能删除自己的信息。(相当于解绑设备)

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.extra.DeviceTelInerface.testDeleteDeviceTel(DeviceTelDTO)
2. 测试例子位置：com.cmiot.test.DeviceTelTest.testDeleteDeviceTel()

> 请求方式


| 请求路径 | http://<HOST>:<PORT>/platform/device/contacts |
| ---- | ---------------------------------------- |
| 请求方法 | DELETE                                   |

> Header参数


| 参数    | 类型     | 说明      |
| ----- | ------ | ------- |
| companyToken | String | 鉴权token |

> 请求参数


| 参数             | 类型      | 说明      |
| -------------- | ------- | ------- |
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
## 审批流
用户app扫码绑定会请求生成一次审批流供管理员审核
### 获取审批流
> 接口描述

  条件、分页查询通讯录审核信息

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.extra.BindApprovalInterface.queryBindApproval(BindApprovalDTO)
2. 测试例子位置：com.cmiot.test.BindApprovalTest.testQueryBindApproval() 
 

> 请求方式

  | 请求路径 | http://<HOST>:<PORT>/portal/indDevBing/findApproval |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

> 请求参数

  | 参数                | 类型       | 参数说明                |
  | ----------------- | -------- | ------------------- |
  | pageNum           | Integer  | 当前页                 |
  | pageSize          | Integer  | 每页条数                |
  | deviceIds         | String[] | 查询的设备Id，以数组的形式传入    |
  | approvalCondition | Integer  | 处理状态 0：未处理    1：已处理 |
  | startTime         | Long     | 起始时间                |
  | endTime           | Long     | 结束时间                |

> 输出参数

  | 参数                | 类型           | 参数说明                                     |
  | ----------------- | ------------ | ---------------------------------------- |
  | pageNum           | Integer      | 当前页                                      |
  | pageSize          | Integer      | 每页条数                                     |
  | pages             | Integer      | 总页数                                      |
  | total             | Integer      | 总记录数                                     |
  | list              | List<Object> | 通讯录审核信息结果集                               |
  | deviceId          | Integer      | 设备Id                                     |
  | userId            | Integer      | 用户Id                                     |
  | contactName       | String       | 用户姓名                                     |
  | contactPhoneNo    | String       | 联系人电话                                    |
  | userPhoneNo       | String       | 用户手机号码                                   |
  | requestTime       | Long         | 请求时间                                     |
  | approvalCondition | Integer      | 处理状态 0：未处理    1：已处理                      |
  | responseTime      | Long         | 响应时间                                     |
  | approvalResult    | Integer      | 审批结果0：拒绝    1：同意   2：自动通过                |
  | 其他数据              |              | 其他数据请参考com.github.pagehelper.PageInfo<T> |



>	示例:

	

```json
{
    "msg": "请求成功",
    "code": 0,
    "data": {
        "pageNum": 1,
        "pageSize": 10,
        "size": 2,
        "orderBy": null,
        "startRow": 1,
        "endRow": 2,
        "total": 2,
        "pages": 1,
        "list": [
            {
                "id": 20,
                "deviceId": "13723375",
                "userId": 1,
                "contactName": "李四",
                "contactPhoneNo": "17665358353",
                "userPhoneNo": "15712123797",
                "requestTime": 1513739587000,
                "approvalCondition": 0,
                "responseTime": null,
                "approvalResult": null
            },
            {
                "id": 50,
                "deviceId": "13723375",
                "userId": 7,
                "contactName": "大表哥",
                "contactPhoneNo": "15111811216",
                "userPhoneNo": "18825500000",
                "requestTime": 1513842789000,
                "approvalCondition": 0,
                "responseTime": null,
                "approvalResult": null
            }
        ],
        "prePage": 0,
        "nextPage": 0,
        "isFirstPage": true,
        "isLastPage": true,
        "hasPreviousPage": false,
        "hasNextPage": false,
        "navigatePages": 8,
        "navigatepageNums": [
            1
        ],
        "navigateFirstPage": 1,
        "navigateLastPage": 1,
        "lastPage": 1,
        "firstPage": 1
    },
    "operateSuccess": true
}
```



### 处理审批流

> 请求方式

| 请求路径 | http://<HOST>:<PORT>/platform/device/industry/addtel |
| ---- | ---------------------------------------- |
| 请求方法 | POST                                     |

> sdk接口位置

1. 调用接口位置：com.cmiot.interfaces.extra.BindApprovalInterface.dealBindApproval(BindApprovalDTO)
2. 测试例子位置：com.cmiot.test.BindApprovalTest.testDealBindApproval()

> Header参数

| 参数           | 类型     | 说明      |
| ------------ | ------ | ------- |
| companyToken | String | 鉴权token |

> 请求参数

| 参数             | 类型      | 说明                |
| -------------- | ------- | ----------------- |
| deviceId       | String  | 设备ID              |
| contactName    | String  | 联系人姓名             |
| contactPhoneNo | String  | 联系人手机号            |
| userId         | Integer | 用户id              |
| dealReslut     | Integer | 行业平台审批结果1.同意 0.拒绝 |

 

> 输出参数

无

> 示例:

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

# 信息推送
xmat平台通过消息队列推送给第三方平台的消息

备注:

dataType: data数据类型  
```
1—json对象    2—json数组
```
msgCreateTime:消息创建时间

msgType: 消息类型
```
 10001——SOS告警信息
 10002——电子围栏越界告警
 10003——低电量告警
 10004——SIM卡变更
 10005——关机告警
 10006——审批信息
```    

## 设备告警
包含设备sos告警、低电量告警、越界告警等信息

### sos 告警信息
 analyzeState # 1：位置解析成功 2：设备没有定位数据 3：位置解析失败。
 
 位置解析成功的数据 analyzeState是1
```json
{
    "data": {
        "id": 6525,
        "time": 1525676804000,
        "deviceName": "测试人员1",
        "locations": {
            "analyzeState":1, 
            "address":"广东省深圳市南山区海天二路(深圳湾创业广场内)",
            "lat":22.526043906087903,
            "locateType":3,
            "lon":113.93251926154431,
            "precision":30
        },
        "battery": {
            "status": 0,
            "energy": 57
        },
        "type": 1,
        "deviceId": "11393669"
    },
    "dataType": 1,
    "msgCreateTime": "2018-05-07 15:07:08",
    "msgType": 10001
}
```
 没有定位的数据，analyzeState 是2或是3
```json
{
    "data": {
        "id": 6525,
        "time": 1525676804000,
        "deviceName": "测试人员1",
        "locations": {
            "analyzeState":2
        },
        "battery": {
            "status": 0,
            "energy": 57
        },
        "type": 1,
        "deviceId": "11393669"
    },
    "dataType": 1,
    "msgCreateTime": "2018-05-07 15:07:08",
    "msgType": 10001
}
```


### 电子围栏越界告警
  一定会有位置数据
```json
{
    "data":{
        "time":1528430276000,
        "deviceName":"开发",
        "locations":{
            "lon":113.93364879292314,
            "address":"广东省深圳市南山区高新南九道(深圳湾创业广场内)",
            "locateType":2,
            "lat":22.52671637623923
        },
        "battery":{
            "status":0
        },
        "regionName":"开发的多边形",
        "alertCondition":1,
        "type":2,
        "deviceId":"23840577"
    },
    "dataType":1,
    "msgCreateTime":"2018-06-08 11:57:59",
    "msgType":10002
}
```


### 低电量告警

有位置数据
```json
{
    "data": {
        "id": 6514,
        "time": 1525664363000,
        "deviceName": "德吉",
        "locations": {
            "analyzeState":1,
            "lon": 113.93287206605521,
            "precision": 29,
            "address": "广东省深圳市南山区海天一路(深圳湾创业广场内)",
            "locateType": 3,
            "lat": 22.52683161578218
        },
        "battery": {
            "status": 0,
            "energy": 9
        },
        "type": 3,
        "deviceId": "24636610"
    },
    "dataType": 1,
    "msgCreateTime": "2018-05-07 11:39:38",
    "msgType": 10003
}
```

没有位置数据
```json
{
    "data": {
        "id": 6514,
        "time": 1525664363000,
        "deviceName": "德吉",
        "locations": {
            "analyzeState":3
        },
        "battery": {
            "status": 0,
            "energy": 9
        },
        "type": 3,
        "deviceId": "24636610"
    },
    "dataType": 1,
    "msgCreateTime": "2018-05-07 11:39:38",
    "msgType": 10003
}
```

### sim卡变更
```json
{

    "data": {
        "id": 6526,
        "time": 1525677027000,
        "type": 4,
        "deviceId": "23876295",
        "deviceName": "Test"
    },
    "dataType": 1,
    "msgCreateTime": "2018-05-07 15:09:36",
    "msgType": 10004
}
```

### 关机告警
```json
{
	"data": {
		"time": 1523173811000,
		"deviceName": "宇凡",
		"power": 0,
		"type": 5,
		"deviceId": "23840602"
	},
	"dataType": 1,
	"msgCreateTime": "2018-04-08 15:50:15",
	"msgType": 10005
}
```

## 待处理审批流
用户app扫码请求绑定行业平台设备时时会产生一个审批流
```json
{
    "data":{
        "requestTime":1528451104042,
        "contactName":"kk",
        "contactIconNo":0,
        "userId":238,
        "contactPhoneNo":"13828256494",
        "approvalCondition":0,
        "deviceId":"23840605"
    },
    "dataType":1,
    "msgCreateTime":"2018-06-08 17:45:04",
    "msgType":10006
}
```
## 特殊信息推送


### 设备定位数据
由xmat平台处理后的定位信息，主要包含定位精度，定位方式，gps经纬度
注：locateType类型 1:GPS 2:LBS 3:WIFI 4:BT

```json
{
    "data": {
	    "deviceId": "23840602",	
		"time":1514534547000,
        "lon": 113.93277588631825,
        "precision": 29,
        "address": "广东省深圳市南山区海天二路(深圳湾创业广场内)",
        "locateType": 3,
        "lat": 22.52642868527611
    },
    "dataType": 1,
    "msgCreateTime": "2018-05-30 15:09:36",
    "msgType": 20001
}
```

### 设备原始定位数据
设备上报的原始定位数据，主要设备采集到的wifi、蓝牙、lbs、gps数据等

```json
{
    "data": {
        "time": "2018-05-29 17:00:51",
        "SN": "107000217340000036",
        "IMEI": "866545030004367",
        "TrackMode": 4,
        "data": {
            "gsm": 100,
            "battery": "23%",
            "charging": 0,
            "step_count": 171,
            "location": {
                "type": "GPS|LBS|WIFI|BT",
                "lon": 113.93277588631825,
                "lat": 22.526574,
                "precision": 29,
                "LocationTime": "2018-05-2917: 00: 51",
                "LBSDATAS": [
                    {
                        "mcc": 460,
                        "mnc": 0,
                        "lac": 9341,
                        "cid": 4082,
                        "rssi": 52
                    }
                ],
                "WIFIDATAS": [
                    {
                        "ssid": "360-Plus12",
                        "rssi": -41,
                        "mac": "a0-95-0c-00-0d-50"
                    },
                    {
                        "ssid": "中移物联",
                        "rssi": -44,
                        "mac": "d8-38-0d-35-60-41"
                    },
                    {
                        "ssid": "C5",
                        "rssi": -44,
                        "mac": "d8-38-0d-35-60-42"
                    },
                    {
                        "ssid": "C5",
                        "rssi": -46,
                        "mac": "d8-38-0d-43-61-b2"
                    },
                    {
                        "ssid": "C5",
                        "rssi": -46,
                        "mac": "d8-38-0d-43-6b-02"
                    },
                    {
                        "ssid": "@PHICOMM_Guest",
                        "rssi": -47,
                        "mac": "6a-db-54-80-04-6b"
                    },
                    {
                        "ssid": "中移物联",
                        "rssi": -47,
                        "mac": "d8-38-0d-43-61-b1"
                    }
                ],
                "BTDATAS": [
                    {
                        "bt_rssi": -74,
                        "major": 37022,
                        "minor": 408
                    },
                    {
                        "bt_rssi": -80,
                        "major": 37022,
                        "minor": 429
                    },
                    {
                        "bt_rssi": -80,
                        "major": 37022,
                        "minor": 431
                    },
                    {
                        "bt_rssi": -81,
                        "major": 37022,
                        "minor": 441
                    },
                    {
                        "bt_rssi": -82,
                        "major": 37022,
                        "minor": 423
                    },
                    {
                        "bt_rssi": -83,
                        "major": 37022,
                        "minor": 440
                    }
                ],
                
            }
        }
    },
    "dataType": 1,
    "msgCreateTime": "2018-05-29 17:01:51",
    "msgType": 20002
}

```