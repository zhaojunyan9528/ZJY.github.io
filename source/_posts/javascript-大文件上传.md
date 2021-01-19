---
title: javascript-大文件上传
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-01-19 23:05:01
---

整体思路：

+ 前端：
前端大文件上传网上的大部分文章已经给出了解决方案，核心是利用 Blob.prototype.slice 方法，和数组的 slice 方法相似，调用的 slice 方法可以返回原文件的某个切片
这样我们就可以根据预先设置好的切片最大数量将文件切分为一个个切片，然后借助 http 的可并发性，同时上传多个切片，这样从原本传一个大文件，变成了同时传多个小的文件切片，可以大大减少上传时间
另外由于是并发，传输到服务端的顺序可能会发生变化，所以我们还需要给每个切片记录顺序
+ 服务端：
服务端需要负责接受这些切片，并在接收到所有切片后合并切片

上传控件：

```html
    <template>
        <div>
            <input type="file" @change="handleFileChange" />
            <el-button @click="handleUpload">上传</el-button>
        </div>
    </template>

    <script>
    export default {
        data: () => ({
            container: {
            file: null
            }
    }),
    methods: {
        async handleFileChange(e) {
            const [file] = e.target.files;
            if (!file) return;
            Object.assign(this.$data, this.$options.data());
            this.container.file = file;
            },
            async handleUpload() {}
        }
    };
    </script>
```

请求逻辑：

```javascript
    request({
        url,
        method = 'post',
        data,
        headers={},
        requestList
    }) {
        return new Promise(resolve=>{
            const xhr = new XMLHttpRequest();
            xhr.open(method,url);
            Object.keys(headers).forEach(key=>{
                xhr.setRequestHeader(key, headers[key])
            });
            xhr.send(data),
            xhr.onload = e=>{
                resolve({
                    data: e.target.response
                })
            }
        })
    }
```

上传切片：

上传需要做两件事：
1.对文件进行切片
2.将切片传给服务器

```javascript
    <script>
     const LENGTH = 10; // 切片数量

    export default {
    data: () => ({
        container: {
            file: null,
             data: []
        }
    }),
    methods: {
        request() {},
        async handleFileChange() {},
        // 生成文件切片
        createFileChunk(file, length= LENGTH) {
            const fileChunkList = [];
            const chunkSize = Math.ceil(file.size / length);//切片大小
            let cur = 0;
            whilte(cur < file.size) {
                fileChunkList.push({file:file.slice(cur,curchunkSize)});
                cur=chunkSize;
            }
            return fileChunkList;
        }
       // 上传切片
        async uploadChunks(){
            const requestList = this.data.map(({chunk})=>{
                const formData = new formData();
                formData.append("chunk",chunk);
                formData.append("hash",hash);
                formData.append("filename",this.container.file.name);
                return {
                    formData;
                }
            }).map(async ({formData})=>{
                this.request({
                    url:'',
                    data: formData
                })
            });
            await Promise.all(requestList);//并发切片
        },
        async handleUpload() {
          if(!this.container.file) return;
            const fileChunkList = this.createFileChunk(this.container.file);
            this.data = fileChunkList.map(({file},index)=>{
                chunk: file,
                hash: this.container.file.name  '-' index, //文件名数组下标
            });
            await this.uploadChunks();
        }
    }
    };
    </script>
```

当点击上传按钮时，调用 createFileChunk 将文件切片，切片数量通过一个常量 Length 控制，这里设置为 10，即将文件分成 10 个切片上传
createFileChunk 内使用 while 循环和 slice 方法将切片放入 fileChunkList 数组中返回
在生成文件切片时，需要给每个切片一个标识作为 hash，这里暂时使用文件名  下标，这样后端可以知道当前切片是第几个切片，用于之后的合并切片
随后调用 uploadChunks 上传所有的文件切片，将文件切片，切片 hash，以及文件名放入 FormData 中，再调用上一步的 request 函数返回一个 proimise，最后调用 Promise.all 并发上传所有的切片

发送合并请求：

