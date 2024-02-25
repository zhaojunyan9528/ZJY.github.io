---
title: HTML5存储方式
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2022-10-28 10:45:27
---

html5存储方式

1. cookie
2. localStorage
3. sessionStorage
4. 离线缓存application cache
5. Service Workers
6. Web SQL
7. IndexedDB

## 1.cookie

存储在用户客户端
cookie是一段不超过4KB的小型文本数据，由一个名称name,一个值value和其他几个用于控制cookie有效期、安全性、使用范围的可选属性组成。其中：

+ Name/Value:设置Cookie的名称及相对应的值
+ Expires属性：设置Cookie的生存期。有两种存储类型的Cookie：会话性与持久性。Expires属性缺省时，为会话性Cookie，仅保存在客户端内存中，并在用户关闭浏览器时失效；持久性Cookie会保存在用户的硬盘中，直至生存期到或用户直接在网页中单击“注销”等按钮结束会话时才会失效。
+ Path属性：定义了Web站点上可以访问该Cookie的目录
+ Domain属性：指定了可以访问该 Cookie 的 Web 站点或域
+ Secure属性：指定是否使用HTTPS安全协议发送Cookie
+ HTTPOnly 属性 ：用于防止客户端脚本通过document.cookie属性访问Cookie

## 2.本地存储localStorage

以键值对（key-value）的方式存储，存储的数据已字符串形式存在，永久存储，永不失效，除非手动删除。
每个域名5M。
常用API：getItem,setItem,removeItem,key,clear
localStorage只要在相同的协议、相同的主机名、相同的端口下，就能读取/修改到同一份localStorage数据。

## 3.本地存储sessionStorage

协议，域名，端口和窗口
session，只要关闭浏览器（也包括浏览器的标签页），就会被清空

## 4.离线缓存（application cache）

本地缓存应用所需的文件

1.配置manifest文件

```html
<!DOCTYPE HTML>
<html manifest="demo.appcache">
...
</html>
```

需要在web服务器配置正确的 MIME-type，即 "text/cache-manifest"。

2.manifest文件

+ CACHE MANIFEST：此标题下列出的文件将在首次下载后进行缓存。
+ NETWORK：不会被缓存，需要下载，若和CACHE文件有冲突则以CACHE为主
+ FALLBACK：无法被访问时的回退页面如404

```appcache
CACHE MANIFEST
/buju.css

NETWORK:
/source/images

FALLBACK:
base64.html
```

优势：离线浏览、加快页面加载速度、减少服务器负载

注意事项：
每个站点离线存储容量限制5M
manifest文件中缓存的文件如果有一个不能下载整个更新过程失败，浏览器继续使用老的缓存。
使用manifest文件的html必须和manifest文件同源
FALLBACK中的资源必须和manifest文件同源
在manifest中使用的相对路径,相对参照物为manifest文件
站点中的其他页面即使没有设置manifest属性，请求的资源如果在缓存中也从缓存中访问
当manifest文件发生改变时，资源请求本身也会触发更新

3.浏览器是怎么对离线资源进行管理和加载？
在线情况下，浏览器碰到manifest配置，就去请求manifest文件，如果是第一次访问app，就根据manifest文件去下载要离线缓存的资源，并存储。如果已经访问过app并且离线资源已经存储，就使用离线资源，再比对manifest文件有没有改动，如果有重新下载资源并存储。
离线情况下，浏览器就直接使用离线存储的资源。

## 5.Service Workers离线存储

1.使用前设置
在支持Service Workers的浏览器中很多特性没有默认开启，需要开启浏览器的相关配置。

+ Firefox Nightly: 访问 about:config 并设置 dom.serviceWorkers.enabled 的值为 true; 重启浏览器；
+ Chrome Canary: 访问 chrome://flags 并开启 experimental-web-platform-features; 重启浏览器 (注意：有些特性在 Chrome 中没有默认开放支持)；
+ Opera: 访问 opera://flags 并开启 ServiceWorker 的支持; 重启浏览器。

