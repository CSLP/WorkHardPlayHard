* # 背景

  * 正在用typora写一个设计模式的笔记，要插入图，插本地坑定不行，查服务器的话没服务器，于是乎想利用github来搞，但是遇到了一些问题。
  * 插入图还没搞定，突然想起之前做的shell笔记很长，在github上不好看，虽然在typora有大纲，很好看。于是乎想怎么加一个目录。

* # 解决

  * 加目录问题
    * 加目录学名加TOC（table of content)，除了github，一般的markdown编辑器都支持自动生成目录。也就是在文章首尾+[TOC]标识符，则自动生成目录。很好，可惜github不支持
    * 一种办法，手动写，格式    `[文本1](#文本2)`文本一表示链接名字，#定位，文本2输入你想定位的标题。**注意，文本二就是你想定位的标题但是要删除标题中的.,且空格替换为-**
      * 举例    ，标题为    1. shell的 test命令
      * ``[1. shell的 test命令](#1-shell的-test命令)``
    * 另一种，网页在线生成github markdown toc或者利用vscode插件或者别人写好的gh-md-toc（github markdown toc)脚本。但是我三种方式都试了，试了10个小时，都不行，生成是生成了，但是粘贴进去都是挤在一起没有缩进，还得一个一个缩进。遂无奈放弃。
    * 如果愿意手动写就手动写吧，不愿意就别再网页上看了，在typora上看香的一批号好吗
  * 插图问题
    * 关键就是把图片放在哪个位置，放在github上引用的话经常显示不出来，思来想去，放弃了，白嫖简书服务器吧。。。。

* # 血泪史

  * 且听我这10多个小时将近一天的血泪史

  * 首先想在github .md上加个目录，许多编辑器都支持[toc]  [toc]，加这两句即可，但是github不行，遂放弃

  * 于是搜如何在github markdown上加toc，于是找到了手写格式，太慢，继续找

  * 于是找到了在线转换网站，就是将markdown文件复制进去，然后就会生成一个github格式的toc，相当于它帮你写好，然后将这个粘贴进去就行了。嫌麻烦，每次都这么搞，不行。况且这样生成的粘贴进去也不行，有超链接，但是没有缩进。于是继续搜。

  * 于是收到了大家推荐的脚本，gh-md-toc，github上的库，把这个脚本文件下载下来执行就行了，但是必须上网才行，因为这个脚本利用网上的api对markdown文件进行解析然后生成toc。

  * 详情见https://github.com/ekalinin/github-markdown-toc/blob/master/README.md，然后开始安装，照着它的方法安装不行，wget出错，于是只能clone 整个库，然后找到gh-md-toc这个脚本文件，然后就开始按照他的教学开始试用，嗯，还行，然后其中有个 gh-md-toc --insert file 命令，可以直接将生成的toc插入markdown中，哇，太爽了。但是前提是在文件中插入<!--ts-->和<!--te--> 方便定位。

  * 好吗，问题来了，于是我往文件中插入了，但就是提示找不到，一运行找不到，但是我怎么都试了，typora中插入不行，vim中插入，不行，vscode中插入也不行，明明有就是提示没有。但是在shell中创建的示例就毫无问题，输入这两个定位的，然后--insert运行完美，毫无问题。他妈的奇了怪了。遂放弃。

  * 于是继续想办法，又找到了可以利用vscode的markdown toc插件插入。于是又用vscode打开我的笔记，基本把各种markdown toc插件用了个变，都不行，确实可以生成目录，但是老问题，没有缩进。妈的彻底坐蜡了。服了。

  * 没办法，又回到gh-md-toc，为什么就是找不到<!--ts-->,<!--te-->哪，明明shell下的test.md可以找到，typora写好的notes.md就是找不到哪。于是乎打开gh-md-toc看脚本内容，找定位符用的是grep命令，而且还要要求，不仅文字匹配还得独占一行。如果不符合条件，就输出找不到信息。

  * 与这个有关的哪一行如下

  * ```shell
     if grep -Fxq "<!--ts-->" $gh_src && grep -Fxq "<!--te-->" $gh_src; then
                    echo "Found markers"
                else
                    echo "You don't have <!--ts--> or <!--te--> in your file...exiting"
    ```

  * 核心是 grep -Fxq   “<!--ts-->"  file,于是我在终端打grep -Fxq "<!--ts-->"   file ，提示

  * zsh: event not found: -，坐蜡了，从来没遇到过这种报错，明明脚本里面就是这么写的，为啥原样打在终端就是不行，以为zsh的特有问题，于是用bash在试，报错：bash: !-: event not found，同样的问题，去百度，无果。遂放弃。

  * 于是又想，为啥test.md就可以找到，notes.md找不到，于是看grep -Fxq,fxq到底什么参数，F和q看不懂，但是-x, --line-regexp         force PATTERN to match only whole lines,整行匹配，突然灵机一动，要说test.md和notes.md有啥不一样，唯一的区别就是test.md是我在vim写的用来测试脚本的，notes.md实在windows下用typora写的，所以，没错结尾符号不一样，一个是\r\n,一个是\n所以不能整行匹配。

  * 没错，因为我用的是wsl所以，wsl里直接打开windows typora编辑好的notes.md文件，所以出现了这样的问题。于是乎  cat -A test.md    cat -A notes.md | head  ，果然notes.md\r\n结尾，有因为grep要求正行匹配，所以匹配失败，所以找不到<!--t-->,于是vim打开notes.md ,set fileformat=unix，在运行gh-md-toc，果然可行了，而且插入了toc，但是还是老问题，上传到githhub上还是没有缩进，服了。彻底放弃。

  * 于是开始研究grep,终端输入grep  <!--ts--> notes.md  不行，和grep "<!--ts-->"  notes.md 报错一样，但是输入grep '<!--ts-->' notes.md 可行，正常执行。

  * 遂去查grep 命令，但是并没有说到底是无引号还是单引号还是双引号。后来联想到shell脚本的单双引号和无引号的区别，顿悟了，见知识点。

