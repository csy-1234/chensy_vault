
> [!NOTE] Title
>为了“隔离各层 + 控制数据形态”，专门设计的数据对象

1.POJO = Plain Old Java Object

>没有继承框架、没有业务限制的普通 Java 类

	也就是说定义的，非业务定义的java类
	Entity / DTO / VO 本质上全都是 POJO

2.Entity / PO（数据库模型）

	一张表 ≈ 一个 Entity
```java
@Entity   //注解来自jakarta.persistence.Entity/Table
@Table(name = "user")
public class UserEntity {
    private Long id;
    private String username;
    private String password;
    private Date createTime;
}
```

	- 字段 ≈ 表字段
    
	- 可以有数据库注解
    
	- 不适合直接给前端

3.DTO（Data Transfer Object）——传输用

	在“层与层 / 服务与服务”之间传数据
	- Controller → Service
    
	- Service → Service
    
	- RPC / 微服务调用

```java
public class UserCreateDTO {
    private String username;
    private String password;
}

---

@PostMapping("/user")
public void createUser(@RequestBody UserCreateDTO dto) {
    userService.create(dto);
}

```

4.VO（View Object）——返回给前端的

	返回给前端的

```java
@GetMapping("/user/{id}")
public UserVO getUser(@PathVariable Long id) {
    return userService.getUserVO(id);
}
```

5.BO（Business Object）——业务处理中用

	多表 + 规则组合
	BO 一般不会直接出现在 Controller
```

```



controller入参（前端返回）：dto
		返回值（返回给前端）：vo

service处理：entity或bo

DB映射到entity






