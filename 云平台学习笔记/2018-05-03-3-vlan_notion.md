---
layout: post
share: true
title: VLAN技术原理和配置 - VLAN工作原理
description: VLAN的产生原因/通过标签管理实现VLAN/VLAN标签介绍/如何生成VLAN标签/VLAN转发流程/VLAN转发流程
date: 2018-05-03
---

### VLAN的产生原因

1. 用户在二层可以互通，但无法隔离；
2. 广播风暴。

### 通过标签管理实现VLAN / VLAN标签介绍

```
DA(6B)-SA(6B)-TYPE(2B)-DATA(64-1500B)-FCS(4B) //UNTAGGED FRAME
DA(6B)-SA(6B)-TAG(4B)-TYPE(2B)-DATA(64-1500B)-FCS(4B) //TAGGED FRAME
               |
               |
          -------------
          |           |
          |           |
         2B           2B
        0X8100    PRI-CFI-VLAN ID(12b)
        TPID          TCI
      标签协议ID   优先级-格式标识-VLAN ID(4094)
```

### 如何生成VLAN标签

1. 通过接口划分PVID；
2. 通过（接口对应的）MAC地址划分PVID；
3. 通过子网IP划分PVID；
4. 通过协议划分PVID。

### VLAN转发流程

```
收到对端设备以太网帧
      |
      |
    Tagged? --> Y --> 使用自身VLAN ID
      |                     |
      |                     |
      N                     |
      |                     |
      |                     |
    添加PVID---->交换机是否创建了该PVID？--> Y
                            |              |
                            |              |
                            N              |
                            |              |
                            |              |
                           丢弃<-- N <--目的端口是否允许该VLAN通过
                                           |
                                           |
                                           Y
                                           |
                                           |
                                      转发/标签操作
                                           |
                                          END
```

📌 VLAN TAG 是在 OSI 参考模型中的`数据链路层`{: style="color:#FF0000"}实现的。