# -
前端的本地缓存技术—— cookie  localStorage    sessionStorage  ApplicationCache
https://www.cnblogs.com/belove8013/p/8134067.html

正文



1、Cookie
JavaScript是运行在客户端的脚本，因此一般是不能够设置Session的，因为Session是运行在服务器端的。而cookie是运行在客户端的，所以可以用JS来设置cookie。

cookie的结构：简单地说，cookie是以键值对的形式保存的，即key=value的格式。各个cookie之间一般是以“;”分隔。

cookie是浏览器提供的一种机制，它将document 对象的cookie属性提供给JavaScript。可以由JavaScript对其进行控制，而并不是JavaScript本身的性质。cookie是存于用户硬盘的一个文件，这个文件通常对应于一个域名，当浏览器再次访问这个域名时，便使这个cookie可用。因此，cookie可以跨越一个域名下的多个网页，但不能跨越多个域名使用。 

cookie机制将信息存储于用户硬盘，因此可以作为全局变量，这是它最大的一个优点。它可以用于以下几种场合。 

（1）保存用户登录状态。（2）跟踪用户行为。（3）定制页面。（4）创建购物车，例如淘宝网就使用cookie记录了用户曾经浏览过的商品，方便随时进行比较。 

cookie能完成的部分应用，还有更多的功能需要全局变量。cookie的缺点主要集中于安全性和隐私保护。主要包括以下几种： 

（1）cookie可能被禁用。当用户非常注重个人隐私保护时，他很可能禁用浏览器的cookie功能； 
（2）cookie是与浏览器相关的。这意味着即使访问的是同一个页面，不同浏览器之间所保存的cookie也是不能互相访问的； 
（3）cookie可能被删除。因为每个cookie都是硬盘上的一个文件，因此很有可能被用户删除； 
（4）cookie安全性不够高。所有的cookie都是以纯文本的形式记录于文件中，因此如果要保存用户名密码等信息时，最好事先经过加密处理。 

 

JS设置cookie:

 

假设在A页面中要保存变量username的值("jack")到cookie中,key值为name，则相应的JS代码为：

document.cookie = ‘name = ’+ username;

 

复制代码
JS读取cookie:

假设cookie中存储的内容为：name=jack;password=123

则在B页面中获取变量username的值的JS代码如下：

var username=document.cookie.split(";")[0].split("=")[1];

//JS操作cookies方法!

//写cookies

function setCookie(name,value)

{

var Days = 30;

var exp = new Date();

exp.setTime(exp.getTime() + Days*24*60*60*1000);

document.cookie = name + "="+ escape (value) + ";expires=" + exp.toGMTString();

}

读取cookies

function getCookie(name)

{

var arr,reg=new RegExp("(^| )"+name+"=([^;]*)(;|$)");

if(arr=document.cookie.match(reg))

return unescape(arr[2]);

else

return null;

}

 

删除cookies

function delCookie(name)

{

var exp = new Date();

exp.setTime(exp.getTime() - 1);

var cval=getCookie(name);

if(cval!=null)

document.cookie= name + "="+cval+";expires="+exp.toGMTString();

}
复制代码
 

完整代码

复制代码
//如果需要设定自定义过期时间,那么把上面的setCookie　函数换成下面两个函数就ok;

function setCookie(name, value, time) {

    var strsec = getsec(time);

    var exp = new Date();

    exp.setTime(exp.getTime() + strsec * 1);

    document.cookie = name + "=" + escape(value) + ";expires=" + exp.toGMTString();

}

 

function getCookie(name) {

    var arr, reg = new RegExp("(^| )" + name + "=([^;]*)(;|$)");

    if (arr = document.cookie.match(reg))

        return unescape(arr[2]);

    else

        return null;

}

 

function delCookie(name) {

    var exp = new Date();

    exp.setTime(exp.getTime() - 1);

    var cval = getCookie(name);

    if (cval != null)

        document.cookie = name + "=" + cval + ";expires=" + exp.toGMTString();

}

 

function getsec(str) {

    alert(str);

    var str1 = str.substring(1, str.length) * 1;

    var str2 = str.substring(0, 1);

    if (str2 == "s") {

        return str1 * 1000;

    } else if (str2 == "h") {

        return str1 * 60 * 60 * 1000;

    } else if (str2 == "d") {

        return str1 * 24 * 60 * 60 * 1000;

    }

}

setCookie("name", "hayden");

alert(getCookie("name"));

//这是有设定过期时间的使用示例：

//s20是代表20秒