出于安全原因，Service Workers 要求必须在 HTTPS 下才能运行。为了便于本地开发，localhost 也被浏览器认为是安全源。

2.使用
1）注册

```js
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register(
    '/servicework.js',
    {
      scope: '/' // 参数是选填的，可以被用来指定你想让 service worker 控制的内容的子目录
    }
  ).then(function(reg) {
    console.log('Registration succeeded. Scope is ' + reg.scope);
  }).catch(function(error) {
    console.log('Registration failed with ' + error);
  })
}
```

service worker 只能抓取在 service worker scope 里从客户端发出的请求。

2)安装和激活：填充你的缓存

```js
this.addEventListener('install', function(event) {
  event.waitUntil(
    caches.open('v1').then(function(cache) {
      return cache.addAll([
        '/buju.css',
        'buju.html'
      ])
    }).catch(function(error) {
      console.log('创建缓存失败', error)
    })
  )
})
```

注意：localStorage跟service worker的cache工作原理类似，但是他是同步的，所以不能在sw内使用，IndexedDB可以在service worker内做数据存储

3)自定义请求的响应
每次任何被Service Worker控制的资源被请求到时，都会触发fetch事件，这些资源包括指定scope内的文档，和这些文档内引用的任何资源。
在service worker上添加一个fetch事件监听器，接着调用respondWidth方法来劫持我们的http响应，然后自己处理。

```js
this.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.match(event.request).then(function (response) {
      // 如果没有在缓存中找到匹配的资源，你可以告诉浏览器对着资源直接去 fetch 默认的网络请求
      return response || fetch(event.request).then(function (resp) {
        return caches.open('v1').then(function (cache) {
          cache.put(event.request, resp.clone())
          return resp
        })
      })
      // 这里我们用 fetch(event.request) 返回了默认的网络请求，它返回了一个 promise。
      // 当网络请求的 promise 成功的时候，我们 通过执行一个函数用 caches.open('v1') 来抓取我们
      // 的缓存，它也返回了一个 promise。当这个 promise 成功的时候， cache.put() 被用来把这些
      // 资源加入缓存中。资源是从 event.request 抓取的，它的响应会被 response.clone() 克隆一份
      // 然后被加入缓存。这个克隆被放到缓存中，它的原始响应则会返回给浏览器来给调用它的页面。
    }).catch(function () {
      return caches.match('/fallback.html')
    })
    // 当请求没有匹配到请求的资源网络也不可用时，请求会失败，提供一个回退方案
  )
})
```

caches.match(event.request) 允许我们对网络请求的资源和 cache 里可获取的资源进行匹配，查看是否缓存中有相应的资源

4)更新你的service worker
如果你的sw已被安装，但是刷新页面的时候发现一个新的版本可用，新版的sw会在后台安装，但是没被激活，当不再有任何已加载的页面在使用旧版的 service worker 的时候，新版本才会激活。

```js
self.addEventListener('install', function(event) {
  event.waitUntil(
    caches.open('v2').then(function(cache) {
      return cache.addAll([
        '/buju.css',
        'buju.html',
        'responsive.html'
      ])
    })
  )
})

// 删除旧缓存
self.addEventListener('activate', function(event) {
  var cacheWhitelist = ['v2'];

  event.waitUntil(
    caches.keys().then(function(keyList) {
      return Promise.all(keyList.map(function(key) {
        if (cacheWhitelist.indexOf(key) === -1) {
          return caches.delete(key);
        }
      }))
    })
  )
})
```

## 6.Web SQL

Web SQL数据库API并不是HTML5规范的一部分，但它是一个独立的规范，引入了一组使用SQL操作客户端数据库的APIs。
但由于兼容性和性能问题，Web SQL已经被IndexedDB所取代。

核心方法

+ openDatabase：使用现有的数据库或新建的数据库创建一个数据库对象
+ transaction： 控制一个事务，以及基于这种情况执行提交或回滚
+ executeSql：用于执行实际的SQL查询
  
