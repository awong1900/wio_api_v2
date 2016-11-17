# 固件项目
1. /v2/firmwares
2. /v2/firmwares/{firmware_id}
3. /v2/firmwares/{firmware_id}/ulbs
4. /v2/firmwares/{firmware_id}/ulbs/{path_name}
5. /v2/firmwares/{firmware_id}/bin
8. /v2/firmwares/{firmware_id}/import
9. /v2/firmwares/{firmware_id}/export

> 所有请求需要认证信息

## /v2/firmwares
固件资源是一个独立完整的特定固件项目，一般含有基于WIO平台的连接配置信息和一系列用户添加的自定义代码文件。可以用它来生成最终的bin文件。它可以从github的WIO项目导入，也可以用tar或者zip打包输出。

### GET 获取认证用户的所有固件项目
| 参数 | 类型 | 描述 |
| ----|---- | ---- |
| page | int | 显示第几页 |
| per_page | int | 每一页显示数量 |
| sort | string | 默认created_at倒序，[sort格式]() |

### POST 创建一个新的固件项目
#### 输入
| 名字 | 类型 | 描述 |
| ------| ------ | ------ |
| name | string | 固件项目名字 |
| desc | string | 固件项目描述 |
| config | json | 固件连接配置项，配置格式参考：TODO |
#### 例子
```
{
  "name": "Hello-World",
  "desc": "First firmware",
  "config": {
  	"board_name": "Wio Link v1.0",
  	"connections": [
  	  {"sku": "101020019-ffff", "port": "D0"}]
  }
}
```

## /v2/firmwares/{firmware_id}
### GET 获取指定固件项目信息
### PATCH 更新指定项目信息
#### 输入
| 名字 | 类型 | 描述 |
| ------| ------ | ------ |
| name | string | 固件项目名字 |
| desc | string | 固件项目描述 |
| config | json | 固件连接配置项，配置格式参考：TODO |

#### 例子
```
{
  "name": "Hello-World",
  "desc": "First firmware",
  "config": {
  	"board_name": "Wio Link v1.0",
  	"connections": [
  	  {"sku": "101020019-ffff", "port": "D0"}]
  }
}
```

### DELETE 删除指定固件项目

## /v2/firmwares/{firmware_id}/ulbs
ULB(User Logic Block)是用户定义自己的API的逻辑代码。可以通过调用WIO系统的底层API进行功能开发，并提供Web API或者Web Event的能力。
### GET 获取当前用户的所有ULB文件

## /v2/firmwares/{firmware_id}/ulbs/{path_name}
| 参数 | 类型 | 描述 |
| ----|---- | ---- |
| path_name | string | ulb文件相对路径，如'foo/bar.cpp' |

### GET 获取指定的ULB文件
| 参数 | 类型 | 描述 |
| ----|---- | ---- |
| path_name | string | ulb文件相对路径，如'foo/bar.cpp' |
### PUT 增加或者更新指定的ULB文件
#### 输入
采用`Content-Type: text/plain`格式输入。

### DEL 删除指定的ULB文件

## /v2/firmwares/{firmware_id}/bin
### GET 编译固件
系统会现查找是否存在已经编译好的固件，如果没有，则开始一个编译任务。因此会先返回`202 Accepted`, 当完成后会返回`201 Created`。

## /v2/firmwares/{firmware_id}/import
WIO支持从Github导入
