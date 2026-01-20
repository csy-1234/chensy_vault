
 一、请求：method 会“影响是否有 body”（约定，不是绝对）

| Method | 是否通常有 request body | 说明        |
| ------ | ------------------ | --------- |
| GET    | ❌ 通常没有             | 语义是“获取资源” |
| POST   | ✅ 通常有              | 语义是“提交数据” |
| PUT    | ✅                  | 更新资源      |
| DELETE | 可有可无               | RFC 允许    |

**请求**：有 method，method 对 _request body_ 有“语义/习惯上的约束”，写或不写都不一定违法

**响应**：不分 GET / POST / PUT，**响应本身没有 method 一说**

- **HTTP method 是请求的属性**
    
- **响应只有 status code + headers + body**
    
- 响应不会说“这是 GET 响应 / POST 响应”
    
    - 它只是“对某个请求的响应”

- 响应 body：
    - **要么可选**
    - **要么被明确禁止**
- 其他响应
	- 比如200 OK、400 / 500 错误，一般有body，但也可以空着

1️⃣ 明确禁止 response body 的情况（RFC 强规则）

 ✅ 状态码层面

|状态码|是否允许 body|说明|
|---|---|---|
|**1xx**|❌ 不允许|信息性响应|
|**204 No Content**|❌ 不允许|明确表示“没有内容”|
|**304 Not Modified**|❌ 不允许|缓存协商|


