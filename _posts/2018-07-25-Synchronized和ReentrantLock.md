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

synchronized 是Java内建的同步机制，所以有人称其为Intrinsic Locking,
他提供了互斥的语义和可见性，当一个线程已经获取当前锁时，其他试图获取的线程只能等待或者阻塞在那里