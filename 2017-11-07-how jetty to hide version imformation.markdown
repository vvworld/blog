---
layout: post
title: 关于jetty如何隐藏版本信息
date: 2017-11-07 23:21:36
tags:
---

由于某动的需求，要隐藏掉项目中jetty版本信息，再次记录下谷歌的答案。

**Jetty版本8**

    Server server = new Server(port);
    server.setSendServerVersion(false);

**Jetty版本9**

    Server server = new Server(port);
    for(Connector y : server.getConnectors()) {
        for(ConnectionFactory x  : y.getConnectionFactories()) {
            if(x instanceof HttpConnectionFactory) {
                ((HttpConnectionFactory)x).getHttpConfiguration().setSendServerVersion(false);
            }
        }
    }