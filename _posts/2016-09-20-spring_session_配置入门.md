## spring session 配置入门

#### [官方配置在线文档](http://25.io/smaller/)

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




