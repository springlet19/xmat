------



#  x-Mat行业平台接口文档



------

| 版本/状态 | 责任人  | 修改日期       | 版本变更记录                 |
| ----- | ---- | ---------- | ---------------------- |
| v0.50 | 魏立誉  | 2018/04/23 | 人员信息/人员分配设备批量导入        |
| v0.40 | 魏立誉  | 2018/03/26 | 新增获取通讯录列表接口            |
| v0.30 | 魏立誉  | 2018/03/12 | 新增查询部门负责人接口            |
| v0.20 | 魏立誉  | 2018/01/22 | 获取组织机构树形结构数据(全) 配合前端重构 |
| v0.10 | 魏立誉  | 2017/12/12 | x-Mat行业平台接口文档整合        |



[TOC]





## 概述

略

### Header说明

| 参数           | 类型     | 说明                                       |
| ------------ | ------ | ---------------------------------------- |
| Content-Type | String | 请求类型，若无特别，均为“application/json;charset=utf-8” |
| User-Agent   | String | 客户端类型，WEB                                |
| token        | String | 通行token(所有需要认证的接口都需要带上)                  |
| iuserMobile  | String | 用户名(手机号码) (所有需要认证的接口都需要带上)               |

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

| 参数   | 类型            | 参数说明            |
| ---- | ------------- | --------------- |
| code | Integer       | 错误码，参考错误码文档     |
| msg  | String        | 错误信息            |
| data | Object\|Array | 响应内容，如无响应内容则不返回 |

后续所有响应参数说明不再描述这3个参数。

### 错误码说明

错误码用于定位错误类型，原则上每个错误码都代表唯一的错误类型。

特别地定义了一组通用错误码：

| 错误码    | 错误信息         |
| ------ | ------------ |
| 0      | 请求成功         |
| 1      | 请求部分成功       |
| -10000 | 系统错误         |
| -10001 | 请求参数错误       |
| -10002 | token已失效     |
| -10011 | token已失效(挤掉) |

后续所有接口的错误返回均有可能返回通用错误码，所以不再重复列举描述。

---

## 部门模块

### 1.1 获取部门数据

- #### 接口描述

  通过部门ID获取当前部门和所有子部门数据

- #### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/industryOrg/dept/data |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |


- #### 请求参数


| 参数       | 类型      | 说明     |
| -------- | ------- | ------ |
| iorgId   | Long    | 部门ID   |
| pageNo   | Integer | 当前页    |
| pageSize | Integer | 每页显示数量 |

- #### 输出参数

| 参数                      | 类型      | 说明                 |
| :---------------------- | ------- | ------------------ |
| orgs                    | Array   | 部门信息               |
| orgs.iorgId             | Long    | 部门ID               |
| orgs.iorgName           | String  | 部门名称               |
| orgs.iorgLeaderName     | String  | 部门负责人              |
| orgs.iorgLeaderPhone    | String  | 部门负责人手机号码          |
| orgs.monitorCountDirect | Integer | 部门人员数量（包含所有子级部门人员） |
| orgs.iuserId            | Long    | 部门负责人ID            |
| orgs.iuserStatus        | Integer | 负责人标识(0:手动,1:系统)   |
| orgs.iorgType           | Integer | 组织类型（1：公司/集团；2：部门） |

示例

```json
[{
  "iorgId":73,
   "iorgLeaderName":"admin",
   "iorgLeaderPhone":"13265656565",
   "iorgName":"中移物联网",
   "iuserId":84,
   "monitorCountDirect":7,
   "iuserStatus":1
  },
  {
    "iorgId":81,
   "iorgLeaderName":"萨芬",
   "iorgLeaderPhone":"13265654688",
   "iorgName":"飒飒部",
   "iorgParentId":73,
   "iuserId":102,
   "monitorCountDirect":6,
   "iuserStatus":1
  },
  ...
] 
```



### 1.2 获取组织机构树形结构数据(全)

- #### 接口描述

  通过部门ID获取对应组织机构数据

- #### 请求方式

- | 请求路径 | http://<HOST>:<PORT>/system/industryOrg/orgDataZTree |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |

- #### 请求参数

  | 参数     | 类型   | 说明   |
  | ------ | ---- | ---- |
  | iorgId | Long | 部门ID |

  ​

- #### 输出参数

  | 参数          | 类型     | 说明    |
  | ----------- | ------ | ----- |
  | nodes       | Array  | 节点数据  |
  | nodes.id    | Long   | 节点ID  |
  | nodes.name  | String | 节点名称  |
  | nodes.pid   | Long   | 父节点ID |
  | nodes.pName | String | 父节点名称 |

  ​

- ### 错误码

  | 错误码    | 说明    |
  | ------ | ----- |
  | -31000 | 部门不存在 |

  ​

示例:

```json
{
    "id":73,
    "name":"中移物联网",
    "children":[
        {
            "id":84,
            "name":"飞洒1",
            "children":[
                {
                    "id":86,
                    "name":"测试部",
                    "children":[
                        Object{...},
                        Object{...}
                    ]
                },
                Object{...},
                Object{...},
                Object{...},
                Object{...}
            ]
        },
        Object{...},
        Object{...},
        Object{...},
        Object{...},
        Object{...},
        Object{...},
        Object{...},
        Object{...},
        Object{...},
        Object{...},
        Object{...},
        Object{...},
        Object{...}
    ]
}
```

### 1.3 增加/修改部门

- #### 接口描述

  新增部门 : 创建部门同时生成部门负责人

  修改部门 : 修改部门数据

- #### 请求方法

  | 请求路径 | http://<HOST>:<PORT>/system/industryOrg/editDept |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

- #### 请求参数

  | 参数              | 类型     | 说明                 |
  | --------------- | ------ | ------------------ |
  | iorgId          | Long   | 部门ID(传此参数代表更新不传新增) |
  | iorgName        | String | 部门名称               |
  | iorgLeaderName  | String | 部门负责人名称            |
  | iorgLeaderPhone | String | 部门负责人手机号码          |
  | iorgParentId    | Long   | 上级部门ID             |

