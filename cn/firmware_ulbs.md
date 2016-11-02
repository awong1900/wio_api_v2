# 固件项目文件
1. 获取文件列表
2. 创建和更新文件
4. 获取文件
5. 删除文件

## GET

	GET /v2/firmwares/:firmware_id/ulbs
	
### 响应
	Status: 200 OK
	Link: <http://wio.seeed.io/v2/resource?page=2>; rel="next",
	      <http://wio.seeed.io/v2/resource?page=5>; rel="last"
```
{
  "ulbs": [
    {
      "firmware_id": "313be000160b425baf933d47e12b1e9e",
      "path": "Main.cpp",
      "name": "Main.cpp",
      "content": "\n#include \"wio.h\"\n#include \"suli2.h\"\n#include \"Main.h\"\n\nvoid setup()\n{\n}\n\nvoid loop()\n{\n\n}\n            ",
      "encoding": "plain",
      "size": 102,
      "sha": "834bac491bc993ccc7661f352b198072a3a42de2",
      "created_at": "2016-10-27T08:54:59Z",
      "updated_at": "2016-10-27T08:54:59Z"
    }
  ]
}  
```

## 创建和更新文件

	PUT /v2/firmwares/:firmware_id/ulbs/:path
	
### 参数
| 名字 | 类型 | 描述 |
| ------| ------ | ------ |
| path | string | **Required**. 文件相对路径 |

### 例子
	PUT /v2/firmwares/313be000160b425baf933d47e12b1e9e/ulbs/test/m.cpp

Content-Type: text/plain

```
#include "wio.h"
#include "suli2.h"
#include "Main.h"

void setup()
{
}

void loop()
{

}
```
### 响应
	Status: 201 Created
	Location: https://wio.seeed.io/v2/313be000160b425baf933d47e12b1e9e/test/m.cpp
```
#include "wio.h"
#include "suli2.h"
#include "Main.h"

void setup()
{
}

void loop()
{

}
```

## 获取文件

	GET /v2/firmwares/:firmware_id/:path
	
### 响应
	Status: 200 OK
```
#include "wio.h"
#include "suli2.h"
#include "Main.h"

void setup()
{
}

void loop()
{

}
```

## 删除文件

	DELETE /v2/firmwares/:firmware_id/:path
	
### 响应
	Status: 204 No Content