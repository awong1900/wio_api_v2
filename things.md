# 节点设备
1. 获取用户的所有节点设备
2. 创建新设备
3. 获取
4. 更新
5. 删除一个节点设备

所有请求需要认证信息
## 获取用户的所有节点设备
列出所有认证用户下的所有节点设备

	GET /v2/things
	
### 响应
	Status: 200 OK
	Link: <http://wio.seeed.io/v2/resource?page=2>; rel="next",
	      <http://wio.seeed.io/v2/resource?page=5>; rel="last"
```
{
  "things": [
    {
      "thing_id": "18b8a510ee1a49eea3ef5a4dd33d33ef",
      "user_id": "057ee4457d754b049c1bbfb4ddd5412a",
      "firmware_id": null,
      "key": "31d9139c46f64a6a87cdaffd9f6db32b",
      "name": "",
      "board": "WioLink v1.0",
      "online": false,
      "pp_id": null,
      "well_known": "",
      "shared": false,
      "created_at": "2016-10-27T08:16:06Z",
      "updated_at": "2016-10-27T08:16:06Z"
    }    
  ]
}
```

## 创建新设备

	POST /v2/things
	
### 输入
| 名字 | 类型 | 描述 |
| ------| ------ | ------ |
| board | string | **Required**. 节点设备型号名字 |
| name | string | 节点设备名字 |

### 例子
```
{
  "board": "WioLink v1.0"
}
```
### 响应
	Status: 201 Created
	Location: https://wio.seeed.io/v2/things/473a3a7ec7a54a33b60e4a57be5d0a27
```
{
  "thing_id": "473a3a7ec7a54a33b60e4a57be5d0a27",
  "user_id": "057ee4457d754b049c1bbfb4ddd5412a",
  "firmware_id": null,
  "key": "6cf42c96de4c4250864ec4eabf7fca26",
  "name": "",
  "board": "WioLink v1.0",
  "online": false,
  "pp_id": null,
  "well_known": "",
  "shared": false,
  "created_at": "2016-10-27T08:24:36Z",
  "updated_at": "2016-10-27T08:24:36Z"
}
```

## 获取

	GET /v2/things/:thing_id
	
### 响应
	Status: 200 OK
```
{
  "thing_id": "473a3a7ec7a54a33b60e4a57be5d0a27",
  "user_id": "057ee4457d754b049c1bbfb4ddd5412a",
  "firmware_id": null,
  "key": "6cf42c96de4c4250864ec4eabf7fca26",
  "name": "",
  "board": "WioLink v1.0",
  "online": false,
  "pp_id": null,
  "well_known": "",
  "shared": false,
  "created_at": "2016-10-27T08:24:36Z",
  "updated_at": "2016-10-27T08:24:36Z"
}
```

## 更新

	PATCH /v2/things/:thing_id
	
### 输入
| 名字 | 类型 | 描述 |
| ------| ------ | ------ |
| name | string | 节点设备名字 |

### 例子
```
{
  "name": "wio_2016"
}
```
### 响应
	Status: 200 OK
```
{
  "thing_id": "473a3a7ec7a54a33b60e4a57be5d0a27",
  "user_id": "057ee4457d754b049c1bbfb4ddd5412a",
  "firmware_id": null,
  "key": "6cf42c96de4c4250864ec4eabf7fca26",
  "name": "wio_2016",
  "board": "WioLink v1.0",
  "online": false,
  "pp_id": null,
  "well_known": "",
  "shared": false,
  "created_at": "2016-10-27T08:24:36Z",
  "updated_at": "2016-10-27T08:34:59Z"
}
```

## 删除一个节点设备

	DELETE /v2/things/:thing_id
	
### 响应
	Status: 204 No Content