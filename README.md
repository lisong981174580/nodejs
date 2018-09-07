# nodejs
nodejs的http模块，爬虫....
## nodejs.js学习
> 谷歌v8引擎+底层c++

> npm支持，很火

> 跨平台，高效开发管理html

> 偶数版本为稳定版，奇数办为不稳定版

#### 创建服务器

```
const http = require('http');
 //由js编写，负责创建web服务器，以及处理http相关的任务


const hostname = '127.0.0.1';
const port = 3000;


const server = http.createServer((req, res) => {
 //createServer创建一个web服务器
 //当请求进来时要告诉nodejs要做什么，于是传递了一个匿名的回调函数
 //通过listen监听端口3000，
 //监听到后，调用回调函数，req, res分别是请求体和响应体
 //req 获取请求信息   res 回应

  res.statusCode = 200; 
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');
});

server.listen(port, hostname, () => {
//listen监听端口

  console.log(`Server running at http://${hostname}:${port}/`);
});

```
或者：
```
const http = require('http');
http.createServer(function(req,res){
    res.writeHead(200,{'Content-type':'text/plain'});
    res.write('hello Nodejs');
    res.end();
    })
    .listen(2015)
```

####  common.js规范
> 是一套规范，包括了模块、二进制、包、系统来约定了js如何编写和组织

> 把不同任务的特定代码块或者代码文件看做是一个独立的模块或独立的作用域，但不是孤立的，可能有一定的依赖关系，对一个模块可以分为三个关键部分(模块的定义、标识、引用)

> nodejs借鉴了这个标准，mnodejs每个js文件就是一个独立的模块

#### 模块
> 核心模块、文件模块、第三方模块

模块的流程:

1、创建模块
```
function add(student){
  console.log('add student'+student);
}

```

2、导出模块
```
exports.add=add;
```

3、加载模块
```
var kclass=require('./class');
```

4、使用模块
```
kclass.add('Scott',['白富美','高富帅'])
```

#### url
> node内置的url模块
##### parse
> 解析url诸如host、protocol等
```
var url=require('url');
console.log(url.parse('http://localhost:7001/user/detail/index?uid=1617256082593208&tab=3'))

//返回
{
  protocol: 'http:',
  slashes: true,
  auth: null,
  host: 'localhost:7001',
  port: '7001',
  hostname: 'localhost',
  hash: null,//hash
  search: '?uid=1617256082593208&tab=3',
  query: 'uid=1617256082593208&tab=3',
  pathname: '/user/detail/index',
  path: '/user/detail/index?uid=1617256082593208&tab=3',
  href: 'http://localhost:7001/user/detail/index?uid=1617256082593208&tab=3' 
}

----------------------------------------------------
query以对象的形式解析
console.log(url.parse('http://localhost:7001/user/detail/index?uid=1617256082593208&tab=3'，true))

//返回
 {
  protocol: 'http:',
  slashes: true,
  auth: null,
  host: 'localhost:7001',
  port: '7001',
  hostname: 'localhost',
  hash: null,
  search: '?uid=1617256082593208&tab=3',
  query: { uid: '1617256082593208', tab: '3' },
  pathname: '/user/detail/index',
  path: '/user/detail/index?uid=1617256082593208&tab=3',
  href: 'http://localhost:7001/user/detail/index?uid=1617256082593208&tab=3' 
}



```
##### format
> 还原url
```
var url=require('url');
console.log(url.format({
  protocol: 'http:',
  slashes: true,
  auth: null,
  host: 'localhost:7001',
  port: '7001',
  hostname: 'localhost',
  hash: null,//hash
  search: '?uid=1617256082593208&tab=3',
  query: 'uid=1617256082593208&tab=3',
  pathname: '/user/detail/index',
  path: '/user/detail/index?uid=1617256082593208&tab=3',
  href: 'http://localhost:7001/user/detail/index?uid=1617256082593208&tab=3' 
}))

//返回
http://localhost:7001/user/detail/index?uid=1617256082593208&tab=3
```


##### resolve
> 组合路径
```
var url=require('url');
console.log(url.resolve('http://localhost:7001','/user/detail/index?uid=1617256082593208&tab=3'))

//返回
http://localhost:7001/user/detail/index?uid=1617256082593208&tab=3

```

#### qurystring

##### querystring.stringify
> 序列化
```
var querystring=require('querystring');
console.log(querystring.stringify({ uid: '1617256082593208', tab: ['lisong','lihnag'] }))

//返回
uid=1617256082593208&tab=lisong&tab=lihnag

------------------------------
第二个参数传递分隔符
var querystring=require('querystring');
console.log(querystring.stringify({ uid: '1617256082593208', tab: ['lisong','lihnag'] },','))

//返回
uid=1617256082593208,tab=lisong,tab=lihnag



第三个参数
var querystring=require('querystring');
console.log(querystring.stringify({ uid: '1617256082593208', tab: ['lisong','lihnag'] },',',':'))

