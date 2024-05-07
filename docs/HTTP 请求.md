### 1. 请求状态码
- 1XX：消息状态码。
  - 100：Continue 继续。客户端应继续其请求。
  - 101：Switching Protocols 切换协议。服务器根据客户端的请求切换协议。只能切换到更高级的协议，例如，切换到 HTTP 的新版本协议。
- 2XX：成功状态码。
  - 200：OK 请求成功。一般用于 GET 与 POST 请求。
  - 201：Created 已创建。成功请求并创建了新的资源。
  - 202：Accepted 已接受。已经接受请求，但未处理完成。
  - 203：Non-Authoritative Information    非授权信息。请求成功。但返回的 meta 信息不在原始的服务器，而是一个副本。
  - 204：No Content 无内容。服务器成功处理，但未返回内容。在未更新网页的情况下，可确保浏览器继续显示当前文档。
  - 205：Reset Content    重置内容。服务器处理成功，用户终端（例如：浏览器）应重置文档视图。可通过此返回码清除浏览器的表单域。
  - 206：Partial Content     部分内容。服务器成功处理了部分 GET 请求。
- 3XX：重定向状态码。
  - 301：Moved Permanently 永久移动。请求的资源已被永久的移动到新 URI，返回信息会包括新的 URI，浏览器会自动定向到新 URI。今后任何新的请求都应使用新的 URI 代替。
  - 302：Found 临时移动，与 301 类似。但资源只是临时被移动。客户端应继续使用原有URI。
  - 304：Not Modified    未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源。客户端通常会缓存访问过的资源，通过提供一个头信息指出客户端希望只返回在指定日期之后修改的资源。
  - 307：Temporary Redirect 临时重定向。与 302 类似。使用 GET 请求重定向。
- 4XX：客户端错误状态码。
  - 400：Bad Request 客户端请求的语法错误，服务器无法理解。
  - 401：Unauthorized 请求要求用户的身份认证。
  - 403：Forbidden    服务器理解请求客户端的请求，但是拒绝执行此请求。
  - 404：Not Found 服务器无法根据客户端的请求找到资源（网页）。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面。
  - 405：Method Not Allowed 客户端请求中的方法被禁止。
- 5XX：服务端错误状态码。
  - 500：Internal Server Error 服务器内部错误，无法完成请求。
  - 501：Not Implemented 服务器不支持请求的功能，无法完成请求。
  - 502：Bad Gateway 作为网关或者代理工作的服务器尝试执行请求时，从远程服务器接收到了一个无效的响应。
  - 503：Service Unavailable 由于超载或系统维护，服务器暂时的无法处理客户端的请求。延时的长度可包含在服务器的Retry-After头信息中。
  - 504：Gateway Time-out 充当网关或代理的服务器，未及时从远端服务器获取请求。
  - 505：HTTP Version not supported 服务器不支持请求的HTTP协议的版本，无法完成处理。

### 2. 请求方法
1. GET: 向服务器获取数据；
2. POST：将实体提交到指定的资源，通常会造成服务器资源的修改；
3. PUT：上传文件，更新数据；
4. DELETE：删除服务器上的对象；
5. HEAD：获取报文首部，与GET相比，不返回报文主体部分；
6. OPTIONS：询问支持的请求方法，用来跨域
7. CONNECT：要求在与代理服务器通信时建立隧道，使用隧道进行TCP通信；
8. TRACE: 回显服务器收到的请求，主要⽤于测试或诊断。

### 3. HTTP请求头和响应头
*HTTP Request Header 常见的请求头：*

Accept:浏览器能够处理的内容类型

Accept-Charset:浏览器能够显示的字符集

Accept-Encoding：浏览器能够处理的压缩编码

Accept-Language：浏览器当前设置的语言

Connection：浏览器与服务器之间连接的类型

Cookie：当前页面设置的任何Cookie

Host：发出请求的页面所在的域

Referer：发出请求的页面的URL

User-Agent：浏览器的用户代理字符串


*HTTP Responses Header 常见的响应头：*

Date：表示消息发送的时间，时间的描述格式由rfc822定义

server:服务器名称

Connection：浏览器与服务器之间连接的类型