- #### 输出参数

  无

- ### 错误码

  | 错误码    | 说明                              |
  | ------ | ------------------------------- |
  | -31000 | 部门不存在                           |
  | -31002 | 部门负责人联系电话已经存在                   |
  | -31003 | 存在相同部门                          |
  | -31005 | 请输入长度1~20部门负责人名称(中文、英文、数字包括下划线) |
  | -31006 | 请输入正确联系手机号码                     |
  | -31007 | 请输入长度1~20部门名称(中文、英文、数字包括下划线)    |
  | -31008 | 上级部门为空                          |
  | -31009 | 上级部门不能为本部门                      |
  | -31010 | 上级部门不能为该部门的子部门                  |

  ​

  示例:

  ```json
  {
    "code": 0,
    "msg": "请求成功"
  }
  ```

### 1.4 删除部门

- ### 接口描述

  删除部门包含部门下的所有子部门和各部门的负责人

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/industryOrg/delete/dept |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |


- ### 请求参数

  | 参数     | 类型   | 说明   |
  | ------ | ---- | ---- |
  | iorgId | Long | 部门ID |

- ### 错误码

  | 错误码    | 说明             |
  | ------ | -------------- |
  | -31000 | 部门不存在          |
  | -31004 | 不能删除本部门        |
  | -31011 | 不能删除此架构,权限不足   |
  | -31012 | 部门下有设备，不可执行此操作 |
  | -31001 | 部门下有人员，不可执行此操作 |

- ### 输出参数

  无

  示例:

  ```json
  {
    "code": 0,
    "msg": "请求成功"
  }
  ```



### 1.5 查询部门负责人

- ### 接口描述

  查询部门负责人列表

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/industryOrg/queryPrincipal |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |


- ### 请求参数

  | 参数     | 类型   | 说明   |
  | ------ | ---- | ---- |
  | iorgId | Long | 部门ID |

- ### 错误码

  | 错误码    | 说明    |
  | ------ | ----- |
  | -31000 | 部门不存在 |

- ### 输出参数

  | 参数          | 类型      | 说明               |
  | ----------- | ------- | ---------------- |
  | iuserId     | Long    | 负责人ID            |
  | iorgId      | Long    | 部门ID             |
  | iuserName   | String  | 负责人名称            |
  | iuserMobile | String  | 负责人手机号           |
  | iuserStatus | Integer | 负责人标识(0:手动,1:系统) |

  ​

  示例:

  ~~~json
  {
    "msg":"请求成功",
    "code":0,
    "data":[{"iuserId":84,"iuserMobile":"13265656565","iuserName":"admin"}, 
            {"iuserId":111,"iuserMobile":"17688737595","iuserName":"魏生"}],
    "operateSuccess":true
  }
  ~~~

  ​

### 1.6 新增部门负责人

- ### 接口描述

  新增部门负责人

  条件 : 手机号为登录用户。不能重复

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/industryUser/addPrincipal |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

- ### 请求参数

  | 参数          | 类型     | 说明        |
  | ----------- | ------ | --------- |
  | iorgId      | Long   | 部门ID      |
  | iuserName   | String | 部门负责人名称   |
  | iuserMobile | String | 部门负责人手机号码 |

- ### 错误码

  | 错误码    | 说明                            |
  | ------ | ----------------------------- |
  | -21009 | 手机号已经存在                       |
  | -21011 | 请输入长度1~20负责人名称(中文、英文、数字包括下划线) |
  | -21012 | 请输入正确的手机号                     |
  | -31000 | 部门不存在                         |

  ​

- ### 输出参数

  无

  示例:

  ```json
  {
    "code": 0,
    "msg": "请求成功"
  }
  ```



### 1.7 删除部门负责人

- ### 接口描述

  删除部门负责人

  条件 : 手机号为登录用户。不能重复

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/industryUser/deletePrincipal |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |

- ### 请求参数

  | 参数      | 类型   | 说明    |
  | ------- | ---- | ----- |
  | iuserId | Long | 负责人ID |

- ### 错误码

  | 错误码    | 说明                            |
  | ------ | ----------------------------- |
  | -21009 | 手机号已经存在                       |
  | -21011 | 请输入长度1~20负责人名称(中文、英文、数字包括下划线) |
  | -21012 | 请输入正确的手机号                     |
  | -31000 | 部门不存在                         |

- ### 输出参数

  无

  示例:

  ```json
  {
    "code": 0,
    "msg": "请求成功"
  }
  ```

### 1.8编辑部门负责人

- ### 接口描述

  编辑部门负责人

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/industryUser/deletePrincipal |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |

- ### 请求参数

  | 参数      | 类型   | 说明    |
  | ------- | ---- | ----- |
  | iuserId | Long | 负责人ID |

- ### 错误码

  | 错误码    | 说明                            |
  | ------ | ----------------------------- |
  | -21009 | 手机号已经存在                       |
  | -21011 | 请输入长度1~20负责人名称(中文、英文、数字包括下划线) |
  | -21012 | 请输入正确的手机号                     |
  | -31000 | 部门不存在​                        |

- ### 输出参数

  无

  示例:

  ```json
  {
    "code": 0,
    "msg": "请求成功"
  }
  ```

### 

## 人员模块

### 2.1 人员新增

- ### 接口描述

​       添加新人员

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/monitorManager/addMonitor |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

- ### 请求参数

  | 参数             | 类型     | 说明        |
  | -------------- | ------ | --------- |
  | monitorIorgId  | Long   | 部门ID      |
  | monitorName    | String | 人员名称      |
  | contactPhone   | String | 人员手机号码    |
  | emergencyName  | String | 紧急联系人名称   |
  | emergencyPhone | String | 紧急联系人手机号码 |