//返回

uid:1617256082593208,tab:lisong,tab:lihnag


```
##### querystring.parse
> 反序列化
```
var querystring=require('querystring');
console.log(querystring.parse('uid=1617256082593208&tab=lisong&tab=lihnag'))

//返回
{ uid: '1617256082593208', tab: [ 'lisong', 'lihnag' ] }


---------------------------------------------
第二个参数传递分隔符
var querystring=require('querystring');
console.log(querystring.parse('uid=1617256082593208,tab=lisong,tab=lihnag',','),)

//返回
{ uid: '1617256082593208', tab: [ 'lisong', 'lihnag' ] }


第三个参数
var querystring=require('querystring');
console.log(querystring.parse('uid:1617256082593208,tab:lisong,tab:lihnag',',',':'),)

//返回
{ uid: '1617256082593208', tab: [ 'lisong', 'lihnag' ] }


```
##### querystring.escape
```
console.log(querystring.escape('<哈哈>'),)

//返回
%3C%E5%93%88%E5%93%88%3E

```
##### querystring.unescape
```
console.log(querystring.escape('<哈哈>'),)

//返回
<哈哈>

```

#### http
> 是一种协议，计算机等终端遵循的一种规则，bicentennial之间以此协议通讯

1.http客户端发起请求，创建端口

2.http服务器在端口监听客户端请求

3.http服务器向客端返回状态合内容


详细部分：

1.浏览器搜索自身的DNS缓存（chrome://net-internals/#-Google%20Search）

2.搜索操作系统自身的DNS缓存(浏览器没有找到缓存或缓存已经失效)

3.读取本地的host文件

4.浏览器发起一个DNS的一个系统调用
```
1.宽带运营商服务器查看本身缓存
2.运营商服务器发起一个迭代DNS解析的请求

运营商服务器吧结果返回操作系统内核同时缓存起来

操作系统内核结果返回浏览器

最终浏览器拿到了对应域名的ip地址

