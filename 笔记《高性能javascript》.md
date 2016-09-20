## 高性能javascript
#### defer 运行在window.onload之前
````javascript
<script defer>
  alert('defer')
</script>
<script>
  alert('script')
</script>
<script>
window.onload = function(){
  alert('load')
}
</script>

弹出顺序 script defer load
````
#### 性能优化之动态加载脚本
````javascript
/*
* 动态加载脚本凭借着它在浏览器兼容性和易用的优势，成为最通用的无阻塞加载解决方案。
*/
function loadScript(url,callback){
  var script = doc.createElement('script');
  script.type = "text/javascript";
  if( script.readyState ){// IE
    script.onreadystatechange = function(){
      if( script.readyState == 'loaded' || script.readyState == 'complete' ){
        script.onreadystatechange = null;
        callback();
      }
    }
  }else{
    script.onload = function(){
      callback();
    }
  }
  script.src = "file1.js";
  doc.getElementsByTagName('head')[0].appendChild(script); 
}
/*
* 动态加载脚本，可以尽可能多的加载，如果要确保加载顺序，如下
* 更好的做法是 把它们按顺序合并为一个文件，因为过程是异步的，文件大一点，不会影响
*/
loadScript('file1.js',function(){
  laodScript('file2.js',function(){
    loadScript('file3.js',function(){
      alert('all files loaded!')
    })
  })
})
````
#### 性能优化之XMLHttpRequest脚本注入
````javascript
/*
* 优点是兼容性强，各浏览器都支持。
* 不足是不支持跨域，也就不能从CDN下载
*/
var xhr = new XMLHttpRequest();
xhr.open('get','file1.js',true);
xhr.onreadystatechange = function(){
  if( xhr.readyState == 4 ){
    if( xhr.status >= 200 && xhr.status < 300 || xhr.status == 304 ){
      var script = doc.createElement('script');
      script.type = 'text/javascript';
      script.text = xhr.responseText;
      doc.body.appendChild(script);
  }
};
xhr.send(null);
````
#### 推荐的无阻塞模式
````javascript
/*
* 添加大量script做法，只需两步
* 先添加动态加载所需的代码，然后加载初始化页面所需的剩下的代码
* 因为第一部分代码尽量精简，它下载执行很快，所以不会对页面有太多影响
* 记得把这些代码压缩到最小尺寸
*/
<script>
function loadScript(url,callback){
  var script = doc.createElement('script');
  script.type = "text/javascript";
  if( script.readyState ){// IE
    script.onreadystatechange = function(){
      if( script.readyState == 'loaded' || script.readyState == 'complete' ){
        script.onreadystatechange = null;
        callback();
      }
    }
  }else{
    script.onload = function(){
      callback();
    }
  }
  script.src = "file1.js";
  doc.getElementsByTagName('head')[0].appendChild(script); 
}

loadScript('the-rest.js',function(){
  Application.init();// Application是the-rest.js里面的对象
})
</script>
````
#### loadScript增加版 Yahoo工程师编辑了延迟加载工具，压缩后（gzip）之前，只有1.5k[github地址](https://github.com/rgrove/lazyload/)
````javascript
/*
* LazyLoad同样可以动态加载css文件，这没有太大意义，因为css文件已是并行下载，不会阻塞页面的其他进程
*/
<script type="tetxt/javascript" src="lazyload-min.js"></script>
<script>
  LazyLoad.js(['first-file.js','the-rest.js'],function(){
    Application.init();
  })
</script>
````
#### LABjs 另一个开源的无阻塞脚本加载工具 对加载过程更加精细，压缩后只有4.5k（非gzip）
````javascript
<script src="lab.js" type="text/javascript"></script>
<script>
  $LAB.script("first-file.js").script("the-rest.js").wait(function(){
    Application.init();
  })
</script>
````
#### 标识符解析的性能
````javascript
/*
* 标识符解析是有代价的。在执行环境的作用域链中，一个标识符的位置越深，它的读写速度也就越慢。因此，函数中读写局部变量总是最快的，而读写全局变量通常是最慢的（优化js引擎在某些情况下能有所改善）。请记住，全局变量总是存在于执行环境作用域链的最末端，因此它也是最远的。
* 所以，在没有优化js引擎的浏览器中，建议尽量使用局部变量。如果某个跨作用域的值在函数中被引用一次以上，那么就把它存储到局部变量中 For 如下
*/
function initUI(){
  var bd = document.body,
  links = document.getElementsByTagName('a'),
  i = 0,
  len = links.length;
  while(i<len){
    update( links[i++] );
  }
  document.getElementById('go-btn').onclick = function(){
    start();
  }
  bd.className = 'active';
}
// document引用了三次，而且是全局对象，搜索该变量过程必须遍历整个作用域链，直到最后在全局变量对象中找到。减少性能影响：把全局变量的引用存储在一个局部变量中，然后使用这个局部变量代替全局变量。For
function initUI(){
  var doc = document,
      bd = doc.body,
      links = doc.getElementsByTagName('a'),
      i = 0,
      len = links.length;
  while(i<len){
    update( links[i++] );
  }
  doc.getElementById('go-btn').onclick = function(){
    start();
  }
  bd.className = 'active';
}
````
#### 算法与流程控制优化
````txt
1、for、while和do-while循环性能特性相当，并没有一种循环类型明显快于或慢于其它类型。
2、避免使用for-in类型，除非你需要遍历一个属性数量未知的对象。
3、改善循环性能的最佳方式是减少每次迭代的运算量和减少循环迭代次数。
4、通常来说，switch总是比if-else快，但并不是最佳解决方案。
5、在判断条件较多时，使用查找表比if-else和switch更快（查找表是把条件值放数组中）
6、浏览器的调用栈大小限制了递归算法在JavaScript中的应用，栈溢出错误会导致其他代码中断
7、如果你遇到栈溢出错误，可将方法改为迭代算法，或使用Memoization来避免重复运算。
````
