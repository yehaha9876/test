[tomcat 启动流程](/daipeilei/article/details/17485137)
======================================================

tomcat启动流程

tocmat.start-----335行启动standserver

standServer.startInternal----730行启动service

StandardService.startInternal-----459行启动connector类

org.apache.catalina.connector.Connector.startInternal()-------------

在1010行启动对http请求等待：protocolHandler.start(); ------处理链接请求 org.apache.coyote.http11.Http11Protocol

在protocolHandler.start()中启动org.apache.tomcat.util.net.JIoEndpoint.start(),endpoint应该可以配置，以后找在哪里配置。

启动后endpoint.start()中startInternal开始监控网络链接

startInternal方法中调用startAcceptorThreads

startAcceptorThreads中启动规定个数到Acceptor

Acceptor用于处理http请求

Acceptor多线程处理请求，每来一个请求，调用processSocket处理
