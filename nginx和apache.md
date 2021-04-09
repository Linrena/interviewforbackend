## 1. nginx和apache http服务器

Apache拥有丰富的模块组件支持，稳定性强，BUG少，动态内容处理稍强。

Nginx轻量级，占用资源少，负载均衡，高并发处理强，静态内容处理高效。

Apache和Nginx最大的不同在于它们对连接的处理方式。Apache提供一系列多重处理模块，通过这些多重处理模块来使用操作系统的资源，对进程和线程池进行管理，控制处理用户请求。

Apache提供了三种多重处理模块：mpm_prefork、mpm_worker、mpm_envent，下面我们做简要说明对比。

mpm_prefork：模块产生众多子进程，每个子进程是单线程的，每个线程链接一个请求，如此一对一的关系。所以如果请求数大于进程数时，服务器的性能就表现得差强人意了。

![img](https://pic2.zhimg.com/80/v2-af6ee5b1cddb31dad0acc812704f1ec5_1440w.png)



mpm_worker：与prefork不同，worker中子进程是多线程的，每个线程管理一个用户连接。线程数要多于进程数量，这也就意味着新的连接能立刻得到一个空闲的线程，而不用等待进程空闲。

![img](https://pic3.zhimg.com/80/v2-5ef7980a200b620b77f03042da38864e_1440w.png)



mpm_event：该模块与worker相似，区别在于event可以处理长连接(keep-alive)，以避免线程被请求长期占用而造成资源浪费，同时也增强了高并发场景下的请求处理能力。

![img](https://pic1.zhimg.com/80/v2-f9a22acfa50a3e061b6cea2d80a3814c_1440w.png)



与Apache不同，Nginx是通过异步的、非阻塞的、事件驱动的方式在实现的。Nginx的工作进程是单线程的，每个线程可以异步的处理大量的用户请求。下面是Nginx的工作原理图：

<img src="https://pic4.zhimg.com/80/v2-277f3a0e99263480b0222b35155960af_1440w.png" alt="img" style="zoom:80%;" />

## 2. nginx负载均衡

## 2.1 轮询

轮询方式是Nginx负载默认的方式，顾名思义，所有请求都按照时间顺序分配到不同的服务上，如果服务Down掉，可以自动剔除，如下配置后轮训10001服务和10002服务。



```undefined
upstream  dalaoyang-server {
       server    localhost:10001;
       server    localhost:10002;
}
```

## 2.2 权重

指定每个服务的权重比例，weight和访问比率成正比，通常用于后端服务机器性能不统一，将性能好的分配权重高来发挥服务器最大性能，如下配置后10002服务的访问比率会是10001服务的二倍。

```undefined
upstream  dalaoyang-server {
       server    localhost:10001 weight=1;
       server    localhost:10002 weight=2;
}
```

## 2.3 iphash

每个请求都根据访问ip的hash结果分配，经过这样的处理，每个访客固定访问一个后端服务，如下配置（ip_hash可以和weight配合使用）。

```undefined
upstream  dalaoyang-server {
       ip_hash; 
       server    localhost:10001 weight=1;
       server    localhost:10002 weight=2;
}
```

## 2.4 最少连接

将请求分配到连接数最少的服务上。

```undefined
upstream  dalaoyang-server {
       least_conn;
       server    localhost:10001 weight=1;
       server    localhost:10002 weight=2;
}
```

## 2.5 fair

按后端服务器的响应时间来分配请求，响应时间短的优先分配。

```undefined
upstream  dalaoyang-server {
       server    localhost:10001 weight=1;
       server    localhost:10002 weight=2;
       fair;  
}
```

# 3.Nginx配置

以轮训为例，如下是nginx.conf完整代码。

```nginx
worker_processes  1;

events {
    worker_connections  1024;
}


http {
   upstream  dalaoyang-server {
       server    localhost:10001;
       server    localhost:10002;
   }

   server {
       listen       10000;
       server_name  localhost;

       location / {
        proxy_pass http://dalaoyang-server;
        proxy_redirect default;
      }

    }

}
```