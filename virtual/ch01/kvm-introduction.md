## 读书笔记

### VDI、IDV、VOI

**VDI**：
virtual desktop infrastructure
虚拟桌面基础架构
集中计算，集中管理

**IDV**：
intelligent desktop virtualization
智能桌面虚拟化
分布式计算，集中管理

**VOI**:
virtual OS infrastructure
对操作系统存储层进行重新定向，无盘工作站方案



### 虚拟化

云计算的云端系统，实质上是一个大型的分布式系统，虚拟化通过在一个物理平台上虚拟出更多的虚拟平台，其中每个虚拟平台可作为独立的终端加入云端的分布式系统。

![虚拟化](C:\Users\rj\AppData\Roaming\Typora\typora-user-images\image-20201207105702678.png)



#### hypervisor

在x86平台虚拟化技术中，新引入的虚拟化层成为虚拟机监控器（virtual machine monitor，VMM) ，也叫hypervisor。虚拟机监控器运行的环境，也就是真实的物理平台，叫宿主机。而虚拟出来的平台成为客户机，里面运行的系统成为客户机操作系统。



#### 虚拟化技术实现方式：

##### 软件虚拟化和硬件虚拟化

实现虚拟化的重要一步：虚拟化层必须能够截获计算元件对物理资源的直接访问，并将其重新定向到虚拟资源池中。





##### 准虚拟化和全虚拟化







#### Iaas vs Paas vs Saas

Iaas，infrastructure as a service，基础设施即服务

Paas，platform as a service，平台即服务

Saas，software as a service，软件即服务

直观对比：

![saas vs paas vs iaas breakdown](https://www.bigcommerce.com/blog/wp-content/uploads/2018/10/saas-vs-paas-vs-iaas-breakdown.jpg)



### 虚拟化方案

#### 开源的KVM、Xen

#### VMware的VMware Workstation和VMware ESX Server

#### Oracle的VirtualBox

#### 微软的Hyper-V





### KVM

kernel virtual machine，内核虚拟机。

KVM架构中，**虚拟机实现为常规的Linux进程**，由标准的Linux调度程序进行调度。**每个虚拟CPU显示为一个Linux进程**。KVM能享受到Linux内核的所有功能。

![KVM架构](C:\Users\rj\AppData\Roaming\Typora\typora-user-images\image-20201207110958887.png)

KVM是基于虚拟化扩展（Intel VT或AMD-V）的x86硬件，是Linux完全原生的全虚拟化解决方案。



### Xen虚拟机

![Xen架构](C:\Users\rj\AppData\Roaming\Typora\typora-user-images\image-20201207113741568.png)



### KVM核心基础功能

KVM是完全虚拟化技术，在KVM环境中运行的客户机操作系统是未经过修改的普通操作系统。在硬件虚拟化技术的支持下，内核的KVM模块和QEMU的设备模拟协同工作，构成了一整套与物理计算机系统完全一致的虚拟化的计算机软硬件系统。



一个完整的计算机系统，必不可少的最重要的子系统包括：处理器CPU、内存memory、存储storage、网络network、显示display等。



硬件平台支持，打开BIOS的VT-x和VT-d的支持。

软件支持，linux内核中集成了KVM模块。用户态程序qemu。

#### CPU配置

vCPU: virtual CPU，每个虚拟客户机看来起所拥有的CPU即是vCPU。KVM中，每个客户机是个标准的Linux进程（QEMU进程），而每个vCPU在宿主机中是QEMU进程派生的一个普通线程。

一般Linux系统中，进程一般有两种模式：内核模式、用户模式。在KVM中，增加了第三种模式：客户模式。

![image-20201207151547642](C:\Users\rj\AppData\Roaming\Typora\typora-user-images\image-20201207151547642.png)

![image-20201207151649120](C:\Users\rj\AppData\Roaming\Typora\typora-user-images\image-20201207151649120.png)



qemu可以设定CPU模型，让每个VMM客户机看起来有一个默认的CPU类型。



#### 内存配置

QEMU可-m 参数配置内存。



#### 存储配置

QEMU中提供了对多种块存储设备的模拟，包括IDE设备、SCSI设备、U盘、virtio磁盘等。



#### 网络配置

QEMU中向客户机提供了4中不同模式的网络：

基于网桥bridge的虚拟网卡；

基于NAT的虚拟网络；

QEMU内置的用户网络模式（user mode networking）；

直接分配网络设备的网络（包括VT-d和SR-IOV）；



#### 图形显示

QEMU模拟器中的图形显示默认使用SDL库。



VNC（virtual network computing）桌面分享系统，采用RFB（remote framebuffer）协议来远程控制另一台计算机。不依赖于操作系统，windows和linux可互用。



### KVM高级功能

#### 半虚拟化驱动

KVM是必须使用硬件虚拟化辅助技术（如Intel VT-x、AMD-V）的hypervisor。

在KVM中可以使用半虚拟化驱动来提高客户机的I/O性能。





#### libvirt

libvirt是对KVM虚拟机进行管理的工具和应用程序接口，而且一些常用的虚拟机管理工具（如virsh、virt-install、virt-manager等）和云计算框架平台（如OpenStack、OpenNebula、Eucalyputs等）都在底层使用libvirt的应用程序接口。

![image-20201207160638613](C:\Users\rj\AppData\Roaming\Typora\typora-user-images\image-20201207160638613.png)



### KVM性能测试

评价一个系统的性能标准，一般可用以下几个因素衡量：

* 响应时间（response time）
* 吞吐量（throughput）
* 并发用户数（concurrent users）
* 资源占用率（utilization）



#### CPU性能测试工具

* SPECCPU2006
* SPECjbb2005
* UnixBench
* SysBench
* PCMark
* 内核编译
* Super PI（CPU密集型测试工具）



#### 内存性能测试工具

* LMbench
* Memtest86+
* STREAM



#### 网络性能测试工具

* Netperf
* Iperf
* NETIO
* SCP



#### 磁盘I/O性能测试工具

* DD
* IOzone
* Bonnie++
* hdparm

