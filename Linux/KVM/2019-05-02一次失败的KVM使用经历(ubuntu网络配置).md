- 背景:还是学习鸟哥要装Centos吗,然后虚拟机有好几种,KVM,virtualbox,vmware workstations等等,上网络攻防课已经用过了VirtualBox,又加上上IBM课的时候听这个KVM听得多,所以有了一些兴趣,就准备用KVM.
- 失败过程
  1. 首先搜索Ubuntu下KVM使用配置,基本参考这两篇
[Ubuntu18.04上安装KVM虚拟机](https://blog.csdn.net/shizao/article/details/85112933)
 [Ubuntu 16.04 搭建KVM环境](https://www.cnblogs.com/ccskun/p/5527014.html)
核心基本差不多,首先就是下载KVM及其相关依赖,然后修改/etc/network/interfaces文件,配置IP,DNS等信息(因为我是静态IP),让虚拟网卡与原来的网卡是桥接关系.这个文件原来什么都没有,我就感觉很奇怪(奇怪是没错的,果然后面有坑),然后就是备份一下它,然后修改给新的的虚拟网卡配置静态IP,然后重启网络服务或重启电脑.
  2. 然后失败了,因为抄别人代码抄错了,没有结合自己的网卡,虚拟网卡信息,无脑抄.后来改对了,继续重启.此时ifconfig查看,原enp3s0已经没有IP信息,取而代之的是virbro虚拟网卡有了IP信息,按理说这样已经搞定了,于是登录上网客户端发现让允许UDP64110端口,不会用UFW于是让同学改了,发现还不行.....只好放弃.我猜是那个界面网络设置的原因,改之后那里并没有显示配置信息,又去大致搜了一下,好像Ubuntu18.04配置不是用的/etc/network/interface,好像用的是netplay来管理network.所以原来的在GUI配置的静态IP信息并没有在那里显示,所以在那里改了虚拟网卡也并没有成功.
- 清晰过程
    1. 后来就放弃了,又改回原来的模样.于是乎先不弄KVM了,开始弄明白Ubuntu 18.04 desktop版是怎么配置IP信息的(我当时是直接在GUI配的).总结如下:
    2. Ubuntu之前网络配置都是修改/etc/network/interfaces(配置IP信息,也可加上DNS信息,如果配置了DNS信息,就不用改/etc/systemd/resolved.conf )和修改/etc/systemd/resolved.conf (配置DNS).以上的方法是基于ifupdown进行网络管理的.
    3. Ubuntu 18.04 就不一样了,虽然也支持ifupdown管理,但是默认没有ifupdown工具.而新的网络管理工具是netplan,而且desktop和server还不一样.desktop也支持netplan,但是缺省是用Networkmanager管理的,所以配置信息存储在/etc/NetworkManager/system-connections,在这里配置和用GUI配置的是一致的,也就是说在GUI配置的信息就是存在这里.而server就是用netplan管理的,配置就是修改/etc/netplan/*.yaml文件(可以同时配置IP信息和DNS,如果不配置DNS的话需要再在/etc/resolv.conf添加DNS信息).

- 知识点
  1. 改什么配置文件前一个好的习惯先备份,直接备份在原位置名字后面加-bak.
  2. Ubuntu desktop和server版有些东西不一样,搜索时要指明.
  3. 不管是ifupdown还时netplan都是基于linux的网络底层命令的,所以说还是要打牢基础,再看这些花里胡哨的瞬间就懂了.
  4. 关于yaml格式,YAML是"YAML Ain't a Markup Language"（YAML不是一种标记语言）的递归缩写.详情见百度.
