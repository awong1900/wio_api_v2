# 用户
1. 获取认证用户信息
2. 获取所有用户信息
3. 获取认证用户的所有令牌信息

所有请求必须包含认证信息。
## 获取认证用户信息
列出当前认证用户的用户信息

	GET /v2/user
	
### 响应
	Status: 200 OK
```
{
  "user_id": "057ee4457d754b049c1bbfb4ddd5412a",
  "created_at": "2016-10-26T02:27:33Z" 
}
```

## 获取所有用户信息
以创建用户顺序列出所有用户信息。  
注意只有认证用户为系统管理员时，才会返回所有用户信息。

	GET /v2/users
	
### 响应
	Status: 200 OK
	Link: <http://wio.seeed.io/v2/resource?page=2>; rel="next",
	      <http://wio.seeed.io/v2/resource?page=5>; rel="last"
```
{
  "users": [
    {
      "user_id": "057ee4457d754b049c1bbfb4ddd5412a",
      "created_at": "2016-10-26T02:27:33Z"
    }
  ]
}
```

## 获取认证用户的所有令牌信息

	GET /v2/user/tokens
	
### 响应
	Status: 200 OK
	Link: <http://wio.seeed.io/v2/resource?page=2>; rel="next",
	      <http://wio.seeed.io/v2/resource?page=5>; rel="last"
```
{
  "tokens": [
    {
      "token": "347ee4457d754b049c1ccfb4ddd5413d",
      "user_id": "057ee4457d754b049c1bbfb4ddd5412a",
      "expire": 0,
      "created_at": "2016-10-26T02:27:33Z",
      "updated_at": "2016-10-26T02:27:33Z"
    }
  ]
}
```