//h是指小时，如12小时则是：h12

//d是天数，30天则：d30

setCookie("name", "hayden", "s20");
复制代码
2、localStorage
一、什么是localstorage？

在HTML5中，新加入了一个localStorage特性，这个特性主要是用来作为本地存储来使用的，解决了cookie存储空间不足的问题(cookie中每条cookie的存储空间为4k)，localStorage中一般浏览器支持的是5M大小，这个在不同的浏览器中localStorage会有所不同。

二、localstorage的优势与局限

localStorage的优势

1、 localStorage拓展了cookie的4K限制

2、 localStorage会可以将第一次请求的数据直接存储到本地，这个相当于一个5M大小的针对于前端页面的数据库，相比于cookie可以节约带宽，但是这个却是只有在高版本的浏览器中才支持的

localStorage的局限

1、 浏览器的大小不统一，并且在IE8以上的IE版本才支持localStorage这个属性

2、 目前所有的浏览器中都会把localStorage的值类型限定为string类型，这个在对我们日常比较常见的JSON对象类型需要一些转换

3、 localStorage在浏览器的隐私模式下面是不可读取的

4、 localStorage本质上是对字符串的读取，如果存储内容多的话会消耗内存空间，会导致页面变卡

5、 localStorage不能被爬虫抓取到

localStorage与sessionStorage的唯一一点区别就是localStorage属于永久性存储，而sessionStorage属于当会话结束的时候，sessionStorage中的键值对会被清空

 

三、localstorage的使用

这里要特别声明一下，如果是使用IE浏览器的话，那么就要UserData来作为存储，这里主要讲解的是localStorage的内容，所以userData不做过多的解释，而且以博主个人的看法，也是没有必要去学习UserData的使用来的，因为目前的IE6/IE7属于淘汰的位置上，而且在如今的很多页面开发都会涉及到HTML5\CSS3等新兴的技术，所以在使用上面一般我们不会去对其进行兼容

首先在使用localStorage的时候，我们需要判断浏览器是否支持localStorage这个属性

复制代码
if(！window.localStorage){

      alert("浏览器支持localstorage");

      return false;

}else{

     //主逻辑业务

}

localStorage的写入，localStorage的写入有三种方法

if(！window.localStorage){

            alert("浏览器支持localstorage");

            return false;

        }else{

            var storage=window.localStorage;

            //写入a字段

            storage["a"]=1;

            //写入b字段

            storage.a=1;

            //写入c字段

            storage.setItem("c",3);

            console.log(typeof storage["a"]);

            console.log(typeof storage["b"]);

            console.log(typeof storage["c"]);

 }
复制代码
 

这里要特别说明一下localStorage的使用也是遵循同源策略的，所以不同的网站直接是不能共用相同的localStorage

注意：存储进去的是int类型，但是打印出来却是string类型，这个与localStorage本身的特点有关，localStorage只支持string类型的存储。

 

localStorage的读取

 

复制代码
if(!window.localStorage){

            alert("浏览器支持localstorage");

        }else{

            var storage=window.localStorage;

            //写入a字段

            storage["a"]=1;

            //写入b字段

            storage.a=1;

            //写入c字段

            storage.setItem("c",3);

            console.log(typeof storage["a"]);

            console.log(typeof storage["b"]);

            console.log(typeof storage["c"]);

            //第一种方法读取

            var a=storage.a;

            console.log(a);

            //第二种方法读取

            var b=storage["b"];

            console.log(b);

            //第三种方法读取

            var c=storage.getItem("c");

            console.log(c);

        }
复制代码
 

这里面是三种对localStorage的读取，其中官方推荐的是getItem\setItem这两种方法对其进行存取，不要问我这个为什么，因为这个我也不知道

我之前说过localStorage就是相当于一个前端的数据库的东西，数据库主要是增删查改这四个步骤，这里的读取和写入就相当于增、查的这两个步骤

下面我们就来说一说localStorage的删、改这两个步骤

改这个步骤比较好理解，思路跟重新更改全局变量的值一样，这里我们就以一个为例来简单的说明一下

复制代码
if(!window.localStorage){

            alert("浏览器支持localstorage");

        }else{

            var storage=window.localStorage;

            //写入a字段

            storage["a"]=1;

            //写入b字段

            storage.b=1;

            //写入c字段

            storage.setItem("c",3);

            console.log(storage.a);

            // console.log(typeof storage["a"]);

            // console.log(typeof storage["b"]);

            // console.log(typeof storage["c"]);

            /*分割线*/

            storage.a=4;

            console.log(storage.a);

        }
