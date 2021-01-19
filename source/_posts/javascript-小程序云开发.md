---
title: javascript-小程序云开发
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-01-19 22:53:12
---

1.云开发：

云开发是原生Serverless云服务。
云开发与传统模式相比少了后端开发这一块。

2.云开发能力：

存储
云函数
云数据库
音视频服务
智能图像服务
一天多交付，快速迭代产品

3.初始化

```javascript
    wx.cloud.init({
        // env 参数说明：
        //   env 参数决定接下来小程序发起的云开发调用（wx.cloud.xxx）会默认请求到哪个云环境的资源
        //   此处请填入环境 ID, 环境 ID 可打开云控制台查看
        //   如不填则使用默认环境（第一个创建的环境）
        env: '';
        traceUser: true, //是否在云开发控制台可见用户信息
    })
```

4.数据库

```javascript
    const db = wx.cloud.database();
    const _ = db.command
    db.collection('集合名').get().then(res=>{})
```

1.get()查询
2.where条件：

```javascript
    counter不等于1  3  4的记录
    where({
        counter: _.nin([1,3,4])
    })
    counter等于1  3  4的记录
    where({
        counter: _.in([1,3,4])
    })
```

3.字段类型查询

```javascript
    db.collection("data")
    .field({
        desc: true
    })
    .get().then(console.log)
    查询所有数据但只显示有desc字段的内容，例如：
    0: {_id: "dbbb40a0-a4e0-479d-8432-926d05b002a8"}
    1: {_id: "37aa3ea0-db49-4c12-849f-34e5342966da", desc: "name2"}
    2: {_id: "1d6ff516-5714-4970-9a28-a0f8b3f7a3e6"}
```

4.正则查询
可以使用原生的正则对象：
    /miniprogram/i
db.RegExp对象

```javascript
    db.RegExp({
        regexp: 'miniprogram',
        options: 'i'
    })
```

查询name值以name-0开头的数据

```javascript
    db.collection("data")
    .where({
        name: new db.RegExp({
            regexp: 'name-0[1-9]',
            options: 'i'
        })
    })
    .get().then(console.log)
0: {_id: "dbbb40a0-a4e0-479d-8432-926d05b002a8", counter: 1, name: "name-01"}
1: {_id: "37aa3ea0-db49-4c12-849f-34e5342966da", counter: 2, name: "name-02"}
```

5.地理位置查询

添加：

```javascript
    db.collection('location').add({
        data: {
            location: db.Geo.Point(100.2333,10.9978)
        }
    })
```

查询：

```javascript
    db.collection('location').get().then( res=>{
        console.log(res.data[0].location)
    })
```

6.云开发数据库权限：

1.仅创建者可写，所有人可读
2.仅创建者可读写
3.仅管理端可写，所有人可读
4.仅管理端可读写

存储：

可使用fileid引用存储的资源

生成文件的临时链接：

为什么生成临时链接： fileid无法在小程序以外的平台使用
如何生成：

```javascript
    wx.cloud.getTempFileURL({
        fileList: ['cloud://zjy20200115-8v2oc.7a6a-zjy20200115-8v2oc-1258009129/oL40f5Rxp9wplmcEtUO3Av-4Yr9M/0.5019968125178349_1579073268192.jpg'],//fileid
        success:res=>{
            console.log(res.fileList[0].tempFileURL)
        }
    })
```

使用云函数定时器：

1.在云函数目录下创建config.json文件，并设置触发器
2.上传触发器
步骤：
    1.创建云函数trigger
    2.上传云函数
    3.云函数目录下trigger创建config.json文件：添加

```javascript
        {
            "triggers": [
                {
                    "name": "trigger", //触发器名字
                    "type": "timer", //类型：定时器
                    "config": "* * * * * * *" //触发事件配置，七位从右到左代表：年 星期 月 日 时 分 秒 ；七个*代表每秒钟执行一次
                }
            ]
        }
    ```

