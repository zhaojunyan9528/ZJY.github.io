---
title: Cookie、Session、Token、JWT的区别
tags:
  - 前端

date: 2021-02-04 09:37:50
---

## Cookie、Session、Token、JWT

### 什么是认证（Authentication）?

认证就是验证当前用户的身份，证明你是你自己。
认证方式：
+ 用户名密码登录
+ 手机号发送验证码
+ 邮箱发送链接，点击验证登录

### 什么是授权（Authorization）?

用户授予第三方应用访问用户某些资源的权限
+ 安装app时，app询问是否允许授予权限（位置，存储等）
+ 访问微信小程序，授权登录，询问是否允许授予权限（昵称，头像，地区等信息）

实现授权的方式：cookie、session、token、OAuth

### 什么是凭证（Credentials）?

+ 实现认证和授权的前提
+ 需要一种媒介（证书）来标记访问者的身份
+ 比如居民身份证
+ 比如bilibili，有游客模式和登录模式，游客模式可以正常浏览观看，但是若要点赞，收藏，则需要用户登录。用户登录后服务器会给该用户使用的浏览器一个令牌token，这个令牌来表明你的身份，每次浏览器发送请求时都会带上这个令牌。

### 什么是Cookie？

+ HTTP是无状态的协议（对于事务处理没有记忆能力，每次客户端和服务端会话完成时，服务端不会保存任何会话信息）：每个请求都是完全独立的，服务端无法确认当前访问者的身份信息，无法分辨上一次的请求发送者和这一次的发送者是不是同一个人。所以服务器与浏览器为了进行会话跟踪（知道是谁在访问我），就必须主动的去维护一个状态，这个状态用于告知服务端前后两个请求是否来自同一浏览器。而这个状态需要通过 cookie 或者 session 去实现。
+ cookie存储在客户端：cookie是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器发送请求时被携带并发送到服务器上。
+ cookie是不可跨域的：每个cookie都会绑定单一的域名，无法在别的域名下获取使用，一级域名和二级域名之间是允许共享使用的（靠domain）
+ cookie重要的属性：
    + name=value健值对，设置cookie的名称及相应的值，都必须是字符串类型。
        如果值为unicode字符，需要转为字符编码
        如果值为二进制，需要转为base64编码

    + domain指定cookie所属域名，默认是当前域名，path指定cookie在哪个路径下生效，默认是'/'。如果设置为/abc，则只有/abc下的路由可以访问到该cookie，如/abc/read。
    + maxAgeCookie：失效的时间，单位秒，如果为整数，则该cookie在maxAge秒后失效。如果为负数，则该cookie为临时cookie，关闭浏览器即失效，浏览器也不会以任何形式保存该cookie。如果为0，表示删除该cookie。默认为-1。比expires好用。一般浏览器的 cookie 都是默认储存的，当关闭浏览器结束这个会话的时候，这个 cookie 也就会被删除。
    + expires：过期时间，在设置的某个时间点后该cookie就会失效。
    + secure：该cookie是否仅被使用安全协议传输。安全协议有HTTPS，SSL等。在网络上传输数据先将数据进行加密。默认是false，当secure为true时，cookie在http中是无效的，在https才有效。
    + httpOnly：如果给某个cookie设置来httpOnly属性，则无法通过js脚本读取到该cookie的信息，但是还是能够通过Application中手动修改cookie，所以只是在一定程度上可以防止 XSS 攻击，不是绝对的安全

### 什么是Session？

+ session是另一种记录服务器和客户端会话状态的机制
+ session是基于cookie实现的，session存储在服务端，sessionId会被存储在客户端的cookie中
+ session认证流程：
    + 用户第一次请求服务器的时候，服务器根据用户提交的相关信息，创建对应的session
    
    + 请求返回将此session的唯一标识sessionId返回给浏览器
    + 浏览器接收到服务器返回的sessionId后会将该信息存储到cookie中，同时cookie将记录此sessionId属于哪个域名
    + 当用户第二次访问服务器的时候，请求会自动判断此域名下是否存在cookie信息，如果存在自动将cookie信息也发送给服务器，服务端会从cookie中获取sessionId，再根据sessionId查找对于session信息，如果没有找到说明用户没有登录或者登录失效，如果找到session证明用户已经登录可执行后续操作。
+ sessionId是连接cookie和session的一道桥梁，大部分系统也是根据此原理来验证用户登录状态

### cookie和session的区别？

+ 安全性：session比cookie安全，session是存储在服务端的，cookie是存储在客户端的
+ 存取值的类型不同： cookie只支持字符串数据，想要设置其他类型的数据，需要将其转换为字符串，session可以存任意数据类型。
+ 有效期不同：cookie可设置为长时间保持，比如我们经常使用的默认登录功能。session一般失效时间较短，客户端关闭或者session超时都会失效。
+ 存储大小不同：单个cookie存储数据不能超过4k，session可存储数据远高于cookie，但是当访问量过多，会占用过多的服务器资源。


### 什么是token（令牌）？

