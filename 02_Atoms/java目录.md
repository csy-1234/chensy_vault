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
       

```