- ### 错误码

  | 错误码    | 说明                            |
  | ------ | ----------------------------- |
  | -80000 | 人员已经存在                        |
  | -80001 | 请输入长度1~20人员名称(中文、英文、数字包括下划线)  |
  | -80002 | 请输入长度1~20紧急联系人(中文、英文、数字包括下划线) |
  | -80003 | 请输入正确联系人手机号码                  |
  | -80004 | 请输入正确手机号码                     |
  | -80005 | 请完善紧急联系人信息或者不录入               |
  | -80006 | 请选择所属部门                       |
  | -31000 | 部门不存在                         |

  ​

- ### 输出参数

  无

  示例:

  ```json
  {
    "code": 0,
    "msg": "请求成功"
  }
  ```

### 2.2 人员编辑

- ### 接口描述

​        人员信息变更

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/monitorManager/updateMonitor |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

- ### 请求参数

  | 参数             | 类型     | 说明        |
  | -------------- | ------ | --------- |
  | monitorId      | Long   | 人员ID      |
  | monitorName    | String | 人员名称      |
  | contactPhone   | String | 人员手机号码    |
  | emergencyName  | String | 紧急联系人名称   |
  | emergencyPhone | String | 紧急联系人手机号码 |
  | monitorIorgId  | Long   | 部门ID      |

- ### 错误码

  | 错误码    | 说明                            |
  | ------ | ----------------------------- |
  | -80000 | 人员已经存在                        |
  | -80001 | 请输入长度1~20人员名称(中文、英文、数字包括下划线)  |
  | -80002 | 请输入长度1~20紧急联系人(中文、英文、数字包括下划线) |
  | -80003 | 请输入正确联系人手机号码                  |
  | -80004 | 请输入正确手机号码                     |
  | -80005 | 请完善紧急联系人信息或者不录入               |
  | -80006 | 请选择所属部门                       |
  | -31000 | 部门不存在                         |
  | -10009 | 相关数据请求失败，稍后重试                 |

  ​

- ### 输出参数

  无

  示例:

  ```json
  {
    "code": 0,
    "msg": "请求成功"
  }
  ```

### 2.3 人员删除

- ### 接口描述    

​        删除人员

​        注: 不能删除已成为部门负责人的人员

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/monitorManager/deleteMonitor |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |

- ### 请求参数

  | 参数        | 类型     | 说明                  |
  | --------- | ------ | ------------------- |
  | monitorId | Long   | 人员ID                |
  | deviceSn  | String | 设备Sn(当人员绑有设备需要传此参数) |

- ### 错误码

  | 错误码    | 说明            |
  | ------ | ------------- |
  | -10009 | 相关数据请求失败，稍后重试 |

  ​

- ### 输出参数

  无

  示例:

  ```json
  {
    "code": 0,
    "msg": "请求成功"
  }
  ```

### 2.4 人员获取

- ### 接口描述

  获取人员信息列表

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/monitorManager/queryMonitor |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |

- ### 请求参数

  | 参数            | 类型      | 说明         |
  | ------------- | ------- | ---------- |
  | monitorIorgId | Long    | 部门ID       |
  | pageNo        | Integer | 当前页        |
  | pageSize      | Integer | 每页显示数量     |
  | monitorName   | String  | 人员名称（可选）   |
  | contactPhone  | String  | 人员手机号码（可选） |

- ### 输出参数

  | 参数                                   | 类型      | 说明                          |
  | ------------------------------------ | ------- | --------------------------- |
  | MointorListVo                        | Array   | 人员信息                        |
  | MointorListVo.monitorId              | Long    | 人员ID                        |
  | MointorListVo.monitorName            | String  | 人员名称                        |
  | MointorListVo.contactPhone           | String  | 人员手机号码                      |
  | MointorListVo.emergencyName          | String  | 紧急联系人名称                     |
  | MointorListVo.emergencyPhone         | String  | 紧急联系人手机号码                   |
  | MointorListVo.monitorIorgId          | Long    | 部门ID                        |
  | MointorListVo.deviceSn               | String  | 设备Sn                        |
  | MointorListVo.freq                   | Integer | 设备模式 (0-省电模式 4-普通模式 2-追踪模式) |
  | MointorListVo.switchState            | Integer | 设备开关机状态(0是关机1是开机)           |
  | MointorListVo.orgName                | String  | 部门名称                        |
  | MointorListVo.monitorCompanyId       | Long    | 公司ID                        |
  | count                                | Integer | 总数量                         |
  | pageCount                            | Integer | 总页数                         |
  | MointorListVo. deviceDetailVO.online | Integer | 设备长连接在线状态 0离线 1在线           |

  示例:

  ```json
  {
    "code": 0,
    "data":{
      "contactPhone":"13265654577",
      "deviceId":"13723376",
      "deviceSn":"102000218290000102",
      "emergencyName":"撒发生",
      "emergencyPhone":"13265656565",
      "freq":4,
      "monitorCompanyId":5,
      "monitorId":108,
      "monitorIorgId":81,
      "monitorName":"复旦复华",
      "orgName":"飒飒部",
      "orgParentId":73,
      "switchState":1
    },
    "pageInfo":{
      "pageCount":2,
    	"count":20
    },
    "msg": "请求成功"
  }
  ```

### 2.5 人员分配设备

- ### 接口描述

​        手动输入设备Sn给人员分配设备,

​	分配条件: 1.设备必须是未分配

​                          2.紧急联系人信息必须设置

​                          3.该设备属与该公司

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/monitorManager/monitorBand |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

- ### 请求参数

  | 参数        | 类型     | 说明   |
  | --------- | ------ | ---- |
  | monitorId | Long   | 人员ID |
  | deviceSn  | String | 设备Sn |