+ Access Token
    + 访问资源接口（API）时所需要的资源凭证

    + 简单token的组成： uid(用户唯一身份标识)、time(当前时间戳)、sign（签名，token的前几位以哈希算法压缩为一定长度的十六进制字符串）
    + 特点： 服务端无状态化、可扩展行好、支持移动端设备、安全、支持跨程序调用
    + token的身份验证流程：

        + 浏览器发送用户名密码等信息给服务器，服务器将登录凭证做成数字签名，加密之后得到字符串作为token
        + 服务器将token返回给浏览器，拿到token后，将token保存到本地
        + 请求时携带token给服务端
        + 服务器拿到token串，做解密和签名认证，判断其有效性，将数据返回给浏览器
    + 每一次请求都需要携带token，需要把token放到http的header里
    + 基于token的用户认证是一种服务端无状态的方式，服务端不用存放token数据。用解析token的计算时间来换取session的存储空间，从而减轻服务器的压力。
    + token完全由应用管理，所以它可以避开同源策略

+ Refresh Token
    + refresh token是一种专用于刷新access token的token。如果没有refresh token也可以刷新access token，但每次刷新都需要用户输入登录名和密码，会很麻烦。有了refresh token可以减少这个麻烦。客户端直接用 refresh token 去更新 access token，无需用户进行额外的操作。
    + Access Token 的有效期比较短，当 Acesss Token 由于过期而失效时，使用 Refresh Token 就可以获取到新的 Token，如果 Refresh Token 也失效了，用户就只能重新登录了。
    + Refresh Token 及过期时间是存储在服务器的数据库中，只有在申请新的 Acesss Token 时才会验证，不会对业务接口响应时间造成影响，也不需要向 Session 一样一直保持在内存中以应对大量的请求。

### token和session的区别？

+ session是一种记录客户端和服务器会话状态的机制，使服务端有状态化，可以记录会话信息。而token是令牌，访问资源接口（API）时所需要的资源凭证。token使服务端无状态化，不会存储会话信息。

+ session和token并不矛盾，作为身份认证token安全性比session好，因为每一个请求都有签名还能防止监听以及重放攻击，而session就必须依赖链路层来保障通讯安全了。如果你需要实现现有状态的会话，仍然可以增加session来在服务端保存一些状态。
+ 所谓session认证只是简单的把user信息存储到session里，因为sessionId的不可预测性，暂且认为是安全的。而token，如果指的是OAuth token或类似机制的话，提供的是认证和授权。认证是真的用户，授权是针对app。

### 什么是JWT？

+ JSON Web Token（简称JWT）是目前最流行的跨域认证解决方案。

+ 是一种认证授权机制
+ JWT是为了在网络应用环境间传递声明而执行的一种基于JSON的开发标准。jwt的声明一般被用来在身份提供者和服务提供者间传递被认证的用户身份信息，以便于从资源服务器获取资源。比如用在用户登录上。
+ 可以使用 HMAC 算法或者是 RSA 的公/私秘钥对 JWT 进行签名。因为数字签名的存在，这些传递的信息是可信的。

### JWT的原理

+ JWT认证流程：
    + 用户输入用户名/密码登录，服务端认证成功后，会返回给客户端一个JWT
    + 客户端将token保存到本地（通常使用localStorage，也可以使用cookie）
    + 当用户希望访问一个受保护的路由或者资源的时候，需要请求头的Authorization字段中使用bearer模式添加JWT，
        Authorization: bearer <token>
    + 服务端的保护路由将会检查请求头Authorization中的JWT信息，如果合法，则允许用户的行为。
    + 因为JWT是自包含的（内部包含了一些会话信息），因此减少了需要查询数据库的需要。
    + 因为 JWT 并不使用 Cookie 的，所以你可以使用任何域名提供你的 API 服务而不需要担心跨域资源共享问题（CORS）
    + 因为用户的状态不再存储在服务端的内存中，所以这是一种无状态的认证机制

### JWT的使用方式

客户端收到服务端返回的jwt，可以存储到cookie里面，也可以储存到localStorage里面。
+ 方式一
    + 当用户希望访问一个受保护的路由或者资源的时候，可以把它放在cookie里面自动发送，但是这样不能跨域，所以更好的做法是放在http请求头信息的Authorization字段里，使用bearer模式添加jwt。
        get: /user/page
        host: http://api.example.com
        Authorization: Bearer<token>
    + 用户的状态不会保存在服务端，这是一种无状态的认证机制。
    + 服务端的保护路由会检查请求头Authorization中的JWT信息，如果合法，则允许用户的行为
    + 由于JWT是自包含的，因此减少了需要查询数据库的需要
    + JWT 的这些特性使得我们可以完全依赖其无状态的特性提供数据 API 服务，甚至是创建一个下载流服务
    + 因为JWT并不使用cookie，所以你可以使用任何域名提供你的 API 服务而不需要担心跨域资源共享问题（CORS）
+ 方式二
    + 跨域的时候，可以把 JWT 放在 POST 请求的数据体里。
+ 方式三
    + 通过 URL 传输：http://www.example.com/user?token=xxx

### token和JWT的区别？

