---
layout: zh/default
---
<h2>第三方账号登录</h2>

为了方便用户的登录，可以通过第三方的账号(例如新浪微博、微信、钉钉等)登录MaxKey，简单配置即可实现用户登录。

本文以新浪微博为例

<img src="{{ "/static/images/authn/authn_s_1.png" | prepend: site.baseurl }}?{{ site.time | date: "%Y%m%d%H%M" }}"  alt=""/>

<h3>登录流程</h3>

<img src="{{ "/static/images/authn/authn_s.png" | prepend: site.baseurl }}?{{ site.time | date: "%Y%m%d%H%M" }}"  alt=""/>

<h3>第三方认证配置</h3>
在新浪微博开放平台https://open.weibo.com/申请接入，新浪配置如下

<img src="{{ "/static/images/authn/authn_s_2.png" | prepend: site.baseurl }}?{{ site.time | date: "%Y%m%d%H%M" }}"  alt=""/>

<img src="{{ "/static/images/authn/authn_s_3.png" | prepend: site.baseurl }}?{{ site.time | date: "%Y%m%d%H%M" }}"  alt=""/>

<h3>账号绑定</h3>
登录MaxKey，并绑定新浪微博账号

<img src="{{ "/static/images/authn/authn_s_4.png" | prepend: site.baseurl }}?{{ site.time | date: "%Y%m%d%H%M" }}"  alt=""/>

<h3>登录测试</h3>

退出后，进入登录界面，点击新浪微博图标，跳转到新浪微博，输入用户名和密码后，直接登录MaxKey,即MaxKey信任了微博账号，


<h3>第三方支持</h3>
MaxKey使用JustAuth作为第三方OAuth2登录认证库，认证所支持的第三方，请见JustAuth官方说明

<a href="https://www.justauth.cn/" target="_blank"  alt="JustAuth">
<img style="width:250px;" src="{{ "/static/images/authn/justauth.png" | prepend: site.baseurl }}?{{ site.time | date: "%Y%m%d%H%M" }}"  alt=""/>
</a>