```javascript
    await Promise.all(requestList);
    // 合并切片
   await this.mergeRequest();
    async mergeRequest() {
        await this.request({
            url: '',
            headers: {
                'content-type': 'application/json',
            },
            data: JSON.stringify({
                filename: this.container.file.name
            })
        })
    }
```

服务端部分：

```javascript
    const http = require('http');
    const server = http.createServer();

    server.on("request", async(req, res)=>{
        res.retHeader("Access-Control-Allow-Origin","*");
        res.setHeader("Access-Control-Allow-Headers","*");
        if(res.method == 'OPTIONS') {
            res.status = 200;
            res.end();
            return;
        }
    });
    server.listen(3000, ()=> console.log("listening on port:3000"));
```

接受切片：

使用multiparty包处理前端传来的formdata,在 multiparty.parse 的回调中，files 参数保存了 FormData 中文件，

```javascript
    fields 参数保存了 FormData 中非文件的字段.
    const http = require("http");
    const path = require("path");
    const fse = require("fs-extra");
    const multiparty = require("multiparty");

    const server = http.createServer();
     const UPLOAD_DIR = path.resolve(__dirname, "..", "target"); // 大文件存储目录

    server.on("request", async (req, res) => {
    res.setHeader("Access-Control-Allow-Origin", "*");
    res.setHeader("Access-Control-Allow-Headers", "*");
    if (req.method === "OPTIONS") {
        res.status = 200;
        res.end();
        return;
    }

      const multipart = new multiparty.Form();

      multipart.parse(req, async (err, fields, files) => {
        if (err) {
          return;
        }
        const [chunk] = files.chunk;
        const [hash] = fields.hash;
        const [filename] = fields.filename;
        const chunkDir = `${UPLOAD_DIR}/${filename}`;

       // 切片目录不存在，创建切片目录
        if (!fse.existsSync(chunkDir)) {
          await fse.mkdirs(chunkDir);
        }

        // 重命名文件
        await fse.rename(chunk.path, `${chunkDir}/${hash}`);
        res.end("received file chunk");
      });
    });

    server.listen(3000, () => console.log("正在监听 3000 端口"));
```

合并切片：

```javascript
    const http = require("http");
    const path = require("path");
    const fse = require("fs-extra");

    const server = http.createServer();
    const UPLOAD_DIR = path.resolve(__dirname, "..", "target"); // 大文件存储目录

     const resolvePost = req =>
       new Promise(resolve => {
         let chunk = "";
         req.on("data", data => {
           chunk = data;
         });
         req.on("end", () => {
           resolve(JSON.parse(chunk));
         });
       });

     // 合并切片
     const mergeFileChunk = async (filePath, filename) => {
       const chunkDir = `${UPLOAD_DIR}/${filename}`;
       const chunkPaths = await fse.readdir(chunkDir);
       await fse.writeFile(filePath, "");
       chunkPaths.forEach(chunkPath => {
         fse.appendFileSync(filePath, fse.readFileSync(`${chunkDir}/${chunkPath}`));
         fse.unlinkSync(`${chunkDir}/${chunkPath}`);
       });
       fse.rmdirSync(chunkDir); // 合并后删除保存切片的目录
     };

    server.on("request", async (req, res) => {
    res.setHeader("Access-Control-Allow-Origin", "*");
    res.setHeader("Access-Control-Allow-Headers", "*");
    if (req.method === "OPTIONS") {
        res.status = 200;
        res.end();
        return;
    }

       if (req.url === "/merge") {
         const data = await resolvePost(req);
         const { filename } = data;
         const filePath = `${UPLOAD_DIR}/${filename}`;
         await mergeFileChunk(filePath, filename);
         res.end(
           JSON.stringify({
             code: 0,
             message: "file merged success"
           })
         );
       }

    });

    server.listen(3000, () => console.log("正在监听 3000 端口"));
```

总结：
前端上传大文件时使用blob.prototype.slice将文件切片，并发上传多个切片，最后发送一个合并的请求通知服务端合并切片。
服务端接收切片并存储，收到合并请求后使用fs.appendFileSync对多个切片进行合并
原生XMLHttpRequest的upload.onpropgress对切片上传进度的监听
使用vue计算属性根据每个切片的进度算出整个文件的上传进度