+ 相同：
    + 都是访问资源的令牌
    + 都可以记录用户的信息
    + 都是使服务端无状态化
    + 都是只有验证成功后，客户端才能访问服务端上受保护的资源
+ 区别：
    + token：服务端验证客户端发送过来的token时，还需要查询数据库获取用户信息，然后验证token是否有效
    + JWT：将token和payload加密后存储于客户端，服务端只需要使用密钥进行校验（校验也是JWT自己实现的）即可，不需要查询或者减少查询数据库，因为JWT自包含了用户信息和加密的数据。

### 常见的前后的鉴权方式
+ session/cookie
+ token验证（包括JWT，SSO）
+ OAuth2.0（开发授权）

### 常见问题
+ 使用cookie时需要考虑的问题
    + 存储在客户端容易被客户端篡改，使用前需要验证合法性
    + 不能存户敏感数据
    + 使用httpOnly在一定程度上可以提高安全性
    + 尽量减少cookie的体积，能存储的数据不能超过4k
    + 设置正确的domain和path，减少数据传输
    + cookie无法跨域
    + 一个浏览器针对一个网站最多存20个cookie，浏览器一般只允许存放300个cookie
    + 移动端对 cookie 的支持不是很好，而 session 需要基于 cookie 实现，所以移动端常用的是 token

+ 使用session时需要考虑的问题
    + 将 session 存储在服务器里面，当用户同时在线量比较多时，这些 session 会占据较多的内存，需要在服务端定期的去清理过期的 session
    + 当网站采用集群部署的时候，会遇到多台 web 服务器之间如何做 session 共享的问题。因为 session 是由单个服务器创建的，但是处理用户请求的服务器不一定是那个创建 session 的服务器，那么该服务器就无法拿到之前已经放入到 session 中的登录凭证之类的信息了
    + 当多个应用要共享 session 时，除了以上问题，还会遇到跨域问题，因为不同的应用可能部署的主机不一样，需要在各个应用做好 cookie 跨域的处理。
    + sessionId 是存储在 cookie 中的，假如浏览器禁止 cookie 或不支持 cookie 怎么办？ 一般会把 sessionId 跟在 url 参数后面即重写 url，所以 session 不一定非得需要靠 cookie 实现
+ 使用 token 时需要考虑的问题
    + 如果你认为用数据库来存储 token 会导致查询时间太长，可以选择放在内存当中。比如 redis 很适合你对 token 查询的需求
    + token 完全由应用管理，所以它可以避开同源策略
    + token 可以避免 CSRF 攻击(因为不需要 cookie 了)
+ 使用 JWT 时需要考虑的问题
    + 因为 JWT 并不依赖 Cookie 的，所以你可以使用任何域名提供你的 API 服务而不需要担心跨域资源共享问题（CORS）
    + JWT 默认是不加密，但也是可以加密的。生成原始 Token 以后，可以用密钥再加密一次。
    + JWT 不加密的情况下，不能将秘密数据写入 JWT。
    + JWT 不仅可以用于认证，也可以用于交换信息。有效使用 JWT，可以降低服务器查询数据库的次数。
    + JWT 最大的优势是服务器不再需要存储 Session，使得服务器认证鉴权业务可以方便扩展。但这也是 JWT 最大的缺点：由于服务器不需要存储 Session 状态，因此使用过程中无法废弃某个 Token 或者更改 Token 的权限。也就是说一旦 JWT 签发了，到期之前就会始终有效，除非服务器部署额外的逻辑。
    + JWT 本身包含了认证信息，一旦泄露，任何人都可以获得该令牌的所有权限。为了减少盗用，JWT的有效期应该设置得比较短。对于一些比较重要的权限，使用时应该再次对用户进行认证
    + JWT 适合一次性的命令认证，颁发一个有效期极短的 JWT，即使暴露了危险也很小，由于每次操作都会生成新的 JWT，因此也没必要保存 JWT，真正实现无状态
    + 为了减少盗用，JWT 不应该使用 HTTP 协议明码传输，要使用 HTTPS 协议传输

### 只要关闭浏览器 ，session 真的就消失了？

不对。对 session 来说，除非程序通知服务器删除一个 session，否则服务器会一直保留，程序一般都是在用户做 log off 的时候发个指令去删除 session。然而浏览器从来不会主动在关闭之前通知服务器它将要关闭，因此服务器根本不会有机会知道浏览器已经关闭，之所以会有这种错觉，是大部分 session 机制都使用会话 cookie 来保存 session id，而关闭浏览器后这个 session id 就消失了，再次连接服务器时也就无法找到原来的 session。如果服务器设置的 cookie 被保存在硬盘上，或者使用某种手段改写浏览器发出的 HTTP 请求头，把原来的 session id 发送给服务器，则再次打开浏览器仍然能够打开原来的 session。恰恰是由于关闭浏览器不会导致 session 被删除，迫使服务器为 session 设置了一个失效时间，当距离客户端上一次使用 session 的时间超过这个失效时间时，服务器就认为客户端已经停止了活动，才会把 session 删除以节省存储空间。