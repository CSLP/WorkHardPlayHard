* # 背景

  * 工欲善其事必先利其器，听说zsh很强大，完全兼容bash，so决定一试一试。（其实是搞动态规划代码搞得头皮发麻，想偷会儿懒）

* # 安装

  * ### WSL

    * #### 步骤

      * 前提：因为wsl配置了ssh，而网上搜的大部分安装方法都是用https方式克隆。所以不能那么安装。所以要**手动安装**（详情见下面的辛酸泪）。如果没有配置SSH，那么就像下文的ubuntu那样安装即可，很简单。

        1. cat /etc/shells  查看可用shell

        2. echo $SHELL 查看默认shell

        3. echo $0 查看当前使用的shell。

        4. sudo apt-get install zsh 安装zsh

        5. ```git
           git clone git@github.com:ohmyzsh/ohmyzsh.git ~/.oh-my-zsh
           在家目录创建一个.oh-my-zsh目录并将仓库克隆到这里
           ```

        6. ```
           cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
           刚安装zsh是无.zshrc配置文件的，你可以自己写一个，网上找一个，或者直接复制.bashrc的内容即可，因为zsh完全兼容bash.或者不管等下载完oh-my-zsh后用它的。
           如果你要保存原来的.zshrc可备份。
           ```

        7. ```
           chsh -s $(which zsh)
           改变默认shell
           ```

        8. logout 或者重启使改变生效。

        9. 重开一个终端出现以下情况

           ```
           [oh-my-zsh] Insecure completion-dependent directories detected:
           drwxrwxrwx 1 wuyulp wuyulp 4096 Mar  4 23:07 /home/wuyulp/.oh-my-zsh
           drwxrwxrwx 1 wuyulp wuyulp 4096 Mar  4 23:07 /home/wuyulp/.oh-my-zsh/plugins
           drwxrwxrwx 1 wuyulp wuyulp 4096 Mar  4 23:07 /home/wuyulp/.oh-my-zsh/plugins/git
           
           [oh-my-zsh] For safety, we will not load completions from these directories until
           [oh-my-zsh] you fix their permissions and ownership and restart zsh.
           [oh-my-zsh] See the above list for directories with group or other writability.
           
           [oh-my-zsh] To fix your permissions you can do so by disabling
           [oh-my-zsh] the write permission of "group" and "others" and making sure that the
           [oh-my-zsh] owner of these directories is either root or your current user.
           [oh-my-zsh] The following command may help:
           [oh-my-zsh]     compaudit | xargs chmod g-w,o-w
           
           [oh-my-zsh] If the above didn't help or you want to skip the verification of
           [oh-my-zsh] insecure directories you can set the variable ZSH_DISABLE_COMPFIX to
           [oh-my-zsh] "true" before oh-my-zsh is sourced inyour zshrc file.
           ```

        10. 大意是这是手动安装吗，可能一些设置没有完全安好。仔细读文字发现给了修复方案。就是去掉上面列出的三个文件的同组和其他人员的写权限即可。

        11. ```
            sudo chmod 755 ~/.oh-my-zsh
            sudo chmod 755 ~/.oh-my-zsh/plugins
            sudo chmod 755 ~/.oh-my-zsh/plugins/git
            ```

        12. 修改完权限之后新开一个终端，ok，没有提示信息了，搞定。

        13. vim ~/.zshrc   设置ZSH_THEME="ys".保存退出

        14. source ~/.zshrc 使改动生效。OK终于大功告成。以后再慢慢体验zsh的强大之处吧。

    * #### 知识点

      * 从windows视角下操作wsl文件，尤其是比如直接在wsl根目录中增删文件，需要重启wsl才生效
      * 还是要学一下shell编程啊。不然遇到这种问题。太难了。其实install.sh 就是个脚本，平常运行脚本直接./install.sh即可运行，表示用当前使用的shell来运行这个脚本。而上述命令中的sh不是别的，正是最老的shell，它的名字就是sh，所以上述命令中的sh的意思就是用sh shell来运行install.sh脚本。哭了。
      * 硬着头皮看官方英文文档吧，啥都有，强过自己瞎几把安装，瞎几把搜索引擎。真的看似看英文慢，实则是解决问题的最好办法。遇到问题先简单搜索一下，搜不到就搜英文，还搜不到别硬瞎几把搜了，看官方的文档，一秒钟搞定。。唉。
      * 谷歌学术国内打不开，bing学术可以打开，也不错。
      * 高级搜索技巧：site:csdn.com冒号为英文。 “大数据规模“冒号表示精准匹配。
      * oh-my-zh不是简单的配置一下zsh的配置文件就完了，它是一个框架，不仅配置了zsh的配置文件，还提供了各种插件，主题等待探索的东西。
      * git clone https://github.com/ohmyzsh/ohmyzsh.git ~/.oh-my-zsh  用了这么久的github了这都没看出来，第一个ohmyzsh是用户名，第二个ohmyzsh是用户的仓库名
      * 本篇文章涉及的git clone或者wget后面的都是ohmyzsh，但是链接稍有不同。不影响观看。因为他们都是github上的，有的是最原始的用户的库，有得是别人fork了的库，其实都是一样的，只不过是从不同用户那里克隆罢了。本质下载、克隆的是同一个东西。
      * **有两种克隆方式，可以简单的推断出来**
        * https方式：```git clone https://github.com/user/repo.git```
        * ssh方式：```git clone  git@github.com:usr/repo.git```

    * #### 各种问题汇总（辛酸泪)

      * sudo apt-get install zsh没问题。然后使用 chsh -s /bin/zsh 切换，发现echo $SHELL,echo $0没有变化。只有显式输入zsh才进入zsh。后来关了wsl再开才行。所以同真正的ubuntu，改shell需log out或重启.

      * 然后就是```sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"```这句或者```wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh```这句。

      * 前面一句报错，Unable to establish SSL connection.后面一句报错ERROR: no certificate subject alternative name matches。大意差不多。都是建立不了SSL连接或者验证不了证书，就是ssl连接需要验证，认证吗，然后都报错所以连不上，认证不了。后面一句提示加--no-check-certificate参数

      * 于是乎第二句开始加这个参数，不知道加在那个位置于是加在了-O -后面，于是报错，一脸懵逼。(哦，对了，这里是字母大O,不是数字0，一般linux平台0中间有一个点表示0，而O中间没有)。

      * 于是乎查wget不能建立SSL连接这个错误，于是乎才发现参数是加在wget后面的。遂加在第二句的wget后面，成功执行，但是会一直不断地重连，就是连不上。于是乎改用第一句加参数。直接报连不上错误。于是乎又陷入僵局。

      * 受以前安装typora的启发，于是乎用浏览器直接打开https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh,发现就是一个install.sh文件，也就是说是一个脚本文件。于是选择另存为把它存了下来。但是当时还是搞不清```wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh```这句命令的意思，尤其不知道这个sh是什么东西。后来才知道，很简单，其实是wget命令把这个install.sh脚本文件下下来由管道交给sh执行。sh表示sh shell。这句命令等价于

        ```shell
        wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh(把脚本下载下来)
        sh install.sh (用sh执行)
        ```

        但是当时啥都不知道啊，把install.sh下载下来了，其实直接sh install.sh运行就行了，也没有看看这个脚本的内容，唉。此路断了。

      * 于是乎两句加--no-check-certificate都不管用，遂查之。千查万查，百度查，必应中文查，英文查。终于查到，改参数不可用可试试https改成http。遂两句都试都改。

      * 依然失败。然后莫名其妙，第一句加参数这个突然成功。后来发现，第一句加参数，可能不成功，也可能成功。多试几次就成功了。于是乎试几次终于连接成功。

      * 好吗，连接成功，问题又来了

        ```shell
        wuyulp@LAPTOP-P65UEB1C:~$ sh -c "$(wget --no-check-certificate  https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
        Will not apply HSTS. The HSTS database must be a regular and non-world-writable file.
        ERROR: could not open HSTS store at '/home/wuyulp/.wget-hsts'. HSTS will be disabled.
        --2020-03-04 21:02:34--  https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh
        Resolving raw.github.com (raw.github.com)... 151.101.108.133
        Connecting to raw.github.com (raw.github.com)|151.101.108.133|:443... connected.
        HTTP request sent, awaiting response... 301 Moved Permanently
        Location: https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh [following]
        --2020-03-04 21:02:37--  https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh
        Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 151.101.108.133
        Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|151.101.108.133|:443... connected.
        HTTP request sent, awaiting response... 200 OK
        Length: 8445 (8.2K) [text/plain]
        Saving to: ‘STDOUT’
        
        -                             100%[=================================================>]   8.25K  --.-KB/s    in 0s
        
        2020-03-04 21:02:38 (36.2 MB/s) - written to stdout [8445/8445]
        
        Cloning Oh My Zsh...
        Cloning into '/home/wuyulp/.oh-my-zsh'...
        fatal: unable to access 'https://github.com/ohmyzsh/ohmyzsh.git/': SSL: certificate subject name (*.kandian.qq.com) does not match target host name 'github.com'
        Error: git clone of oh-my-zsh repo failed
        ```
        
    * 到19句，说明连接完成，一切安好。20句往后，克隆失败。而且这错误信息是如此的熟悉。
      
    * 没错，windows刚配置git，然后配置ssh后就遇到了这样的问题，一模一样。问题就是配置完ssh后不能以https方式克隆包了。这句命令就是连上这个网站，然后克隆这个包，然后运行包中的这个install.sh完成zsh的配置。(后面发现其实不完全是这样)。
      
    * 于是乎想的就是配置完ssh如何再克隆https的包，然后就去搜这个话题，搜来搜去怎么都搜不到，我就无语了，难道没有人有这个问题？大家配置完ssh之后就再也不用https方式克隆包了。好像确实如此，因为没人问这个问题。陷入僵局，好，那么既然配置了ssh后不知道如何克隆https方式的包，那么关掉ssh就行了吧？
      
    * 首先是自己试了试，就是把家目录下的当时配置ssh时候生成的.ssh目录改名，想的是这样他就不用ssh了，结果失败。然后去搜如何关掉git 的ssh登陆，结果搜不到。唯一搜到的就是ssh登陆与自己没关，与仓库有关，要设置仓库。看不懂，放弃。然后还有一种方案就是彻底删除ssh公私钥，本地的和github上的。不想这么干。遂也放弃。又陷入僵局。
      
    * 于是去英文搜如何关掉git的ssh，搜了半天才发现了用什么关键词搜，应该用disable 这个词表示关闭。比如这样搜：How to disable SSL verification for git或者How to disable SSH in GitBash and use HTTPS instead。最终结果指向一个关键参数http.sslVerify.于是乎继续搜找到了这个[Dealing with misconfigured https repositories](https://git.seveas.net/dealing-with-misconfigured-https-repositories.html)大致内容如下：
      
    * Disabling SSL certificate verification puts your data at risk
      
      This article was written to counteract some really stupid advice to disable SSL certificate verification completely. Never *ever* do `git config --global http.sslVerify false` or a grue will absolutely eat you.
      
      When dealing with an https host that has a misconfigured SSL certificate, such as a self-signed or expired certificate, the correct action to take is to fix the problem with that certificate. But you're not always in the position to do so, so there are workarounds.
      
      Git will let you disable SSL certificate verification on a global, per host or per command invocation basis. But before you do any of this, you have to understand that this is a really bad thing: it opens you up to man-in-the-middle attacks and you should really consider all data (including passwords) sent this way to be compromised.
      
      With that lecture out of the way, here's how you actually do this without compromising security too much.
      
      ```
        $ git clone https://git.example.com/example.git
        Cloning into 'example'...
        fatal: unable to access 'https://git.example.com/example.git/': SSL: certificate subject name (www.example.com) does not match target host name 'git.example.com'
        $ git -c http.sslVerify=False clone https://git.example.com/example.git
        remote: Counting objects: 404, done.
        remote: Compressing objects: 100% (261/261), done.
        remote: Total 404 (delta 227), reused 235 (delta 131)
        Receiving objects: 100% (404/404), 124.40 KiB | 0 bytes/s, done.
        Resolving deltas: 100% (227/227), done.
        Checking connectivity... done.
        ```
      
      看到这里，开心了，卧槽，这不是与我配置了ssh后克隆https包失败的情况一模一样吗，原来就是验证不匹配，misconfigured，另外配置ssh后克隆https失败，果然是与自己无关，需要被克隆的仓库有相关的设定。ok，原来加了-c http.sslVerify参数即可解决这个问题，太棒了，终于要搞我的zsh了。于是花加了-c http.sslVerify参数，于是乎终于不报SSL验证不匹配问题了，但是又出现了以下问题：fatal: unable to access 'https://github.com/CSLP/ohmyzsh.git/': Empty reply from server。吐了。还是不能访问，彻底服务器无回复。于是乎心态彻底崩塌，搞这个搞太久了，都没弄数据结构了，要去弄了，于是乎此时临近放弃边缘。决定最后一试，去ubuntu一试。
        
    * 果然就是那三板斧，下载zsh，装oh-my-zsh，因为那个没有配置ssh。一开始遇到ssl连接不上，直接加--no-check-certificate搞定，install.sh执行起来，安装完毕，卧槽，真好看，真好用。安装的过程有意外之喜，找到了一个这个教程：
      
      * １、安装zsh
      
        ```
        sudo apt-get install zsh
        ```
      
        ２、安装　oh my zsh
      
        ```
        git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
        ```
      
        ３、配置文件
        3.1创建配置文件
      
        ```
        cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
        ```
      
      3.2设置zsh为默认的shell
      
        ```
        chsh -s /bin/zsh
        ```
      
        此时重新启动Ubuntu即可享受新的终端.
      
        ４、注意事项
        4.1修改主题
        可以去https://github.com/robbyrussell/oh-my-zsh/wiki/themes看看你喜欢什么主题，然后在.zshrc文件中进行更改
      
      * 唉，意外之喜，这个教程是ssh方式克隆，我的wsl准行。遂记住这个网址返回wsl.
      
      * 严格按照这个执行，嘿，不错zsh运行了起来，但是刚一打开，出来以下提示:
      
        ```
        [oh-my-zsh] Insecure completion-dependent directories detected:
        drwxrwxrwx 1 wuyulp wuyulp 4096 Mar  4 23:07 /home/wuyulp/.oh-my-zsh
        drwxrwxrwx 1 wuyulp wuyulp 4096 Mar  4 23:07 /home/wuyulp/.oh-my-zsh/plugins
        drwxrwxrwx 1 wuyulp wuyulp 4096 Mar  4 23:07 /home/wuyulp/.oh-my-zsh/plugins/git
        [oh-my-zsh] For safety, we will not load completions from these directories until
        [oh-my-zsh] you fix their permissions and ownership and restart zsh.
        [oh-my-zsh] See the above list for directories with group or other writability.
      
        [oh-my-zsh] To fix your permissions you can do so by disabling
        [oh-my-zsh] the write permission of "group" and "others" and making sure that the
        [oh-my-zsh] owner of these directories is either root or your current user.
        [oh-my-zsh] The following command may help:
        [oh-my-zsh]     compaudit | xargs chmod g-w,o-w
      
        [oh-my-zsh] If the above didn't help or you want to skip the verification of
        [oh-my-zsh] insecure directories you can set the variable ZSH_DISABLE_COMPFIX to
        [oh-my-zsh] "true" before oh-my-zsh is sourced inyour zshrc file.
        ```
      
      看到第一句，不安全完全依赖目录被探测到了。我都没看后面，又断定这样安装失败，不如那种自动化运行脚本，这样虽然zsh运行起来了，但是坑定缺东西了，遂又投入了普遍安装方式。心态稍微平稳一些。此时在看这个错误信息：
      
    * ```
      wuyulp@LAPTOP-P65UEB1C:~$ sh -c "$(wget --no-check-certificate  https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
      Will not apply HSTS. The HSTS database must be a regular and non-world-writable file.
      ERROR: could not open HSTS store at '/home/wuyulp/.wget-hsts'. HSTS will be disabled.
      --2020-03-04 21:02:34--  https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh
      Resolving raw.github.com (raw.github.com)... 151.101.108.133
      Connecting to raw.github.com (raw.github.com)|151.101.108.133|:443... connected.
      HTTP request sent, awaiting response... 301 Moved Permanently
      Location: https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh [following]
      --2020-03-04 21:02:37--  https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh
      Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 151.101.108.133
      Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|151.101.108.133|:443... connected.
      HTTP request sent, awaiting response... 200 OK
      Length: 8445 (8.2K) [text/plain]
      Saving to: ‘STDOUT’
      
      -                             100%[=================================================>]   8.25K  --.-KB/s    in 0s
      
      2020-03-04 21:02:38 (36.2 MB/s) - written to stdout [8445/8445]
      
      Cloning Oh My Zsh...
      Cloning into '/home/wuyulp/.oh-my-zsh'...
      fatal: unable to access 'https://github.com/ohmyzsh/ohmyzsh.git/': SSL: certificate subject name (*.kandian.qq.com) does not match target host name 'github.com'
      Error: git clone of oh-my-zsh repo failed
      ```

    * 在结合搜索了这么久的一点认知，网上说安装oh-my-zsh其实就是由install.sh这个脚本完成的。刚才直接克隆了那个ohmyzsh包。看这条命令，于是进入相应路径找到了install.sh这个脚本，于是乎./install.sh直接运行。报错，说.oh-my-zsh已存在。于是我又把.oh-my-zsh改名为ohmyzsh ，继续运行，唉，这次直接卡到这一步:

    * ``` 
      Cloning Oh My Zsh...
      Cloning into '/home/wuyulp/.oh-my-zsh'...
      fatal: unable to access 'https://github.com/ohmyzsh/ohmyzsh.git/': SSL: certificate subject name (*.kandian.qq.com) does not match target host name 'github.com'
      Error: git clone of oh-my-zsh repo failed
      ```

    * 卧槽，终于弄懂了，脚本里坑定有这个克隆的命令。现在也有点思路，就是既然无法克隆https方式的包，克隆ssl方式的不就行了，每个仓库都可以通过两种方式克隆的，而且稍微修改一下即可。于是乎，打开install.sh一看，恍然大悟，主要以下几个字段

    * ```shell
      #!/bin/sh
      #
      # This script should be run via curl:
      #   sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
      # or wget:
      #   sh -c "$(wget -qO- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
      #
      # As an alternative, you can first download the install script and run it afterwards:
      #   wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh
      #   sh install.sh
      ```

      此时搞懂了sh是什么，继续看：

      ```shell
      # Respects the following environment variables:
      #   ZSH     - path to the Oh My Zsh repository folder (default: $HOME/.oh-my-zsh)
      #   REPO    - name of the GitHub repo to install from (default: ohmyzsh/ohmyzsh)
      #   REMOTE  - full remote URL of the git repo to install (default: GitHub via HTTPS)
      #   BRANCH  - branch to check out immediately after install (default: master)
      #
      ```

      看第4句，卧槽，和我想的一样，默认是https方式。继续看：

      ```shell
      # Default settings
      ZSH=${ZSH:-~/.oh-my-zsh}
      REPO=${REPO:-ohmyzsh/ohmyzsh}
      REMOTE=${REMOTE:-https://github.com/${REPO}.git}
      BRANCH=${BRANCH:-master}
      
      ```

      卧槽，REMOTE参数，罪魁祸首找到了，接下里改这个remote改为ssh方式就可以了。遂改为

      ```shell
      REPO=${REPO:-ohmyzsh/ohmyzsh}
      REMOTE=${REMOTE:-git@github.com:${REPO}.git}
      ```

      这下应该没问题了吧。sh install.sh，报错，这次没问题了，但是是报连不上22号端口，ssl服务不可用错误。

      ```s
      ssh: connect to host github.com port 22: Resource temporarily unavailable
      fatal: Could not read from remote repository.
      
      Please make sure you have the correct access rights
      and the repository exists.
      Error: git clone of oh-my-zsh repo failed
      ```

      此时坐蜡。心一横，不能放弃。遂决定直接到repo指定的github仓库ohmyzsh/ohmyzsh一看（这才是最应该第一时间或者第二时间应该解决问题的方式）。于是去看了，看到了README，于是硬着头皮再看，哈哈开心了，看到了关键部分

      #### Installing from a forked repository

      The install script also accepts these variables to allow installation of a different repository:

      - `REPO` (default: `ohmyzsh/ohmyzsh`): this takes the form of `owner/repository`. If you set this variable, the installer will look for a repository at `https://github.com/{owner}/{repository}`.

      - `REMOTE` (default: `https://github.com/${REPO}.git`): this is the full URL of the git repository clone. You can use this setting if you want to install from a fork that is not on GitHub (GitLab, Bitbucket...) or  **if you want to clone with SSH instead of HTTPS (`git@github.com:user/project.git`).**

        *NOTE: it's incompatible with setting the `REPO` variable. This setting will take precedence.*

      - `BRANCH` (default: `master`): you can use this setting if you want to change the default branch to be checked out when cloning the repository. This might be useful for testing a Pull Request, or if you want to use a branch other than `master`.这个就是刚才我试的改install.sh 的REMOTE的方式，又去试了一遍，报相同错，还是不行。于是在往下看：

      - #### Manual Installation

        ##### 1. Clone the repository:

        ```
        git clone https://github.com/ohmyzsh/ohmyzsh.git ~/.oh-my-zsh
        ```

        ##### 2. *Optionally*, backup your existing `~/.zshrc` file:

        ```
        cp ~/.zshrc ~/.zshrc.orig
        ```

        ##### 3. Create a new zsh configuration file

        You can create a new zsh config file by copying the template that we have included for you.

        ```
        cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
        ```

        ##### 4. Change your default shell

        ```
        chsh -s $(which zsh)
        ```

        You must log out from your user session and log back in to see this change.

        ##### 5. Initialize your new zsh configuration

        Once you open up a new terminal window, it should load zsh with Oh My Zsh's configuration.

        哦哦哦，原来上面我已经试过的那种办法就是手工安装的办法，我当时就疑惑直接拷贝一个.zshrc文件就行了吗，不会出错吗，后来果然出错了，于是认为那种方法不行。原来却是可以，拷贝了它就会加载相关配置。于是乎又这样搞了一遍，不出意外，还是出了那个不安全目录探测的那个错。一开始想不管了，后来发现每次打开都要弹一遍。遂认真看了一遍，原来也不是什么难事，改一下列出的三个目录的权限。终于搞定了！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！

      - 提示信息很有用，别一看到英文的提示信息就头皮发麻，去瞎几把搜索，其实很简单的。啊，搞了两天，终于搞定了，话不多说，赶紧去搞数据结构了。

  * ### Ubuntu 
    * #### 步骤
      
      1. 因为这个Ubuntu上git没有配置ssh的缘故,直接脚本一键安装zsh简直太爽了,反之wsl上就因为配置了一个git的ssh链接,我曹,搞了我一整天,真的现在心态稳健多了,没那么容易崩了,放以前现在鼠标电脑必须烂一个了. 
      2. ```cat /etc/shells```查看可用的shell
      3. ```echo $SHELL ``` 查看默认的shell
      4. ```echo $0```查看当前正在使用的shell
      5. 安装zsh ```sudo apt-get install zsh```
      6. 安装oh-my-zsh ```sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"```这一步大概率会报错,错误信息大同小异,反正就是连不上这个网址,因为证书不匹配,验证不通过.办法就是加```--no-check-certificate```参数,加在wget 后面,也就是这样```wget --no-check-certificate https://....```即可.一般这样就行了,如果还不行,可以试试https改成http即可.如果还不行,就这两种方法一直试,试个十几次就坑定行了.
      7. ok,上一步成功后就安装了zsh,想改主题的话```vim ~/.zshrc```,推荐ys和af-magic.修改后```source ~/.zshrc```生效.搞定.
      8. 此时```echo $SHELL ```或```echo $0```都显示zsh.但是新开一个终端,唉?出来的还是bash,然后这两条命令显示的结果都变成了bash.明明安装oh-my-zsh脚本的时候选择了zsh作为默认shell,没生效吗?对,就是没生效,重启Ubuntu或者log out一下就生效了,此时默认shell即为zsh.
      9. 如果想使用别的shell,如bash,直接终端输bash即可.如果想改变默认shell,先```cat /etc/shells```查看当前可用shell.然后```chsh -s shell名```然后log out在登录后生效.
      
    * #### 知识点
    
      * wget失败可以试试加--no-check-certificate参数或者https改为http。一般多试几次就行了，实在不行直接浏览器访问那个网址下载要下的东西，然后在执行之后的相应步骤。