- ### 错误码

  | 错误码    | 说明            |
  | ------ | ------------- |
  | -80007 | 人员不存在         |
  | -50000 | 设备不存在         |
  | -50001 | 设备已分配         |
  | -50002 | 请先输入设备SIM号码   |
  | -10009 | 相关数据请求失败，稍后重试 |

  ​

- ### 输出参数

  无

  示例:

  ```json
  {
    "code": 0,
    "msg": "请求成功"
  }
  ```

### 2.6 人员随机分配设备

- ### 接口描述

​        随机分配设备给人员

​	分配条件: 紧急联系人信息必须设置

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/monitorManager/monitorRndBand |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

- ### 请求参数

  | 参数            | 类型   | 说明               |
  | ------------- | ---- | ---------------- |
  | monitorId     | Long | 人员ID             |
  | monitorIorgId | Long | 部门ID(登录用户所属部门ID) |

- ### 错误码

  | 错误码    | 说明            |
  | ------ | ------------- |
  | -80007 | 人员不存在         |
  | -31000 | 部门不存在         |
  | -50003 | 没有可分配的设备      |
  | -50004 | 设备没有SIM号码     |
  | -10009 | 相关数据请求失败，稍后重试 |

  ​

- ### 输出参数

​        无

​        示例:

```json
   {

  "code": 0,

  "msg": "请求成功"

}
```

### 2.7 人员批量删除

- ### 接口描述

​        批量删除人员 (人员若已分配设备需传设备Sn)

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/monitorManager/deleteMonitorList |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

- ### 请求参数

  | 参数        | 类型     | 说明                   |
  | --------- | ------ | -------------------- |
  | monitorId | String | 字符串数组(人员ID和已分配设备Sn ) |

- ### 错误码

  | 错误码    | 说明            |
  | ------ | ------------- |
  | -10009 | 相关数据请求失败，稍后重试 |

- ### 输出参数

  | 参数         | 类型      | 说明       |
  | ---------- | ------- | -------- |
  | total      | Integer | 要删除的总记录数 |
  | success    | Integer | 删除成功总记录数 |
  | deviceSucc | Integer | 解绑成功总记录数 |

  示例:

  ```json
  {
    "code": 0,
    "data":{
      	"total":10,
      	"success":10,
      	"deviceSucc":5
    }
    "msg": "请求成功"
  }
  ```

### 2.8 人员信息/人员分配设备批量导入

- ### 接口描述

  使用excel模板导入人员信息/ 人员分配设备

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/monitorManager/importMonitor |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

- ### 请求参数

  | 参数   | 类型   | 说明        |
  | ---- | ---- | --------- |
  | file | file | excel模板文件 |

- ### 错误码

  | 错误码    | 说明              |
  | ------ | --------------- |
  | -80008 | excel解析失败返回失败原因 |

  ​

- ### 输出参数

  | 参数                         | 类型      | 说明                   |
  | -------------------------- | ------- | -------------------- |
  | total                      | Integer | 上传总数量                |
  | success                    | Integer | 成功数量                 |
  | errorOrgList               | Array   | 不存在的部门数组             |
  | errorDeviceList            | Array   | 设备不存在/已激活/已分配/sim卡重复 |
  | errorMonitorList           | Array   | 部门下相同的人员             |
  | errorMonitorBandDeviceList | Array   | 人员已绑定设备              |

  示例:

  ```jsoN
  {
    "code": 0,
    "data":{
      	"total":10,
      	"success":9,
      	"errorOrgList":["会计部"],
          "errorMonitorList":["李四"],
          "errorDeviceList":["102000218290002118"]
    }
    "msg": "请求成功"
  }
  ```

###  2.9 发送手机验证码

- ### 接口描述

  通过部门负责人手机号码获取六位数验证码（2分钟内有效）

- ### 请求方式

  | 请求路径 | httphttp://<HOST>:<PORT>/sendVerifyCode |
  | ---- | --------------------------------------- |
  | 请求方法 | GET                                     |

- ### 请求参数

  | 参数          | 类型     | 说明     |
  | ----------- | ------ | ------ |
  | iuserMobile | String | 人员手机号码 |

- ###  输出参数

  无

  示例:

  ```json
  {
    "code": 0,
    "msg": "请求成功"
  }
  ```

### 2.10 找回密码

- ### 接口描述 

  通过手机验证码重置人员（部门负责人）密码

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/user/resetPassword |
  | ---- | --------------------------------------- |
  | 请求方法 | POST                                    |

- ### 请求参数

  | 参数         | 类型     | 说明     |
  | ---------- | ------ | ------ |
  | verifyCode | String | 验证码    |
  | mobile     | String | 人员手机号码 |
  | newPwd     | String | 新密码    |
  | repeatPwd  | String | 确认密码   |

- ### 输出参数

  无

  示例:

  ```json
  {
    "code": 0,
    "msg": "请求成功"
  }
  ```

### 2.11 修改密码

- ### 接口描述

  人员（部门负责人）密码修改

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/changePassword |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

- ### 请求参数

  | 参数        | 类型     | 说明   |
  | --------- | ------ | ---- |
  | oldPwd    | String | 原密码  |
  | newPwd    | String | 新密码  |
  | repeatPwd | String | 确认密码 |

- ### 输出参数

  无

  示例:

  ```json
  {
    "code": 0,
    "msg": "请求成功"
  }
  ```

  ### 错误码

  | 错误码    | 说明          |
  | ------ | ----------- |
  | -21013 | 新密码和原密码一样   |
  | -21014 | 请输入原密码      |
  | -21016 | 请输入新密码      |
  | -21017 | 请再次输入新密码    |
  | -21018 | 新密码不一致      |
  | -21019 | 请输入6~12位新密码 |
  | -21020 | 原密码错误       |

  ​

### 2.12 登录

- ### 接口描述

  人员手机号密码登录

- ### 请求方式 

  | 请求路径 | http://<HOST>:<PORT>/login |
  | ---- | -------------------------- |
  | 请求方法 | POST                       |

