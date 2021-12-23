
<h2>CAS应用集成</h2>
本文介绍CAS应用如何与MaxKey进行集成。

<h2>应用注册</h2>

应用在MaxKey管理系统进行注册，注册的配置信息如下

<img src="/static/images/sso/sso_cas_conf.png"  alt=""/>


<h2>CAS客户端配置</h2>

本文使用JAVA WEB程序为例

源代码地址

https://github.com/MaxKeyTop/MaxKey-Demo/blob/master/maxkey-demo-cas


<h3> 第一步，引入cas 客户端所需包</h3>

jar包依赖如下

```ini
cas-client-core-3.2.1.jar
commons-codec-1.9.jar
commons-io-2.2.jar
commons-logging-1.1.1.jar
```


<h3> 第二步，web.xml安全及CAS配置</h3>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app>
	<display-name></display-name>
	<listener>
		<listener-class>org.jasig.cas.client.session.SingleSignOutHttpSessionListener</listener-class>
	</listener>
	<filter>
		<filter-name>CAS Single Sign Out Filter</filter-name>
		<filter-class>org.jasig.cas.client.session.SingleSignOutFilter</filter-class>
	</filter>
	<filter-mapping>
		<filter-name>CAS Single Sign Out Filter</filter-name>
		<url-pattern>/index.jsp</url-pattern>
	</filter-mapping>
	<filter>
		<filter-name>CAS Filter</filter-name>
		<filter-class>org.jasig.cas.client.authentication.AuthenticationFilter</filter-class>
		<!-- cas server login url -->
		<init-param>
			<param-name>casServerLoginUrl</param-name>
			<param-value>>https://sso.maxkey.top/maxkey/authz/cas/login</param-value>
		</init-param>
		<!-- cas client url, in end of url / is required -->
		<init-param>
			<param-name>serverName</param-name>
			<param-value>http://cas.demo.maxkey.top:8080/</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>CAS Filter</filter-name>
		<url-pattern>/index.jsp</url-pattern>
	</filter-mapping>

	<!-- Cas10TicketValidationFilter Cas20ProxyReceivingTicketValidationFilter -->
	<filter>
		<filter-name>CAS Validation Filter</filter-name>
		<filter-class>org.jasig.cas.client.validation.Cas20ProxyReceivingTicketValidationFilter</filter-class>
		<!-- cas server Validation url -->
		<init-param>
			<param-name>casServerUrlPrefix</param-name>
			<param-value>https://sso.maxkey.top/maxkey/authz/cas/</param-value>
		</init-param>
		<!-- cas client url -->
		<init-param>
			<param-name>serverName</param-name>
			<param-value>http://cas.demo.maxkey.top:8080/</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>CAS Validation Filter</filter-name>
		<url-pattern>/index.jsp</url-pattern>
	</filter-mapping>
	<filter>
		<filter-name>CAS HttpServletRequest Wrapper Filter</filter-name>
		<filter-class>
			org.jasig.cas.client.util.HttpServletRequestWrapperFilter
		</filter-class>
	</filter>
	<filter-mapping>
		<filter-name>CAS HttpServletRequest Wrapper Filter</filter-name>
		<url-pattern>/index.jsp</url-pattern>
	</filter-mapping>
	<filter>
		<filter-name>CAS Assertion Thread Local Filter</filter-name>
		<filter-class>org.jasig.cas.client.util.AssertionThreadLocalFilter</filter-class>
	</filter>
	<filter-mapping>
		<filter-name>CAS Assertion Thread Local Filter</filter-name>
		<url-pattern>/index.jsp</url-pattern>
	</filter-mapping>
	<welcome-file-list>
		<welcome-file>index.jsp</welcome-file>
	</welcome-file-list>
