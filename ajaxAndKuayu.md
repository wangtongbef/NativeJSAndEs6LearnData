#ajax学习笔记
  
  ajax基础：

    Asynchronous JavaScript and XML，意思就是用JavaScript执行异步网络请求。
    如果仔细观察一个Form的提交，你就会发现，一旦用户点击“Submit”按钮，表单开始提交，浏览器就会刷新页面，然后在新页面里告诉你操作是成功了还是失败了。如果不幸由于网络太慢或者其他原因，就会得到一个404页面。一次HTTP请求对应一个页面。

    如果要让用户留在当前页面中，同时发出新的HTTP请求，就必须用JavaScript发送这个新请求，接收到数据后，再用JavaScript更新页面，这样一来，用户就感觉自己仍然停留在当前页面，但是数据却可以不断地更新。

    AJAX请求是异步执行的，也就是说，要通过回调函数获得响应。
    下面是基于原生js的ajax运用
    ex:
      ______________________________________________________________________________
      | function success(text) {
      |   var textarea = document.getElementById('test-response-text');
      |   textarea.value = text;
      | }
      |
      | function fail(code) {
      |   var textarea = document.getElementById('test-response-text');
      |   textarea.value = 'Error code: ' + code;
      | }
      |
      | var request;
      | if (window.XMLHttpRequest) {
      |   request = new XMLHttpRequest(); // 新建XMLHttpRequest对象
      | } else {    // 对于低版本的IE，需要换一个ActiveXObject对象：
      |   request = new ActiveXObject('Microsoft.XMLHTTP');//新建Microsoft.XMLHTTP对象
      | } //低版本兼容写法
      |
      | request.onreadystatechange = function () { // 状态发生变化时，函数被回调
      |   if (request.readyState === 4) { // 成功完成
      |     // 判断响应结果:
      |     if (request.status === 200) {
      |       // 成功，通过responseText拿到响应的文本:
      |       return success(request.responseText);
      |     } else {
      |       // 失败，根据响应码判断失败原因:
      |       return fail(request.status);
      |     }
      |   } else {
      |     // HTTP请求还在继续...
      |   }
      | }
      |
      | // 发送请求:
      | request.open('GET', '/api/categories');
      | request.send();
      |
      | alert('请求已发送，请等待响应...');
      |_______________________________________________________________________________

    现代浏览器上的AJAX主要功能实现基于XMLHttpRequest对象

      XMLHttpRequest对象：
        
        可参考资料：XMLHttpRequest 对象(未收录)   http://www.w3school.com.cn/xmldom/dom_httprequest.asp
                  XML DOM - XMLHttpRequest 对象(已收录)  http://www.w3school.com.cn/xmldom/dom_http.asp
                  AJAX(收录部分)  https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001434499861493e7c35be5e0864769a2c06afb4754acc6000

        属性：

          readyState：
          HTTP 请求的状态.当一个 XMLHttpRequest 初次创建时，这个属性的值从 0 开始，直到接收到完整的 HTTP 响应，这个值增加到 4。
          5 个状态中每一个都有一个相关联的非正式的名称，下表列出了状态、名称和含义：
          _________________________________________________________________________________________________________________________
          |状态   |名称              | 描述                                                                                          |
          |_______|_________________|_______________________________________________________________________________________________|
          |0	  |Uninitialized	| 初始化状态。XMLHttpRequest 对象已创建或已被 abort() 方法重置。                                   |
          |_______|_________________|_______________________________________________________________________________________________|
          |1	    |Open	            |open() 方法已调用，但是 send() 方法未调用。请求还没有被发送。                                      |
          |_______|_________________|_______________________________________________________________________________________________|
          |2	    |Sent	            |Send() 方法已调用，HTTP 请求已发送到 Web 服务器。未接收到响应。                                    |
          |_______|_________________|_______________________________________________________________________________________________|
          |3	    |Receiving	      |所有响应头部都已经接收到。响应体开始接收但未完成。                                                  |
          |_______|_________________|_______________________________________________________________________________________________|
          |4	    |Loaded	          |HTTP 响应已经完全接收。                                                                          |
          |_______|_________________|_______________________________________________________________________________________________|
          readyState 的值不会递减，除非当一个请求在处理过程中的时候调用了 abort() 或 open() 方法。每次这个属性的值增加的时候，都会触发 onreadystatechange 事件句柄。

          responseText:
          目前为止为服务器接收到的响应体（不包括头部），或者如果还没有接收到数据的话，就是空字符串。
          如果 readyState 小于 3，这个属性就是一个空字符串。当 readyState 为 3，这个属性返回目前已经接收的响应部分。如果 readyState 为 4，这个属性保存了完整的响应体。

          responseXML:
          对请求的响应，解析为 XML 并作为 Document 对象返回。

          status:
          由服务器返回的 HTTP 状态代码，如 200 表示成功，而 404 表示 "Not Found" 错误。当 readyState 小于 3 的时候读取这一属性会导致一个异常。

          statusText:
          这个属性用名称而不是数字指定了请求的 HTTP 的状态代码。也就是说，当状态为 200 的时候它是 "OK"，当状态为 404 的时候它是 "Not Found"。和 status 属性一样，当 readyState 小于 3 的时候读取这一属性会导致一个异常。

        事件句柄:

          onreadystatechange:
          每次 readyState 属性改变的时候调用的事件句柄函数。当 readyState 为 3 时，它也可能调用多次。

        方法:

          abort():
          取消当前响应，关闭连接并且结束任何未决的网络活动。
          这个方法把 XMLHttpRequest 对象重置为 readyState 为 0 的状态，并且取消所有未决的网络活动。例如，如果请求用了太长时间，而且响应不再必要的时候，可以调用这个方法。

          getAllResponseHeaders():
          把 HTTP 响应头部作为未解析的字符串返回。
          如果 readyState 小于 3，这个方法返回 null。否则，它返回服务器发送的所有 HTTP 响应的头部。头部作为单个的字符串返回，一行一个头部。每行用换行符 "\r\n" 隔开。

          getResponseHeader():
          返回指定的 HTTP 响应头部的值。其参数是要返回的 HTTP 响应头部的名称。可以使用任何大小写来制定这个头部名字，和响应头部的比较是不区分大小写的。
          该方法的返回值是指定的 HTTP 响应头部的值，如果没有接收到这个头部或者 readyState 小于 3 则为空字符串。如果接收到多个有指定名称的头部，这个头部的值被连接起来并返回，使用逗号和空格分隔开各个头部的值。

          open():
          初始化 HTTP 请求参数，例如 URL 和 HTTP 方法，但是并不发送请求。

          send():
          发送 HTTP 请求，使用传递给 open() 方法的参数，以及传递给该方法的可选请求体。

          setRequestHeader():
          向一个打开但未发送的请求设置或添加一个 HTTP 请求。

        方法解析：
          XMLHttpRequest.open()
          仅仅是初始化 HTTP 请求参数
          语法
            open(method, url, async, username, password)
            
            method 参数是用于请求的 HTTP 方法。值包括 GET、POST 和 HEAD。
            url 参数是请求的主体。大多数浏览器实施了一个同源安全策略，并且要求这个 URL 与包含脚本的文本具有相同的主机名和端口。
            async 参数指示请求使用应该异步地执行。如果这个参数是 false，请求是同步的，后续对 send() 的调用将阻塞，直到响应完全接收。如果这个参数是 true 或省略，请求是异步的，且通常需要一个 onreadystatechange 事件句柄。
            username 和 password 参数是可选的，为 url 所需的授权提供认证资格。如果指定了，它们会覆盖 url 自己指定的任何资格。
          说明
            这个方法初始化请求参数以供 send() 方法稍后使用。它把 readyState 设置为 1，删除之前指定的所有请求头部，以及之前接收的所有响应头部，并且把 responseText、responseXML、status 以及 statusText 参数设置为它们的默认值。当 readyState 为 0 的时候（当 XMLHttpRequest 对象刚创建或者 abort() 方法调用后）以及当 readyState 为 4 时（已经接收响应时），调用这个方法是安全的。当针对任何其他状态调用的时候，open() 方法的行为是为指定的。
            除了保存供 send() 方法使用的请求参数，以及重置 XMLHttpRequest 对象以便复用，open() 方法没有其他的行为。要特别注意，当这个方法调用的时候，实现通常不会打开一个到 Web 服务器的网络连接。
            
          XMLHttpRequest.setRequestHeader()
          语法
            setRequestHeader(name, value)
            name 参数是要设置的头部的名称。这个参数不应该包括空白、冒号或换行。
            value 参数是头部的值。这个参数不应该包括换行。
          说明
            setRequestHeader() 方法指定了一个 HTTP 请求的头部，它应该包含在通过后续 send() 调用而发布的请求中。这个方法只有当 readyState 为 1 的时候才能调用，例如，在调用了 open() 之后，但在调用 send() 之前。
            如果带有指定名称的头部已经被指定了，这个头部的新值就是：之前指定的值，加上逗号、空白以及这个调用指定的值。
            如果 open() 调用指定了认证资格，XMLHttpRequest 自动发送一个适当的 Authorization 请求头部。但是，你可以使用 setRequestHeader() 来添加这个头部。类似地，如果 Web 服务器已经保存了和传递给 open() 的 URL 相关联的 cookie，适当的 Cookie 或 Cookie2 头部也自动地包含到请求中。可以通过调用 setRequestHeader() 来把这些 cookie 添加到头部。XMLHttpRequest 也可以为 User-Agent 头部提供一个默认值。如果它这么做，你为该头部指定的任何值都会添加到这个默认值后面。
            有些请求头部由 XMLHttpRequest 自动设置而不是由这个方法设置，以符合 HTTP 协议。这包括如下和代理相关的头部：
              ·Host
              ·Connection
              ·Keep-Alive
              ·Accept-charset
              ·Accept-Encoding
              ·If-Modified-Since
              ·If-None-Match
              ·If-Range
              ·Range

          XMLHttpRequest.send()
          发送一个 HTTP 请求
          语法
            send(body)

            如果通过调用 open() 指定的 HTTP 方法是 POST 或 PUT，body 参数指定了请求体，作为一个字符串或者 Document 对象。如果请求体不适必须的话，这个参数就为 null。对于任何其他方法，这个参数是不可用的，应该为 null（有些实现不允许省略该参数）。
          说明
            这个方法导致一个 HTTP 请求发送。如果之前没有调用 open()，或者更具体地说，如果 readyState 不是 1，send() 抛出一个异常。否则，它发送一个 HTTP 请求，该请求由以下几部分组成：
              ·之前调用 open() 时指定的 HTTP 方法、URL 以及认证资格（如果有的话）。
              ·之前调用 setRequestHeader() 时指定的请求头部（如果有的话）。
              ·传递给这个方法的 body 参数。
            一旦请求发布了，send() 把 readyState 设置为 2，并触发 onreadystatechange 事件句柄。
            如果之前调用的 open() 参数 async 为 false，这个方法会阻塞并不会返回，直到 readyState 为 4 并且服务器的响应被完全接收。否则，如果 async 参数为 true，或者这个参数省略了，send() 立即返回，并且正如后面所介绍的，服务器响应将在一个后台线程中处理。
            如果服务器响应带有一个 HTTP 重定向，send() 方法或后台线程自动遵从重定向。当所有的 HTTP 响应头部已经接收，send() 或后台线程把 readyState 设置为 3 并触发 onreadystatechange 事件句柄。如果响应较长，send() 或后台线程可能在状态 3 中触发 onreadystatechange 事件句柄：这可以作为一个下载进度指示器。最后，当响应完成，send() 或后台线程把 readyState 设置为 4，并最后一次触发事件句柄。


  跨域实现：
    
    可参考资料：AJAX(收录部分)  https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001434499861493e7c35be5e0864769a2c06afb4754acc6000
    
    安全限制：
      浏览器的同源策略，指的是浏览器不能执行其他网站的脚本。默认情况下，JavaScript在发送AJAX请求时，URL的域名必须和当前页面完全一致。
      完全一致的意思是，域名要相同（www.example.com和example.com不同），协议要相同（http和https不同），端口号要相同（默认是:80端口，它和:8080就不同）。有的浏览器口子松一点，允许端口不同，大多数浏览器都会严格遵守这个限制。

    跨域解决方案：（4种）
      1、通过Flash插件发送HTTP请求，这种方式可以绕过浏览器的安全限制，但必须安装Flash，并且跟Flash交互。不过Flash用起来麻烦，而且现在用得也越来越少了。
        参考资料：flash跨域访问问题：https://blog.csdn.net/vince6799/article/details/4586312 （此种方式基本已被淘汰，资料都比较老，仅作了解，可自行搜集）
                Flash跨域的完全解决方案：https://blog.csdn.net/lcg_ryan/article/details/42007443

      2、通过在同源域名下架设一个代理服务器来转发，JavaScript负责把请求发送到代理服务器：
         ___________________________________________________________________________
        | '/proxy?url=http://www.sina.com.cn'
        |___________________________________________________________________________
        代理服务器再把结果返回，这样就遵守了浏览器的同源策略。这种方式麻烦之处在于需要服务器端额外做开发。

      3、第三种方式称为JSONP，它有个限制，只能用GET请求，并且要求返回JavaScript。这种方式跨域实际上是利用了浏览器允许跨域引用JavaScript资源：
         _______________________________________________________________________________________
        | <html>
        | <head>
        |     <script src="http://example.com/abc.js"></script>  (举例：引用jQuery)
        |     ...
        | </head>
        | <body>
        | ...
        | </body>
        | </html>
        |_______________________________________________________________________________________
        JSONP通常以函数调用的形式返回，例如，返回JavaScript内容如下：
         _______________________________________________________________________________________
        | foo('data');
        |_______________________________________________________________________________________
        这样一来，我们如果在页面中先准备好foo()函数，然后给页面动态加一个<script>节点，相当于动态读取外域的JavaScript资源，最后就等着接收回调了。

        举个栗子：
          以163的股票查询URL为例:
           _________________________________________________________________________________________________
          | <p id="test-jsonp">当前价格：</p>
          |_________________________________________________________________________________________________
          对于URL：http://api.money.126.net/data/feed/0000001,1399001?callback=refreshPrice，你将得到如下返回：
           _________________________________________________________________________________________________
          | refreshPrice({"0000001":{"code": "0000001", ... });   //注意上面的参数 '?callback=refreshPrice'
          |_________________________________________________________________________________________________
          因此我们需要首先在页面中准备好回调函数：
           _________________________________________________________________________________________________
          | function refreshPrice(data) {
          |   var p = document.getElementById('test-jsonp');
          |   p.innerHTML = '当前价格：' +
          |     data['0000001'].name +': ' + 
          |     data['0000001'].price + '；' +
          |     data['1399001'].name + ': ' +
          |     data['1399001'].price;
          | }
          |_________________________________________________________________________________________________
          最后用getPrice()函数触发：
           _________________________________________________________________________________________________
          | function getPrice() {
          |   var
          |     js = document.createElement('script'),
          |     head = document.getElementsByTagName('head')[0];
          |   js.src = 'http://api.money.126.net/data/feed/0000001,1399001?callback=refreshPrice';
          |   head.appendChild(js);
          | }
          |_________________________________________________________________________________________________
          就完成了跨域加载数据。

      4、CORS

        参考资料：跨域资源共享 CORS 详解：http://www.ruanyifeng.com/blog/2016/04/cors（非常详细的CORS资料，可以着重参考）
                HTTP请求行、请求头、请求体、响应行、响应头、响应体详解：https://blog.csdn.net/u010256388/article/details/68491509

        CORS全称Cross-Origin Resource Sharing，是HTML5规范定义的如何跨域访问资源。（需要浏览器支持HTML5）
        了解CORS前，我们先搞明白概念：
        Origin表示本域，也就是浏览器当前页面的域。

          简单请求：
          当JavaScript向外域（如sina.com）发起请求后，浏览器收到响应后，首先检查 Access-Control-Allow-Origin 是否包含本域，如果是，则此次跨域请求成功，如果不是，则请求失败，JavaScript将无法获取到响应的任何数据。
          用一个图来表示就是：
              ./img/kuayuCORS.jpg
          假设本域是my.com，外域是sina.com，只要响应头Access-Control-Allow-Origin为 http://my.com，或者是*，本次请求就可以成功。
          可见，跨域能否成功，取决于对方服务器是否愿意给你设置一个正确的Access-Control-Allow-Origin，决定权始终在对方手中。
          上面这种跨域请求，称之为“简单请求”。
          简单请求包括GET、HEAD和POST（POST的Content-Type类型仅限application/x-www-form-urlencoded、multipart/form-data和text/plain），并且不能出现任何自定义头（例如，X-Custom: 12345），通常能满足90%的需求。

          简单请求需要同时满足以下两大条件：{
            (1) 请求方法是以下三种方法之一：
              HEAD
              GET
              POST
            (2)HTTP的头信息不超出以下几种字段：
              Accept
              Accept-Language
              Content-Language
              Last-Event-ID
              Content-Type：只限于三个值application/x-www-form-urlencoded、multipart/form-data、text/plain
          }
          凡是不同时满足上面两个条件，就属于非简单请求。

        无论你是否需要用JavaScript通过CORS跨域请求资源，你都要了解CORS的原理。最新的浏览器全面支持HTML5。在引用外域资源时，除了JavaScript和CSS外，都要验证CORS。例如，当你引用了某个第三方CDN上的字体文件时：
         _________________________________________________________________________________________________
        | /* CSS */
        | @font-face {
        |   font-family: 'FontAwesome';
        |   src: url('http://cdn.com/fonts/fontawesome.ttf') format('truetype');
        | }
        |_________________________________________________________________________________________________
        如果该CDN服务商未正确设置Access-Control-Allow-Origin，那么浏览器无法加载字体资源。

          非简单请求：
          对于PUT、DELETE以及其他类型如application/json的POST请求，在发送AJAX请求之前，浏览器会先发送一个OPTIONS请求（称为preflighted请求）到这个URL上，询问目标服务器是否接受：
           _________________________________________________________________________________________________
          | OPTIONS /path/to/resource HTTP/1.1
          | Host: bar.com
          | Origin: http://my.com
          | Access-Control-Request-Method: POST
          |_________________________________________________________________________________________________
          服务器必须响应并明确指出允许的Method：
           _________________________________________________________________________________________________
          | HTTP/1.1 200 OK
          | Access-Control-Allow-Origin: http://my.com
          | Access-Control-Allow-Methods: POST, GET, PUT, OPTIONS
          | Access-Control-Max-Age: 86400
          |_________________________________________________________________________________________________
            上述字段的一些解释：
              （1）Access-Control-Allow-Methods
              该字段必需，它的值是逗号分隔的一个字符串，表明服务器支持的所有跨域请求的方法。注意，返回的是所有支持的方法，而不单是浏览器请求的那个方法。这是为了避免多次"预检"请求。
              （2）Access-Control-Allow-Headers
              如果浏览器请求包括Access-Control-Request-Headers字段，则Access-Control-Allow-Headers字段是必需的。它也是一个逗号分隔的字符串，表明服务器支持的所有头信息字段，不限于浏览器在"预检"中请求的字段。
              （3）Access-Control-Allow-Credentials
              该字段与简单请求时的含义相同。
              （4）Access-Control-Max-Age
              该字段可选，用来指定本次预检请求的有效期，单位为秒。上面结果中，有效期是20天（1728000秒），即允许缓存该条回应1728000秒（即20天），在此期间，不用发出另一条预检请求。
          
          浏览器确认服务器响应的Access-Control-Allow-Methods头确实包含将要发送的AJAX请求的Method，才会继续发送AJAX，否则，抛出一个错误。
          由于以POST、PUT方式传送JSON格式的数据在REST中很常见，所以要跨域正确处理POST和PUT请求，服务器端必须正确响应OPTIONS请求。

          一旦服务器通过了"预检"请求，以后每次浏览器正常的CORS请求，就都跟简单请求一样，会有一个Origin头信息字段。服务器的回应，也都会有一个Access-Control-Allow-Origin头信息字段。

      
