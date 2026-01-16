一、最基础的 Java 项目目录
```
project-name/
├── src/
├── lib/                # 第三方 jar 包
├── bin/                # 编译后的 .class 文件
└── README.md


├── src/
    ├── com/
	  └── example/
	     └── Main.java
```
二、Maven / Gradle 标准目录
```
project-name/
├── pom.xml 
├── src/
└── target/ 

├── src/
    ├── main/
    └── test/

    ├── main/
	   ├── java/             # Java 源码
	   ├── resources/        # 资源文件
       └── webapp/           # 传统 Web 项目（可选）
       
    └── test/  
       ├── java/            # 测试代码   
       └── resources/
       
	  ├── java/  
		   └── com/
		       └── example/
		          └── App.java

      ├── resources/   
		   ├── application.yml
		   ├── mapper/      # MyBatis XML
		   └── static/

```
三、Spring Boot 项目标准结构
```
├── src/
    ├── main/
    └── test/
  
    ├── main/
	   ├── java/             # Java 源码
	   ├── resources/ 
	   

	   ├── java/  
		   └── com/
		       └── xxx/
		           ├── Application.java   # 启动类
		           ├── controller/        # 控制层
		           ├── service/            # 业务层
		           │   └── impl/
		           ├── mapper/             # DAO / Mapper
		           ├── entity/             # 实体类
		           ├── dto/                # 数据传输对象
		           ├── vo/                 # 返回视图对象
		           ├── config/              # 配置类
		           ├── exception/           # 异常处理
		           └── util/                # 工具类

	   ├── resources/ 
	       ├── application.yml  
	       ├── mapper/                      # MyBatis XML   
	       ├── static/                      # 静态资源
	       └── templates/                   # Thymeleaf    
    
```
四、按「业务模块」划分
```

```