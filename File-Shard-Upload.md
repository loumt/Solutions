Base Module : Vue Express async formidable

1.Vue Page

```
  Vue页面
        <div class="upload_box">
            <input type="file" @change="uploadFile"/>
            <div>{{process}}</div><div>{{uploadMessage}}</div>
        </div>
        
  Vue Data数据
        process: '0%'
        uploadMessage:''
  
 methods方法
        uploadFile(e) {
                let file = e.target.files[0]
                let {name, size} = file

                let success = 0;  //当前上传数
                let shardSize = 5 * 1024 * 1024;    //以2MB为一个分片
                let shardCount = Math.ceil(size / shardSize);  //总片数

                console.log(`上传的文件名字为 ： ${name}`)
                console.log(`上传的文件大小为 ： ${size}`)
                console.log('上传文件分片数：' + shardCount)

                let config = {
                    headers: {'Content-Type': 'multipart/form-data'}
                }
                /**
                 * 任务数
                 * @type {Array}
                 */
                var tasks = [];
                for (var i = 0; i < shardCount; ++i) {
                    tasks.push(i);
                }


                //console.log(attr)
                async.eachLimit(tasks, 1, (item, callback) => {
                    //计算上传的文件位置
                    let start = item * shardSize
                    let end = Math.min(size, start + shardSize);

                    let uploadParams = new FormData()
                    uploadParams.append("resources", file.slice(start, end));
                    uploadParams.append("size", size);
                    uploadParams.append("totalShard", shardCount);
                    uploadParams.append("name", name);
                    uploadParams.append("currentShard", item + 1);


                    this.$http.post('http://192.168.20.91:3100/upload', uploadParams, config).then(res => {
                        this.process = Math.round(++ success / shardCount * 100) + '%';
                        if(res.data.message){
                            this.uploadMessage = res.data.message
                        }

                        callback()
                    }).catch(e => {
                        callback(e)
                    })
                }, () => {
                    console.log('上传结束,等待合并')
                })
            }

```


2.Request API

```
"use strict";

const formidable = require('formidable')
const fs = require('fs')
const async = require('async')
const path = require('path')


/**
 * 文件上传成功
 * @param req {curPath:当前目录}
 * @param res
 */
exports.upload = (req, res, next) => {
    try {
        //创建上传表单
        var form = new formidable.IncomingForm();
        //设置编辑
        form.encoding = 'utf-8';
        //设置上传目录
        // form.uploadDir = curPath;
        form.uploadDir = 'F://fs-test'
        //保留后缀(不可更改)
        form.keepExtensions = true;
        //能申请到的内存值,非文件大小
        // form.maxFieldsSize = docConfig.maxSize;
        // form.maxFileSize = 1 * 1024 * 1024;

        form.on('progress', (bytesReceived, bytesExpected) => {
            // console.log(`${bytesExpected} => ${bytesReceived}`)
            //在这里判断接受到数据是否超过最大，超过截断接受流
        });

        form.parse(req, (err, fields, files) => {
            if (err) {
                console.log(err)
            }

            //获取文件名
            var {name,size,totalShard,currentShard} = fields
            //文件路径
            var p = files.resources.path;

            // console.log(` 文件名:${name} 总大小${size}  分片信息 ${currentShard}/${totalShard}  => ${p}`)
            fs.renameSync(p, path.join(path.dirname(p),[name,'SHARD',currentShard].join('.')));
            //当最后一片分片上传结束,合并所有分片
            if(currentShard === totalShard){

                let dirname = path.dirname(p)
                console.log(` 文件名:${name} 文件夹 ${dirname} 总大小${size}  分片数量 ${totalShard}`)

                let writeStream = fs.createWriteStream(path.join(dirname,name));

                let filePathList = []

                for(let i =1 ;i <= totalShard ; i ++){
                    filePathList.push(path.join(dirname,[name,'SHARD',i].join('.')))
                }
                async.eachLimit(filePathList,1,(filePath,callback)=>{

                    fs.readFile(filePath,(err,data)=>{
                        if(err){
                            console.log(err)
                        }
                        writeStream.write(data)
                        fs.unlink(filePath,()=>{
                            callback()
                        })
                    })

                },(err)=>{
                    if(err){
                        res.status(500).json({success:false,message:'合并文件错误,请稍后再试'})
                    }else{
                        writeStream.end();
                        res.status(200).json({success:true,message:'合并文件成功'})
                    }
                },50)
            }else{
                res.status(200).json({success:true})
            }
        })
    } catch (e) {
        res.status(500).json({success:false})
    }
}
```
