---
permalink: /quality/action/
---

## 单元测试 - SpringBoot2+Mockito实战

> 在真实的开发中，我们通常是使用SpringBoot的，目前SpringBoot是v2.4.x的版本（SpringBoot 2.2.2.RELEASE之前默认是使用 JUnit4，之后版本默认使用Junit5）；所以我们写个基于SpringBoot2.4+H2的内存库的简单例子，同时加点必要的单元测试

* SpringBoot对单测试的差异
* 项目实践
    * Demo程序准备
    * 测试类
    * Util测试

### SpringBoot对单测试的差异

> SpringBoot 2.2.2.RELEASE之前默认是使用 JUnit4，之后版本默认使用Junit5

Springboot+junit4:

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class SpringBootQuickStartApplicationTests {

    private MockMvc mvc;

    @Before
    public void setUp() throws Exception {
        mvc = MockMvcBuilders.standaloneSetup(new UserController()).build();
    }

    @Test
    public void contextLoads() throws Exception {
        RequestBuilder request = null;
       
        request = MockMvcRequestBuilders.get("/")
                .contentType(MediaType.APPLICATION_JSON);
        mvc.perform(request)
                .andExpect(MockMvcResultMatchers.status().isOk())
                .andDo(MockMvcResultHandlers.print())
                .andReturn();
   }
}
```

Springboot+junit5:

```java
@SpringBootTest
// 使用spring的测试框架
@ExtendWith(SpringExtension.class)
class SpringbootQuickStartApplicationTests {

    private MockMvc mockMvc;

    @BeforeEach // 类似于junit4的@Before
    public void setUp() throws Exception {
        mockMvc = MockMvcBuilders.standaloneSetup(new UserController()).build();
    }

    @Test
    void contextLoads() throws Exception {
        RequestBuilder request = null;

        request = MockMvcRequestBuilders.get("/")
                .contentType(MediaType.APPLICATION_JSON);
        mockMvc.perform(request)
                .andExpect(MockMvcResultMatchers.status().isOk())
                .andDo(MockMvcResultHandlers.print())
                .andReturn();
    }
}
```

### 项目实践

Spring Boot 2.4.2 + H2 + Lombok + Spring Boot Test (默认包含了 Junit5 + Mockito)。

![dev-ut-springboot-1.png](/knowledge/assets/images/quality/dev-ut-springboot-1.png)

#### Demo程序准备

* pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.2</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>tech.pdai</groupId>
    <artifactId>java-springboot-unit5</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>java-springboot-unit5</name>
    <description>java-springboot-unit5</description>
    <properties>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>

```

* application.yml

```yml
spring:
  datasource:
    platform: h2
    driverClassName: org.h2.Driver
    url: jdbc:h2:mem:testdb;MODE=MYSQL;DB_CLOSE_DELAY=-1;DATABASE_TO_UPPER=false
    username: pdai
    password: pdai
    schema: classpath:db/schema/user-schema.sql
    data: classpath:db/data/user-data.sql
    initialization-mode: always
  h2:
    console:
      settings:
        trace: true
        web-allow-others: true
      enabled: true
      path: /h2-console
  jpa:
    show-sql: true
    hibernate:
      ddl-auto: update
    generate-ddl: false
    open-in-view: false
```

* 数据库文件准备

schema

```sql
DROP TABLE IF EXISTS user;
CREATE TABLE user (
    id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    name varchar(35),
    phone varchar(35)
);

```

data

```sql
insert into user(id,name,phone) values(1,'pdai','123456');
insert into user(id,name,phone) values(2,'zhangsan','123456');
```

* entity

```java
package tech.pdai.springboot2unit5.entity;

import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

import javax.persistence.*;

/**
 * User.
 */
@Entity
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Table(name = "user")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    @Column
    private String name;

    @Column
    private String phone;
}
```

* dao

```java
package tech.pdai.springboot2unit5.dao;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import tech.pdai.springboot2unit5.entity.User;

/**
 * user dao.
 */
@Repository
public interface UserRepository extends JpaRepository<User, Integer> {
}
```

* service

```java
package tech.pdai.springboot2unit5.service;

import tech.pdai.springboot2unit5.entity.User;

import java.util.List;

/**
 * user service.
 */
public interface IUserService {

    /**
     * find all user.
     *
     * @return list
     */
    List<User> findAll();
}
```

