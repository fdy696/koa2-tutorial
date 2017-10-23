# log 日志中间件 
> 最困难的事情就是认识自己。

<br/> 

在一个真实的项目中，开发只是整个投入的一小部分，版本迭代和后期维护占了极其重要的部分。项目上线运转起来之后，我们如何知道项目运转的状态呢？如何发现线上存在的问题，及时进行补救呢？记录日志就是解决困扰的关键方案。正如我们每天写日记一样，不仅能够记录每天都做了什么，便于日后回顾，也可以将做错的事情记录，进行自我反省。完善的日志记录不仅能够还原问题场景，还有助于统计访问数据，分析用户行为。 

<br/> 

## 日志的作用 

* 显示程序运行状态
* 帮助开发者排除问题故障
* 结合专业的日志分析工具（如 ELK ）给出预警 

<br/> 

## 关于编写 log 中间件的预备知识 

<br/> 

### log4js 

本项目中的 `log` 中间件是基于 `log4js 2.x` 的封装，[Log4js](https://github.com/nomiddlename/log4js-node) 是 `Node.js` 中记录日志成熟的第三方模块，下文也会根据中间件的使用介绍一些 `log4js` 的使用方法。 

<br/> 

### 日志分类 

日志可以大体上分为访问日志和应用日志。访问日志一般记录客户端对项目的访问，主要是 `http` 请求。这些数据属于运营数据，也可以反过来帮助改进和提升网站；应用日志是项目中需要特殊标记和记录的位置打印的日志，包括出现异常的情况，方便开发人员查询项目的运行状态和定位 `bug` ，应用日志包含了`debug`，`info`，`warn`，`error`等级别的日志。 

<br/>

### 日志等级

`log4js` 中的日志输出可分为如下7个等级：

![LOG_LEVEL.957353bf.png](http://upload-images.jianshu.io/upload_images/3860275-7e8db4f9d1aed430.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 

<br/>

在应用中按照级别记录了日志之后，可以按照指定级别输出高于指定级别的日志。 

<br/> 

### 日志切割 

当我们的项目在线上环境稳定运行后，访问量会越来越大，日志文件也会越来越大。日益增大的文件对查看和跟踪问题带来了诸多不便，同时增大了服务器的压力。虽然可以按照类型将日志分为两个文件，但并不会有太大的改善。所以我们按照日期将日志文件进行分割。比如：今天将日志输出到 `task-2017-10-16.log` 文件，明天会输出到 `task-2017-10-17.log` 文件。减小单个文件的大小不仅方便开发人员按照日期排查问题，还方便对日志文件进行迁移。

<br/> 

## 如何实现 

<br/> 

### 初始化日志 

1. 安装 `log4js` 模块 

```js
npm i log4js -S
``` 

<br/>

2. `log4js` 官方文档示例

```js
const log4js = require('log4js');
log4js.configure({
  appenders: { cheese: { type: 'file', filename: 'cheese.log' } },
  categories: { default: { appenders: ['cheese'], level: 'error' } }
});

const logger = log4js.getLogger('cheese');
logger.trace('Entering cheese testing');
logger.debug('Got cheese.');
logger.info('Cheese is Gouda.');
logger.warn('Cheese is quite smelly.');
logger.error('Cheese is too ripe!');
logger.fatal('Cheese was breeding ground for listeria.');
``` 

<br/> 

上述代码中，`configure` 函数配置了日志的基本信息 

```js
{
  /**
   * 指定要记录的日志叫作 cheese
   * 展示方式为文件类型 file
   * 展示的文件名 cheese.log
   */
  appenders: { cheese: { type: 'file', filename: 'cheese.log' } },

  /**
   * 指定日志的默认配置项
   * 如果 log4js.getLogger 中没有指定，默认为 cheese 日志的配置项
   * 指定 cheese 日志的记录内容为 error 及 error 以上级别的信息
   */
  categories: { default: { appenders: ['cheese'], level: 'error' } }
}
``` 

<br/> 

在上述代码的基础上，我们只需要提取出公用的配置项，并封装一些函数方式，比如在上下文中挂载 `log` 函数，基本上就能实现项目中的日志功能。 

<br/> 

## 提取日志中间件 












