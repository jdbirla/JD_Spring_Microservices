## Multiple DS in spring Boot App
src: https://www.youtube.com/watch?v=iDogrHEo4x0
github: https://github.com/Java-Techie-jt/spring-boot-multiple-datasource/tree/master/src/main/java/com/javatechie/multiple/ds/api

### /src/main/java/com/javatechie/multiple/ds/api/SpringBootMultipleDsApplication.java
```java
package com.javatechie.multiple.ds.api;

import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

import javax.annotation.PostConstruct;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import com.javatechie.multiple.ds.api.book.repository.BookRepository;
import com.javatechie.multiple.ds.api.model.book.Book;
import com.javatechie.multiple.ds.api.model.user.User;
import com.javatechie.multiple.ds.api.user.repository.UserRepository;

@SpringBootApplication
@RestController
public class SpringBootMultipleDsApplication {

	@Autowired
	private BookRepository bookRepository;

	@Autowired
	private UserRepository userRepository;

	@PostConstruct
	public void addData2DB() {
		userRepository.saveAll(Stream.of(new User(744, "John"), new User(455, "Pitter")).collect(Collectors.toList()));
		bookRepository.saveAll(
				Stream.of(new Book(111, "Core Java"), new Book(222, "Spring Boot")).collect(Collectors.toList()));
	}

	@GetMapping("/getUsers")
	public List<User> getUsers() {
		return userRepository.findAll();
	}

	@GetMapping("/getBooks")
	public List<Book> getBooks() {
		return bookRepository.findAll();
	}

	public static void main(String[] args) {
		SpringApplication.run(SpringBootMultipleDsApplication.class, args);
	}
}
```

### /src/main/java/com/javatechie/multiple/ds/api/model/book/Book.java /
```java
package com.javatechie.multiple.ds.api.model.book;

import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
@Entity
@Table(name="BOOK_TB")
public class Book {

	@Id
	private int id;
	private String name;
}
```

### /src/main/java/com/javatechie/multiple/ds/api/model/user/User.java 
```java
package com.javatechie.multiple.ds.api.model.user;

import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
@Entity
@Table(name="USER_TB")
public class User {
	@Id
	private int id;
	private String userName;
}
```

### /src/main/java/com/javatechie/multiple/ds/api/book/repository/
```java
package com.javatechie.multiple.ds.api.book.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import com.javatechie.multiple.ds.api.model.book.Book;

@Repository
public interface BookRepository extends JpaRepository<Book, Integer>{

}
```

### /src/main/java/com/javatechie/multiple/ds/api/user/repository/UserRepository.java
```java
package com.javatechie.multiple.ds.api.user.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import com.javatechie.multiple.ds.api.model.user.User;

@Repository
public interface UserRepository extends JpaRepository<User, Integer> {

}
```

### /src/main/java/com/javatechie/multiple/ds/api/config/BookDBConfig.java /
```java
package com.javatechie.multiple.ds.api.config;

import java.util.HashMap;

import javax.persistence.EntityManagerFactory;
import javax.sql.DataSource;

import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.jdbc.DataSourceBuilder;
import org.springframework.boot.orm.jpa.EntityManagerFactoryBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.jpa.repository.config.EnableJpaRepositories;
import org.springframework.orm.jpa.JpaTransactionManager;
import org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean;
import org.springframework.transaction.PlatformTransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;

@Configuration
@EnableTransactionManagement
@EnableJpaRepositories(entityManagerFactoryRef = "bookEntityManagerFactory", transactionManagerRef = "bookTransactionManager", basePackages = {
		"com.javatechie.multiple.ds.api.book.repository" })
public class BookDBConfig {

	@Bean(name = "bookDataSource")
	@ConfigurationProperties(prefix = "spring.book.datasource")
	public DataSource dataSource() {
		return DataSourceBuilder.create().build();
	}

	@Bean(name = "bookEntityManagerFactory")
	public LocalContainerEntityManagerFactoryBean bookEntityManagerFactory(EntityManagerFactoryBuilder builder,
			@Qualifier("bookDataSource") DataSource dataSource) {
		HashMap<String, Object> properties = new HashMap<>();
		properties.put("hibernate.hbm2ddl.auto", "update");
		properties.put("hibernate.dialect", "org.hibernate.dialect.MySQL5Dialect");
		return builder.dataSource(dataSource).properties(properties)
				.packages("com.javatechie.multiple.ds.api.model.book").persistenceUnit("Book").build();
	}

	@Bean(name = "bookTransactionManager")
	public PlatformTransactionManager bookTransactionManager(
			@Qualifier("bookEntityManagerFactory") EntityManagerFactory bookEntityManagerFactory) {
		return new JpaTransactionManager(bookEntityManagerFactory);
	}
}
```

### /src/main/java/com/javatechie/multiple/ds/api/config/UserDBConfig.java /
```java
package com.javatechie.multiple.ds.api.config;

import java.util.HashMap;

import javax.persistence.EntityManagerFactory;
import javax.sql.DataSource;

import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.jdbc.DataSourceBuilder;
import org.springframework.boot.orm.jpa.EntityManagerFactoryBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Primary;
import org.springframework.data.jpa.repository.config.EnableJpaRepositories;
import org.springframework.orm.jpa.JpaTransactionManager;
import org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean;
import org.springframework.transaction.PlatformTransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;

@Configuration
@EnableTransactionManagement
@EnableJpaRepositories(entityManagerFactoryRef = "entityManagerFactory", basePackages = {
		"com.javatechie.multiple.ds.api.user.repository" })
public class UserDBConfig {
	@Primary
	@Bean(name = "dataSource")
	@ConfigurationProperties(prefix = "spring.user.datasource")
	public DataSource dataSource() {
		return DataSourceBuilder.create().build();
	}

	@Primary
	@Bean(name = "entityManagerFactory")
	public LocalContainerEntityManagerFactoryBean entityManagerFactory(EntityManagerFactoryBuilder builder,
			@Qualifier("dataSource") DataSource dataSource) {
		HashMap<String, Object> properties = new HashMap<>();
		properties.put("hibernate.hbm2ddl.auto", "update");
		properties.put("hibernate.dialect", "org.hibernate.dialect.MySQL5Dialect");
		return builder.dataSource(dataSource).properties(properties)
				.packages("com.javatechie.multiple.ds.api.model.user").persistenceUnit("User").build();
	}

	@Primary
	@Bean(name = "transactionManager")
	public PlatformTransactionManager transactionManager(
			@Qualifier("entityManagerFactory") EntityManagerFactory entityManagerFactory) {
		return new JpaTransactionManager(entityManagerFactory);
	}
}
```

### /src/main/resources/application.properties
```java
#MYSQL
spring.user.datasource.jdbcUrl=jdbc:mysql://localhost:3306/datasource1
spring.user.datasource.username=root
spring.user.datasource.password=Password
spring.user.datasource.driver-class-name=com.mysql.jdbc.Driver

spring.jpa.show-sql = true

#Oracle
spring.book.datasource.jdbcUrl=jdbc:mysql://localhost:3306/datasource2
spring.book.datasource.username=root
spring.book.datasource.password=Password
spring.book.datasource.driver-class-name=com.mysql.jdbc.Driver

spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
```