```java
package tech.pdai.springboot2unit5.service.impl;

import org.springframework.stereotype.Service;
import tech.pdai.springboot2unit5.dao.UserRepository;
import tech.pdai.springboot2unit5.entity.User;
import tech.pdai.springboot2unit5.service.IUserService;

import java.util.List;

/**
 * User service impl.
 */
@Service
public class UserServiceImpl implements IUserService {

    /**
     * user dao.
     */
    private final UserRepository userRepository;

    /**
     * init.
     *
     * @param userRepository2 user dao
     */
    public UserServiceImpl(final UserRepository userRepository2) {
        this.userRepository = userRepository2;
    }

    /**
     * find all user.
     *
     * @return list
     */
    @Override
    public List<User> findAll() {
        return userRepository.findAll();
    }
}
```

* Controller

```java
package tech.pdai.springboot2unit5.controller;

import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import tech.pdai.springboot2unit5.entity.User;
import tech.pdai.springboot2unit5.service.IUserService;

import java.util.List;

/**
 * User controller.
 */
@RestController
public class UserController {

    /**
     * user service.
     */
    private final IUserService userService;

    /**
     * init.
     *
     * @param userService2 user service
     */
    public UserController(final IUserService userService2) {
        this.userService = userService2;
    }

    /**
     * find user list.
     *
     * @return list
     */
    @GetMapping("user/list")
    public ResponseEntity<List<User>> list() {
        return ResponseEntity.ok(userService.findAll());
    }
}
```

#### 测试类

> 在实际的项目中可以使用profile来区分测试ut，使用test profile(包含H2内存库)，真实环境使用MySQL或其它

* main

```java
package tech.pdai.springboot2unit5;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.context.annotation.Profile;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import tech.pdai.springboot2unit5.service.IUserService;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertFalse;

/**
 * A way to test from H2.
 * <p>
 * Just a demo, and change profile to 'test' for H2, and 'product' for MySQL.
 */
@Profile("default")
@ExtendWith(SpringExtension.class)
@SpringBootTest
class JavaSpringbootUnit5ApplicationTests {

    @Autowired
    IUserService userService;

    @Test
    @DisplayName("Integration test")
    void contextLoads() {
        assertFalse(userService.findAll().isEmpty());
        assertEquals("pdai", userService.findAll().get(0).getName());
    }

}
```

* controller

mockMvc

```java
package tech.pdai.springboot2unit5.controller;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mockito;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.MediaType;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;
import tech.pdai.springboot2unit5.entity.User;
import tech.pdai.springboot2unit5.service.IUserService;

import java.util.Collections;

import static org.mockito.Mockito.times;
import static org.mockito.Mockito.verify;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

/**
 * user controller test - use mockito.
 */
@ExtendWith(SpringExtension.class)
@WebMvcTest(value = UserController.class)
class UserControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private IUserService userService;

    /**
     * test find all user.
     *
     * @throws Exception exception
     */
    @Test
    @DisplayName("Test findAll()")
    public void list() throws Exception {
        Mockito.when(userService.findAll()).thenReturn(
                Collections.singletonList(new User(1, "pdai.tech", "1221111")));

        mockMvc.perform(MockMvcRequestBuilders.get("/user/list")
                .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk());

        verify(userService, times(1)).findAll();
    }
}
```

* service

```java

package tech.pdai.springboot2unit5.service.impl;

import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mock;
import org.mockito.Mockito;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import tech.pdai.springboot2unit5.entity.User;

import java.util.Collections;
import java.util.List;

import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.mockito.Mockito.times;
import static org.mockito.Mockito.verify;

@ExtendWith(SpringExtension.class)
class UserServiceImplTest {

    @Mock
    private UserServiceImpl userService;

    @Test
    public void findAll() {
        //Given
        Mockito.when(userService.findAll()).thenReturn(
                Collections.singletonList(new User(1, "pdai.tech", "1221111")));

        //When
        List<User> userDtoList = userService.findAll();

        //Then
        assertFalse(userDtoList.isEmpty());
        verify(userService, times(1)).findAll();
    }
}
```

* 测试结果

![dev-ut-springboot-2.png](/knowledge/assets/images/quality/dev-ut-springboot-2.png)

#### Util测试

> 如果包含静态util的测试还可以加PowerMokito.

具体可以参考：测试结合powermock支持静态方法