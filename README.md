# yueche
海淀驾校约车软件

## 为什么要写这个软件呢？
   2014年7月份报考的海淀驾校，因为我的周六日时间不固定，所以报的是需要自己约车的班型，顺利考完科目一之后，结果连续两周在网上约车，都约不到，好气啊。嗯，这就是我为啥要写这个软件的原因，而且通过自己的技术能够约到车的感觉也是蛮好的。
   
## 前期准备、大概思路
   于是熟悉了一下网上约车流程，看到了一个拦路虎：验证码，刚好当时做的产品里面用到了OCR（Optical character recognition）技术[tesseract](https://github.com/tesseract-ocr/tesseract)，可以识别简单的验证码，类似这样的![海驾的登陆验证码](http://haijia.bjxueche.net/tools/CreateCode.ashx?key=ImgCode&random=0.4004024330433442)
，都是字母和数字的组合，识别率还不错，基本可以达到80%，对这个软件来说已经足够了；另外这个网站用的.net写的，如果熟悉.net开发的话，有一些简单表单验证机制，这也不难，通过[jsoup](https://github.com/jhy/jsoup)来解析页面信息，最后跟着表单一起提交就可以了。表单的具体信息通过[fiddler2](https://www.telerik.com/download/fiddler/fiddler2)来看，很方便。当然，还有一些运气，因为我发现了一个很有用的地方（这个在程序里能够体现出来），帮助我大大的提高了约车的成功率；处理http请求用的[HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html)，后来发现jsoup也可以处理请求。还有一个定时，约车是从每天早上7点35分开始，到晚上10点结束，定时器用的[quartz](https://github.com/quartz-scheduler/quartz)；还用到了一些jdk内部的东西，线程池等等。

## 目前软件的进度
 代码目前还没有上传到github，最开始的一个版本是通过在IDEA里面起一个main方法来约车的， 这当然我自己用是可以的。不过后来我发现好多人也遇到了这个问题，然后又更新了一个版本，可以提交约车订单，来给不同的人来约车，而且有了web界面，不过只能我一个人登陆上去，订单由我自己来提交，并给还给tomcat启用了https；而且有一段时间阿里云打折的时候，我买了一个ECS，部署上去了。帮助几个人约到了车，这当然是收费的了，不过很便宜，不过也高兴了一阵子。但是本质上这个软件还是单机的，这样带来了问题，所有的请求都是通过一个ip地址出去的，刷车越频繁，越容易被封ip，但是不频繁，约车成功率会降低。不过我已经有了一个办法，就是让这个软件变成分布式的，其实就是通过下载一个客户端（android、windows、或者是web只要能在本地发起请求），然后由客户端来发起约车的请求，服务器端的作用是管理这些约车订单，发布任务就可以了。这样既不容易被封，而且也更容易抢到票。
 
## 后续 & 立flag
  当我再看这些代码的时候，感觉有些不够通用，不够灵活，有些瑕疵，所以暂时还不会上传到github，等我改到自己满意的时候，就会上传上来了。不过，这也好像给自己立了一个[flag](http://www.guokr.com/article/440659/)，哈哈。而且这个软件有点类似抢票软件，会存在一些争议，也给我心理上增加了负担。最后，当然是希望能够帮助到大家，也希望大家能够踊跃的pr，哈哈。
