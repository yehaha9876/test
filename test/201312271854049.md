[pomelo源码分析(一)](/youyudehexie/article/details/8613862)
===========================================================

        千里之行始于足下，一直说想了解pomelo，对pomelo有兴趣，但一直迟迟没有去碰，虽然对pomelo进行源码分析，在网络上肯定不止我一个，已经有很优秀的前辈走在前面，如http://golanger.cn/，在阅读Pomelo代码的时候，已经连载到了11篇了，在我的源码分析参考了该博客，当然，也会加入我对pomelo的理解，借此希望能提高一下自己对node.js的了解和学习一些优秀的设计。

-   开发环境：win7
-   调试环境：webstorm5.0
-   node.js版本：v0.8.21
-   源码版本package.json：

~~~~ {.javascript name="code"}
{
    "name": "chatofpomelo",
    "version": "0.0.1",
    "private": false,
    "dependencies": {
        "pomelo": "0.2.0",
        "log4js": ">= 0.4.1",
        "crc": ">=0.0.1"
    }
}
~~~~

gameserver/app.js

~~~~ {.javascript name="code"}
var pomelo = require('pomelo');
var routeUtil = require('./app/util/routeUtil');
/**
 * Init app for client.
 */
var app = pomelo.createApp();     //创建Application
app.set('name', 'chatofpomelo');  //设置Application名字
// app configure  
app.configure('production|development', function() {  
    // route configures
    app.route('chat', routeUtil.chat);
    // filter configures
    app.filter(pomelo.timeout());
});
// start app
app.start();
process.on('uncaughtException', function(err) {
    console.error(' Caught exception: ' + err.stack);
});
~~~~

注意：在webstorm下调试，可能因为工作目录的设置原因会导致应用的执行路径问题，导致无法读取配置文件，所以需要根据实际情况修改如下

~~~~ {.javascript name="code"}
var opt = {'base':'D:\\src\\pomelo\\chatofpomelo\\game-server'}
var app = pomelo.createApp(opt);
app.set('name', 'chatofpomelo');
~~~~

 opt.base 是你的game-server的实际目录路径，具体可以根据自己需要来定制

app.js 是game-server的主要入口，主要负责创建application，读取配置文件，应用到application设置上，并利用app.start()来执行实际的master，monitor等服务器的start，对于聊天室程序来说，还要做简单的路由和过滤设置。

*application, 应用的定义、component管理，上下文配置， 这些使pomelo framework的对外接口很简单， 并且具有松耦合、可插拔架构。
*

*所有服务器的启动都是从运行app.js开始。每一个服务器的启动都首先创建一个全局唯一的application对象，该对象中挂载了所在服务器的所有信息，包括服务器物理信息、服务器逻辑信息、以及pomelo的组件信息等。同时，该对象还提供应用管理和配置等基本方法。 在app.js中调用app.start()方法后，application对象首先会通过loadDefaultComponents方法加载默认的组件。*

pomelo/lib/pomelo.js

~~~~ {.javascript name="code"}
var application = require('./application');
Pomelo.createApp = function (opts) {
  var app = application;
  app.init(opts);
  self.app = app;
  return app;
};
~~~~

 pomelo/lib/application.js

~~~~ {.javascript name="code"}
/**
 * Application prototype.
 *
 * @module
 */
var Application = module.exports = {};
/**
 * Application states
 */
var STATE_INITED  = 1;  // app has inited
var STATE_START = 2;  // app start
var STATE_STARTED = 3;  // app has started
var STATE_STOPED  = 4;  // app has stoped
/**
 * Initialize the server.
 *
 *   - setup default configuration
 *
 * @api private
 */
Application.init = function(opts) {
  opts = opts || {};
  logger.info('app.init invoked');
  this.loaded = [];
  this.components = {};
  this.settings = {};   // set，和get功能的容器
  this.set('base', opts.base);  //设置服务器的工作目录
  this.defaultConfiguration();  //根据配置文件，加载master，monitor等服务器
  this.state = STATE_INITED;  //application工作状态
  logger.info('application inited: %j', this.get('serverId'));
};
~~~~

 pomelo/lib/application.js

~~~~ {.javascript name="code"}
/**
 * Initialize application configuration.
 *
 * @api private
 */
Application.defaultConfiguration = function () {
  var args = utils.argsInfo(process.argv);
  this.setupEnv(args); //application环境设置
  this.loadServers();  //加载服务器配置信息
  this.loadConfig('master', this.getBase() + '/config/master.json'); //加载mater服务器配置信息
  this.processArgs(args); //根据启动参数设定服务器配置
  this.configLogger();
};
~~~~

 utils.argsInfo(process.argv); 获取系统启动参数，我们不妨看看到底有哪些参数支持 

*启动game-server服务器：\>pomelo start [development | production] [--daemon]*

根据args参数设定application的工作环境是development或production

~~~~ {.javascript name="code"}
/**
 * Setup enviroment.
 * @api private
 */
Application.setupEnv = function(args) {
  this.set('env', args.env || process.env.NODE_ENV || 'development', true);
};
~~~~

 加载服务器信息，并且保存在\_\_serverMap\_\_\_内存下

