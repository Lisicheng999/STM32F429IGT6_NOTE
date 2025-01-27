### DTS  设备树
DTS（Device Tree Source）设备树是一种用于描述硬件设备和资源的数据结构，它在嵌入式系统开发中扮演着重要的角色。以下是关于DTS设备树的详细介绍：

基本概念：DTS设备树采用树形结构来描述硬件设备的层次关系，每个节点代表一个硬件设备或子系统。节点可以包含设备的名称、类型、地址、中断号等信息，还可以包含子节点来表示该设备所连接的其他设备。
作用：通过DTS设备树，操作系统可以在运行时动态地识别和配置硬件，而无需在代码中硬编码驱动程序。这大大简化了硬件驱动的编写和管理工作，提高了系统的可移植性和可扩展性。
文件格式：DTS文件通常使用文本格式编写，易于阅读和编辑。文件名通常以.dts为扩展名，而.dtsi文件则用于描述SOC级信息（如CPU数量、主频、外设控制器信息等），可以被多个DTS文件共享以提高可重用性。
工作流程：在嵌入式系统开发中，DTS文件首先被编译成DTB（Device Tree Blob）二进制文件。这个编译过程通常使用设备树编译器（DTC）来完成。然后，在boot loader运行时，DTB文件被传递给操作系统。操作系统通过解析DTB文件来获取系统中所有硬件设备的配置信息，并据此进行初始化和配置。
应用：DTS设备树在嵌入式系统中的应用非常广泛，尤其是在嵌入式Linux系统中。通过DTS设备树，Linux内核可以自动识别并配置各种硬件设备，如CPU、内存、网卡、串口等。此外，DTS还支持热插拔功能，即在系统运行时动态添加或删除硬件设备，这对于需要频繁更换硬件设备的场景非常有用。
优点：DTS设备树将硬件信息从内核代码中分离出来，使得硬件信息更加清晰、易于管理和维护。同时，它也提高了系统的可移植性和可扩展性，使得开发人员可以更加容易地将代码移植到不同的硬件平台上。
总之，DTS设备树是一种非常重要的数据结构，它简化了硬件驱动的编写和管理工作，提高了系统的可移植性和可扩展性。在嵌入式系统开发中，DTS设备树发挥着不可替代的作用。


dtbs 可能是指Device Tree Blob Source的缩写，用于描述或存储设备树源文件的集合。Device Tree Blob（DTB）是设备树描述（Device Tree Description）的二进制表示，它描述了硬件的结构和配置，特别是在嵌入式系统中。

### 使用虚拟机发现网络上的本地主机
sudo netstat -ntu | grep :22 | awk '{print $5}' | cut -d: -f1 | sort | uniq
192.168.174.1
### 使用Windows主机Powershell查看连接的虚拟机
显示当前系统上所有状态为 “ESTABLISHED” 且端口号为 22 的网络连接
netstat -an | Select-String "ESTABLISHED" | Select-String ":22"

 192.168.174.132:22
### 解压tar.xz 和 tar.gz
tar -xzvf file.tar.gz
tar -xJvf file.tar.xz
-x 解压
-z 使用gzip解压.gz
-v 显示进度
-f 指定文件名
-J 使用xz解压.xz

### MMU 内存管理单元

### 栈
在 x86 架构中，SS:BP 通常用于指向栈的底部或者栈帧的开始，而 SS:SP 用于指向栈的顶部。

SS（Stack Segment）寄存器：它包含了栈段的段选择器，用于指示当前栈所在的内存段。
BP（Base Pointer）寄存器：在栈帧中，BP 通常被用作基址指针，它指向当前栈帧的底部。在函数的栈帧中，BP 通常用来访问函数的局部变量和参数。
SP（Stack Pointer）寄存器：它指向栈的顶部。每当向栈中压入一个值时，SP 的值会减小；当从栈中弹出值时，SP 的值会增加。