- ### 请求参数

  | 参数            | 类型     | 说明     |
  | ------------- | ------ | ------ |
  | iuserMobile   | String | 人员手机号码 |
  | iuserPassword | String | 密码     |

- ### 输出参数

  | 参数          | 类型     | 说明                    |
  | ----------- | ------ | --------------------- |
  | iuserId     | Long   | 用户ID                  |
  | iorgId      | Long   | 部门ID                  |
  | companyId   | Long   | 公司ID                  |
  | iuserName   | String | 用户名称                  |
  | iorgName    | String | 部门名称                  |
  | token       | String | 通行token               |
  | iuserState  | String | 初次登录系统是否修改过密码 (0否 1是) |
  | companyName | String | 公司名称                  |
  | sysName     | String | 公司备注信息                |

  ​

  示例:

  ```json
  {
      "msg": "请求成功",
      "code": 0,
      "data": {
          "iuserId": 106,
          "iorgId": 84,
          "companyId": 5,
          "iuserName": "admin",
          "iorgName": "飞洒1",
          "token": "91f492ff13ad34b3669d1aba979e4056"
      },
      "operateSuccess": true
  }
  ```

  ​

### 2.13 退出登录

- ### 接口描述

  退出登录

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/logout |
  | ---- | ---------------------------------- |
  | 请求方法 | GET                                |

- ### 请求参数

  无

- ### 输出参数

  无

  示例:

  ```json
  {
    "code": 0,
    "msg": "请求成功"
  }
  ```



### 2.14 人员模板下载

- ### 接口描述

  人员模板提供导入数据样式

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/monitorManager/downloadExcelTemplate |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |

- ### 请求参数

  无

- ### 输出参数





### 2.15人员批量导出

- ### 接口描述

  批量导出人员

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/monitorManager/exportMonitor |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

- ### 请求参数

  | 参数            | 类型    | 说明          |
  | ------------- | ----- | ----------- |
  | monitorIorgId | Long  | 部门ID(所选)    |
  | monitorIdList | Array | 需要导出的人员ID数组 |

  ​

- ### 输出参数

  无


### 2.16 人员批量绑定设备模板下载

- ### 接口描述

  人员模板提供导入数据样式

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/monitorManager/downloadMonitorBatchBandTemplate |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |

- ### 请求参数

  无

- ### 输出参数



### 2.17 人员批量绑定设备导入

- ### 接口描述

  使用excel模板导入人员信息 

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/monitorManager/monitorBatchBandDeviceImport |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

- ### 请求参数

  | 参数   | 类型   | 说明                |
  | ---- | ---- | ----------------- |
  | file | file | excel人员批量绑定设备模板文件 |

- ### 错误码

  | 错误码    | 说明              |
  | ------ | --------------- |
  | -80008 | excel解析失败返回失败原因 |

  ​

- ### 输出参数

  | 参数               | 类型      | 说明             |
  | ---------------- | ------- | -------------- |
  | total            | Integer | 上传总数量          |
  | success          | Integer | 成功数量           |
  | errorOrgList     | Array   | 不存在的部门数组       |
  | errorMonitorList | Array   | 部门下相同的人员       |
  | errorDeviceList  | Array   | 设备不存在,已绑定或者已激活 |

  示例:

  ```jsoN
  {
    "code": 0,
    "data":{
      	"total":10,
      	"success":9,
      	"errorOrgList":["会计部"],
          "errorMonitorList":["李四"],
          "errorDeviceList":["102000217340000341"]
    }
    "msg": "请求成功"
  }
  ```



## 告警模块

### 3.1 获取告警信息

- ### 接口描述

  获取部门下分配设备的人员产生的告警信息

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/alertmanager/data |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |

- ### 请求参数

  | 参数          | 类型      | 说明                                       |
  | ----------- | ------- | ---------------------------------------- |
  | iorgId      | Long    | 部门ID                                     |
  | pageNo      | Integer | 当前页                                      |
  | pageSize    | Integer | 每页显示数量                                   |
  | startTime   | Long    | 开始时间（可选）                                 |
  | endTime     | Long    | 结束时间（可选）                                 |
  | alarmType   | Integer | 告警类型（1.SOS告警 2.电子围栏告警 3.低电报警 4.SIM卡变更报警5.关机报警）（可选） |
  | iuserMobile | String  | 人员名称/设备Sn（可选）                            |

- ### 错误码

  | 错误码    | 说明    |
  | ------ | ----- |
  | -90001 | 无相关数据 |

- ### 输出参数

  | 参数                         | 类型      | 说明                                       |
  | -------------------------- | ------- | ---------------------------------------- |
  | AlertMessageVo             | Array   | 告警信息                                     |
  | AlertMessageVo.monitorName | String  | 人员名称                                     |
  | AlertMessageVo.iorgName    | String  | 部门名称                                     |
  | AlertMessageVo.deviceSn    | String  | 设备Sn                                     |
  | AlertMessageVo.iuserMobile | String  | 人员手机号码                                   |
  | AlertMessageVo.time        | String  | 告警时间                                     |
  | AlertMessageVo.address     | String  | 告警地址                                     |
  | AlertMessageVo.alarmType   | Integer | 告警类型（1.SOS告警 2.电子围栏告警 3.低电报警 4.SIM卡变更报警5.关机报警） |
  | count                      | Integer | 总数量                                      |
  | pageCount                  | Integer | 总页数                                      |

​       示例:

```json
{
  "code": 0,
  "data":[
    {
      "monitorName":"张三",
      "iorgName":"研发不",
      "deviceSn":"102000217340000341",
      "iuserMobile":"15100107762",
      "time":"2017-12-13 15:50:43",
      "address":"广东省深圳市南山区海科路(深圳市软件产业基地(学府路)附近8米)"
      "alarmType":1
    }
  ],
  "pageInfo":{
      "pageCount":1,
      "count":1
  },
  "msg": "请求成功"
}
```

