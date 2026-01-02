# 浏览器行为

|触发方式|HTTP 方法|能力|
|---|---|---|
|地址栏直接输入 URL|GET|只能访问/下载资源|
|HTML 表单用户提交|GET / POST|提交数据、上传文件|
|JS 脚本自动或用户触发|GET / POST / PUT / DELETE / PATCH|完整 API 调用，操作资源|

💡 核心理解：

- **地址栏 = 浏览器的最简单 API 客户端，只能发 GET**
    
- **HTML/JS = 浏览器的完整 API 客户端，可以发任意 HTTP 方法**

# url、api、sdk

## **url**
	(Uniform Resource Locator） = 资源定位符
	不局限于 HTTP
## API 
- Application Programming Interface

- 程序方法的操作规范

- 规定请求如何发起（URL/方法/参数/数据）
    
- 规定响应如何返回（格式/字段/错误码）
    
- HTTP 只是最常见的实现方式
- 不局限于 HTTP

API规定了什么

- 它规定了 **两次交互规范**：

 (1) 请求规范

- API 定义调用者应该如何发起请求，包括：
    
    1. **URL 或方法名**（定位资源或操作）
        
    2. **HTTP 方法**（GET/POST/PUT/DELETE，或方法调用名）
        
    3. **请求参数**（Query、Path、Body、Header 等）
        
    4. **数据格式**（JSON、XML、二进制等）
        
    5. **认证/权限**（Token、AccessKey 等）
        

 (2) 响应规范

- API 规定服务端返回的内容，包括：
    
    1. **数据格式**（JSON、XML、二进制等）
        
    2. **返回字段**（status、message、data 等）
        
    3. **错误码和错误信息**（调用失败时的处理方式）

>API 就像程序的骨架和轨道，规定了方法、参数和数据流动路线，调用者沿着它操作资源，不关心内部实现。


## **sdk**

	Software Development Kit，软件开发工具包
	
	SDK = API 的封装工具
    
		 本质上仍然是发送 API 对应的请求
		 
		优点：简化调用、隐藏底层复杂逻辑、减少错误