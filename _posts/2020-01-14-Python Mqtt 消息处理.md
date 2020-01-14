---
layout:     post
title:      Python Mqtt使用
subtitle:
date:       2020-01-14
author:     SWM
header-img: img/mqtt.jpg
catalog: true
tags:
    - Python
    - Mqtt

---

# 背景需求
    由于全息项目图片较多，多大1G左右，采用网络带宽加载会很慢，所以采用增量更新到本地的方案，达到快速加载很多图片的逻辑目的
    目前采用MQTT轻量的消息协议，可以保持服务端长连接并且低带宽消耗,MQTT已被广泛应用到IOT领域。
# 实现代码

       

       import socketclient
       import logger
       import demjson
       import common
       import syncfile
       
       log = logger.Logger("info")
       
       #HOST = "******"
       HOST = "*****"
       PORT = 1883
       
       
       def client_loop():
           client_id = time.strftime('%Y%m%d%H%M%S', time.localtime(time.time()))
           client = mqtt.Client(client_id)  # ClientId不能重复，所以使用当前时间
           client.username_pw_set("****", "*****")   # 必须设置，否则会返回「Connected with result code 4」
           client.on_connect = on_connect
           client.on_message = on_message
           log.info('开始连接mqtt' + HOST + ':' + str(PORT))
           client.connect(HOST, PORT, 60)
           log.info('mqtt连接完成' + HOST + ':' + str(PORT))
           client.loop_forever()
       
       def on_connect(client, userdata, flags, rc):
           config = common.readJson()
           command = str(config['mall'])
           log.info("Connected with result code " + str(rc))
           client.subscribe("projector-remote-control")
           log.info("订阅消息 projector-remote-control")
           client.subscribe("holo-file-sync-"+command)
           log.info("订阅消息 holo-file-sync-"+command)
           client.subscribe("sync")
           log.info("订阅消息 sync")
           client.subscribe("sync-single-work-"+command)
           log.info("订阅消息 sync-single-work-"+command)
       
       def on_message(client, userdata, msg):
           recvmsg = msg.payload.decode("utf-8")
           log.info("收到消息" + recvmsg + "，开始执行命令")
           print(msg.topic + "" + recvmsg)
           config = common.readJson()
           command = str(config['mall'])
           if(msg.topic=='sync-single-work-'+command):
             common.insert_sql(recvmsg)
           elif(msg.topic=='sync'):
             common.insert_sql(recvmsg)
           elif(msg.topic=='holo-file-sync-'+command):
             common.insert_sql(recvmsg)
           elif(msg.topic=='projector-remote-control'):
             text = demjson.decode(msg)
             command = text['command']
             projectors = text['projectors']
             #socketclient.sendSocket(recvmsg)
             socketclient.handlerMsg(command,projectors)
       
       if __name__ == '__main__':
           syncfile.scheduletask()
           syncfile.scheduleuncompletedtask()
           client_loop() 
       

         
   
  