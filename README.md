# <img src="static/images/logo_maxkey.png?raw=true"  width="200px"   alt=""/>


<a href="README_en.md" target="_blank"><b>English</b></a>  |  <a href="README_zh.md" target="_blank"><b>中文</b></a>

# 概述

<b>MaxKey</b>单点登录认证系统，谐音马克思的钥匙寓意是最大钥匙,是<b>业界领先的IAM身份管理和认证产品</b>,支持OAuth 2.x/OpenID Connect、SAML 2.0、JWT、CAS、SCIM等标准协议，提供<b>简单、标准、安全和开放</b>的用户身份管理(IDM)、身份认证(AM)、单点登录(SSO)、RBAC权限管理和资源管理等。

官方网站  <a href="https://www.maxkey.top" target="_blank"><b>官网</b></a> |  <a href="https://maxkeytop.gitee.io" target="_blank"><b>官网二线</b></a>

QQ交流群：<b>434469201</b> 

邮箱email: <b>maxkeysupport@163.com</b>

代码托管 <a href="https://gitee.com/dromara/MaxKey" target="_blank"><b>Gitee</b></a> | <a href="https://github.com/dromara/MaxKey" target="_blank"><b>GitHub</b></a>

 
><b>单点登录(Single Sign On）</b>简称为<b>SSO</b>，用户只需要登录认证中心一次就可以访问所有相互信任的应用系统，无需再次登录。
>
>**主要功能：**
>1) 所有应用系统共享一个身份认证系统
>2) 所有应用系统能够识别和提取ticket信息
 
 
# 产品特性

1.  标准认证协议：

| 序号    | 协议    |  支持   |
| --------| :-----  | :----   |
| 1.1     | OAuth 2.x/OpenID Connect    	|  高  |
| 1.2     | SAML 2.0                    	|  高  |
| 1.3     | JWT                         	|  高  |
| 1.4     | CAS                         	|  高  |
| 1.5     | FormBased                  		|  中  |
| 1.6     | TokenBased(Post/Cookie)     	|  中  |
| 1.7     | ExtendApi                   	|  低  |
| 1.8     | EXT                         	|  低  |

2. 登录支持

| 序号    | 登录方式      |   支持  |
| --------| :-----        | :----   |
| 2.1     | 动态验证码    | 字母/数字/算术   | 
| 2.2     | 双因素认证    | 短信/时间令牌/邮件 | 
| 2.3     | 短信认证      | 腾讯云短信/阿里云短信/网易云信  |
| 2.4     | 时间令牌      | 登录易/Google/Microsoft Authenticator/FreeOTP/支持TOTP或者HOTP |
| 2.5     | 域认证        | Kerberos/SPNEGO/AD域 |
| 2.6     | LDAP          | OpenLDAP/ActiveDirectory/标准LDAP服务器 |
| 2.7     | 社交账号      | 微信/QQ/微博/钉钉/Google/Facebook/其他  | 
| 2.8     | 扫码登录      | 企业微信/钉钉/飞书扫码登录  | 


3. 提供标准的认证接口以便于其他应用集成SSO，安全的移动接入，安全的API、第三方认证和互联网认证的整合。

4. 提供用户生命周期管理，支持SCIM 2协议；开箱即用的连接器(Connector)实现身份供给同步。

5. 简化微软Active Directory域控、标准LDAP服务器机构和账号管理，密码自助服务重置密码。

6. 认证多租户功能，支持集团下多企业独立管理或企业下不同部门数据隔离的，降低运维成本。

7. 认证中心具有平台无关性、环境多样性，支持Web、手机、移动设备等, 如Apple iOS，Andriod等，将认证能力从B/S到移动应用全面覆盖。

8. 基于Java EE平台，微服务架构，采用Spring、MySQL、Tomcat、Redis、MQ等开源技术，扩展性强。  

9. 开源、安全、自主可控，许可证 Apache 2.0 License & <a href="https://maxkey.top/zh/about/licenses.html" target="_blank">MaxKey版权声明</a>。  


# 界面

**MaxKey认证**

登录界面
<img src="static/images/maxkey_login.png?raw=true"/>

主界面
<img src="static/images/maxkey_index.png?raw=true"/>

**MaxKey管理**

访问报表
<img src="static/images/maxkey_mgt_rpt.png?raw=true"/>

用户管理
<img src="static/images/maxkey_mgt_users.png?raw=true"/>

应用管理
<img src="static/images/maxkey_mgt_apps.png?raw=true"/>


# 下载

<a href="https://maxkey.top/zh/about/download.html" target="_blank"> 官方版本下载</a>

# Dromara社区
<a href="https://dromara.org/zh/" target="_blank">**Dromara**</a>致力于微服务云原生解决方案的组织。

 - **开放** 技术栈全面开源共建、 保持社区中立、兼容社区 兼容开源生态
 
 - **愿景** 让每一位开源爱好者，体会到开源的快乐
 
 - **口号** 为往圣继绝学，一个人或许能走的更快，但一群人会走的更远