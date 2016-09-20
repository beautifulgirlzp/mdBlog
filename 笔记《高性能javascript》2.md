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