复制代码
 

这个在控制台上面我们就可以看到已经a键已经被更改为4了

localStorage的删除

复制代码
1、 将localStorage的所有内容清除

var storage=window.localStorage;

            storage.a=1;

            storage.setItem("c",3);

            console.log(storage);

            storage.clear();

            console.log(storage);

2、 将localStorage中的某个键值对删除

var storage=window.localStorage;

            storage.a=1;

            storage.setItem("c",3);

            console.log(storage);

            storage.removeItem("a");

            console.log(storage.a);
复制代码
 

 

localStorage的键获取

复制代码
var storage=window.localStorage;

            storage.a=1;

            storage.setItem("c",3);

            for(var i=0;i<storage.length;i++){

                var key=storage.key(i);

                console.log(key);

            }
复制代码
 

使用key()方法，向其中出入索引即可获取对应的键。

四、localstorage的其他注意事项

一般我们会将JSON存入localStorage中，但是在localStorage会自动将localStorage转换成为字符串形式

这个时候我们可以使用JSON.stringify()这个方法，来将JSON转换成为JSON字符串

示例：

复制代码
if(!window.localStorage){

            alert("浏览器支持localstorage");

        }else{

            var storage=window.localStorage;

            var data={

                name:'xiecanyong',

                sex:'man',

                hobby:'program'

            };

            var d=JSON.stringify(data);

            storage.setItem("data",d);

            console.log(storage.data);

        }
复制代码
 

读取之后要将JSON字符串转换成为JSON对象，使用JSON.parse()方法

复制代码
var storage=window.localStorage;

            var data={

                name:'xiecanyong',

                sex:'man',

                hobby:'program'

            };

            var d=JSON.stringify(data);

            storage.setItem("data",d);

            //将JSON字符串转换成为JSON对象输出

            var json=storage.getItem("data");

            var jsonObj=JSON.parse(json);

            console.log(typeof jsonObj);
复制代码
3、sessionStorage
sessionStorage用于本地存储一个会话（session）中的数据，这些数据只有在同一个会话中的页面才能访问并且当会话结束后数据也随之销毁。因此sessionStorage不是一种持久化的本地存储，仅仅是会话级别的存储。下面是其用法:

 

复制代码
<!DOCTYPE HTML>

<html>

<head>

    <meta charset="utf-8">

    <title>SessionStorage</title>

    <script type="text/javascript">

    window.onload = function() {

        //首先获得body中的3个input元素

        var msg = document.getElementById("msg"); //文本框要输入的内容

        var getData = document.getElementById("getData"); //获取数据

        var setData = document.getElementById("setData"); //保存数据

        var removeData = document.getElementById("removeData"); //移除数据

        setData.onclick = function() //存入数据

            {

                if (msg.value) {

                    sessionStorage.setItem("data", msg.value); //把data对应的值保存到sessionStorage

                    alert("信息已保存到data字段中");

                } else {

                    alert("信息不能为空");

                }

            }

        getData.onclick = function() {

            //获取数据

            var msg = sessionStorage.getItem("data");

            if (msg) {

                alert("data字段中的值为：" + msg); //把data对应的值弹出来

            } else {

                alert("data字段无值！");

            }

        }

        removeData.onclick = function() //移除数据

            {

                var msg = sessionStorage.getItem("data");

                sessionStorage.removeChild(msg);

            }

    }

    </script>

</head>

 

<body>

    <input id="msg" type="text" />

    <input id="setData" type="button" value="保存数据" />

    <input id="getData" type="button" value="获取数据" />

    <input id="removeData" type="button" value="移除数据" />

</body>

</html>
复制代码
另外还有一点要注意的是，其他类型读取出来也要进行转换。

 

4、ApplicationCache
 

applicationCache是html5新增的一个离线应用功能

•离线浏览: 用户可以在离线状态下浏览网站内容。
•更快的速度: 因为数据被存储在本地，所以速度会更快.
•减轻服务器的负载: 浏览器只会下载在服务器上发生改变的资源。
在对应用进行缓存的时候需要一个manifest文件，

 

