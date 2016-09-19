### 响应资源http请求的响应头 Set-cookie
````javascript
Set-Cookie: PTOKEN=;expires=Mon,01 Jan 1970 00:00:00 GMT;path=/;domain=.foo.com;HttpOnly;Secure;
expires: 过期时间，如果过期时间是过去，那就表明这个cookie要被删。
path: 相对路径，只有这个路径下的资源可以访问这个cookie。
domain: 域名，有权限设置为更高一级的域名。
HttpOnly: 标志（默认无，如果有的话，表明Cookie存在于HTTP层面，不能被客户端脚本读取）。
Secure: 标志（默认无，如果有的话，表明Cookie仅通过HTTPS协议进行安全传输）。
````
````txt
Http是无状态的，那么每次在连接时，服务端如何知道你是上一次的那个？
这里通过Cookies进行会话跟踪，第一次响应时设置的Cookies在随后的每次请求中都会发送出去。
````
