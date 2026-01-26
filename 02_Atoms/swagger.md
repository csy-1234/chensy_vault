
> [!defination] 
> **Swagger 是一套用于设计、构建、文档化和测试 RESTful API 的开源工具集。它的核心目标是：让 API “自描述”——即代码写好后，能自动生成可视化文档，并支持在线调试。**

Swagger 的主要功能

1.自动生成 API 文档（UI 界面）
	
	 - 通过在代码中添加注解（如 `@Api`, `@ApiOperation`, `@ApiModel` 等
	- 启动应用后，访问一个 Web 页面
	- 自动生成**交互式 API 文档**，包含：
    - 接口路径（GET / POST / PUT / DELETE）
    - 请求参数（Header、Path、Query、Body）
    - 响应结构（含示例）
    - 数据模型（DTO/VO 结构说明）

> 📌 这个 UI 就是著名的 **Swagger UI** —— 一个漂亮的前端页面

2. 在线接口测试（Try it out

- 在 Swagger UI 页面上，你可以：
    - 点击 “Try it out”
    - 填写参数
    - 直接发送请求
    - 查看响应结果（状态码、返回体、Headers）
- **无需 Postman 或 curl**，就能快速验证接口！