```
5.浏览器获得域名对应的ip地址后，发起http三次握手

```
浏览器通过随机端口向服务器web程序(如：ngx、80端口）发送tcp连接请求（这个连接请求通过层层的路由设备到达服务器端以后进入网卡，然后进入内核的tcp/ip协议站,还有可能经过防火墙的过滤）


三次握手：

第一次：第一次握手：建立连接时，客户端发送syn包（syn=j）到服务器，并进入SYN_SENT状态，等待服务器确认；SYN：同步序列编号（Synchronize Sequence Numbers）。

第二次：第二次握手：服务器收到syn包，必须确认客户的SYN（ack=j+1），同时自己也发送一个SYN包（syn=k），即SYN+ACK包，此时服务器进入SYN_RECV状态；

第三次：第三次握手：客户端收到服务器的SYN+ACK包，向服务器发送确认包ACK(ack=k+1），此包发送完毕，客户端和服务器进入ESTABLISHED（TCP连接成功）状态，完成三次握手。

完成三次握手，客户端与服务器开始传送数据，在上述过程中，还有一些重要的概念：

```
6.tcp/ip建立起来后，浏览器就可以向服务器发送HTTP请求了，比如用http的get方法请求一个根域里的一个域名，协议可以采纳http1.0的一个协议

7.服务器端接受到了这个请求，根据路径参数，经过后端的一些处理之后，把处理的一个结果的数据返回给浏览器

8.浏览器拿到了数据后渲染页面

#### http头和正文信息
> http头：发送的是一些附加的信息：内容类型、服务器发送响应的日期、http状态码

> 正文就是提交的数据

### 模块
> 支持更多特性

> 不缓冲请求和响应

> 处理流相关

#### 什么是回调？
> 异步编程的一种方法，需要按顺利执行异步逻辑的时候，一般采用后续传递的方式，
#### 什么是单线程？
> 程序只能按顺序执行，

#### 什么是多线程？
> 程序可以并发执行，只要合理分配硬盘资源

#### 什么是i/o?
> 磁盘的写入和读出，数据的进和出

#### 设么是非阻塞和阻塞？

#### 什么是事件和事件驱动？
> 为了某个事件注册了一个回调函数，而不是立即执行，而是基于事件的就是事件驱动。

### 作用域和上下文

#### 什么是作用域？
> 和访问变量的能力有关，内层的可以访问外层的，外层无法访问内层的

#### 什么是上下文？
> this的指向问题，可以通过bind、call、aplay改变，利用call和aplay可以很方便的的实现继承

```
function Pet（words){
    this.words=words;
    this.speak=function(){
        console.log(this.word)
    }
}

function Dog(words){
    Pet.call(this,words);
   // Pet.apply(this,arguments)
}

var dog=new Dog('wang');

dog.speak();

```

#### 性能测试
> 命令行输入 
```
 ab -n1000  -c10  http://localhost:2015/  

 -n1000 //请求次数
 -c10 //并发次数

```

#### http爬虫

> 所谓的爬虫就是利用http请求去拦截响应，而拿到网站的响应数据，或者通过一定处理，拿到自己想要的数据

```
var http=require('http');
var url='http://www.imooc.com/learn/348';
http.get(url,function(res){
    var html='';
    res.on('data',function(data){
        html+=data;
    })

    res.on('end',function(){
        console.log(html)
    })
}).on('error',function(){
    console.log('获取课程数据出错')
})

```

```
var http=require('http');
var cheerio=require('cheerio');//操作装在后端的html
var url='http://www.imooc.com/learn/348';


 function filterChapters(html){
   var $=cheerio.load(html);
   var chapters=$('div');
   var courseData=[];
   chapters.each(function(item){
     var chapter=$(this);
     var chapterTitle=chapter.find('strong').text();
     var videos=chapter.find('.voideo').children('li');
     var chapterData={
        chapterTitle:chapterTitle,
        videos:[]
     }
     videos.each(function(item){
        var video=$(this).find('.studyvideo');
        var videoTitle=video.text();
        var id=video.attr('href').split('video/')[1];
        chapterData.videos.push({
            title:videoTitle,
            id:id
        })
     })
     courseData.push(chapterData)
   })
   return courseData;
}


function printCourseInfo(courseData){

    courseData.forEach(function(item){
        var chapterTitle=item.chapterTitle;
        console.log(chapterTitle+'\n')

        item.videos.forEach(function(video){
            console.log('【'+video.id+'】'+video.title+'\n')
        })
    })
}


http.get(url,function(res){
    var html='';
    res.on('data',function(data){
        html+=data;
    })

    res.on('end',function(){
       var courseData=filterChapters(html);
       printCourseInfo(courseData);
    })

}).on('error',function(){
    console.log('获取课程数据出错')
})


```
### nodejs事件模块（events)

```
//引入模块
var EvenEmitter=require('events').EvenEmitter;

//实例化一个
var life= new EvenEmitter();

//定义函数
function water(who){
   console.log('给'+who+'倒水')
}

function water2(who){
   console.log('给'+who+'倒水')
}

//注册事件(可同时注册多个，最多10个)
life.on('求安慰',water)
life.on('求安慰',water2)

//触发事件
life.emit('求安慰','汉子')


//移除事件
life.removeListener('求安慰',water)

//移处所有事件
life.removeAllListener()

```
#### http模块（get/request)
> http.request(options,[callback])

```
var http=require('http');
var querystring=require('querystring');

var postData=querystring.stringify({
   content: '李松测试的',
   cid: 348,
   mid: 8837
})

var options={
    honstname:'www.imooc.com',
    port:80,
    path:'/course/docomment',
    method:'POST',
    headers:{
        'Accept':'application/json, text/javascript, */*; q=0.01',
        'Accept-Encoding': 'gzip, deflate, br',
        'Accept-Language': 'zh-CN,zh;q=0.9',
        'Connection': 'keep-alive',
        'Content-Length': postData.length,
        'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
        'Cookie': 'imooc_uuid=cd451376-f774-4847-8941-ea8462f23af7; imooc_isnew_ct=1523589671; imooc_isnew=2; loginstate=1; apsid=JlZWZkYzhlYjNjNjY0OGU3MzIzZTRiYTc1ZmQwM2MAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAANTQyMDcxMwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA5ODExNzQ1ODBAcXEuY29tAAAAAAAAAAAAAAAAAAAAADMzNGI4MTM3ZDU2YjMxZGM0MzdhYzc1MTc2NjllZTY2geRwW4HkcFs%3DNT; PHPSESSID=ptpir51qm36siihlmnqasu4dp3; cninfo=bdqdkj-5931f8e07e353edf2b94a098f44a062c; Hm_lvt_fb538fdd5bd62072b6a984ddbc658a16=1534731285,1535775579,1536022581,1536054823; Hm_lvt_f0cfcccd7b1393990c78efdeebff3968=1534731199,1534731285,1536022581,1536054823; Hm_lpvt_fb538fdd5bd62072b6a984ddbc658a16=1536116583; Hm_lpvt_f0cfcccd7b1393990c78efdeebff3968=1536291077; cvde=5b7a21019bfc7-179',
        'Host':' www.imooc.com',
        'Origin': 'https://www.imooc.com',
        'Referer': 'https://www.imooc.com/video/8837',
        'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36',
        'X-Requested-With': 'XMLHttpRequest'
    }
}

var req=http.request(options,function(res){
    console.log('Status:'+res.statusCode);
    console.log('header:'+JSON.stringity(res.headers))

    res.on('data',function(chunk){
        console.log(Buffer.isBuffer(chunk))
        console.log(typeof chunk)
    })

    res.on('end',function(){
        console.log('评论完毕')
    })
})

 req.on('error',function(e){
    console.log('Error:'+e.messsage)
 })
 req.write(postData);
 req.end()


 ```
 #### 高级教程 https://www.imooc.com/video/11554
