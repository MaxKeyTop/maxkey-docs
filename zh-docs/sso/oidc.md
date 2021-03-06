<h2>OpenID Connect应用集成</h2>
本文介绍OpenID Connect应用如何与MaxKey进行集成。

<h2>认证流程</h2>

请参照OAuth2认证流程

<h2>应用注册</h2>
应用在MaxKey管理系统进行注册，注册的配置信息如下

<img src="/static/images/sso/sso_oidc_conf.png"   alt=""/>

<h2>集成和接口</h2>
<h4>3)用户属性接口/api/connect/v10/userinfo</h4>

通过访问token 获取登录用户信息及签名信息，在程序中必须验证相关的签名信息。
 
<table  border="0" class="table table-striped table-bordered ">
 	   <tr>
	    <th> 接口名称 </th>
	    <th> OIDC授权用户信息查询接口 </th>
  	  </tr>
	  <tr>
	    <td> 接口地址 </td>
	    <td>http://sso.maxkey.org/sign/api/connect/v10/userinfo</td>
	  </tr>
	  <tr>
	    <td> 请求方式 </td>
	    <td> <code>GET</code> <code>POST</code> </td>
	  </tr>
</table>
 	 
<h5>请求参数</h5>

<table  border="0" class="table table-striped table-bordered ">
 	   <tr>
	    <th>参数 </th>
	    <th> 说明 </th>
  	  </tr>
	  <tr>
	    <td> access_token </td>
	    <td> 调用/authz/oauth/v20/token获得的token值。 </td>
	  </tr>
	  <tr align="left">
	  	<td colspan="2" align="left">
	  				实际请求如下：
<pre><code class="http hljs"> 
POST /api/connect/v10/userinfo HTTP/1.1
Host: sso.maxkey.org/openapi
Content-Type: application/x-www-form-urlencoded
access_token= PQ7q7W91a-oMsCeLvIaQm6bTrgtp7
</code></pre>
	  	</td>
	  </tr>
	  <tr>
	  		<td colspan="2" align="left">
	  		返回数据/ response data
	  		</td>
	  </tr>
	  <tr>
	  		<td colspan="2">
	  		<p>成功返回JSON数据，如下：</p>
<pre v-pre="" data-lang="json"><code class="lang-json"> 
{
userid     :  "zhangs",
				…
}
</code></pre>
<br/>
zhangs是认证的用户ID
	  		</td>
	  </tr>
 	 </table> 	



OAuth认证接口属性列表

<table   border="0" class="table table-striped table-bordered ">
   <tr >
	<th> 属性名(Attribute) </th>
	<th> 描述 </th>
	<th>数据类型</th>
  </tr>
  <tr>
	<td>uid</td>
	<td>uid</td>
	<td>字符串</td>
  </tr>
 </table> 	

其他请参照OAuth2


<h2>OIDC V1客户端集成</h2>

本文使用JAVA WEB程序为例

<h3> 第一步，引入客户端所需包</h3>

```ini
gson-2.2.4.jar
maxkey-client-sdk.jar
nimbus-jose-jwt-8.10.jar
commons-codec-1.9.jar
commons-io-2.2.jar
commons-logging-1.1.1.jar
```

<h3> 第二步，认证跳转</h3>

```java
<%@ page language="java" import="java.util.*" pageEncoding="ISO-8859-1"%>
<%@ page language="java" import="org.maxkey.client.oauth.oauth.*" %>
<%@ page language="java" import="org.maxkey.client.oauth.builder.*" %>
<%@ page language="java" import="org.maxkey.client.oauth.builder.api.MaxkeyApi20" %>
<%@ page language="java" import="org.maxkey.client.oauth.model.Token" %>

<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+path+"/";

String callback="http://oauth.demo.maxkey.top:8080/demo-oauth/oauth20callback.jsp";
OAuthService service = new ServiceBuilder()
                            .provider(MaxkeyApi20.class)
                            .apiKey("ae20330a-ef0b-4dad-9f10-d5e3485ca2ad")
                            .apiSecret("KQY4MDUwNjIwMjAxNTE3NTM1OTEYty")
                            .callback(callback)
                            .build();
Token EMPTY_TOKEN = null;
String authorizationUrl = service.getAuthorizationUrl(EMPTY_TOKEN);

request.getSession().setAttribute("oauthv20service", service);

%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">
    
    <title>OIDC V1 SSO</title>
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
	<meta http-equiv="description" content="This is my page">
  </head>
  
  <body>
    <a href="<%=authorizationUrl%>&approval_prompt=auto">OIDC V1 SSO</a>
  </body>
</html>

```


<h3> 第三步，获取令牌和用户信息及验证签名 (id_token及用户信息)</h3>

