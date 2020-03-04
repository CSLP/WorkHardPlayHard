* # 背景

  * 工欲善其事必先利其器，听说zsh很强大，完全兼容bash，so决定一试一试。（其实是搞动态规划代码搞得头皮发麻，想偷会儿懒）

* # 安装

  * ### WSL

    * 
  * ### Ubuntu 
    * 步骤
      1. 因为这个Ubuntu上git没有配置ssh的缘故,直接脚本一键安装zsh简直太爽了,反之wsl上就因为配置了一个git的ssh链接,我曹,搞了我一整天,真的现在心态稳健多了,没那么容易崩了,放以前现在鼠标电脑必须烂一个了. 
      2. ```cat /etc/shells```查看可用的shell
      3. ```echo $SHELL ``` 查看默认的shell
      4. ```echo $0```查看当前正在使用的shell
      5. 安装zsh ```sudo apt-get install zsh```
      6. 安装oh-my-zsh ```sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"```这一步大概率会报错,错误信息大同小异,反正就是连不上这个网址,因为证书不匹配,验证不通过.办法就是加```--no-check-certificate```参数,加在wget 后面,也就是这样```wget --no-check-certificate https://....```即可.一般这样就行了,如果还不行,可以试试https改成http即可.如果还不行,就这两种方法一直试,试个十几次就坑定行了.
      7. ok,上一步成功后就安装了zsh,想改主题的话```vim ~/.zshrc```,推荐ys和af-magic.修改后```source ~/.zshrc```生效.搞定.
      8. 此时```echo $SHELL ```或```echo $0```都显示zsh.但是新开一个终端,唉?出来的还是bash,然后这两条命令显示的结果都变成了bash.明明安装oh-my-zsh脚本的时候选择了zsh作为默认shell,没生效吗?对,就是没生效,重启Ubuntu或者log out一下就生效了,此时默认shell即为zsh.
      9. 如果想使用别的shell,如bash,直接终端输bash即可.如果想改变默认shell,先```cat /etc/shells```查看当前可用shell.然后```chsh -s shell名```然后log out在登录后生效.
        
