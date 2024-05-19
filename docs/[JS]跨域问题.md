> https://cloud.tencent.com/developer/article/1953193

### 1. 浏览器同源策略

  同源策略判断依据： 协议、域名、端口相同

> 为什么要有同源策略？

  同源策略是一个重要的安全策略，它用于限制一个origin的文档或者它加载的脚本如何能与另一个源的资源进行交互。它能帮助阻隔恶意文档，减少可能被攻击的媒介。

### 2. 解决跨域方法

#### 2.1 JSONP跨域

    原理： 利用 script标签色src属性，可以跨域请求内容
    优缺点：
        缺点： 只能进行get请求
        优点： 兼容性强，可以向下兼容比较古老的浏览器
    实现

```html
<script type='text/javascript'>
    window.jsonpCallback = function (res) {
        console.log(res)
    }
</script>
<script src='http://localhost:8080/api/jsonp?id=1&cb=jsonpCallback' type='text/javascript'></script>

<!--实现-->
<script>
    function JSONP({
        url,
        params = {},
        callbackKey = 'cb',
        callback
    }) {
        // 定义本地的一个callback的名称
        const callbackName = 'jsonpCallback';
        // 把这个名称加入到参数中: 'cb=jsonpCallback'
        params[callbackKey] = callbackName;
        //  把这个callback加入到window对象中，这样就能执行这个回调了
        window[callbackName] = callback;

        // 得到'id=1&cb=jsonpCallback'
        const paramString = Object.keys(params).map(key => {
            return `${key}=${params[key]}`
        }).join('&')
        // 创建 script 标签
        const script = document.createElement('script');
        script.setAttribute('src', `${url}?${paramString}`);
        document.body.appendChild(script);
    }
    JSONP({
        url: 'http://localhost:8080/api/jsonp',
        params: { id: 1 },
        callbackKey: 'cb',
        callback (res) {
            console.log(res)
        }
    })
</script>
```

#### 2.2 CORS 跨域

    CORS （Cross-Origin Resource Sharing，跨域资源共享）是一个系统，它由一系列传输的HTTP头组成，这些HTTP头决定浏览器是否阻止前端 JavaScript 代码获取跨域请求的响应。
    
    同源安全策略 默认阻止“跨域”获取资源。但是 CORS 给了web服务器这样的权限，即服务器可以选择，允许跨域请求访问到它们的资源。
    
    *请求头*
    Access-Control-Request-Headers
    用于发起一个预请求，告知服务器正式请求会使用那些 HTTP 头。
    Access-Control-Request-Method
    用于发起一个预请求，告知服务器正式请求会使用哪一种 HTTP 请求方法。
    
    *响应头*
    Access-Control-Allow-Origin
    指示请求的资源能共享给哪些域。
    Access-Control-Allow-Credentials
    指示当请求的凭证标记为 true 时，是否响应该请求。
    Access-Control-Allow-Headers
    用在对预请求的响应中，指示实际的请求中可以使用哪些 HTTP 头。
    Access-Control-Allow-Methods
    指定对预请求的响应中，哪些 HTTP 方法允许访问请求的资源。
    Access-Control-Expose-Headers
    指示哪些 HTTP 头的名称能在响应中列出。
    Access-Control-Max-Age
    指示预请求的结果能被缓存多久。
    Origin
    指示获取资源的请求是从什么域发起的。

```
curl 'https://otheve.beacon.qq.com/analytics/v2_upload?appkey=0WEB0OEX9Y4SQ244' \
  -H 'accept: application/json, text/plain, */*' \
  -H 'accept-language: zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6' \
  -H 'content-type: application/json;charset=utf-8' \
  -H 'origin: https://cloud.tencent.com' \
  -H 'priority: u=1, i' \
  -H 'referer: https://cloud.tencent.com/' \
  -H 'sec-ch-ua: "Chromium";v="124", "Microsoft Edge";v="124", "Not-A.Brand";v="99"' \
  -H 'sec-ch-ua-mobile: ?0' \
  -H 'sec-ch-ua-platform: "macOS"' \
  -H 'sec-fetch-dest: empty' \
  -H 'sec-fetch-mode: cors' \
  -H 'sec-fetch-site: cross-site' \
  -H 'user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0.0.0 Safari/537.36 Edg/124.0.0.0' \
  --data-raw '{"appVersion":"","sdkId":"js","sdkVersion":"4.5.20-web","mainAppKey":"0WEB0OEX9Y4SQ244","platformId":3,"common":{"atversion":"4.1.15","uid":"","fromsource":"qcloud.bing.seo","from":"","from_column":"","islogined":"false","pagetag":"通用技术-服务器技术-http,通用技术-数据库开发-access,通用技术-编程语言-php,通用技术-开发技术-运维,通用技术-网络技术-https","trafficparams":"***$;timestamp%3D1714183033972;from_type%3Dserver;track%3Db25d4fa6-7e49-4ab5-b89d-fdb8809d4458;$***","owneruin":"","qclouduid":"Csv_O-OD9l-M","articleSource":"B","magicSource":"N","authorType":"Z","subjectTime":"2022-03-09 11:28:49","productSlug":"tke,crg","system":"Mac OS(10.15.7)-Blink","browser":"Edge(124.0.0.0)","spider":"","seoKeywords":"通用技术-服务器技术-http,通用技术-数据库开发-access,通用技术-编程语言-php,通用技术-开发技术-运维,通用技术-网络技术-https","A2":"f3NKee1bGyr6Tn089tP9WtcZd4j12aWS","A8":"","A12":"zh-CN","A17":"1792*1120*2","A23":"","A50":"","A76":"0WEB0OEX9Y4SQ244_1714183032935","A101":"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0.0.0 Safari/537.36 Edg/124.0.0.0","A102":"https://cloud.tencent.com/developer/article/1953193","A104":"https://cn.bing.com/","A119":"","A153":""},"events":[{"eventCode":"qcdeveloper_pageStay","eventTime":"1714184436398","mapValue":{"viewportWidth":"952","viewportHeight":"953","url":"https://cloud.tencent.com/developer/article/1953193","pathname":"/developer/article/1953193","query":"","hostname":"cloud.tencent.com","pagefrom":"cn.bing.com$$/$$","urlfrom":"https://cn.bing.com/","title":"别在问我跨域问题了，跨域详解以及前端、后端、运维解决的方法统统写在这里了。-腾讯云开发者社区-腾讯云","duration":"315746","pvId":"gSo8aFZFpyl63wqgMed02","$url_path":"/developer/article/1953193","$referrer":"https://cn.bing.com/","A99":"N","A100":"252","A72":"4.5.20-web","A88":"1714183032935"}}]}'
```

#### 2.3 通过代理解决

```
// nginx  配置
location /api {
   proxy_pass https://b.test.com; # 设置代理服务器的协议和地址
}

// webpack-dev-server
devServer: {
    host: 'localhost', // 主机地址
    port: '8000', // 端口
    proxy: {
        '/api': {
            target: 'xxxxxxxx', // 真实地址
            changeOrigin: true,
            pathRewrite: {
                    '/api': ''
            }
        }
    }
}
```
