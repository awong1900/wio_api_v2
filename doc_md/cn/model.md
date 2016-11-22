# 0
1. 
2. 
3. 

## GET

	GET /v2/
	
### 响应
	Status: 200 OK
	Link: <http://wio.seeed.io/v2/resource?page=2>; rel="next",
	      <http://wio.seeed.io/v2/resource?page=5>; rel="last"
```

```

## POST

	POST /v2/
	
### 输入
| 名字 | 类型 | 描述 |
| ------| ------ | ------ |
| board | string | **Required**. 节点设备型号名字 |
| name | string | 节点设备名字 |

### 例子
```

```
### 响应
	Status: 201 Created
	Location: https://wio.seeed.io/v2/
```

```

## PATCH

	PATCH /v2/
	
### 输入
| 名字 | 类型 | 描述 |
| ------| ------ | ------ |
| name | string | 节点设备名字 |

### 例子
```

```
### 响应
	Status: 200 OK
```

```

## DELETE

	DELETE /v2/
	
### 响应
	Status: 204 No Content