# 固件项目
1. 获取用户的所有固件项目
2. 创建新固件项目
3. 获取
4. 更新
5. 删除一个固件项目

## GET

	GET /v2/firmwares
	
### 响应
	Status: 200 OK
	Link: <http://wio.seeed.io/v2/resource?page=2>; rel="next",
	      <http://wio.seeed.io/v2/resource?page=5>; rel="last"
```
{
  "firmwares": [
    {
      "firmware_id": "5aee807f21c24a86b7ddfeac0f3bca3a",
      "user_id": "057ee4457d754b049c1bbfb4ddd5412a",
      "name": "Hello-World",
      "desc": "First firmware",
      "firmware_type": "flexible",
      "fixed_url": null,
      "bin_url": null,
      "size": null,
      "sha": null,
      "config": {
        "board_name": "Wio Link v1.0",
        "connections": []
      },
      "well_known": "[]",
      "created_at": "2016-10-27T08:49:08Z",
      "updated_at": "2016-10-27T08:49:08Z"
    }
  ]
}
```

## 创建一个新的固件

	POST /v2/firmwares
	
### 输入
| 名字 | 类型 | 描述 |
| ------| ------ | ------ |
| name | string | 固件项目名字 |
| desc | string | 固件项目描述 |
| config | json | 固件连接配置项，配置格式参考：TODO |
### 例子
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
### 响应
	Status: 201 Created
	Location: https://wio.seeed.io/v2/firmwares/d6a0023b4e3b410c9fc8eceb1387eb83
```
{
  "firmware_id": "d6a0023b4e3b410c9fc8eceb1387eb83",
  "user_id": "057ee4457d754b049c1bbfb4ddd5412a",
  "name": "Hello-World",
  "desc": "first firmware with config",
  "firmware_type": "flexible",
  "fixed_url": null,
  "bin_url": null,
  "size": null,
  "sha": null,
  "config": {
    "board_name": "Wio Link v1.0",
    "connections": [
      {
        "sku": "101020019-ffff",
        "port": "D0"
      }
    ]
  },
  "well_known": "[]",
  "created_at": "2016-10-27T08:52:04Z",
  "updated_at": "2016-10-27T08:53:33Z"
}
```

## 获取

	GET /v2/firmwares/:firmware_id
	
### 响应
	Status: 200 OK
```
{
  "firmware_id": "313be000160b425baf933d47e12b1e9e",
  "user_id": "057ee4457d754b049c1bbfb4ddd5412a",
  "name": "Hello-World",
  "desc": "First firmware",
  "firmware_type": "flexible",
  "fixed_url": null,
  "bin_url": null,
  "size": null,
  "sha": null,
  "config": {
    "board_name": "Wio Link v1.0",
    "connections": [
      {
        "sku": "101020019-ffff",
        "port": "D0"
      }
    ]
  },
  "well_known": "[]",
  "created_at": "2016-10-27T08:54:59Z",
  "updated_at": "2016-10-27T08:54:59Z"
}
```

## 更新

	PATCH /v2/firmwares/:firmware_id
	
### 输入
| 名字 | 类型 | 描述 |
| ------| ------ | ------ |
| name | string | 固件项目名字 |
| desc | string | 固件项目描述 |
| config | json | 固件连接配置项，配置格式参考：TODO |

### 例子
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
### 响应
	Status: 200 OK
```
{
  "firmware_id": "313be000160b425baf933d47e12b1e9e",
  "user_id": "057ee4457d754b049c1bbfb4ddd5412a",
  "name": "Hello-World",
  "desc": "First firmware",
  "firmware_type": "flexible",
  "fixed_url": null,
  "bin_url": null,
  "size": null,
  "sha": null,
  "config": {
    "board_name": "Wio Link v1.0",
    "connections": [
      {
        "sku": "101020019-ffff",
        "port": "D0"
      }
    ]
  },
  "well_known": "[]",
  "created_at": "2016-10-27T08:54:59Z",
  "updated_at": "2016-10-27T08:54:59Z"
}
```

## DELETE

	DELETE /v2/firmwares/:firmware_id
	
### 响应
	Status: 204 No Content