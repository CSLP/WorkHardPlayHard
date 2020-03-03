* # 背景
    * github经常要写一些.md文件,以前在ubuntu下都是直接vim写,看不到自己写的效果就很烦,后来用remarkable这个软件写,还行吧.再后来因为简书也要发布这些文档吗.所以都是现在简书写一遍,然后再去网页github上复制.windows下也是这么弄.后来这样感觉一直用网页写不方便.遂准备安装一个markdown编辑器.
  * 其实很多编辑器都支持写markdown,不支持的也可以下插件支持.比如vim,subline,vscode等.但是我一般用这些来写代码,并不想再开一个或者直接在写代码的编辑器写文档.而且这些编辑器写markdown时也不是很舒服.后来决定使用typora,听说挺爽的.
* # 安装
    * ## Linux
      * #### 步骤
        * 参考官网[https://typora.io/#linux](https://typora.io/#linux).代码如下
        ```
        # or run:
        # sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
        wget -qO - https://typora.io/linux/public-key.asc | sudo apt-key add -
        # add Typora's repository
        sudo add-apt-repository 'deb https://typora.io/linux ./'
        sudo apt-get update
        # install typora
        sudo apt-get install typora
        ```
        * 执行```sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE```出来一个结果不知道对不对.
        * 然后执行 ```sudo add-apt-repository 'deb https://typora.io/linux ./'```提示segmentation fault,段错误.遂放弃
        * 决定使用第二句.于是执行```wget -qO - https://typora.io/linux/public-key.asc | sudo apt-key add -```直接报错,提示```gpg: no valid OpenPGP data found.```.于是查找原因.貌似是这个管道执行有问题.这句的大致意思时前半句生成一个文件,然后后半句打开利用这个文件.不知为何前半句好像没有生成这个文件,所以后半句提示没有文件.网上说试试分开执行.
        * 于是分开执行,执行```wget -qO - https://typora.io/linux/public-key.asc ```,灭有任何反应,也没有生成任何文件(网上说单独执行这句会生成一个文件).于是试试另一种方案.
        * 直接浏览器访问https://typora.io/linux/public-key.asc ,然后就会自动下载一个public-key.asc文件,然后执行```sudo apt-key add public-key.asc```,成功.提示OK.
        * 然后继续执行```sudo add-apt-repository 'deb https://typora.io/linux ./'```提示段错误.爷吐了.搜索似乎没人有这种错误.遂放弃.因为我这个Ubuntu好像有问题,所以容易出些奇奇怪怪的问题.每次一开机就会提示system program problem detected,然后让report这个错误.不搞了,等下次重装一下系统在搞.
      * #### 知识点
        * linux命令行长命令中的|,||,&,&&的含义
          * |:表示管道
          * ||:表示或短路求值.即||之前的命令执行成功,后面的命令就不执行.前面的执行失败,后面的才执行.
          * &:执行所有命令,不管每一个命令是否执行成功.
          * &&:表示与短路求值,只有当&&前的命令执行成功,后面的命令才执行.
        * 关于Segmentation fault(core dumped) 错误
            * segmentation fault表示段错误,一般都是访问空指针,不存在的地址或者栈溢出导致的.过程是MMU(内存管理单元)发现这个错误,然后给操作系统发11号信号,然后操作系统终止进程.
            * core dumped(吐核或核心已转储):吐出了一个"核心转储文件"(coredump文件).程序确认出现错误时的“临终遗言” 写入核心转储文件，也是使用gdb调试器最常用到的场景.这个文件是可以查看以确定错误的.具体查看方法这里不谈了.
    
  * ## Windows
  
    * 傻瓜式官网无脑安装即可
  
    * 卧槽，这也太好用了吧，无敌，为什么没有早发现typora这个宝藏软件，这颜值，这方便程度，无敌号好吗。
  
      

