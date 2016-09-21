## spring session 配置入门

#### [官方配置在线文档](http://docs.spring.io/spring-session/docs/1.2.2.RELEASE/reference/html5/)

##### pom.xml
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.codebt</groupId>
	<artifactId>springsession</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>war</packaging>
	<name>springsessiontest</name>

	<dependencies>
		<dependency>
			<groupId>org.springframework.session</groupId>
			<artifactId>spring-session</artifactId>
			<version>1.2.2.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-pool2</artifactId>
			<version>2.4.2</version>
		</dependency>

		<dependency>
			<groupId>com.hazelcast</groupId>
			<artifactId>hazelcast</artifactId>
			<version>3.5.4</version>
		</dependency>
		<dependency>
			<groupId>commons-logging</groupId>
			<artifactId>commons-logging</artifactId>
			<version>1.2</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.data</groupId>
			<artifactId>spring-data-gemfire</artifactId>
			<version>1.8.1.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.data</groupId>
			<artifactId>spring-data-mongodb</artifactId>
			<version>1.9.1.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.data</groupId>
			<artifactId>spring-data-redis</artifactId>
			<version>1.7.1.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>4.2.5.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.security</groupId>
			<artifactId>spring-security-web</artifactId>
			<version>4.0.2.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>4.2.5.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>4.2.5.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-messaging</artifactId>
			<version>4.2.5.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>4.2.5.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-websocket</artifactId>
			<version>4.2.5.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>3.0.1</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>org.assertj</groupId>
			<artifactId>assertj-core</artifactId>
			<version>2.3.0</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.mockito</groupId>
			<artifactId>mockito-core</artifactId>
			<version>1.10.19</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>redis.clients</groupId>
			<artifactId>jedis</artifactId>
			<version>2.8.1</version>
		</dependency>
		<!-- Log4j -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>1.7.5</version>
		</dependency>
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>1.2.15</version>
			<exclusions>
				<exclusion>
					<groupId>javax.jms</groupId>
					<artifactId>jms</artifactId>
				</exclusion>
				<exclusion>
					<groupId>com.sun.jdmk</groupId>
					<artifactId>jmxtools</artifactId>
				</exclusion>
				<exclusion>
					<groupId>com.sun.jmx</groupId>
					<artifactId>jmxri</artifactId>
				</exclusion>
			</exclusions>
			<scope>runtime</scope>
		</dependency>
	</dependencies>
</project>
```

##### spring-content.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:util="http://www.springframework.org/schema/util"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-4.1.xsd
	http://www.springframework.org/schema/context 
	http://www.springframework.org/schema/context/spring-context-4.1.xsd
	http://www.springframework.org/schema/util 
	http://www.springframework.org/schema/util/spring-util.xsd">
		

	<context:annotation-config />
	<bean class="org.springframework.session.data.redis.config.annotation.web.http.RedisHttpSessionConfiguration" />

	<bean class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory">
        <property name="hostName" value="127.0.0.1" />
        <property name="port" value="6379" />
    </bean>
	
</beans>
```

- tips:redis要求2.8+版本以上

##### servlet-content.xml spring上下文配置文件
```

<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:beans="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">


	<!-- <default-servlet-handler /> -->

	<context:component-scan base-package="com.codebt"
		use-default-filters="false">
		<context:include-filter type="annotation"
			expression="org.springframework.stereotype.Controller" />
	</context:component-scan>
	<beans:bean class="org.springframework.security.web.session.HttpSessionEventPublisher"/>
	<beans:bean
		class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
		<beans:property name="messageConverters">
			<beans:list>
				<beans:ref bean="jsonHttpMessageConverter" />
			</beans:list>
		</beans:property>
	</beans:bean>

	<beans:bean id="jsonHttpMessageConverter"
		class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
		<beans:property name="supportedMediaTypes">
			<beans:list>
				<beans:value>application/json;charset=UTF-8</beans:value>
			</beans:list>
		</beans:property>
	</beans:bean>

	<!-- Enables the Spring MVC @Controller programming model -->
	<annotation-driven />

	<resources mapping="/resources/**" location="/resources/" />

</beans:beans>

```

##### web.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://java.sun.com/xml/ns/javaee"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
	version="3.0">
	<context-param>
		<param-name>spring.profiles.active</param-name>
		<param-value>development</param-value>
	</context-param>
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:root-context.xml</param-value>
	</context-param>
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
	
	<filter>
		<filter-name>springSessionRepositoryFilter</filter-name>
		<filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
	</filter>
	<filter-mapping>
		<filter-name>springSessionRepositoryFilter</filter-name>
		<url-pattern>/*</url-pattern>
		<dispatcher>REQUEST</dispatcher>
		<dispatcher>ERROR</dispatcher>
	</filter-mapping>
	<filter>
		<filter-name>Set Character Encoding</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>utf-8</param-value>
		</init-param>
		<init-param>
			<param-name>forceEncoding</param-name>
			<param-value>true</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>Set Character Encoding</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

	<servlet>
		<servlet-name>appServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:servlet-context.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>appServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
</web-app>
```

##### Controller
```
@Controller
@RequestMapping("/common")
public class CommonController {
	
	@ResponseBody
	@RequestMapping("/index")
	public String index(HttpServletRequest request) {
		 String attributeName = request.getParameter("attributeName");
         String attributeValue = request.getParameter("attributeValue");
         request.getSession().setAttribute(attributeName, attributeValue);
		return "index";
	}
}
```

- tips:去掉index函数HttpServletRequest request参数后 访问不会有session创建 蛋疼的几个小时 原理之后补上

- redis-cli 
- 127.0.0.1:6379> keys *
- (empty list or set)
- 127.0.0.1:6379> keys *
- 1) "spring:session:sessions:expires:af1411e3-746c-4dc4-b4e5-cd6bb071b1f9"
- 2) "spring:session:expirations:1474389540000"
- 3) "spring:session:sessions:af1411e3-746c-4dc4-b4e5-cd6bb071b1f9"
- 127.0.0.1:6379> get "spring:session:sessions:expires:af1411e3-746c-4dc4-b4e5-- cd6bb071b1f9"
- ""