在Linux系统中，以及大多数采用x86架构的计算机系统中，栈是向下增长的，即从高地址向低地址增长。在这种设计中，栈指针（SP，即Stack Pointer）指向栈顶元素。当进行push操作，也就是将数据压入栈中时，系统会先将SP的值减去数据的大小，然后将数据放入SP所指向的内存位置。因此，push操作后，SP的值会变小。

简单来说，执行push操作后：

SP的值 变小。
相对应地，当执行pop操作，从栈中取出数据时，SP的值会增加，从而指向新的栈顶。
```assembly
    .glolbal
    .align 4
    .type   x, @object
    .size   x, 4
x:
    .long 4

push_x:
    movl x, %eax
    pubsh %eax
```

### 编译linux源码
解压到使用ext4文件系统的目录下，确保符号连接被正确设置。
1. xz -xvf linux-6.x.tar.xz

2. make clean
3. make x86_64_defconfig
4. make prepare  # 准备编译环境
5. make kvm_guest.config  # 配置内核

CONFIG_HYPERVISOR_GUEST: 启用虚拟化监控程序来宾支持。
CONFIG_VIRTIO_PCI: 启用 VirtIO PCI 驱动，这对于虚拟化设备通信至关重要。
CONFIG_VIRTIO_CONSOLE: 启用 VirtIO 控制台，用于虚拟机与宿主机之间的控制台通信。
CONFIG_NET_9P: 启用 9P 网络协议支持，通常用于在 QEMU/KVM 虚拟机中共享文件。
CONFIG_SCSI_LOWLEVEL: 启用 SCSI 低级别驱动支持。
CONFIG_DRM_VIRTIO_GPU: 启用 VirtIO GPU 支持，允许在虚拟机中使用图形加速。

5. nproc  # 查看cpu核数
配置完成后编译内核

6. apt-get update
    apt-get install build-essential libssl-dev libelf-dev flex bison

7. make -j$(nproc)


创建根文件系统(基于debian发行版bookworm - debian 12)
8. sudo debootstrap --arch amd64 bookworm /path/to/rootfs http://deb.debian.org/debian/

创建启动盘
9. qemu-img create -f raw kernel.img 1G

格式化映像为 ext4 文件系统
10. mkfs.ext4 kernel.img

创建一个挂载点
11. mkdir /mnt/kernel_img

挂载映像文件
12. mount -o loop kernel.img /mnt/kernel_img

将根文件系统拷贝到虚拟磁盘
13. cp -a /path/to/rootfs/* /mnt/kernel_img/

进入创建的根文件系统的 chroot 环境
14. cd /mnt/kernel_img

15. chroot .  # 进入当前文件，或者替换为根文件系统路径，因为挂载了根文件系统到kernerl_img, 直接在这个目录编辑
adduser user
passwd 修改root密码
adduser username sudo  添加到sudo组
groups username  查看username是否在用户组
su - username  切换到用户并测试 sudo 
退出chroot 环境
exit 
exit (刚刚使用su - username 测试，exit两次退到原本的环境)

卸载映像
16.   umount /mnt/kernel




启动qemu, 指定内核镜像和磁盘镜像
qemu-system-x86_64 \
  -drive file=kernel.img,format=raw \
  -m 1024 \
  --enable-kvm \
  -kernel bzImage \
  -append "root=/dev/sda rw console=ttyS0" \
  -nographic
或
qemu-system-x86_64 \
  -hda kernel.img \
  -m 1024 \
  --enable-kvm \
  -kernel bzImage \
  -append "root=/dev/sda1 rw console=ttyS0" \
  -nographic

启动报错可能是需要关闭Hyper-V同时在VMware中设置开启虚拟化支持
1.在windows功能中关闭Hyper-v
1. powershell管理员模式下执行: bcdedit /set hypervisorlaunchtype off
### egrep
egrep -c '(vmx|svm)' /proc/cpuinfo # 检查CPU是否支持虚拟化
返回值>0 表示支持虚拟化


###