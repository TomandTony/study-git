# Node.js第二天笔记

# 1.复习

## 1.1 概念
- Node.js是JavaScript吗？
- Node.js基于哪个JavaScript引擎？
- Node.js的特性？
- Node.js只能运行在Windows上吗？
- Chrome和Node对比
  + Chrome的结构
  + Node的结构

## 1.2 nvm常用命令
- nvm项目地址：[nvm-github地址](https://github.com/coreybutler/nvm-windows)
- 查看所有已安装的node版本
``` nvm list ```
- 切换node版本
``` nvm use 版本号 ```
- 安装指定版本的node
``` nvm install 版本号 ```
- 卸载已安装的指定版本的node
``` nvm uninstall 版本号 ```

## 1.3 cmd
- cd（change directory）切换目录
- md（make directory）新建目录
- rd（remove directory）删除非空目录
- dir（directory）查看目录中的条目
    ls  在powershell 中对于所有的系统都是可以的
    但是  在cmd中  ls  却不可以原因是 unix系统中才会解析
- ren（rename）重命名文件
- del（delete）删除文件
- cls（clear screen）清屏

## 1.4 path 环境变量
- path环境变量的作用是什么？
- 在Windows中配置path环境变量的格式是什么？

## 1.5 REPL运行环境
- 作用：
- 进入REPL运行环境
- 退出REPL运行环境

## 1.6 Node.js基础知识
- 如何让Node.js执行一个js文件


## 1.6 global
- __dirname和__filename
- setTimeout和clearTimeout
- setInterval和clearInterval
- console
  time()
  作用是   计算一段代码的运行时间
    参数是字符串，随便定义  ，但是必需一致
      例如形式 console.log()
        console.time()
         代码要写在来年；两个函数之间
           单位都是毫秒级
           ++
               console.time("timer");
           	     for(var i=0;i<100000000;i++){};
           	     //要执行的代码
           	   console.timeEnd("timer");
              ///结果是 timer:135ms
           ++
        console.timeEnd()

## 1.7 模块系统
- 什么是模块
- 模块作用域
- require
- exports
- module.exports
- exports和module.exports
```JavaScript
function Person(name) {
  this.name = name;
}

var p1 = new Person('Jack');
var p2 = p1;

console.log(p1.name);
console.log(p2.name);

p2 = {
  name:'shit'
};

console.log(p1.name);
console.log(p2.name);

```

## 1.8 作业：模块计算器

# 2.模块系统

在Node.js中，模块分为两类：
一种是Node提供的模块，称为`核心模块`，
 

一种是用户编写的模块，称为`文件模块`



## 2.1 优先从缓存加载
  指的是多次加载相同的文件模块，那么只输出一次，加载之后文件会存在
  内存中，那么第二次加载的时候就会直接从内存中读取文件
## 2.2 require参数解析
- 核心模块
  + fs
  + http
  + os
  + path
  + querystring
  + url
- ./或../开始的相对路径文件模块
- 以/开始的绝对路径文件模块
    ++++在Linux或者MAc的操作系统中，/表示系统的根路径
         * 在Windows中，/表示当前文件模块所属的根磁盘路径
   ++不推荐使用，表示磁盘根目录从最底层开始（window中）
      在其他系统中那么就只是从根目录开始
      
---- 标识符的运用（请求的文件名称）
可以不包含后缀名，node会一次次按照js，node，json 依次补全，依次尝试
   如果是json后缀名 最好加上，这样首位提高加载的速度
    最后也是通过fs读取文件的形式 json.parse 解析字符串成一个json对象
- 标识符中可以不包含扩展名
  + Node会按照`.js、.node、.json`的次序补足扩展名，依次尝试
     一般杂家在js文件是 可以省略js后缀，为了提高加载的效率那么在在加载文件时
      应该补齐后置名称以以保证不会在循环遍历后缀名，节省时间加快运行效率
           json文件 会先执行fs 读取文件获得字符串，然后解析json.parse解析字符串，输出json对象

  #2. process
     ++++process 就表示当前正在执行的nodejs应用程序退出之后
       就没有了
     
      】  那么这是一个全局对象，不需要加载，对于这个对象有一个用法
        process.argv  得到的是一个数组，前两个元素分别别是 node.exe 文件爱你的绝对鲁晶晶
        一个是node 后面的具体的文件爱你的绝对路径，
        如果在命令框中 输入  node index.js "ip"
         那么就会被添加到数组中的最后一项，作为一个完整的数组被输出
  #2.3
   +++ 如果加载的文件内容与核心内容的名字一样，那么，只会识别核心模块，而忽略文件名称，
       原因是核心内容的优先级高于普通的文件模块


# 3.包和npm

- 问题：你开的一个功能模块，还是以文件的形式散列的，你给别人使用的时候很麻烦。
- 包的作用：在模块的基础上进一步组织JavaScript代码
 ++++ （前提是必须联网）在命令行中 输入npm install 模块名称 
 那么就会自动连接在npm网站 （这些包都被托管在github上）
   网站上寻找github地址  连接，下载相应压缩包 然后再当前目录下找到一个node_module
   文件夹，再次文件夹下解压，如果没有那么就自动创建个
  npm install 执行的时候会自动寻找pakage.json里面有一个属性叫dependenscices
   里面包含了下载过的包，假设如果开发中node_module 
   文件夹被删那么我们也能够知道有哪些文件依赖的包，还有就是可以发送这个只要连网就能联网
   下载只需一行命令就可以了，方便快捷 直接输入npm install 就会直接 安装其中的所有包
   npm install --save 包名  自动保存到packagejson文件的dependencices 中，那么
   npm 网站托管的项目包都在github上
  +++ nrm切换镜像源  比如引用外部的包，  如 nrm add (name) http://..
     nrm use  baoname 
     是用过之后在切换回来
     npm install baoming --save

## 3.1 包的组成
- 包结构
- 包描述文件package.json
描述当包的的一些相关信息

## 3.2 package.json
package.json文件内部就是一个JSON对象，该对象的每一个成员就是当前项目的一项设置，
比如name就是项目名称，version就是项目的版本号

## 3.3


  /*
 * require的参数中，还有一种特殊的加载方式：

    文件模块 加载机制，寻找路径 （找到目录 ，然后）


 *  既不是文件模块（加路径），也不是核心模块
 *  而是一种特殊的自定义模块，这种模块就叫做包
 *  Node.js中，有一种默认的路径加载规则，那么这个规则就是module.paths中的目录规则
 *  Node.js根据module.paths中的第一个：c:\\Users\\iroc\\Desktop\\code\\node_modules拼接cal
 *    c:\\Users\\iroc\\Desktop\\code\\node_modules\cal
 *      在该目录下找一个叫做package.json的文件
 *          如果找到：通过JSON.parse的方式拿到该对象，
 *            获取main属性，如果main属性中的值可以拼接为一个完整的路径并且是正确的
 *             直接加载该模块，拿到module.exports
 *          如果找不到package.json文件或者找到了但是里面没有main属性或者main属性的值是错误的
 *
 *          index.js
 *          index.node
 *          index.json
 *   如果在当前目录下的node_modules目录下找不到该模块
 *   按照module.paths中的路径规则，逐级查找，重复上面的步骤
 *    如果最后找到了最后一个路径还是找不到，报错
 * */
  //  console.log(module.paths); 
    var obj= require("val");
    console.log(obj);

---- 理解线程与进程
++


适合有大量IO操作，比如：读文件，处理用户请求
在异步操作中，最好不要写有大量计算的代码。也就是消耗CPU的代码
  获取文件的时候有一个异步阻塞操作  fs.readFileSync("")

// 读取文件肯定是需要时间，取决于文件的大小
// 同步操作也就意味着，只有当前的代码执行完毕之后，后面的代码才会继续执行
// 这种方式其实也就是阻塞式的操作，也叫同步IO阻塞
fs.readFileSync('1');



---- 异步IO模型 与非异步IO模型  代码执行顺序
++ 那么对于图的理解 应为js是单线程的因此在执行事件的时候，一个（主线程）线程
只能做一件件事，如果有许多的事件，那么在node中就会讲所有的事件放如一个事件队列中，
对于主线程来说会按照事件队列的顺序依次调出事件执行
++对于所有的异步，那么就会放在事件队列里面，因为他们不阻碍后面代码的执行，
  那么 在事件循环是首先判断是不是iO模型，是的话直接扔到线程池中，然后循环循环，
  直到不存在io代码，就是异步代码

1.如果是异步的IO模型，那么就会将其放入线程池的线程中（这是提前准备好的线程为的是
  解放主线程不让主线程的cpu因为等待而白白浪费执行后获取回调函数，返回事件队列，由事件循环
  再依次获取事件，直到不存在IO模型，为止，那么就直接由回调函数返回响应）
 2.如果是普通代码那么就会在异步代码之前执行，对于大量的计算代码，那么就会阻塞异步IO模型的执行
 3.如果在异步代码之中有大量的计算那么就会阻塞异步代码的执行，因此nodejs不适合大量计算的代码执行

 --nodejs  中的api是使用时所有的回调函数的参数（第一个参数必须为err  回调函数必须放在最后）
 另外---对于抛出错误  try  catch  无法获得错误

++++
对于异步操作，应该就是现有事件循环解析，但是不执行其中的代码
只是将含有数据的回调函数立即返回，放进事件队列，等到事件循环
再次解析到时，那么才会执行其中的的代码，（事件循环也就是在一部代码中没有IO操作时
就会结束循环，然后执行同步代码，将数据结果返回）；
  例如 settimeout 在与同步代码运行时，会直接接跳到同步代码，等到打印后才会执行
  那么在执行到其后辉县解析外面的壳子然后  解析其中的同步代码
 真异步互不影响，

 ++模版字符串
` `其中可以传入一些字符串，  ${ 这里可以放入许字符串，也可以使函数表达式}
  其实相当于pre  预格式化标签 。输入什么样字那么就输出什么样子

    var str= 例如  querySelectorAll();
    转化伪数组的新方法，  Array.from(str).foreach(function(item,index){})
    Array.prototype.slice.call();
+++对于箭头函数
 注意的小细节：（）=>｛｝，this的 用法，不是指向调用的对象，而是指向定义的对象

 ++  d对于request 访问的一些看法
  1. 在npm中 npm init 命令
  2.添加到 packagejson  文件中
  3.包在下载的时候会自动添加到 node_modules中（）这里是自己添加的
  4.request  中的url 先用一个软件
=====06========  
  +++
   注意一些内容
     客户端（浏览器）与服务器
    实际上两者之间的通信是通过二进制数据来完成的，两者实际上就是两个计算机之间的通信
    那么对于浏览器，服务器  都是软件 软件之间的交流也是二进制数据的交流
    ++端口号的讲解
    -- 我们通过 域名访问，经过DNS域名解析器的解析将其转换为IP地址，然后找到了服务器的电脑
     那么要想网成请求与访问，就需要与服务器中的软件进行交流，也就是通过某个端口号进行数据的交互
    ---另外两个计算机一旦进行HTTP协议传输数据，那么就必然形成一个客户端一个服务器
   另外监听端口默认的就是8080
     webstorm 乱码===》
   ctrl +alt+ s ==>apperance =>override 选项取消打勾就能改回来了
   
  ++对于http请求的一些看法
   -- 在客户端向服务器发送请求的时候，会先建立连接，那么连接存在一个“三次握手原则”
     第一步：客户端向服务器发送消息，为了确定是否收到那么需要服务器返回一个消息
     第二部：服务器向客户端发送消息，为了确定是否收到那么需要客户端返回一个消息
     第三部：服务器向客户端发送消息，为了确定是否收到那么需要客户端返回一个消息
   
   +++ 对于模版那引擎
    ———— 配置完 pakagejson之后 查看那么有一个cat 命令
      ——express框架模版 route 路由的分发响应 对不同的用户请求进行不同的响应
       
  
  

  
