Cache-Control：控制HTTP缓存

Content-type:表示后面的文档属于什么MIME类型


<b><details><summary>1. GET和POST的请求的区别</summary></b>
Post 和 Get 是 HTTP 请求的两种方法，其区别如下：

应用场景： GET 请求是一个幂等的请求，一般 Get 请求用于对服务器资源不会产生影响的场景，比如说请求一个网页的资源。而 Post 不是一个幂等的请求，一般用于对服务器资源会产生影响的情景，比如注册用户这一类的操作。

是否缓存： 因为两者应用场景不同，浏览器一般会对 Get 请求缓存，但很少对 Post 请求缓存。

发送的报文格式： Get 请求的报文中实体部分为空，Post 请求的报文中实体部分一般为向服务器发送的数据。

安全性： Get 请求可以将请求的参数放入 url 中向服务器发送，这样的做法相对于 Post 请求来说是不太安全的，因为请求的 url 会被保留在历史记录中。

请求长度： 浏览器由于对 url 长度的限制，所以会影响 get 请求发送数据时的长度。这个限制是浏览器规定的，并不是 RFC 规定的。
参数类型： post 的参数传递支持更多的数据类型。
</details>

<b><details><summary>2. POST和PUT请求的区别</summary></b>
PUT请求是向服务器端发送数据，从而修改数据的内容，但是不会增加数据的种类等，也就是说无论进行多少次PUT操作，其结果并没有不同。（可以理解为时更新数据）
POST请求是向服务器端发送数据，该请求会改变数据的种类等资源，它会创建新的内容。（可以理解为是创建数据）
</details>

<b><details><summary>3. 常见的 Content-Type 属性值</summary></b>
- application/x-www-form-urlencoded：浏览器的原生 form 表单，如果不设置 enctype 属性，那么最终就会以 application/x-www-form-urlencoded 方式提交数据。该种方式提交的数据放在 body 里面，数据按照 key1=val1&key2=val2 的方式进行编码，key 和 val 都进行了 URL转码。
- multipart/form-data：该种方式也是一个常见的 POST 提交方式，通常表单上传文件时使用该种方式。
- application/json：服务器消息主体是序列化后的 JSON 字符串。
- text/xml：该种方式主要用来提交 XML 格式的数据。
</details>

<b><details><summary>4. 301、302、307的区别</summary></b>
PUT请求是向服务器端发送数据，从而修改数据的内容，但是不会增加数据的种类等，也就是说无论进行多少次PUT操作，其结果并没有不同。（可以理解为时更新数据）
POST请求是向服务器端发送数据，该请求会改变数据的种类等资源，它会创建新的内容。（可以理解为是创建数据）
</details>

<b><details><summary>5. 手写实现 ajax请求</summary></b>
```js
function objToString(params) {
    if(!params) {
        return '';
    }
    return Object.keys(params).map(key=>{
        return key+'='+ encodeURLComponent(params[key]);
    }).join('&');
}
function ajax(options){
    return new Promise((reslove, reject) => {
        let xhr = null;
        const str = objToString(options.params);
        if(window.XMLHttpRequest) {
            xhr = new XMLHttpRequest();
        }else{
            xhr = new ActiveObject("Microsoft.XMLHttp")
        }
        
        if(options.methods === "GET") {
            xhr.open(options.methods, url, true);
            xhr.send();
        }else{
            xhr.open(options.methods, url, true);
            const headers = {
                "Content-type": "application/x-www-form-urlencoded",
                ...options.headers
            }
            Object.keys(headers).forEach(key => {
                xhr.setRequestHeader(key, headers[key]);
            });

            xhr.send(str);
        }
        xhr.onreadystatechange = function () {
            if (xhr.readyState === 4) {
                if (xhr.status >= 200 && xhr.status < 300 || xhr.status == 304) {
                    reslove(xhr.responseText)
                } else {
                    reject(xhr)
                }
            }
        }
        xhr.onabort = function() {
            reject('请求被取消');
        }
        xhr.timeout = options.timeout || 1000;
        xhr.ontimeout = function() {
            reject('请求超时')
        }
        xhr.onerror = function() {
            reject('请求报错');
        }

    });
}
```
</details>

