# 节点设备
1. /v2/things
2. /v2/things/{thing_id}
4. /v2/things/{thing_id}/page
5. /v2/things/{thing_id}/ota

> 所有请求需要认证信息

## /v2/things
### GET 获取用户的所有节点设备
列出认证用户的所有节点设备

| 参数 | 类型 | 描述 |
| ----|---- | ---- |
| page | int | 显示第几页 |
| per_page | int | 每一页显示数量 |
| sort | string | 默认created_at倒序，[sort格式]() |

### POST 创建新设备
根据输入创建一个节点设备。

#### 输入
| 名字 | 类型 | 描述 |
| ------| ------ | ------ |
| board | string | **必须**. 节点设备型号 |
| name | string | 节点设备名字 |

#### 例子
```
{
  "board": "WioLink v1.0",
  "name": "myWio"
}
```

## /v2/things/:thing_id
> 必须是认证用户的节点设备

### GET 获取节点设备信息
#### 输入
| 名字 | 类型 | 描述 |
| ------| ------ | ------ |
| name | string | 节点设备名字 |

#### 例子
```
{
  "name": "myWio"
}
```

### PATCH 更新节点设备信息
#### 输入
| 名字 | 类型 | 描述 |
| ------| ------ | ------ |
| name | string | 节点设备名字 |

#### 例子
```
{
  "name": "myWio2"
}
```

#### DEL 删除节点设备

## /v2/things/{thing_id}/page
### GET 获取节点设备的API页面

## /v2/things/{thing_id}/ota
OTA (Over-The-Air) 在线升级功能是WIO系统最重要的功能之一。当你完成固件开发，可以通过OTA把固件下载到设备中。
### GET 获得当前的OTA设置和状态
### PATCH 修改OTA设置
| 名字 | 类型 | 描述 |
| ------| ------ | ------ |
| firmware_id | string | 升级固件项目ID |
| cancel | boolean | `True` 取消升级 |
| ota_at | datetime | 定义升级时间 |

#### 例子
```
{
  "firmware_id": "000",
  "ota_at": "2018-02-01T01:00Z"
}
```
### POST 启动OTA
| 名字 | 类型 | 描述 |
| ------| ------ | ------ |
| firmware_id | string | 升级固件项目ID |
| ota_at | datetime | 定义升级时间 |

#### 例子
```
{
  "firmware_id": "000",
  "ota_at": "2018-02-01T01:00Z"
}
```