4.上传触发器
5.在云开发控制台-云函数-日志可查看

获取集合中指定记录的引用

db.collection('todos').doc('my-todo-id')
方法接受一个 id 参数，指定需引用的记录的 _id

limit(number):指定查询结果集数量上限:

```javascript
    db.collection('todos').limit(10)
    .get()
    .then(console.log)
    .catch(console.error)
```

skip(offset:number): 指定查询返回结果时从指定序列后的结果开始返回，常用于分页

```javascript
    db.collection('todos').skip(10)
    .get()
    .then(console.log)
    .catch(console.error)
```

云上传：

```javascript
    wx.chooseImage({
        success: res=>{
            wx.cloud.uploadFile({
                cloudPath: 'images/xx.png',
                filePath: res.tempFilePaths[0],
            }).then(res=>{
                console.log(res.fileID)
            })
        }
    })
```

定位：

```javascript
    wx.chooseLocation({
        success: function(res) {
            console.log(res)
            that.setData({
                location:{
                    name: res.name,
                    latitude: res.latitude,
                    longitude: res.longitude,
                    address: res.address
                }
            })
        },
    })
```

查看定位：

```javascript
    wx.openLocation({
        latitude: this.data.todoInfo.location.latitude,
        longitude: this.data.todoInfo.location.longitude,
        name: this.data.todoInfo.location.name,
        address: this.data.todoInfo.location.address   
    })
```

report-submit： true,给表单此属性设置为true,会返回formId,用于发送模板消息

云函数查询数据：

新建云函数：query：

```javascript
    const cloud = require('wx-server-sdk')
    cloud.init()
    const db = cloud.database()
    // 云函数入口函数
    exports.main = async (event, context) => {
        return await db.collection('todos').get();
    }
    页面查询触发：
    wx.cloud.callFunction({
        name: 'query',
    }).then(console.log)

    添加： add({data:{}})
    删除： remove()
    编辑： update({
        data:{}
    })
```

在云函数中使用存储资源：

1.上传文件： uploadFile

```javascript
    exports.main = async (event, context) => {
        const fileStream = fs.createReadStream(path.join(__dirname, 'demo.jpg'))
        return await cloud.uploadFile({
            cloudPath: 'demo.jpg',
            fileContent: fileStream,
        })
    }
```

2.下载文件： downloadFile
3.获取临时文件链接： getTempFileURL
4.删除文件： deleteFile

云函数访问第三方服务器:

1.安装got （版本9.6.0，10以上版本调用失败）
2.开启npm 模块
3.云函数修改后需重新上传

云函数访问数据库：

1.安装mysql2: npm i mysql2
2.引入： const mysql2 = require('mysql2');
3.连接：
    const connection = await mysql.createConnection({
        host: '192.168.3.66',
        port: '3306',
        user: 'root',
        database: 'sample',
        password: '123456'
    })
    await connection.execute("SELECT version();")


云函数中生成小程序二维码：

1.引用安装wx-js-utils：npm install wx-js-utils
2.获取token和小程序id等信息：

```javascript
    const {
        WXMINIUser,
        WXMINIQR
    } = require('wx-js-utils');

    const appId = ''; // 小程序 appId
    const secret = ''; // 小程序 secret

    // 获取小程序码，A接口
    let wXMINIUser = new WXMINIUser({
        appId,
        secret
    });

    // 一般需要先获取 access_token
    let access_token = await wXMINIUser.getAccessToken();
    let wXMINIQR = new WXMINIQR();
```

3.获取二维码api: getQR

```javascript
    options: {
        access_token,
        path,//
        width
    }
    示例：
    let qrResult = await wXMINIQR.getQR({
        access_token,
        path: 'pages/index/index'
    });
    上传：
    wx.cloud.uploadFile({bn 
        cloudPath: 'qr/qr.png',
        fileConent: qrResult
    })
```

腾讯云短信sdk: qcloudsms_js

npm i qcloudsms_js

微信支付： node tenpay

npm i tenpay