1.打开数据库

```js
var db = openDatabase('mydb', '1.0', 'TEST DB', 2 * 1024 * 1024, fn);
```

2.执行查询操作

```js
var db = openDatabase('mydb', '1.0', 'TEST DB', 2 * 1024 * 1024);
db.transaction(function (tx) {
    tx.executeSql('CREATE TABLE IF NOT EXISTS WIN (id unique, name)');
})
```

3.插入数据

```js
var db = openDatabase('mydb', '1.0', 'Test DB', 2 * 1024 * 1024);
db.transaction(function (tx) {
   tx.executeSql('CREATE TABLE IF NOT EXISTS WIN (id unique, name)');
   tx.executeSql('INSERT INTO WIN (id, name) VALUES (1, "winty")');
   tx.executeSql('INSERT INTO WIN (id, name) VALUES (2, "LuckyWinty")');
})
```

4.读取数据

```js
db.transaction(function (tx) {
   tx.executeSql('SELECT * FROM WIN', [], function (tx, results) {
      var len = results.rows.length, i;
      msg = "<p>查询记录条数: " + len + "</p>";
      document.querySelector('#status').innerHTML +=  msg;

      for (i = 0; i < len; i++){
         alert(results.rows.item(i).name );
      }

   }, null);
})
```

![image](/images/websql.jpg)

## 7.indexedDB

```js
let dbName = 'helloIndexedDB', version = 1, storeName = 'helloStore'
let indexedDB = window.indexedDB
let db
const request = indexedDB.open(dbName, version)
request.onsuccess = function (event) {
  db = event.target.result // 数据库对象
  console.log('数据库打开成功')
}
request.onerror = function (event) {
  console.log('数据库打开报错')
}
// 数据库创建或升级时触发
request.onupgradeneeded = function (event) {
  console.log('onupgradeneeded')
  db = event.target.result // 数据库对象
  let objStore
  if (!db.objectStoreNames.contains(storeName)) {
    objStore = db.createObjectStore(storeName, {keyPath: 'id'})
  }
}
// 添加数据
function addData(db, storeName, data) {
  let request = db.transaction([storeName], 'readwrite')
    .objectStore(storeName) // 仓库对象
    .add(data)
  request.onsuccess = function(event) {
    console.log('数据写入成功')
  }
  request.onerror = function(event) {
    console.log('数据写入失败')
    throw new Error(event.target.error)
  }
}

// 根据id获取数据
function getDataBykey(db, storeName, key) {
  let req = db.transaction([storeName]).objectStore(storeName).get(key)
  req.onsuccess = function (event) {
    console.log('查询结果', req.result)
  }
  req.onerror = function (event) {
    console.log('查询失败')
  }
}

// 根据id修改数据
function updateDB(db, storeName, data) {
  let req = db.transaction([storeName], 'readwrite')
    .objectStore(storeName)
    .put(data)
  req.onsuccess = function (event) {
    console.log('数据更新成功')
  }
  req.onerror = function (event) {
    console.log('数据更新失败')
  }
}

// 根据id删除数据
function deleteBD(db, storeName, id) {
  let req = db.transaction([storeName], 'readwrite')
    .objectStore(storeName)
    .delete(id)
  req.onsuccess = function (event) {
    console.log('数据删除成功')
  }
  req.onerror = function (event) {
    console.log('数据删除失败')
  }
}

// 由于打开indexDB是异步的加个定时器避免 db对象还没获取到值导致 报错
setTimeout(() => {
  addData(db, storeName, {
    id: 1,
    name: 'zhangsan',
    age: 18,
    desc: 'a girl'
  })

  getDataBykey(db, storeName, 1)

  updateDB(db, storeName, {id: 1, desc: 'a boy'})

  // deleteBD(db, storeName, 1)
}, 1000)
```