### 3.2 告警信息导出

- ### 接口描述

  导出告警信息

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/ system/alertmanager/exportAlertMessage |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

- ### 请求参数

  | 参数          | 类型    | 说明       |
  | ----------- | ----- | -------- |
  | alertIdList | Array | 告警信息ID数组 |

- ### 输出参数


### 3.3 Websocket连接

- ### 接口描述

websocket连接

- ### 请求方式

| 请求路径 | ws://<HOST>:7397 |
| ---- | ---------------- |
| 请求方法 |                  |

- ### 请求参数

| 参数          | 类型     | 说明      |
| ----------- | ------ | ------- |
| token       | String | 通行token |
| iuserMobile | String | 手机号码    |

- ### 输出参数

  无



### 3.4通知更新已读状态

- ### 接口描述

  通知更新已读状态

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/ system/alertmanager/updateNoticeCount |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

  ​

- ### 请求参数

  | 参数     | 类型   | 说明   |
  | ------ | ---- | ---- |
  | userId | Long | 用户id |

  ​

- ### 输出参数

  示例

  ```json
  {
      "msg": "请求成功",
      "code": 0,
      "data": [
          {
              "noticeId": 14,
              "noticeCount": 0,
              "noticeState": 1,
              "noticeModular": 10001,
              "noticeTime": 1527748064000,
              "noticeUpdateTime": 1528188357199,
              "userId": 111
          },
          {
              "noticeId": 22,
              "noticeCount": 0,
              "noticeState": 1,
              "noticeModular": 10002,
              "noticeTime": 1527468516000,
              "noticeUpdateTime": 1528188357211,
              "userId": 111
          },
          {
              "noticeId": 26,
              "noticeCount": 0,
              "noticeState": 1,
              "noticeModular": 0,
              "noticeTime": 1526522666000,
              "noticeUpdateTime": 1528188357216,
              "userId": 111
          },
          {
              "noticeId": 39,
              "noticeCount": 0,
              "noticeState": 1,
              "noticeModular": 10005,
              "noticeTime": 1527500381000,
              "noticeUpdateTime": 1528188357220,
              "userId": 111
          },
          {
              "noticeId": 48,
              "noticeCount": 0,
              "noticeState": 1,
              "noticeModular": 10003,
              "noticeTime": 1527675685000,
              "noticeUpdateTime": 1528188357225,
              "userId": 111
          }
      ],
      "num": 5,
      "operateSuccess": true
  }
  ```

  ​



### 3.5查询通知状态

- ### 接口描述

  根据返回的值来确定是否加小红点

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/ system/alertmanager/queryNoticeCount |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

  ​

- ### 请求参数

  | 参数     | 类型   | 说明   |
  | ------ | ---- | ---- |
  | userId | Long | 用户id |

  ​

- ### 输出参数

  示例（只有第一种加小红点）

  ```json
  {
      "msg": "请求成功",
      "code": 0,
      "data": {
          "noticeId": 14,
          "noticeCount": 96,
          "noticeState": 0,
          "noticeModular": 10001,
          "noticeTime": 1527748064000,
          "noticeUpdateTime": null,
          "userId": 111
      },
      "operateSuccess": true
  }

  {
      "msg": "数据库中无此通知！",
      "code": -10000,
      "operateSuccess": false
  }

  {
      "msg": "数据库中不存在未读的通知",
      "code": -10000,
      "operateSuccess": false
  }
  ```

  ​

## xmat 和 xmat运维平台 推送模块

### 4.1 xmat运维平台推送公司信息

- ### 接口描述

  xmat运维平台新增公司推送到本平台生成对应的公司组织架构和root用户

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/user/addCompanyInfo |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

- ### 请求参数

  | 参数               | 类型     | 说明        |
  | ---------------- | ------ | --------- |
  | id               | Long   | 公司ID      |
  | companyName      | String | 公司名称      |
  | token            | String | Token     |
  | companyAdminUser | String | 公司负责人名称   |
  | userPhoneNo      | String | 公司负责人手机号码 |

- ### 错误码

  | 错误码    | 说明                            |
  | ------ | ----------------------------- |
  | -40000 | 请输入长度1~20公司名称(中文、英文、数字包括下划线)  |
  | -40001 | 存在相同公司                        |
  | -21011 | 请输入长度1~20负责人名称(中文、英文、数字包括下划线) |
  | -21012 | 请输入正确的手机号                     |
  | -21009 | 手机号已经存在                       |

  ​

- ### 输出参数

  无

  示例:

  ```json
  {
    "code": 0,  
    "msg": "请求成功"
  }
  ```

### 4.2 xmat运维平台推送设备信息 

- ### 接口描述

   xmat运维平台定时推送设备信息,做新增更新操作

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/deviceManager/pushCompanyDevice |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

- ### 请求参数

  | 参数                   | 类型      | 说明                 |
  | -------------------- | ------- | ------------------ |
  | list                 | Array   | 设备信息               |
  | list.companyId       | Long    | 公司ID               |
  | list.deviceSn        | String  | 设备Sn               |
  | list.deviceId        | String  | 设备ID               |
  | list.deviceImei      | String  | 设备imsi             |
  | list.devicePhoneNo   | String  | 设备SIM卡号码           |
  | list.deviceStructure | Integer | 设备结构类型（1.插卡版2.贴片版） |

- ### 输出参数

  无

  示例:

  ```json
  {
    "code": 0,  
    "msg": "请求成功"
  }
  ```




## 位置服务模块

### 5.1 获取部门下的人员设备信息

- ### 接口描述

根据部门ID 获取已绑定设备的人员

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/monitorManager/queryMonitor |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |

- ### 请求参数

  | 参数            | 类型     | 说明                           |
  | ------------- | ------ | ---------------------------- |
  | monitorIorgId | Long   | 部门                           |
  | lbsMng        | String | 位置服务模块标识(值为:locationService) |