cache manifest 格式如下
首行必须是CACHE MANIFEST
接下来可以分为三段： CACHE， NETWORK，与 FALLBACK
CACHE:这是缓存文件中记录所属的默认段落。在 CACHE: 段落标题后(或直接跟在 CACHE MANIFEST 行后)列出的文件会在它们第一次下载完毕后缓存起来。NETWORK:在 NETWORK: 段落标题下列出的文件是需要与服务器连接的白名单资源。所有类似资源的请求都会绕过缓存，即使用户处于离线状态。可以使用通配符。FALLBACK:FALLBACK: 段指定了一个后备页面，当资源无法访问时，浏览器会使用该页面。该段落的每条记录都列出两个 URI—第一个表示资源，第二个表示后备页面。两个 URI 都必须使用相对路径并且与清单文件同源。可以使用通配符。
CACHE， NETWORK， 和 FALLBACK 段落可以以任意顺序出现在缓存清单文件中，并且每个段落可以在同一清单文件中出现多次。

 

applicationcache/

 

├── static/

 

│   ├── css/

 

│   │ └── example.css

 

│   └── js/

 

│     └── example.js

 

└── index.jsp

 

 

 

#代码如下

 

#example.css

 

#clock { font: 2em sans-serif; }

 

 

 

#example.js

 

window.onload = function(){

 

        　　window.onload = function(){

 

        　　　　setInterval(function(){

 

                　　　　　　document.getElementById('clock').innerHTML = new Date();

 

        　　　　},1000);

 

　　};

 

};

 

 

 

#index.jsp

 

<link type="text/css" href="static/css/example.css" rel="stylesheet"/>

 

<script src="static/js/example.js"></script>

 

<div id="clock"></div>

 

我们加html5新增的神器manifest文件

 

文件添加在static文件夹下，文件内容如下

 

#example.manifest

 

CACHE MANIFEST

 

./js/example.js

 

../index.jsp

 

NETWORK:

 

./css/exmaple.css

 

这时需要修改index.jsp文件内容

 

<html> --> <html manifest="static/example.manifest">

 

文件说明对js和jsp进行缓存，而css文件则列入白名单不缓存，再次启动服务。。。。

访问的时候你看到的页面是没任何变化的，跟第一次访问的页面是一个鸟样的，不过你按下F12就会看到浏览器已经对的的文件进行缓存了

 

在resources->application cache选项下可以看到浏览器缓存的是什么东东，也可以使用 chrome://appcache-internals/ 命令在Chrome浏览器中进行查看删除。

 

好了**奇迹的时刻到了，我再次把服务停了。。。。。。再次访问，竟然可以访问到，不会像第一次那样提示挂了。

 

但是样式没了。。。那就是我们刚刚把样式置为白名单了，没有缓存下来

如果你的manifest文件修改了，你可以手动或自动更新它
1.自动更新 
浏览器除了在第一次访问 Web 应用时缓存资源外，只会在 cache manifest 文件本身发生变化时更新缓存。而 cache manifest 中的资源文件发生变化并不会触发更新。

2.手动更新 
开发者也可以使用 window.applicationCache 的接口更新缓存。方法是检测 window.applicationCache.status 的值，如果是 UPDATEREADY，那么可以调用 window.applicationCache.update() 更新缓存。示范代码如下。

 

if (window.applicationCache.status == window.applicationCache.UPDATEREADY) {

 

　　window.applicationCache.update();

 

}

 

applicationCache状态值

 

UNCACHED(未缓存)　　一个特殊的值，用于表明一个应用缓存对象还没有完全初始化。

 

IDLE(空闲)　　应用缓存此时未处于更新过程中。

 

CHECKING(检查)　　清单已经获取完毕并检查更新。

 

DOWNLOADING(下载中)　　下载资源并准备加入到缓存中，这是由于清单变化引起的。

 

UPDATEREADY(更新就绪)　　一个新版本的应用缓存可以使用。有一个对应的事件 updateready，当下载完毕一个更新，并且还未使用 swapCache() 方法激活更新时，该事件触发，而不会是 cached 事件。

 

OBSOLETE(废弃)　　应用缓存现在被废弃。

 

也可以在线状态检测

1.navigator.onLine 
navigator.onLine 属性表示当前是否在线。如果为 true, 表示在线；如果为 false, 表示离线。当网络状态发生变化时，navigator.onLine 的值也随之变化。开发者可以通过读取它的值获取网络状态。

2.online/offline 事件 
当开发离线应用时，通过 navigator.onLine 获取网络状态通常是不够的。开发者还需要在网络状态发生变化时立刻得到通知，因此 HTML5 还提供了 online/offline 事件。当在线 / 离线状态切换时，online/offline 事件将触发在 body 元素上，并且沿着 document.body、document 和 window 的顺序冒泡。因此，开发者可以通过监听它们的 online/offline 事件来获悉网络状态。
