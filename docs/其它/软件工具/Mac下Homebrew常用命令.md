### [Homebrew](https://brew.sh/index_zh-cn.html) 是什么

Homebrew 是一款 Mac OS 平台下的软件包管理工具，拥有安装、卸载、更新、查看、搜索等很多实用的功能。简单的一条指令，就可以实现包管理，而不用你关心各种依赖和文件路径的情况，十分方便快捷。

### Homebrew 的安装方法

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Homebrew 常用命令

```
brew -v           //查看当前版本

brew config       //查看当前版本,详细安装环境信息

brew udpate       //更新 Homebrew

brew install git  //安装软件包

brew outdated     //查询可更新的包

brew upgrade      //更新本地安装所有包

brew upgrade git  //更新指定包

brew cleanup      //清理所有包的旧版本

brew cleanup git  //清理指定包的旧版本

brew cleanup -n   //查看可清理的旧版本包，不执行实际操作

brew uninstall git//卸载安装包

brew info git     //查看包信息

brew list         //查看安装列表

brew search git   //查询可用包

brew services list //查询所有包

brew services start activemq //启动包（以activemq为例子）

brew services stop activemq //停止包（以activemq为例子）

卸载Homebrew
cd `brew --prefix`
rm -rf Cellar
brew prune
rm `git ls-files`
rm -r Library/Homebrew Library/Aliases Library/Formula Library/Contributions
rm -rf .git
rm -rf ~/Library/Caches/Homebrew
```

**常见错误**去下方[地址](https://link.zhihu.com/?target=https%3A//gitee.com/cunkai/HomebrewCN/blob/master/error.md)查看

```
https://gitee.com/cunkai/HomebrewCN/blob/master/error.md
```