- ### 输出参数

  | 参数                                       | 类型      | 说明                            |
  | ---------------------------------------- | ------- | ----------------------------- |
  | MointorListVo                            | Array   | 人员信息                          |
  | MointorListVo.monitorId                  | Long    | 人员ID                          |
  | MointorListVo.monitorName                | String  | 人员名称                          |
  | MointorListVo.contactPhone               | String  | 人员手机号码                        |
  | MointorListVo.monitorIorgId              | Long    | 部门ID                          |
  | MointorListVo.deviceSn                   | String  | 设备Sn                          |
  | MointorListVo.orgName                    | String  | 部门名称                          |
  | MointorListVo.monitorCompanyId           | Long    | 公司ID                          |
  | MointorListVo. deviceDetailVO            | Object  | 设备详细信息                        |
  | MointorListVo. deviceDetailVO.deviceId   | String  | 设备ID                          |
  | MointorListVo. deviceDetailVO. locations | Object  | 位置信息                          |
  | MointorListVo. deviceDetailVO. locations. timeStr | String  | 最近一次定位的时间                     |
  | MointorListVo. deviceDetailVO. locations. address | String  | 位置(文字描述)                      |
  | MointorListVo. deviceDetailVO. locations. lon | double  | 经度                            |
  | MointorListVo. deviceDetailVO. locations.lat | double  | 纬度                            |
  | MointorListVo. deviceDetailVO. locations.precision | Integer | 定位精度                          |
  | MointorListVo. deviceDetailVO.Battery.energy | Integer | 电量                            |
  | MointorListVo. deviceDetailVO. locations.locateType | Integer | 定位方式(1:GPS,2:LBS,3:WIFI,4:BT) |

示例:

```json
{
    "pageInfo":null,
    "data":[
        {
            "deviceDetailVO":{
                "gender":null,
                "birth":null,
                "height":null,
                "weight":null,
                "description":null,
                "locations":{
                    "time":1519374486000,
                    "lon":113.93299894798281,
                    "lat":22.52671151932491,
                    "precision":35,
                    "address":"广东省深圳市南山区海天一路(深圳湾创业广场附近29米)",
                    "locateType":3,
                    "timeStr":"2018-02-23 16:28:06"
                },
                "battery":{
                    "status":0,
                    "energy":90
                },
                "deviceId":"11393865",
                "updatefwverVersion":0,
                "fwverVersion":"CMCC-DST1A_V1.1_201801101500_Debug",
                "deviceOnoffMode":1,
                "deviceCardType":0,
                "online":1,
                "deviceName":"胡才庆",
                "deviceKey":null,
                "deviceSn":"102000217340000341",
                "contactPhoneNo":null,
                "devicePhoneNo":"13902959214",
                "devicePhoneType":0,
                "deviceIcon":null,
                "deviceStep":11,
                "userType":1,
                "userId":98,
                "deviceLocateMode":4
            },
            "monitorId":98,
            "monitorName":"胡才庆",
            "userName":null,
            "contactPhone":"15111811215",
            "emergencyPhone":"15111811215",
            "emergencyName":"胡紧急",
            "monitorIorgId":80,
            "monitorCompanyId":5,
            "deviceSn":"102000217340000341",
            "freq":4,
            "orgName":"nn部nn",
            "switchState":1,
            "deviceId":"11393865",
            "orgParentId":null
        },
        Object{...},
        Object{...},
        Object{...},
        Object{...},
        Object{...},
        Object{...}
    ],
    "code":0,
    "msg":"请求成功",
    "operateSuccess":true
}
```



### 5.2实时定位

- ### 接口描述

  通过设备ID实时定位

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/ system/locationManager/getLocation |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |

- ### 请求参数

  | 参数       | 类型     | 说明   |
  | -------- | ------ | ---- |
  | deviceId | String | 设备ID |

- ### 输出参数

  | 参数                | 类型     | 说明       |
  | ----------------- | ------ | -------- |
  | locations         | Object | 位置信息对象   |
  | locations.time    | number | 定位时间戳    |
  | locations.lon     | double | 经度       |
  | locations.lat     | double | 纬度       |
  | locations.address | String | 位置(文字描述) |

  示例:

  ```json
       
  {
      "data":{
          "locations":{
              "time":1519375389000,
              "lon":113.93316398934309,
              "precision":29,
              "address":"广东省深圳市南山区高新南九道(深圳湾创业广场附近14米)",
              "locateType":3,
              "lat":22.526618460077042
          },
          "deviceId":"11393865"
      },
      "code":0,
      "msg":"请求成功",
      "operateSuccess":true
  }
  ```

### 5.3 获取设备历史位置

- ### 接口描述

  通过设备ID获取历史位置

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/ system/locationManager/ getHistroyLocation |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |

- ### 请求参数

  | 参数       | 类型     | 说明                 |
  | -------- | ------ | ------------------ |
  | deviceId | String | 设备ID               |
  | dateTime | String | 日期(格式: yyyy-MM-dd) |

- ### 输出参数

  | 参数                   | 类型      | 说明                            |
  | -------------------- | ------- | ----------------------------- |
  | locations            | Array   | 位置信息                          |
  | locations.lon        | double  | 经度                            |
  | locations.lat        | double  | 纬度                            |
  | locations.address    | String  | 位置(文字)                        |
  | locations.beginTime  | number  | 开始时间戳                         |
  | locations.endTime    | number  | 结束时间戳                         |
  | locations.locateType | Integer | 定位方式(1:GPS,2:LBS,3:WIFI,4:BT) |

示例:

```json
     
{
    "data":[
        {
            "lon":113.93299894798281,
            "precision":35,
            "address":"广东省深圳市南山区海天一路(深圳湾创业广场附近29米)",
            "locateType":3,
            "beginTime":1519374486000,
            "endTime":1519375389000,
            "lat":22.52671151932491
        }
    ],
    "code":0,
    "msg":"请求成功",
    "operateSuccess":true
}
```



