# 固件二进制文件
1. 编译固件
2. 下载固件

## 编译固件
当第一次编译时，会先响应`202`，直到后台编译模块完成编译，然后返回`200`。编译完成的固件会缓存一段时间，通常是几周时间。所以下一次请求时，会优先返回缓存的固件地址。
	GET /v2/firmwares/:firmware_id/bin
	
### 响应
	Status: 202 Accepted
```
{
  "status": "building"
}
```	
	Status: 200 OK
```
{
  "sha": "761e224a29654c1a41e648bf9bea10266efbe278",
  "size": 574472,
  "well_known": "[{\"bd\": {\"p\": [], \"rw\": \"r\", \"v\": [[\"float\", \"humidity\"]], \"n\": \"GroveTempHumD0/humidity\"}, \"tp\": \"prop\"}, {\"bd\": {\"p\": [], \"rw\": \"r\", \"v\": [[\"float\", \"celsius_degree\"]], \"n\": \"GroveTempHumD0/temperature\"}, \"tp\": \"prop\"}, {\"bd\": {\"p\": [], \"rw\": \"r\", \"v\": [[\"float\", \"fahrenheit_degree\"]], \"n\": \"GroveTempHumD0/temperature_f\"}, \"tp\": \"prop\"}]",
  "bin_url": "http://wio.seeed.io/v2/bins/761e224a29654c1a41e648bf9bea10266efbe278",
  "status": "success",
  "status_text": "firmware binary has been built successfully!",
  "created_at": "2016-10-27T12:27:54Z",
  "updated_at": "2016-10-27T12:28:23Z"
}
```

## 下载固件
每个固件都会有特定的sha。从编译固件结果可以拿到正确的sha。

	GET /v2/bins/:sha

### 参数
| 名字 | 类型 | 描述 |
| ------| ------ | ------ |
| q | string | 可以`1`或者`2`, 请求单个bin文件 (for esp8266) |
| format | string | 可以是`zip`或者`tar`, 默认`zip` |

### 响应
	Status: 200 OK
	Content-Disposition: attachment;filename="user1.bin"
	Content-Transfer-Encoding: binary
	Content-Type: application/octet-stream
