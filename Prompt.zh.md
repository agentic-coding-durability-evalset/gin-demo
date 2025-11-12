## 任务1

**任务类型：** 需求实现

**提示词：**
```
为Gin应用添加JWT Auth Middleware支持，算法为HS256，secret为`your-256-bit-secret`。要求如下：

- 为`/api/v1/*`增加JWT验证，其他的URL放行
- 验证失败时返回401状态码和JSON格式的错误信息
- 增加基于username和password获取jwt的功能，生成的JWT token的payload的sub为username，过期时间为30天
- JWT token来自于请求头的`Authorization`字段，格式为`Bearer <token>`，同时也支持x-api-key查询字段传递JWT Token
```
---

## 任务2

**任务类型：** 需求实现

**提示词：**
```
请为项目增加SQLite数据支持，具体需求如下：

- 集成SQLite数据库，确保项目可以使用SQLite进行数据存储和查询。
- 创建users表，主要包括： ID，用户名，电子邮件，创建时间等列
- 创建user repository，实现基本的CRUD（创建、读取、更新、删除）
- 创建user handler，处理HTTP请求，支持用户的增删改查操作
```