## 设备模块

### 6.1 批量分配

- ### 接口描述

  批量分配设备给部门(未分配的设备)

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/deviceManager/distributionDevice |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

- ### 请求参数

  | 参数    | 类型    | 说明     |
  | ----- | ----- | ------ |
  | orgId | Long  | 部门ID   |
  | ids   | Array | 设备ID数组 |

- ### 输出参数

  无

```json
{
  "code":0,
  "msg":"请求成功",
  "operateSuccess":true
}
```

### 6.2获取设备通讯录

- ### 接口描述

  获取设备通讯录中所有成员信息。

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/deviceManager/getAddressBook |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |

- ### 请求参数

  | 参数       | 类型     | 说明   |
  | :------- | ------ | ---- |
  | deviceId | String | 设备ID |

- ### 输出参数

| 参数                      | 类型      | 说明                          |
| ----------------------- | ------- | --------------------------- |
| contacts                | Array   | 联系人列表                       |
| contacts.contactId      | Long    | 联系人ID                       |
| contacts.userType       | Integer | 用户类型(0:紧急联系人,1:app用户,2:白名单) |
| contacts.contactIconNo  | Integer | 联系人头像序号                     |
| contacts.contactName    | String  | 联系人姓名                       |
| contacts.contactPhoneNo | String  | 联系人手机号                      |
| contacts.userId         | Long    | 用户id                        |

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

- #### 错误码

| 错误码    | 说明       |
| ------ | -------- |
| -10003 | 设备不存在    |
| -10004 | 设备已离线    |
| -10006 | 设备权限验证失败 |



### 6.3增加设备通讯录

- ### 接口描述

  通讯录只能增加白名单

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/deviceManager/addAddressBook |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

- ### 请求参数

  | 参数             | 类型     | 说明   |
  | :------------- | ------ | ---- |
  | contactName    | String | 姓名   |
  | contactPhoneNo | String | 手机   |
  | deviceId       | String | 设备ID |

- ### 输出参数

  无

示例:

~~~json
{
  "code":0,
  "msg":"请求成功",
  "operateSuccess":true
}
~~~

### 6.3修改设备通讯录

- ### 接口描述

  修改设备通讯录

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/deviceManager/editAddressBook |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

- ### 请求参数

  | 参数             | 类型      | 说明             |
  | :------------- | ------- | -------------- |
  | contactName    | String  | 姓名             |
  | contactPhoneNo | String  | 手机             |
  | deviceId       | String  | 设备ID           |
  | contactId      | Long    | 通讯录ID          |
  | userType       | Integer | 用户类型(不能修改需要传递) |

- ### 输出参数

  无

示例:

```json
{
  "code":0,
  "msg":"请求成功",
  "operateSuccess":true
}
```



### 6.4删除设备通讯录

- ### 接口描述

  删除设备通讯录

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/deviceManager/delAddressBook |
  | ---- | ---------------------------------------- |
  | 请求方法 | POST                                     |

- ### 请求参数

  | 参数        | 类型      | 说明    |
  | :-------- | ------- | ----- |
  | deviceId  | String  | 设备ID  |
  | contactId | Long    | 通讯录ID |
  | userType  | Integer | 用户类型  |

- ### 输出参数

  无

示例:

```json
{
  "code":0,
  "msg":"请求成功",
  "operateSuccess":true
}
```



### 6.5获取设备状态

- ### 接口描述

  获取设备状态: 是否在线

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/deviceManager/getDeviceOnline |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |

- ### 请求参数

  | 参数       | 类型     | 说明   |
  | :------- | ------ | ---- |
  | deviceId | String | 设备ID |

- ### 输出参数

  | 参数     | 类型      | 说明                |
  | ------ | ------- | ----------------- |
  | online | Integer | 设备长连接在线状态 0离线 1在线 |

  ​

示例:

```json
{
  "code":0,
  "msg":"请求成功",
  "online":1
  "operateSuccess":true
}
```



### 6.6 设备批量激活

- ### 接口描述

设备批量激活（在account对象里authority里有admin 暴露这个接口）

- ### 请求方式

| 请求路径 | http://<HOST>:<PORT>/system/deviceManager/deviceRegistBatch |
| ---- | ---------------------------------------- |
| 请求方法 | GET                                      |



- ### 请求参数

| 参数    | 类型   | 说明   |
| ----- | ---- | ---- |
| orgId | Long | 部门ID |

- ### 输出参数

| 参数      | 类型      | 说明          |
| ------- | ------- | ----------- |
| total   | Integer | 需要激活设备的总数量  |
| solved  | Integer | 已经尝试激活的设备数量 |
| success | Integer | 成功激活的设备数量   |

示例

```json
{
	"data": {
		"registNum": {
			"total": 100,
			"solved": 56,
			"success": 52
		}
	},
	"code": 0,
	"msg": "请求成功",
	"operateSuccess": true
}

```





### 6.7设备批量激活详情查询

- ### 接口描述

  设备批量激活详情查询

- ### 请求方式

  | 请求路径 | http://<HOST>:<PORT>/system/deviceManager/deviceRegistBatchQuery |
  | ---- | ---------------------------------------- |
  | 请求方法 | GET                                      |

  ​


- ### 请求参数

  无

- ### 输出参数

  | 参数      | 类型      | 说明          |
  | ------- | ------- | ----------- |
  | total   | Integer | 需要激活设备的总数量  |
  | solved  | Integer | 已经尝试激活的设备数量 |
  | success | Integer | 成功激活的设备数量   |

  示例

  ```json
  {
  	"data": {
  		"registNum": {
  			"total": 100,
  			"solved": 56,
  			"success": 52
  		}
  	},
  	"code": 0,
  	"msg": "请求成功",
  	"operateSuccess": true
  }
  ```

  ​