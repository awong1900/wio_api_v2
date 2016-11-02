# API Overview
Wio IOT 平台是一款物联网应用平台，真正的WOT产品。Restful API帮助用户完成产品定义和部署。

## 端点
API请求的端点为
	
	https://wio.seeed.io/v2
	
所有的返回都为json格式。
	
	
## Token
每个请求都需要带上认证信息，有两种方式携带认证信息。

- 在`Header`添加`Authorization`，例如：
	
		Authorization: token 313be000160b425baf933d47e12b1e9e
	
- 在请求参数中添加`access_token`，例如：

		https://wio.seeed.io/v2/user?access_token= 313be000160b425baf933d47e12b1e9e
		
## 响应码
API完全遵循HTTP 状态码协议规定，不同响应码：

状态码 | 描述
---|---
200 | OK，正常响应
201 | Created，资源创建成功
202 | Accepted，请求已接受，后台服务正在处理
302 | Retry，重定向
400 | Error，出现错误，会带上错误消息
403 | Forbidden，没有访问资源的权限
404 | Not Found，没有发现资源
500 | Internal Server Error，系统错误，服务器停止

## 错误消息
`status code：400`

	{
	  "error": "Invalid user",
	}
	
## 时间格式
时间采用`ISO8601`格式。如`YYYY-MM-DDTHH:MM:SSZ`

	