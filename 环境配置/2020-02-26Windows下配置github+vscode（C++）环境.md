* # 背景：
1. 此前一直在ubuntu下使用vscode简单C++编程，在GitHub建仓库记录。因某些原因，现需要在windows下配置一套相同的环境。
* # 解决方法：
  ## 1.配置git
  * #### 背景：
    *  linux下当时使用git就糊里糊涂的能用了，每次就是在github上新建一个仓库，然后用shell克隆仓库，然后git add,git commit,git push.这次借此在windows下配置git的机会，基本理清了如何使用git和github的基本操作过程。
  * #### 步骤:
    1. 安装git：
        - 基本就是安装git for windows。这个软件提供了几种操作git的方式，一是用自带的git bash shell，类似linux的shell，二是用powershell（powershell更强大支持cmd所有命令或cmd），三是GUI界面。一般情况下都推荐用git bash，操作类似linux shell。比较方便。但是最方便的就是利用window自带的linux子系统。我就是直接安装了ubuntu子系统。windows中的各个系统盘都挂载在/mnt下，可以在子系统中随意访问。然后打开ubuntu的shell，发现其已经安装了git。
    2. 配置基础信息：
       - 接下来就是git的常规操作。首先是配置用户名和邮箱，编辑器等。这里最关键的就是用户名和邮箱，这个是随意配置的，并不需要和github的账号一致。这个仅仅代表这个机器上的git的信息，以后如果这个机器推送仓库到github，那么就用这个信息来标识。到目前为止，就可以使用github了。可以进入一个随便的目录，然后使用https方式克隆一个自己github上的仓库，然后就可以本地修改使用了。如果要把修改后的结果推送到github，直接git push，然后输入github的用户名密码即可推送。这样的坏处就是每次git push 都要输入用户名密码。
    3. 配置SSH（可选）：
       - 如果不想每次git push都输入用户名密码。配置ssh即可。首先ssh -T git@github.com测试是否可以。一般都是可以的。然后使用ssh-keygen -t rsa 生成一对公私钥（网上都说ssh-keygen -t rsa -C "xx@xx.com" 命令，-C设置注释文字，比如邮箱，可省略）。生成过程会让你选择秘钥生成在哪里，默认都在用户家目录的.ssh目录中（windows在C:\Users\用户名\.ssh,linux在/home/用户名/.ssh）还会让你输入一个密码，可不输入。然后在.ssh生成一对秘钥。复制其中的公钥，在github上添加ssh秘钥，起个名字即可。这样，如果想克隆github上的某一个库，就可以使用ssh方式了，克隆该库后，以后本地修改再推送就不用再输密码了。
  * #### 新出现的问题：
   1. 配置完SSH之后用ssh格式git clone了一个仓库，当时是可以用的 ，但是第二天用ssh -T git@github.com 测试发现不能用了。
      * 方法：进入.ssh目录，就是那个本地存储公私钥对的目录。新建config文件。输入如下：
