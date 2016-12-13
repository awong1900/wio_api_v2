## Thing
## Firmware
## ULB
## OTA
## USER 其实是隐藏了起来的

## 一个简单流程
1. 通过SSO注册和登陆用户，获取用户token和ID
2. 通过token和ID创建Thing
3. 烧写Thing ID和Thing key到设备上，并校验激活
3. 通过token和ID创建Fireware
4. 增加ULB逻辑（可选）
5. 编译OTA固件（可选）
6. 把Firmware OTA到thing，会自动编译固件
7. 查询Thing的可用APIs
8. 通过Github分享Fireware项目
9.