```java
<%@ page language="java" import="java.util.*" pageEncoding="utf-8"%>
<%@ page language="java" import="org.maxkey.client.oauth.oauth.*" %>
<%@ page language="java" import="org.maxkey.client.oauth.builder.*" %>
<%@ page language="java" import="org.maxkey.client.oauth.builder.api.MaxkeyApi20" %>
<%@ page language="java" import="org.maxkey.client.oauth.model.*" %>
<%@ page language="java" import="org.maxkey.client.oauth.*" %>
<%@ page language="java" import="org.maxkey.client.oauth.domain.*" %>
<%@ page language="java" import="org.maxkey.client.utils.*" %>
<%@ page language="java" import="com.nimbusds.jwt.JWTClaimsSet" %>
<%@ page language="java" import="com.nimbusds.jose.*" %>
<%@ page language="java" import="com.nimbusds.jwt.*" %>
<%@ page language="java" import="com.connsec.oidc.jose.keystore.*" %>
<%@ page language="java" import="com.nimbusds.jose.jwk.*" %>
<%@ page language="java" import="java.io.File" %>
<%@ page language="java" import="com.nimbusds.jose.crypto.*" %>
<%@ page language="java" import="com.google.gson.*" %>

<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";

OAuthService service = (OAuthService)request.getSession().getAttribute("oauthv20service");

if(service==null){
	String callback="http://oauth.demo.maxkey.top:8080/demo-oauth/oidc10callback.jsp";
	service = new ServiceBuilder()
     .provider(MaxkeyApi20.class)
     .apiKey("ae20330a-ef0b-4dad-9f10-d5e3485ca2ad")
     .apiSecret("KQY4MDUwNjIwMjAxNTE3NTM1OTEYty")
     .callback(callback)
     .build();
}

Token EMPTY_TOKEN = null;
Verifier verifier = new Verifier(request.getParameter("code"));
Token accessToken = service.getAccessToken(EMPTY_TOKEN, verifier);

//JWTClaimsSet idClaims = JWTClaimsSet.parse(accessToken.getId_token());
SignedJWT signedJWT=null;

//JWKSetKeyStore jwkSetKeyStore=new JWKSetKeyStore();

File jwksFile=new File(PathUtils.getInstance().getClassPath()+"jwk.jwks");
JWKSet jwkSet=JWKSet.load(jwksFile);

RSASSAVerifier rsaSSAVerifier = new RSASSAVerifier(((RSAKey) jwkSet.getKeyByKeyId("maxkey_rsa")).toRSAPublicKey());
try {
    signedJWT = SignedJWT.parse(accessToken.getId_token());
} catch (java.text.ParseException e) {
    // Invalid signed JWT encoding
}
;

OAuthClient restClient=new OAuthClient("https://sso.maxkey.top/maxkey/api/connect/v10/userinfo",accessToken.getToken());
 
OIDCUserInfo userInfo=restClient.getOIDCUserInfo(accessToken.getToken());
 
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">

   <title>OpenID Connect 1.0 Demo</title>
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
	<meta http-equiv="description" content="OpenID Connect 1.0 Demo">
	<link rel="shortcut icon" type="image/x-icon" href="<%=basePath %>/static/images/favicon.ico"/>
	<script type="text/javascript" src="<%=basePath %>/jquery-3.5.0.min.js"></script>
	<script type="text/javascript" src="<%=basePath %>/jsonformatter.js"></script>
	<link   type="text/css" rel="stylesheet"  href="<%=basePath %>/demo.css"/>

  </head>
  
  <body>
  		<div class="container">
	  		<table class="datatable">
	  			<tr>
	  				
	  				<td colspan="2" class="title">OpenID Connect 1.0 Demo</td>
	  			</tr>
	  			
	  			<tr>
	  				<td>OpenID Connect 1.0 Logo</td>
	  				<td> <img src="<%=basePath %>/static/images/openid.png"  width="124px" height="124px"/></td>
	  			</tr>
	  			<tr>
	  				<td>Login</td>
	  				<td><%=userInfo.getSub() %></td>
	  			</tr>
	  			<tr>
	  				<td>DisplayName</td>
	  				<td><%=userInfo.getName()%></td>
	  			</tr>
	  			<tr>
	  				<td>Department</td>
	  				<td><%=userInfo.getGender() %></td>
	  			</tr>
	  			
	  			<tr>
	  				<td>email</td>
	  				<td><%=userInfo.getEmail() %></td>
	  			</tr>
	  			<tr>
	  				<td>ResponseString</td>
	  				<td style="word-wrap: break-word;">
						<textarea cols="68" rows="20" v-model="text2"><%=userInfo.getResponseString() %></textarea>
					</td>
	  			</tr>
	  			<tr>
	  				<td>Id_token</td>
	  				<td style="word-wrap: break-word;"><%=accessToken.getId_token() %></td>
	  			</tr>
				<tr>
	  				<td>Verify</td>
	  				<td style="word-wrap: break-word;"><%=signedJWT.verify(rsaSSAVerifier) %></td>
	  			</tr>
	  			<tr>
	  				<td>Issuer</td>
	  				<td style="word-wrap: break-word;"><%=signedJWT.getJWTClaimsSet().getIssuer() %></td>
	  			</tr>
	  			<tr>
	  				<td>JWTClaims</td>
	  				<td style="word-wrap: break-word;">
						<textarea cols="68" rows="20" v-model="text2"><%=signedJWT.getPayload() %></textarea>
					</td>
	  			</tr>
	  			
	  		</table>
  		</div> 
		<script type="text/javascript">
			FormatTextarea();
		</script>
  </body>
</html>

```
