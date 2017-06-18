## github

    
我是在步入社会后第二年接触到GitHub，在这之前完全不知道有这个玩意在(太无知了)。在杭州，也是我才进入游戏行业的第一年，当然是了解[skynet](https://github.com/cloudwu/skynet),然后慢慢的喜欢上了，期间有很多断断续续的尝试，我很幸运。

### 常用操作

* git clone
* git pull
* git init
* git push
* 添加本地远程库：
    * git init --bare (server)初始化一个空仓库
    * git remote add origin ssh://git@HOST/WHERE/WHAT.git
    * git push orgin master
    * 对应的拉取：git clone git@HOST:/WHERE/WHAT.git

### 拉取fork更新
* 在new pull request的时候 请注意 *base fork* 和 *head fork*对应的仓库名字，时候是以自己的fork的仓库为*base fork*,不然你就会闹和我一样的笑话了。
* *这个我还闹了个笑话，在准备拉去TensorFlow更新的时候，没注意选择 **base fork**和**head fork**提交一个pull request到TensorFlow的仓库上去了*

*如果我遇到新的东西，我会尝试更新这个文件。*