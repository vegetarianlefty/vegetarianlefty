### node 版本管理

node 更新版本过快，常常出现不兼容问题，工程跑不起来，需要更换不同的版本来兼容运行。比较常见的 node 版本管理工具有 n 和 nvm ，还有 fnm

### n 使用说明

#### 安装

```
sudo npm install -g n
#或使用 brew 安装
brew install n
```

#### 常用命令

在命令行中 输入 n 来查看，输出当前已经安装的node版本以及正在使用的版本（前面有一个o）

```
n

    node/12.22.5
  ο node/14.17.5
    node/16.8.0

Use up/down arrow keys to select a version, return key to install, d to delete,q to quit
/**********用法**********/
1.使用上下箭头选择对应的版本
2.选择好版本之后，按回车键切换当前node版本
3.选择好版本之后，敲[d]键删除对应版本的node
4.输入[q]退出管理界面

n latest  //安装最新版本并使用  
n latest -d   //下载最新版但不使用，-d参数表示为仅下载  
n stable  //安装最新稳定版本并使用  
n <version>  //安装某个版本并使用，如$n 6.2.2  
n rm <version ...> //删除某些版本  
n ls    //查看可用版本  
n --latest    //查看最新版本  
n --stable    //查看最新稳定版  
n -h    //查看帮助信息，更多命令在这里查看  
```

#### 问题解决办法

管理范围问题

n只能管理自己下载的node。若还未用n下载node，单独使用n指令会出现下图报错。

报错信息

![image-20230224171033203](http://image.wangxiaohuan.com/blog/image/202302241710850.png)

权限问题

若n的指令报出以下错误，在指令前添加sudo，填写本机密码即可正常使用

报错信息

![image-20230224171139490](/Users/wangxiaohuan/Library/Application Support/typora-user-images/image-20230224171139490.png)



### nvm 使用说明

nvm 全称 Node Version Manager ,是一个独立软件包

#### 安装方式

```ruby
# 方式一
$ curl https://raw.github.com/creationix/nvm/v0.4.0/install.sh | sh
# 方式二
$ wget -qO- https://raw.github.com/creationix/nvm/v0.4.0/install.sh | sh
# 以上脚本会把nvm库clone到~/.nvm 然后会在~/.bash_profile, ~/.zshrc或`~/.profile末尾添加source，安装完成之后，你可以用以下命令来安装node
```

#### 常用命令

```cobol
nvm install version   //安装node， version是特定版本也可以是最新稳定版本latest
nvm uninstall <version>       //卸载指定版本node
nvm list            //显示已安装的列表,可简写为 nvm li
nvm version         //显示nvm版本,version可简化为v。
nvm on              //开启node.js版本管理
nvm off             //关闭node.js版本管理
nvm use 0.10         //使用指定的版本   
      
nvm ls               //查看当前已经安装的版本  
.nvm  
->  v0.10.24  
      
nvm current         //查看正在使用的版本  
-> v0.10.24  
      
nvm run 0.10.24 myApp.js      //以指定版本执行脚本  

rm -rf ~/.nvm     //卸载nvm
```



### n 和 nvm 区别

n 是一个 npm 包，说白了，就是 node 自己管自己，一但当前 node 出问题，就切不过去了，另外，经常需要用到 sudo 权限


nvm 最近一个比较严重的问题是，nvm 与 prefix 不兼容，还有就是慢

n 这种安装方式更符合 node 的惯性思维

nvm 安装起来显然是要麻烦不少

n 对全局模块毫无作为，因此有可能在切换了 node 版本后发生全局模块执行出错的问题

nvm 的全局模块存在于各自版本的沙箱中，切换版本后需要重新安装，不同版本间也不存在任何冲突

n 是万年不变的 /usr/local/bin

nvm 需要手动指定路径



### npm 设置镜像源

```text
npm ------- https://registry.npmjs.org/
cnpm ------ http://r.cnpmjs.org/
taobao ---- http://registry.npmmirror.com
nj -------- https://registry.nodejitsu.com/
rednpm ---- http://registry.mirror.cqupt.edu.cn/
npmMirror - https://skimdb.npmjs.com/registry/
edunpm ---- http://registry.enpmjs.org/
jnpm ------ http://registry.m.jd.com/
jd -------- http://registry.m.jd.com/


sudo npm config set registry http://registry.npmmirror.com

使用nrm镜像源管理工具
npm install nrm -g

查看列表
nrm ls

查看当前源
nrm current

切换源 npm 下载地址
nrm use taobao

新增镜像源
nrm add xx https://xxx.com

删除镜像源
nrm del xxx

测试源速度
nrm test xxx
```