（杂七杂八，比较乱，待继续补充整理）

1. 实时操作系统  
   实时操作系统(Real-time Operating System,RTOS)是针对有实时性要求的应用而设计的操作系统。相较于分时操作系统，实时操作系统能够在指定或确定的时间内完成系统功能，对外部或内部、同步或异步时间做出正确响应。  
   硬实时操作系统中，任务必须在规定的时间内完成，否则会导致系统故障或者严重的后果。对时间要求很高，限定时间内不管是否完成必须返回。在硬实时系统中，任务的响应时间是系统设计的关键指标，必须通过严格的调度算法和实时性能分析来保证任务的按时完成。  
   软实时系统，可以允许任务偶尔出现一定的时间偏差，但是随着时间的偏移，整个系统的正确性也会随之下降。  
   
   ![系统效用](https://github.com/Tonghui-Wang/Common_Fieldbus_Notes/assets/23735485/5dab5753-46c7-476d-a916-338c3bc90ea9)
   图1 各类系统时间与效用的关系

   当事件触发，在时间t内完成，则三类系统的效用是相同的。但是当完成时间超出时间t时，则效用发生了变化。  
   非实时系统：超过规定的时间t后，其效用缓慢下降。  
   软实时系统：超过规定的时间t后，其效用迅速下降。  
   硬实时系统：超过规定的时间t后，其效用立即归零。  
  
   分时操作系统：Windows、Linux、MacOS等  
   闭源实时操作系统：VxWorks、RTX、QNX、INtime等  
   开源实时操作系统：RTAI、Xenomai等  

2. 总线技术  
    常见的现场总线技术：CAN Profibus EtherNet Sercos CC-link Modbus RS232/RS485  

    2.1 EtherCAT    
    EtherCAT是Beckhoff基于以太网基础开发的一种开放式的、先进的现场总线，它通过一系列独特的机制来保证数据传输的高效与可靠。  
    EtherCAT只需一根网线就可以实现数据传输，支持总线型、树型、星型及混合结构连接。网线中包含四根线，其中两根用于输出数据（TX），两根用于返回数据（RX），在内部组成了一个环网。主站设备使用RTOS+以太网芯片（MAC）+EtherCAT协议栈，从站设备使用专门的通信芯片（ESC），来保证通信的高速和稳定。  
![ethercat状态机](https://github.com/Tonghui-Wang/Common_Fieldbus_Notes/assets/23735485/90f1e48d-f8ad-4f10-8ab2-741246c4d887)  
    图2 EtherCAT状态机  

    报文数据：  
    从站设备在报文经过其节点时，读取带有相应寻址信息的数据；同时，输入数据也是在报文经过时插入至报文中。整个过程中，报文只有几纳秒的时间延迟。  
    两种数据交换形式：  
    时间关键的过程数据（PDO）：通常周期性发送，通信应用层为CoE协议（CANopen Over EtherCAT）。  
    非时间关键的邮箱数据（SDO）：可非周期性发送，借助mailbox。  

    同步时钟：  
    主站设备会挑选首个带时钟（DC）功能的从站设备作为时钟主站（DC Master），各从站按照时钟主站的时间统一步调。这样多轴插补时，就可以参考精准的时钟信号进行统一时间采样（采集命令数据），统一时间动作。  
    三种同步模式：  
    Free-Run：从站设备的过程数据处理，由内部事件触发，与主站设备循环无关。适用于处理信号变化缓慢的I/O设备。  
    SM-Synchronous：从站设备的过程数据处理，由接收到携带过程数据的周期性数据帧时所产生的硬件中断触发。  
    DC-Synchronous：从站设备的过程数据处理，由基于分布时钟和系统时间的硬件中断触发。适用于任务控制的伺服驱动，以及超采样的I/O设备。  

4. CiA402  
    EtherCAT支持CANopen协议中的行规CiA402和SERCOS协议的应用层，分别称为CoE和SoE。  
    CiA402，作为“伺服与运动控制”行规，广泛应用于基于CAN总线、EtherCAT总线网络的运动控制。该协议将伺服运动控制应用中，具有代表性的设置参数、控制参数、状态参数等项目，归纳整理为具有固定编号（索引号+子索引号）的“对象”，完整的对象定义表就是“对象字典”。各设备厂家遵循这个规范开发的控制器、伺服驱动器（从站设备）均可配合运行，或可替换使用。
![COE设备结构图](https://github.com/Tonghui-Wang/Common_Fieldbus_Notes/assets/23735485/b30d1be2-978b-4398-af39-028d6b4e0fd1)  
   图3 CoE设备结构图