* # 知识点

  * '\r'  ^M  0x0d CR    (Carriage Return 回车)

    * Carriage 运输，运送   return  返回，所以carriage return (CR)表示回车，意思光标回到行首
    * 用CR表示回车，用一个词表示就是return，所以简化为\r

  * '\n'  ^@或$  0x0a  LF  (Line Feed  换行)

    * line 线，行， Feed喂食，进刀（机床加工方面），Line Feed（LF）行进入下一个，换行之意
    * 用LF表示回车，用一个词表示就是newline，所以简化为\n

  * 查看文件末尾的不可见字符命令

    * cat -A  file,可以看文件换行结尾是\r\n(windows)还是\n(unix)还是\r（mac)
    * cat 也有好多参数，详见--help

  * 查看文件前几行

    * head file
    * head -n file

  * cat没法查文件前几行，head没法带参数查看文件，于是组合之

    * cat -A file | head -20

  * **vim 替换(首先输入冒号进入命令模式)(A表示被替换者，B表示替换后的值，可为空)**

    * s/A/B/g   替换当前行所有A为B
    * .s/A/B/g  同上，.表示当前行
    * $s/A/B/g  替换最后一行，$ 表示最后一行
    * ns/A/B/g  替换n指定的哪一行。
    * %s/A/B/g 替换所有行
    * x,ys/A/B/g 替换[x,y]区间内的所有行，x,y可以是具体的行数也可以是.表示当前行，$表示最后一行
    * 如果嫌上面的区间手动输入不好，可以先进入可选模式选中要替换的区域然后在输替换命令
  * A可以使字符串也可以是正则表达式，卧槽vim的替换搭配正则表达式，简直无敌。同理vim中的搜索也可以用正则表达式无敌。以后写代码爽翻了。
  
* shell脚本以及shell终端中单引号，双引号，无引号的区别
  
    * shell脚本中的区别笔记哪里已经记了，shell终端直接输入的区别基本同shell脚本中一样。唯一不同点就是双引号的区别。以上文的grep "<!--ts-->"为例
  * **脚本中的双引号只会识别转义符，引用变量，但是不会执行命令。**
    * **但shell终端的双引号会识别转义符，引用变量，执行命令**
    * 所以shell脚本里写grep "<!--ts-->"没有问题，单引号也行，但不能没引号。
      * 但是shell终端只能用单引号，因为<有重定向的命令，可能会匹配值命令执行，!也是特殊命令，用来匹配历史命令的，比如输入!10会执行历史文件中存的第10条命令，!!执行上一条命令。
    * 所以grep "<!--ts-->"终端报错bash: !-: event not found
      * 因为!右边没有匹配的参数，系统识别到!-命令，！不可以匹配-参数，报错。
  * **补充说明一点，终端中涉及字符串的参数，同样可以用 无引号，单引号，双引号，注意他们的区别了，比如如果是简单的无特殊字符，就可以  grep hello   notes.md,注意三者区别即可**
  
* Linux shell条件下将windows文本文件(\r\n结尾)转化为Linux文本文件
  
  * 为什么要这么转，因为一些linux下的处理文本文件脚本文件是针对Linux文本文件处理的，如果用它来处理windows下文本文件会产生一些莫名其妙的问题，所以最好转一下
  
  * 自动转换方式
  
    * vim打开进入命令模式输入: set fileformat=unix
  
    * 手动转换
  
    * vim -b  file  二进制方式打开文件，这样才会显示^M及\r,而且可编辑，意味着可删除替换
    
  * :%s/\r//g   将所有\r替换为空。搞定
      
  * vscode如何设置插件属性
    
    * 找到安装的插件，右键extension settings
  
  * vscode 如何设置换行符
    
      * 一般用eol（end of  line ） 表示文件结尾的字符设置
    * 找到设置，搜索eol即可，可设为\r,\r\n或者auto（跟随操作系统）
    
  * 关于Unix，Windows和Mac的结尾换行符
    
      * Unix中用\n 表示回车换行这两个动作，遇到换行符("\n")会进行回车+换行的操作，回车符反而只会作为控制字符("^M")显示，不发生回车的操作。
      * Windows用\r\n表示回车换行，windows中要回车符+换行符("\r\n")才会回车+换行，缺少一个控制符或者顺序不对都不能正确的另起一行
      * Mac用\r 表示回车换行两个动作
      * 一个直接后果是，Unix/Mac系统下的文件在Windows里打开的话，所有文字会变成一行；而Windows里的文件在Unix/Mac下打开的话，在每行的结尾可能会多出一个^M符号。
    * 道理是这么个道理，但是其实现在不管是windows下的typora或者vscode或者记事本还是Linux下 vim打开文本文件都会智能识别换行符，即windows下用各种编辑器打开unix文本文件，也可以流畅编辑，而且每行结尾加的是\n，同理vim打开windows文本文件可正常查看编辑且每行结尾加的是\r\n
    
      
      
      
      
      
      
      