</web-app>
```

<h3> 第三步，JSP获取登录名及用户属性</h3>

```java
<%@ page language="java" import="java.util.*" pageEncoding="utf-8"%>
<%@ page language="java" import="java.util.Map.Entry" %>
<%@ page language="java" import="org.apache.commons.codec.binary.Base64" %>
<%@ page language="java" import="org.jasig.cas.client.authentication.AttributePrincipal" %>
<%@ page language="java" import="org.jasig.cas.client.validation.Assertion" %>
<%@ page language="java" import="org.jasig.cas.client.util.AbstractCasFilter" %>
<%
	String path = request.getContextPath();
	String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
	System.out.println("CAS Assertion Success . ");
	Assertion assertion = (Assertion) request.getSession().getAttribute(AbstractCasFilter.CONST_CAS_ASSERTION);
	                          
	String username=     assertion.getPrincipal().getName();
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">
    
    <title>Demo CAS</title>
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
	<meta http-equiv="description" content="CAS Demo">
	<link rel="shortcut icon" type="image/x-icon" href="<%=basePath %>/static/images/favicon.ico"/>

	<style type="text/css">
		body{
			margin: 0;
			margin-top: 0px;
			margin-left: auto;
			margin-right: auto;
			padding: 0 0 0 0px;
			font-size: 12px;
			text-align:center;
			float:center;
			font-family: "Arial", "Helvetica", "Verdana", "sans-serif";
		}
		.container {
			width: 990px;
			margin-left: auto;
			margin-right: auto;
			padding: 0 10px
		}
		table.datatable {
			border: 1px solid #d8dcdf;
			border-collapse:collapse;
			border-spacing:0;
			width: 100%;
		}
		
		table.datatable th{
			border: 1px solid #d8dcdf;
			border-collapse:collapse;
			border-spacing:0;
			height: 40px;
		}
		
		
		table.datatable td{
			border: 1px solid #d8dcdf;
			border-collapse:collapse;
			border-spacing:0;
			height: 40px;
		}
		
		table.datatable td.title{
			text-align: center;
			font-size: 20px;
			font-weight: bold;
		}
	</style>
  </head>
  
  <body>
  		<div class="container">
	  		<table class="datatable">
	  			<tr>
	  				<td colspan="2" class="title">CAS Demo for MaxKey</td>
	  			</tr>
	  			<tr>
	  				<td>CAS Logo</td>
	  				<td> <img src="<%=basePath %>/static/images/cas.png"/></td>
	  			</tr>
	  			<tr>
	  				<td width="50%">CAS Assertion</td>
	  				<td><%=username %></td>
	  			</tr>
	  			<tr>
	  				<td>CAS Has Attributes </td>
	  				<td><%=!assertion.getPrincipal().getAttributes().isEmpty() %> size : <%=assertion.getPrincipal().getAttributes().size() %></td>
	  			</tr>
	  			<%
		  			Map<String, Object> attMap = assertion.getPrincipal().getAttributes();  
		            for (Entry<String, Object> entry : attMap.entrySet()) {   
		            	String attributeValue=entry.getValue()==null?"":entry.getValue().toString();
		            	System.out.println("attributeValue : "+attributeValue);
		            	if(attributeValue.startsWith("base64:")){
		            		attributeValue=new String(Base64.decodeBase64(attributeValue.substring("base64:".length())),"UTF-8");
		            	}
		        %>
	  			<tr>
	  				<td>CAS <%=entry.getKey() %> </td>
	  				<td><%=attributeValue %></td>
	  			</tr>
	  			<%}%>
	  		</table>
  		</div>
  </body>
</html>
```


<h2>基于SpringBoot CAS客户端配置</h2>

源代码地址

https://github.com/MaxKeyTop/MaxKey-SpringBoot4CAS-demo


demo分别写了三个请求:拦截请求 test1/index,test1/index1 以及不拦截请求test1/index2,

<h3> 第一步，引入cas 客户端所需包</h3>

```html
<dependency>
	<groupId>net.unicon.cas</groupId>
	<artifactId>cas-client-autoconfig-support</artifactId>
	<version>2.3.0-GA</version>
</dependency>
```  

<h3>第二步，配置spring boot 配置文件</h3>

```ini
server:
  port: 8989
cas:
  # cas服务端地址
  server-url-prefix: http://sso.maxkey.top/maxkey/authz/cas/
  # cas服务端登陆地址
  server-login-url: http://sso.maxkey.top/maxkey/authz/cas/login
  # 客户端访问地址
  client-host-url: http://localhost:8989/
  # 认证方式，默认cas
  validation-type: cas
  #  客户端需要拦截的URL地址
  authentication-url-patterns:
    - /test1/index
    - /test1/index1
```

扩展配置项
```ini
cas.authentication-url-patterns
cas.validation-url-patterns
cas.request-wrapper-url-patterns
cas.assertion-thread-local-url-patterns
cas.gateway
cas.use-session
cas.redirect-after-validation
cas.allowed-proxy-chains
cas.proxy-callback-url
cas.proxy-receptor-url
cas.accept-any-proxy
server.context-parameters.renew
```

<h3>第三步 在application启动类上加上 @EnableCasClient 注解</h3>

```java
@SpringBootApplication
@EnableCasClient
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

}
```

<h3>第四步 在代码中获取登录用户信息</h3>

``` java
    @GetMapping("test1/index1")
    public String index1(HttpServletRequest request){
        String token =request.getParameter("token");
        System.out.println("token : "+token);
        Assertion assertion = (Assertion) request.getSession().getAttribute(AbstractCasFilter.CONST_CAS_ASSERTION);

        String username=     assertion.getPrincipal().getName();
        System.out.println(username);

        return "test index cas拦截正常,登录账号:"+username;
    }
```
