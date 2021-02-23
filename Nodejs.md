# Nodejs

## 核心API

全局对象 `global`

```js
setTimeout
setInterval
setImmediate
console
__dirname
__filename
Buffer
process
```

## 基本内置模块

### OS

```js
os.EOL  //end-of-line, 换行符
os.arch() // process.arch, CPU的架构名
os.cpus() // cpu信息
os.freemem() // 还有多少内存可用
os.homedir() // 用户的 home 路径
os.hostname() // 用户电脑的主机名
```

### path

```js
path.basename('./file/image/video/good.mp4') //得到包含扩展名的文件名-->good.mp4  获取到和第二个参数（扩展名）匹配上的文件名，如果没有匹配上，返回包含扩展名的文件名
path.sep  // 获取当前操作系统的分割符 windows 是"\"
path.delimiter  //当前操作系统的文件断句方式 linux 是“:” windows是“;” console.log(process.env.PATH.split(path.delimiter))
path.dirname('./file/image/video/good.mp4') //返回目录 file/image/video
path.extname('a/b/c/a.js') //返回扩展名 .js 如果没有扩展名，返回空字符串
path.join('a','b', '..', index.js) // 返回一个组合成的路径
path.normalize() //把传入的路径生成一个规范化路径
path.relative()  // 返回一个第二个路径相对于第一个路径的相对路径
path.resolve() //以相对于 node 运行环境或指定传入的第一个参数 把后面的路径生成一个绝对路径
```

### url

```js
const URL = require('url');
const url = new URL.URL('http://xxx.com:80/a/b/c?a=1&b=3') // const url = URL.parse('http://xxx.com:80/a/b/c?a=1&b=3')
const url = URL.format({
  protocol: 'https:',
  slashes: true,
  auth: null,
  host: 'pic.cardder.com',
  port: null,
  hostname: 'pic.cardder.com',
  hash: null,
  search: '?a=1&b=2',
  query: 'a=1&b=2',
  pathname: '/article/2523',
  path: '/article/2523?a=1&b=2',
  href: 'https://pic.cardder.com/article/2523?a=1&b=2'
}) // 将一个对象转换成一个 url--->'https://pic.cardder.com/article/2523?a=1&b=2'
```

### util

```js
util.callbackify(fn) //将一个异步函数转换成一个回调函数返回
util.promisify(fn) //将一个回调函数转换成异步函数返回
util.isDeepStrictEqual(obj1,obj2) //深度比较两个对象是否相同
```

###  fs

```js
const fs = require('fs');
const path = require('path');
fs.readFile(path.resolve(__dirname,'a/b/c'),(error,content) => {
  
})
```