![](https://upload-images.jianshu.io/upload_images/8438096-9c64d46400ef5a87.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)其中User后面的邮箱可随便填，最好与github注册时邮箱一样或者与git配置的邮箱一样即可。然后更改config权限。输入：```sudo chmod 600 config```.搞定。 
      
   2. 配置完SSH后，之前用https方式克隆下来的仓库无法push。
       *  方法:进入那个仓库，输入（如果你之前已经一直使用https方式进行开发，当前想要切换成为ssh方式进行开发，只需要执行如下几步的操作即可）：
      ``` 
          git remote rm origin  
          git remote add origin  "Git仓库的ssh格式地址"
          git push origin
      ```
    
   3. 注意：配置完SSH之后再克隆库就只能使用SSH方式克隆了，不能再用HTTPS方式克隆了。
  
   4. 关于第三点，确实是这样的，如果迫不得已要克隆https的仓库，比如网上的教程只给了https方式的代码，很简单，改一下网址，https方式改成ssh格式即可。很简单的。
  
* #### 知识点
  * windows下powershell很强大，对标linux下的shell，现在windows取消了右键以管理员方式打开，因此如果某些文件鼠标打不开，可以以管理员方式打开powershell然后进入此文件目录打开这个文件。
  * 可以把powershell理解成windows平台的shell类似linux的各种shell。他真的很强大，有自己的一套各种命令，功能几乎等价于linux下的shell。也支持linux下的一些命令，如ls，rm等。
  * 在文件夹中时：shift+鼠标右键，可选在在cmd/powershell中打开，如果安装了WSL（Windows Subsystem for Linux）,还可选择在linux shell中打开。
  * powershell中输入cmd打开命令提示符。cmd中输入powershell打开powershell。powershell输入wsl（或linux子系统名）还可打开linux shell，如我输入ubuntu（或wsl）则在powershell中就打开了ubuntu的shell（输入exit则退出回到powershell）。还可在应用商店里打开。都说了powershell（cmd）就类似ubuntu下的终端。在里面输入程序名就可以打开相应的程序。linux子系统对windows来说就是个应用程序，无图形界面，所以在powershell中输入ubuntu就打开了ubuntu。或者可以点击桌面的ubuntu图标，出来一个专用的ubuntu shell。这类似在ubuntu终端输入vim，则打开了vim，摁个q退出回到终端。也可在桌面点击打开GVim，出来一个vim的gui。
  * WSL真的是一个值得好好研究一下的东西。从windows视角下看linux子系统，它的根目录在```C:\Users\海内存知己\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc\LocalState\rootfs```。从linux子系统视角下看windows，c,d,e盘都挂载在/mnt下。
  * 关于ssh，ssh（secure shell）ssh就是客户端访问服务端建立一个安全的访问。因为linux既能当客户端又能当服务端。当下载了ssh后。若其当客户端，无需任何设置。直接 ssh username@ip即可连接服务端。客户端用ssh表示。如果该机要充当服务端，得首先开启ssh服务端服务，用sshd表示。所以ssh表示客户端，sshd表示服务端，ssh——>sshd。ssh无需开，sshd是需要开的。当客户端用ssh username@ip连接服务端时每次连接都要输入用户名密码。如果不想这么麻烦。可以ssh-keygen生成一对公私钥，客户端保存私钥，把公钥发给服务端。之后再用ssh连接就不用每次都输入密码了。这就是上面github用ssh秘钥的原理。
  * **linux shell情况下发现使用mv或rm文件命令提示无权，用sudo也不行，因为文件正在使用中。。。但是不给提示。关了使用文件的程序即可。**（我就是先https克隆了一个仓库，然后才配的ssh，之后这个仓库就不能push了，后来按照上面的方法修改后。可以push了，但是想用git mv一个目录名字的时候发现怎么也改不了，直接用mv改也不行。然后以为还是git的原因，没配好。结果搜了搜了半天，才发现，原因就是文件正在使用中，所以不能改名，爷服了。）
  * 小技巧：关于克隆github上大仓库太慢的问题。网上说的改host，或者挂代理什么的就不说了。这里说一个小技巧。可以上码云（gitee）选择导入github仓库的形式新建仓库。（实则是白嫖码云的vpn），然后克隆。然后进仓库的.git 目录，修改config，配置其中的url为github那个仓库的URL（如果不会写这个URL，建议去看看那些直接从github克隆下来的仓库这个url是怎么写的）.这样就搞定了，git push就直接push到github上了。
  * 上文说了，配置git的时候用户名，邮箱随便填，确实是这样，但是如果邮箱与注册github账户的邮箱不同的话，这样push到仓库的是可以的。但是首页的图示（一片绿那个）不会计算这样的push。所以为了图示好看点，邮箱可以配置和github账户邮箱一样，用户名无所谓。官网的说法是这样的：Commits, issues, and pull requests will appear on your contribution graph. Only when the email address used for the commits in local configuration is associated with your GitOSC account, the commits' contribution will be counted。
* #### 总结：（针对自己管理自己github上的自己创建的仓库）
  * 配置git，github巨简单。概括来讲，注册github，下个git，随意配置信息，然后克隆仓库修改提交即可（只能用https克隆且每次提交都要输github的用户名密码）或生成ssh秘钥对，github复制其中的公钥，然后使用ssh方式克隆仓库。这样以后提交就无需密码。
2. ## 配置vsCodeC++编程环境
  * ### 背景：
    * vscode是挺好用的，尤其配上vim简直无敌，但就是一开始配置的时候很懵逼。但是在ubuntu上使用时也搞了半天还是不会配。后来就是在要开发的目录中添加.vscode目录，然后跟姜云鹏要来task.json和launch.json两个文件。这样就可以了。但是这样还有个问题就是不能进行多文件的编译。查了一下，如果是少量文件直接shell中用g++就完事了，或者修改一下tasks.json，添加所有源文件。不过这样每次一个不同的项目都要改tasks.json。。有点麻烦。解决多文件编译的问题的终极答案就是自己写makefile，然后配置到vscode中，不会搞。现阶段也无需求，就不搞了
* ### 解决步骤：
  1. 下载VScode：
     * 官网下载vscode最新版。然后装Brack Pair Colorizer，C/C++,C++ Intellisense,Code Runner(可选），Vim插件。
  2. 下载编译器（MinGW）（MinGW的一种以及一种下载方式）
      * ```www.mingw.org```下载一个```ming-get-setup.exe```.
      * 运行一路点，当出现要选择安装的组件的时候，基本就是带gcc,g++,gdb+bin/dev/lic/字眼的都安装上就完事了。
      * 进入当时选择安装的目录的bin查看是否有g++.exe；gcc.exe；gdb.exe.有的话说明安装成功。（我的是D:\MinGW\bin）
     * 然后Path里添加环境变量。添加完之后PS(powershell)或CMD中输入``` gcc -v;g++ -v,;gdb -v```测试编译器是否安装成功并添加到了环境变量中。我当时就是添加完立马在之前已经打开过得ps中测，发现不行。然后着急的就以为编译器安装失败了。其实没有。关了PS在打开，在运行，发现就可以了。所以添加完环境变量之后，测试不行重启PS，再不行就重启电脑（有得添加环境变量需要重启才能添加成功）。之后再不行再考虑是否添加失败。
     * 注：这里好像安装的是```MinGW32```,网上还有其他安装的```MingGW64```的方法。基本都是下载 一个setup或者installer安装器，运行后选择安装的组件就完了。核心是下载g++，gcc,gdb组件。
  3. 测试编译器
     *  其实当装完mingw并添加环境变量之后。此时就可以编程了。跟linux shell中一样。进入ps，新建一个cpp文件test.cpp，然后直接命令行g++ test.cpp就可以了，然后会生成一个test.exe的程序，.\test.exe运行即可。想调试，直接命令行gdb test.exe即可。此时若打开VScode编了一个程序，在没配置的情况下，装Code Runner就可以直接编译运行这个程序。其实本质就是调用g++编译程序，然后再运行，代替了手动在命令行敲命令。
  4. **配置VScode**
     * 新建一个程序目录Test,在里面编一个test.cpp.然后用Vscode打开Test目录
      * 摁F5,选择第一个```C++(GDB/LLDB)```,之后再选择第一个```g++.exe build and debug active file```.Ok,系统在工作目录（Test）生成一个.vscode目录保存配置信息，此时生成了launch.json配置文件（微调，下详述）。之后再F5,配置task,选择```C/C++:g++.exe build active file```然后系统又在.vscode生成tasks.json配置文件（不用改）。搞定，此时再F5运行程序，成功，黑框一闪而过。解决办法就是在程序末尾写```system("pause")```，还有就是可以在launch.json中配置，但没什么兴趣，就不弄了，手动加代码吧。
     * 所谓配置VScode可以运行调试代码，核心就是配置tasks.json，launch.json两个文件。task.json负责配置编译信息。launch.json负责配置运行（本质就是开始执行（不调试）一般都由调试程序负责）和调试 信息。我的tasks.json和launch.json分析如下（大部分都是系统自动生成的）：tasks.json:
    ```
    {
        "version":"2.0.0"
        "tasks":[
             {
                  "type":"shell",           
                  "label":"g++.exe build active file",
                  "command":"D:\\MinGW\\bin\\g++.exe",
                  "args":[
                         "-g",
                         "${file}",
                         "-std=c++11",
                         "-o",
                         "${fileDirname}\\${fileBasenameNoExtension}.exe"
                   ]   
                  "options" :{
                          "cwd":"D:\\MinGW\\bin"
                   },
                  "problemMatcher":[
                            "$gcc"
                  ],
                  "group" :"build"
             }
        ]
    }
    ```
     * 分析：
       *  其实task.json就是配置如何编译生成一个可执行程序的。vscode本质上是一个编辑器。
       * type表示这是shell类型，label表示这个task任务的名字，后面的launch.json会用到。
        * command表示该程序编译任务调用的编译器，填自己下载好的编译器即可。
        * args表示执行编译器时输入的参数。-g表示产生有调试信息的可执行文件。
        * \${file}表示被编译的文件名
        * ```-o + ${fileDirname}\\${fileBasenameNoExtension}.exe```表示输出的可执行文件名为被编译文件名的不含扩展名的名字+扩展名.exe
       * windows下路径名用\\,需要转义，所以两个\\\\,用一个/也可，会自动转换为win下路径名字    
       * 其实task.json本质就相当于在命令行中输入：（以test.cpp为例）
```g++ -g test.cpp -std=c++11 -o Test/test.exe```(command为g++程序的完整路径，如果加了环境变量，那么其实command参数直接写g++即可）。
launch.json：
```
{
   "version": "0.2.0",
   "configurations": [
       {
           "name": "(gdb) Launch", // 配置名称，将会在启动配置的下拉菜单中显示
           "type": "cppdbg", // 配置类型，这里只能为cppdbg
           "request": "launch", // 请求配置类型，可以为launch（启动）或attach（附加）
           "program": "${workspaceFolder}/${fileBasenameNoExtension}.exe", // 将要进行调试的程序的路径
           "args": [], // 程序调试时传递给程序的命令行参数，一般设为空即可
           "stopAtEntry": false, // 设为true时程序将暂停在程序入口处，一般设置为false
           "cwd": "${workspaceFolder}", // 调试程序时的工作目录，一般为${workspaceRoot}即代码所在目录 workspaceRoot已被弃用，现改为workspaceFolder
           "environment": [],
           "externalConsole": true, // 调试时是否显示控制台窗口，一般设置为true显示控制台
           "MIMode": "gdb",
           "miDebuggerPath": "D:\\MinGW\\bin\\gdb.exe", // miDebugger的路径，注意这里要与MinGw的路径对应
           "preLaunchTask": "g++.exe build active file", // 调试会话开始前执行的任务，一般为编译程序.
           "setupCommands": [
               {
                   "description": "Enable pretty-printing for gdb",
                   "text": "-enable-pretty-printing",
                   "ignoreFailures": false
               }
           ]
       }
   ]
}
```
  * 分析
       * 重点关注program参数，表示被调试器调试的文件的位置。这个一定要与task任务生成的可执行程序的位置对应。这里的workspaceFolder表示vscode打开的那个工作目录，除非你的可执行文件就在这个目录下。但是一般不在。所以更保险的做法就是：```${fileDirname}\\${fileBasenameNoExtension}.exe```
     * args相当于调用调试器（这里是gdb）时给他输入的参数，这里为空。
     * externalConsole如字面意思，是否打开外部的控制台窗口，windows就是黑框，linux下就是shell。不打开的话，程序结果显示在vscode下面内部的终端里。
     * miDebuggerPath如字面，给出调试器所在的路径以便调用。
     * preLaunchTask如字面，在launch任务之前需要调用的任务，显然就是编译任务及前文的task任务，这里的值一定要与task任务的label的值一样，才能保证调试的时候编译任务已经完成，即先进行task任务然后再进行launch任务。
      * launch比task复杂一点，原因在于，编译任务只需生成一个可执行程序就可以了。并不需要与vscode有任何交互，就是vscode上点编译，然后调用g++生成一个编译程序。而调试去不然，如果用命令行调试的话，设置断点，下一步，各种参数，中间变量等等全部要gdb 手动输入。这里为了让调试可以再vscode上以图形化的方式展现。自然需要更多的配置信息。但是本质还是把命令行的gdb调试图形化，简单化为类似在IDE上的调试体验。
    5. 问题排查
       * 刚配置好task.json和launch.json之后运行报错，看文字发现是![](https://upload-images.jianshu.io/upload_images/8438096-e853c70002bde1b3.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)即调试器找不到调试对象，也就是说那个可执行程序。但是此时task是正常的，已经生成了了exe程序。于是乎各种查，疯狂核对task.json的args -o后面的路径与launch.json 的program后面的路径发现是一样的。没有任何错误。后来才发现，原生的linux下的GCC是支持中文路径的。所以我在linux下中文路径没问题。但是**MinGW不支持中文路径，所以改名即可**。
        *  改完中文路径后又出现了这个问题：![](https://upload-images.jianshu.io/upload_images/8438096-a32cfd3bbaea89ce.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
大意是生成的那个test.exe格式不对？？emmm,明明已经是exe了为啥不对。后来仔细对比了自己的task文件和别人的task文件，发现command调用的是```D:\MinGw\bin\cpp.exe```,cpp.exe什么东西？后来才发现是当时选择配置task.json时不小心选了cpp，即指定了编译阶段调用cpp.exe。重配一个task选择g++.exe即可，或者直接command哪里改成g++（记得label也改成和launch.json的preLaunchTask一样）。现在可以了，那么问题来了，这个cpp.exe是个什么程序，他为什么执行生成的文件名也是.exe的。。。。。查了一下，我傻了。cpp是个预处理器程序（CPreProcessor），所以cpp test.cpp之后生成的是一个中间文件，就是预处理之后的结果。至于为什么叫exe，因为args那里指定输出的文件名扩展名是exe(...........emmmm).一般正常情况下他输出的文件扩展名为.ii。既然它是个中间文件，不是可执行文件，调试器当然识别不出来了，所以报错格式不对。。。。
* ### 知识点
  * x86表示32位，x64表示64位。
  * vscode在linux 和windows下应用程序名都叫code，所以不管是在linux shell或者powershell下打开vscode都需输入code而不是visual studio code。
  * 配置完环境变量测试不成功，试试重启电脑或者CMD再测试。
  * 一些下载错误可能不是安装的问题而是需要科学上网的问题
  * 找程序某种错误的时候能复制出错原文就复制出错原文，自己的描述去搜总是搜不到点上。要好好研究原文。
  * #### 关于GCC,gcc,g++,gdb:
    * GCC(GNU Compiler Collection)GNU编译器集合，它可以编译C、C++、JAV、Fortran、Pascal、Object-C、Ada等语言
    * gcc是GCC中的GUN C Compiler
    * g++是GCC中的GUN C++ Compiler（C++编译器）
    * 本质而言，gcc和g++并不是编译器，也不是编译器的集合，它们只是一种驱动器，根据参数中要编译的文件的类型，调用对应的GUN编译器而已，比如，用gcc编译一个c文件的话，会有以下几个步骤；
      1.调用预处理器，如cpp。
      2. 调用实际的编译器，如cc或cc1。
      3. 调用汇编器，如as
      4. 调用链接器，如ld
  * #### 常见的C/C++编译器：
    1. GCC（Linux原生）
        * Windows移植版：
            * MinGW(Minimalist GNU on Windows)
                * MinGW 是用于进行 Windows 应用开发的 GNU 工具链（开发环境），它的编译产物一般是原生 Windows 应用(32位)，虽然它本身不一定非要运行在 Windows 系统下（是的 MinGW 工具链也存在于 Linux、BSD 甚至 Cygwin 下）
               * MSYS(Minimal System):MSYS 是用于辅助 Windows 版 MinGW 进行命令行开发的配套软件包，提供了部分 Unix 工具以使得 MinGW 的工具使用起来方便一些。
               * MinGW-w64：前面提到的 MinGW，是针对 32 位 Windows 应用开发的。而且由于版本问题，不能很好的支持较新的 Windows API。MinGW-W64 则是新一代的 MinGW，支持更多的 API，支持 64 位应用开发，甚至支持 32 位 host 编译 64 位应用以及反过来的“交叉”编译。除此之外，它本身也有 32 位和 64 位不同版本，其它与 MinGW 相同。
             * Cygwin:运行于Windows平台的POSIX“子系统”，提供Windows下的类Unix环境，并提供将部分 Linux 应用“移植”到Windows平台的开发环境的一套软件.(超强大版的MinGW-w64+MSYS2)
            * TDM-GCC
    2. llvm+Clang
        * Low Level Virtual Machine (LLVM) 是一个开源的编译器架构，它已经被成功应用到多个应用领域。Clang ( 发音为 /klæŋ/) 是 LLVM 的一个编译器前端，它目前支持 C, C++, Objective-C 以及 Objective-C++ 等编程语言。Clang 对源程序进行词法分析和语义分析，并将分析结果转换为 Abstract Syntax Tree ( 抽象语法树 ) ，最后使用 LLVM 作为后端代码的生成器。
    3. Watcom C/C++
    4. MSVC系列
       * 与Visual Studio集成发布，微软自己的编译器，VS是一个基本完整的开发工具集，它包括了整个软件生命周期中所需要的大部分工具，如UML工具、代码管控工具、集成开发环境(IDE)等等。所写的目标代码适用于微软支持的所有平台，包括Microsoft Windows、Windows Mobile、Windows CE、.NET Framework、.NET Compact Framework和Microsoft Silverlight 及Windows Phone。
    5. Intel C++
        * Intel C++ Compiler （简称 icc 或 icl）是 Intel 公司开发的 C/C++编译器，适用于 Linux、Microsoft Windows 和 Mac OS X 操作系统.
* ### 总结
  * 开发弱智软件的整个流程是什么：编辑器打代码，编译器编译生成可执行程序，调试器调试。如果不用IDE的话在linux下就是vim打个.cpp文件，然后调用g++编译，然后用gdb调试。VScode为什么需要配置，因为本质上来说它就是个编辑器，强大一点的记事本而已。所以 所谓配置就是让它可以调用相应的编译器，调试器来实现一种轻量级IDE的功能。所以配置的第一步就是下载一个编译器，本文中是直接下载MinGw编译器，并配置了上去。当然，其他编译器也是可行的。比如本机就有VStudio，它自带了MSVC编译器，所以说根本不用下MinGW,让vscode调用msvc编译器即可，另外，如果机器下载了code::block或者Dev c++这样的IDE，它们自带MinGW，所以也可以用它们自带的MinGW编译器。用哪个，配那个不重要，那个简单配哪个。总之就是IDE下载下来的时候一般是集成编辑器，编译器和调试器。直接上手即可。而vscode需要给他配编译器和调试器就跟vim一样，你也可以给他配置编译器，调试器它也可以成为一个"IDE"。
  * 当开发稍微不那么弱智一点的程序的时候就出现了一个新的问题。比如一个项目有几十个源文件。如何编译就变得很重要了。一般都是写一个makefile然后调用make程序来正确编译。命令行下编译调试多个文件就需要自己写makefile，然后make。同理，vscode如果想编写涉及多个源文件的程序时，也需要在配置了编译器和调试器的情况下再配置make和Makefile。如果是IDE的话，这些工作它都帮你自动做了。。。
  * 当程序更复杂一些的时候，比如涉及连接数据库，UML图，git，多人协作，部署等等更复杂的要求时。我觉得命令行也可以做到，但是估计非常复杂。同理，vscode也可以做到，估计也需要很多插件，配置很多东西。这种情况下，你用vscode甚至命令行来开发，其实你就是在写一个IDE。不如用visual studio等一些强大的现成的IDE.他们非常强大，基本满足整个开发过程的一切要求。
