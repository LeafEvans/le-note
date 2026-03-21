# URL 设计规范

## 资源导向

在 RESTful API 中，**URL 仅代表“资源（名词）”，绝不包含“动作（动词）”**。具体的动作（增删改查）应由 HTTP 方法来决定。

> [!tip]
>
> **生活类比**：把 URL 想象成图书馆的书架标签。标签只会写“科幻小说区”（资源在哪），绝不会写“阅读科幻小说”（你要干嘛）。

**正反面设计对比**：

```http
// 好的设计（纯资源）
GET    /api/users      # 获取所有用户
GET    /api/users/123  # 获取 ID 为 123 的用户
POST   /api/users/123  # 创建新用户
PUT    /api/users/123  # 更新用户 123
DELETE /api/users/123  # 删除用户 123

// 糟糕的设计（动词污染）
GET  /api/getUsers         # 动词出现在 URL 中
POST /api/createUser       # 动作导向而非资源导向
GET  /api/user/delete/123  # 结构混乱，违背 REST 语义
```

## 命名规范

为了保证 API 的整洁与统一，URL 拼写需严格遵守以下四条底线：

1. **使用名词，拒绝动词**
   - ✓ `GET /api/books`
   - ✕ `GET /api/getBooks`
2. **统一使用复数**（代表资源集合）
   - ✓ `GET /api/users`
   - ✕ `GET /api/user`
3. **全部使用小写**（避免系统大小写敏感问题）
   - ✓ `GET /api/user-orders`
   - ✕ `GET /api/UserOrders`
4. **多单词使用连字符 `-` 分隔**（禁止使用下划线或驼峰）
   - ✓ `GET /api/user-profiles`
   - ✕ `GET /api/user_profiles`
   - ✕ `GET /api/userProfiles`

## 嵌套资源

当资源之间存在明确的**从属关系（父子关系）**时，使用嵌套 URL 来表达层级。

```http
// 获取用户 123 旗下的所有订单
GET /api/users/123/orders

// 获取用户 123 旗下 ID 为 456 的具体订单
GET /api/users/123/orders/456

// 为用户 123 创建一笔新订单
POST /api/users/123/orders
```

<img src="../images/restful-api/image-20260320-122805.svg" style="zoom:67%;" />

> [!warning]
>
> **嵌套层级警告**：
>
> 嵌套**绝对不要超过 2 到 3 层**！过深的 URL 极其难以维护（如 `/users/1/orders/2/items/3`）。
>
> 层级过深时应“扁平化”处理，例如直接通过订单号 ID 查询商品：`GET /api/items/3`。

## 查询参数

对于不改变资源本身，仅改变**展示形态**的操作，严禁写在 URL 路径中，必须使用 `?` 拼接查询参数。

```http
// 分页
GET /api/users?page=1&limit=10

// 过滤
GET /api/users?status=active&city=beijing

// 排序
GET /api/users?sort=created_at&order=desc

// 搜索
GET /api/users?search=张三
```

## 版本控制

为了保持**向后兼容**（避免后端升级导致老客户端崩溃），API 必须引入版本控制。业内主流有两种方式：

### URL 路径版本控制

将版本号直接写入 URL 根路径。

```http
GET /api/v1/users
GET /api/v2/users
```

### 请求头版本控制

URL 不变，通过 HTTP Header 指定版本。

```http
GET /api/users
Accept: application/vnd.api+json;version=1
```