indexDB兼容性存在一定的问题，异步获取数据，根据浏览器规则存储在50-250M，并且用户清除浏览器缓存时不会将其清除。
在使用时切记只有当修改新增数据库表单时才需要增加版本号，如果只是修改数据库表单里面的数据是不需要改变版本号的。

封装类库

```js

export function openDB(dbName, storeName, version = 1) {
  return new Promise((resolve, reject) => {
    let indexedDB = window.indexedDB
    let db
    const request = indexedDB.open(dbName, version)
    request.onsuccess = function(event) {
      db = event.target.result // 数据库对象
      resolve(db)
    }
 
    request.onerror = function(event) {
      reject(event)
    }
 
    request.onupgradeneeded = function(event) {
      // 数据库创建或升级的时候会触发
      console.log('onupgradeneeded')
      db = event.target.result // 数据库对象
      let objectStore
      if (!db.objectStoreNames.contains(storeName)) {
        objectStore = db.createObjectStore(storeName, { keyPath: 'id' }) // 创建表
        // objectStore.createIndex('name', 'name', { unique: true }) // 创建索引 可以让你搜索任意字段
      }
    }
  })
}
 
/**
 * 新增数据
 */
export function addData(db, storeName, data) {
  return new Promise((resolve, reject) => {
    let request = db.transaction([storeName], 'readwrite') // 事务对象 指定表格名称和操作模式（"只读"或"读写"）
      .objectStore(storeName) // 仓库对象
      .add(data)
 
    request.onsuccess = function(event) {
      resolve(event)
    }
 
    request.onerror = function(event) {
      throw new Error(event.target.error)
      reject(event)
    }
  })
}
......
export default {
  openDB,
  addData,
  getDataByKey,
  cursorGetData,
  getDataByIndex,
  cursorGetDataByIndex,
  updateDB,
  deleteDB,
  deleteDBAll,
  closeDB
}

// 使用
import IndexDB from './IndexDB.js'
(async function() {
  const dbName = 'myDB', storeName = 'db_1'
  const db = await IndexDB.openDB(dbName, storeName, 1)
  var data = await IndexDB.addData(db, storeName, {
    id: 111, // 必须且值唯一
    name: '张三',
    age: 18,
    desc: 'helloWord'
  })
  console.log(data)
}
```

![image](/images/indexedDB.jpg)

### 浏览器常用存储方式有哪些？

+ cookie：本身用于浏览器和server通讯，由服务器发送给浏览器存储，并且每次浏览器向同一服务器发送请求时，都会携带这些Cookies。本身存储大小有限（通常为4KB），并且可以设置过期时间。通常用来存储用户登录信息，个性化设置等。
+ localStorage：用于持久化的本地存储，除非主动删除数据，否则数据是永远不会过期的。以key-value的形式存储，存储数据以字符串形式存在，大小通常为5M或更大。适用于存储大量持久数据。
+ sessionStorage：用于本地存储，数据在当前浏览器窗口或标签关闭后自动删除。适用于存储临时数据只在当前会话中保存。
+ IndexedDB：IndexedDB 是一个事务型数据库，用于在客户端存储大量结构化数据。
+ Web SQL：一种使用 SQL 的数据库，用于在浏览器中存储数据。

### 浏览器cookie和sessionStorage/localStorage的区别

+ 存储大小：cookie数据大小不能超过4K。sessionStorage和localStorage可以达到5M或更大。
+ 有效时间：localStorage存储持久数据，浏览器关闭后数据不丢失除非主动删除数据；sessionStorage数据在当前浏览器窗口关闭后自动删除。cookie设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭。
+ 存储位置：cookie数据存储在浏览器中；sessionStorage和localStorage数据存储在浏览器内存中；如果浏览器配置为默认不保存数据，那么数据将会丢失。
+ 数据共享：cookie、session和localStorage都遵循同源原则进行数据共享。cookie和localStorage在所有同源窗口中都是共享的；sessionStorage在不同的浏览器窗口中不共享。