---
layout:     post
title:      Synchronized和ReentrantLock
subtitle:
date:       2018-07-25
author:     SWM
header-img: img/redis.png
catalog: true
tags:
    - JAVA
    

---

# 1、典型回答
* synchronized 是Java内建的同步机制，所以有人称其为Intrinsic Locking,他提供了互斥的语义和可见性，当一个线程已经获取当前锁时，其他试图获取的线程只能等待或者阻塞在那里

* 在JAVA5以前，synchronized是仅有同步手段，在代码中，synchronized可以用来修饰方法，也可以用来使用在特定的代码块上，本质上sychronized方法等同于把方法全部语句用synchronized块包起来

* ReetrantLock 通常翻译为再入锁，是Java 5 提供的锁实现，他的语义和synchronized基本相同。再入锁通过代码直接调用lock()方法获取，代码书写也简单灵活。与此同时ReetrantLock提供很多实用的方法，能够实现很多synchronized无法做到的细节控制，比如可以控制fairness，也就是公平性或者利用定义条件等。但是编码中也需要注意，必须要明确调用unlock()方法释放，否则会一直持有锁。

* synchronized 和 ReetrantLock的性能不能一概而论，早期版本的synchronized在很多场景下性能相差较大，在后续版本中进行了较多改进，在低竞争环境中表现可能优于ReetrantLock.