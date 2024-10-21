# 三层架构的vo，dto，po分别是什么意思

在三层架构中，VO、DTO 和 PO 是常见的术语，它们分别代表不同层次的数据对象，主要用于数据传输和转换。

### VO（Value Object）
**值对象**（Value Object，简称 VO），通常用于表示层（View Layer），即用户界面层。VO 是一个纯粹的数据对象，不包含任何业务逻辑，只用于在表示层和业务逻辑层之间传递数据。只包含属性和 getter/setter 方法。不包含业务逻辑。

```plain
public class UserVO {
    private String username;
    private String email;

    // Constructors, getters, and setters
    public UserVO(String username, String email) {
        this.username = username;
        this.email = email;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
```

### DTO（Data Transfer Object）
**数据传输对象**（Data Transfer Object，简称 DTO），用于在业务逻辑层和数据访问层之间传递数据。DTO 的主要目的是减少远程调用的次数和数据传输量。DTO 也仅包含数据，没有业务逻辑。只包含属性和 getter/setter 方法。不包含业务逻辑。可以包含多个领域对象的数据，以便在一次调用中传输更多信息。

```plain
public class UserDTO {
    private Long id;
    private String username;
    private String email;
    private String address;

    // Constructors, getters, and setters
    public UserDTO(Long id, String username, String email, String address) {
        this.id = id;
        this.username = username;
        this.email = email;
        this.address = address;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}
```

### PO（Persistent Object）
**持久化对象**（Persistent Object，简称 PO），用于数据访问层（Data Access Layer），即与数据库直接交互的对象。PO 映射到数据库中的表，通常使用 ORM（对象关系映射）框架如 Hibernate 或 MyBatis 来管理。主要是映射数据库表的结构，包含数据库字段和 getter/setter 方法，可以包含一些 ORM 框架的注解，定义表和字段的映射关系。

```plain
@Entity
@Table(name = "users")
public class UserPO {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "username")
    private String username;

    @Column(name = "email")
    private String email;

    @Column(name = "address")
    private String address;

    // Constructors, getters, and setters
    public UserPO() {}

    public UserPO(String username, String email, String address) {
        this.username = username;
        this.email = email;
        this.address = address;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ddgr3akha2h8yzt4>