~~~~ {.javascript name="code"}
/**
 * Load server info from configure file.
 *
 * @api private
 */
Application.loadServers = function() {
  this.loadConfig('servers', this.getBase() + '/config/servers.json');
  var servers = this.get('servers');
  var serverMap = {}, slist, i, l, server;
  for(var serverType in servers) {
    slist = servers[serverType];
    for(i=0, l=slist.length; i<l; i++) {
      server = slist[i];
      server.serverType = serverType;
      serverMap[server.id] = server;
    }
  }
  this.set('__serverMap__', serverMap);
};
~~~~

 server.json

~~~~ {.javascript name="code"}
{
    "development":{
        "connector":[
             {"id":"connector-server-1", "host":"127.0.0.1", "port":4050, "wsPort":3050},
             {"id":"connector-server-2", "host":"127.0.0.1", "port":4051, "wsPort":3051},
             {"id":"connector-server-3", "host":"127.0.0.1", "port":4052, "wsPort":3052}
         ],
        "chat":[
             {"id":"chat-server-1", "host":"127.0.0.1", "port":6050},
             {"id":"chat-server-2", "host":"127.0.0.1", "port":6051},
             {"id":"chat-server-3", "host":"127.0.0.1", "port":6052}
        ],
        "gate":[
         {"id": "gate-server-1", "host": "127.0.0.1", "wsPort": 3014}
    ]
    },
    "production":{
       "connector":[
             {"id":"connector-server-1", "host":"127.0.0.1", "port":4050, "wsPort":3050},
             {"id":"connector-server-2", "host":"127.0.0.1", "port":4051, "wsPort":3051},
             {"id":"connector-server-3", "host":"127.0.0.1", "port":4052, "wsPort":3052}
         ],
        "chat":[
             {"id":"chat-server-1", "host":"127.0.0.1", "port":6050},
             {"id":"chat-server-2", "host":"127.0.0.1", "port":6051},
             {"id":"chat-server-3", "host":"127.0.0.1", "port":6052}
        ],
        "gate":[
         {"id": "gate-server-1", "host": "127.0.0.1", "wsPort": 3014}
    ]
  }
}
~~~~

工具函数，读取json配置文件，在这里读取的是master.json文件

~~~~ {.javascript name="code"}
/**
 * Load Configure json file to settings.
 *
 * @param {String} key environment key
 * @param {String} val environment value
 * @return {Server|Mixed} for chaining, or the setting value
 *
 * @memberOf Application
 */
Application.loadConfig = function (key, val) {
  var env = this.get('env');
  val = require(val);
  if (val[env]) {
    val = val[env];
  }
  this.set(key, val);
};
~~~~

master.json

~~~~ {.javascript name="code"}
{
    "development":{
        "id":"master-server-1",
        "host":"127.0.0.1",
        "port":3005,
        "queryPort":3015,
        "wsPort":2337
    },
    "production":{
        "id":"master-server-1",
        "host":"127.0.0.1",
        "port":3005,
        "queryPort":3015,
        "wsPort":2337
    }
}
~~~~

 根据进程读取配置好的参数，配置服务器。

~~~~ {.javascript name="code"}
Application.processArgs = function(args){
  var serverType = args.serverType || 'master';
  var serverId = args.serverId || this.get('master').id;
  this.set('main', args.main, true);
  this.set('serverType', serverType, true);
  this.set('serverId', serverId, true);
  if(serverType !== 'master') {
    this.set('curServer', this.getServerById(serverId), true);
  } else {
    this.set('curServer', this.get('master'), true);
  }
};
~~~~

 项目的日志配置

~~~~ {.javascript name="code"}
Application.configLogger = function() {
  if(process.env.POMELO_LOGGER !== 'off') {
    log.configure(this, this.getBase() + '/config/log4js.json');
  }
};
~~~~

 log4js.json

~~~~ {.javascript name="code"}
{
  "appenders": [
    {
      "type": "file",
      "filename": "./logs/node-log-${opts:serverId}.log",
      "fileSize": 1048576,
      "layout": {
        "type": "basic"
      }, 
      "backups": 5
    },
    {
      "type": "console"
    },
    {
      "type": "file",
      "filename": "./logs/con-log-${opts:serverId}.log", 
      "pattern": "connector", 
      "fileSize": 1048576,
      "layout": {
          "type": "basic"
        }
      ,"backups": 5,
      "category":"con-log"
    },
    {
      "type": "file",
      "filename": "./logs/rpc-log-${opts:serverId}.log",
      "fileSize": 1048576,
      "layout": {
          "type": "basic"
        }
      ,"backups": 5,
      "category":"rpc-log"
    },
    {
      "type": "file",
      "filename": "./logs/forward-log-${opts:serverId}.log",
      "fileSize": 1048576,
      "layout": {
          "type": "basic"
        }
      ,"backups": 5,
      "category":"forward-log"
    },
    {
      "type": "file",
      "filename": "./logs/crash.log",
      "fileSize": 1048576,
      "layout": {
          "type": "basic"
        }
      ,"backups": 5,
      "category":"crash-log"
    }
  ],
  "levels": {
    "rpc-log" : "ERROR", 
    "forward-log": "ERROR"
  }, 
  "replaceConsole": true
}
~~~~
