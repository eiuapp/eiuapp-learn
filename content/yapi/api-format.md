
YAPI 使用


## 接口集合 ##
### 接口定义 ###

接口定义与使用，要近可能符合 Restful API 风格


#### 方法 - 中文 - 英文 ####

- Get - 查询 - Get ;
- Post - 提交（新增） - Add, Set, Post
- Post, Put - 修改 - Set, Post, Put, Update
- Delete - 删除 - Delete


#### 示例 ####

（优先）

- Get - 查询用户帐户 - GetUserAccout
- Post - 新增用户帐户 - AddUserAccout
- Post, Put - 修改用户帐户 - SetUserAccount 或者 UpdateUserAccout
- Delete - 删除用户帐户 - DeleteUserAccout

或者

（其次）

- Get - 查询用户帐户 - UserAccout/Get
- Post - 新增用户帐户 - UserAccout/Add
- Post, Put - 修改用户帐户 - UserAccout/Update
- Delete - 删除用户帐户 - UserAccout/Delete

#### 注意 ####

- 不要 List, List是名词 , 如果要查询用户帐户列表，
Get - 查询用户帐户列表 - GetUserAccoutList

## 测试集合 ##

### 配置


