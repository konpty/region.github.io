## Raid定义
- RAID,全称Redundant Array ofInexpensive Disks,中文名为廉价磁盘冗余阵列。RAID可分为软RAID和硬RAID,软RAID是通过软件实现多块硬盘冗余的。而硬RAID是一般通过RAID卡来实现RAID的。前者配置简单，管理也比较灵活。对于中小企业来说不失为一最佳选择。硬RAID往往花费比较贵。不过，在性能方面具有一定优势。

  - RAID可分为以下几类：
    - RAID 0 ：存取速度最快 没有容错
    - RAID 1 ：完全容错 成本高，硬盘使用率低
    - RAID 3 ：写入性能最好 没有多任务功能
    - RAID 5 ：具备多任务及容错功能 写入时有overhead
    - RAID 0+1 ：速度快、完全容错 成本高

## 实验目标
- 掌握windows2008的动态磁盘的管理，raid-5的创建和修复

## 实验概述

- 在windows2008中 创建raid-5卷，在磁盘中存入数据
- 使一块硬盘挂掉，观察现象，及数据是否丢失
- 修复raid-5

## 实验步骤:
在windows2008中新建raid-5卷，在磁盘中存入数据。用虚拟机添加3块硬盘，然后把这3块硬盘建一个raid-5卷。需要先把这3块硬盘转换动态硬盘，然后右击磁盘，如图3.1:
https://github.com/konpty/region.github.io/blob/master/3.1.png

在磁盘1右键，新建Raid5-卷，如图3.2：
https://github.com/konpty/region.github.io/blob/master/3.2.png

添加刚才可用动态硬盘到可用，一直下一步配置完成。如图3.3:
https://github.com/konpty/region.github.io/blob/master/3.3.png

https://github.com/konpty/region.github.io/blob/master/3.4.png

Raid配置完成后，就新加卷E，在E盘加入一个test.txt文档。然后在虚拟机移除其中一个磁盘模拟磁盘损坏，这时候在磁盘管理中可以看到有个磁盘损坏。但是E盘文件不受影响，图3.5：
https://github.com/konpty/region.github.io/blob/master/3.5.png

最后进行raid-5修复，我们添加一块硬盘，磁盘3，在磁盘1右键选择修复卷，完成raid-5磁盘修复。如图3.6：
https://github.com/konpty/region.github.io/blob/master/3.6.png
