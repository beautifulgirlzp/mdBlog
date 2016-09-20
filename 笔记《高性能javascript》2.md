### 最快的ajax请求就是没有请求
###### 有两种主要的方法可避免发送不必要的请求
* 在服务端，设置http头信息以确保你的响应会被浏览器缓存。
* 在客户端，把获取到的信息存储到本地，从而避免再次请求。

````txt
设置Expires头信息是确保浏览器缓存Ajax响应最简单的方法。
````
###### 本地储存：把响应文本只在到一个对象中，以URL为键值作为索引。如下
````javascript
var localCache = {}
function xhrRequest(url, cb){
  if(localCache[url]){
    cb.success(localCache[url]);
    return;
  }
  var req = createXhrObject();
  req.onerror = function(){
    cb.error();
  }
  req.onreadystatechange = function(){
    if(req.readyState == 4){
      if(req.responseText == '' || req.status == '404'){
        cb.error();
        return;
      }
      localCache[url] = req.responseText;
      cb.success( localCache[url] );
    }
  }
  req.open('GET',url,true);
  req.send(null);
}
````
### 避免双重求值
`当你在js代码中执行另一段js代码时，会导致双重求值的性能消耗`
````javascript
var num1 = 5,
    num2 = 6,
    result = eval("num1 + num2"),
    sum = new Function("arg1","arg2","return arg1+arg2");
    setTimeout("sum = num1 + num2",1000);
````
`以上代码会先以正常方式求值，然后在执行过程中对包含于字符串中的代码发起另一个求值运算。`
### 位运算
`i & 1`经常用于判断奇偶属性，为0时候，表示偶数，非0时候，表示奇数
````javascript
/*
* 每个选项值必须是2的幂
* 选项不在列表中时，值为0 false
* 选项在列表中时，值为相应值 true
*/
var a = 1,b = 2,c = 4,d = 8,e = 16;
var x = a | b | c;
x & a // 1 true
x & b // 2 true
x & c // 4 true
x & d // 0 false
x & e // 0 false
x & 32 //0 false
````

