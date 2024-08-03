### 系统设置

#### 允许安装任意来源的 App

有时候我们需要安装一些非官方 AppStore 的软件，这个时候就得到「安全与隐私」-「通用」里面来勾选开启任何来源，但是默认是没有这个选项的，在 macOS 早期是可以顺利开启的，但是到了近几年的版本我们得如需下面命令来手动开启：

```bash
# APP安装开启任何来源
➜ sudo spctl --master-disable
```

然后下面的这个任何来源的隐藏选项就出现啦：

![img](http://image.wangxiaohuan.com/blog/image/202309061756724.png)

#### 安装 Xcode Command Line Tools

Command Line Tools 是在 Xcode 中的一款工具，macOS 下不少开发工具都会依赖这个，所以我们手动安装一下，后面安装其他工具可以省下不少麻烦：

```bash
# 安装 xcode 命令行工具
➜ xcode-select --install
```

#### 减少程序坞的响应时间

如果只有一个屏幕的时候，国光我经常在「程序坞与菜单栏」勾选「自动隐藏和显示程序坞」，这样增加屏幕的利用率，但是 macOS 默认情况下唤出这个程序坞有点慢一拍的感觉，我们可以通过命令来减少响应时间：

```

# 设置启动坞动画时间设置为 0.5 秒
➜ defaults write com.apple.dock autohide-time-modifier -float 0.5 && killall Dock

# 设置启动坞响应时间最短
➜ defaults write com.apple.dock autohide-delay -int 0 && killall Dock
```

不要担心改不回来，可以使用下面的命令恢复系统初始化的设置：

```

# 恢复启动坞默认动画时间
➜ defaults delete com.apple.dock autohide-time-modifier && killall Dock

# 恢复默认启动坞响应时间
➜ defaults delete com.apple.Dock autohide-delay && killall Dock
```

修改启动台行和列数

启动台里面也可以设置应用的列和宽，使用如下命令即可：

```

# 设置列数为 9
➜ defaults write com.apple.dock springboard-columns -int 9

# 设置行数为 6
➜ defaults write com.apple.dock springboard-rows -int 6

# 重启 Dock 生效
➜ killall Dock
```

设置的 9 列 6 行的效果如下：

[![img](http://image.wangxiaohuan.com/blog/image/202309061759625.png)

这个效果因人而异 ，大家可以自己去摸索，如果你想恢复默认的话可以使用下面的命令：

```

# 恢复默认的列数和行数
➜ defaults write com.apple.dock springboard-rows Default
➜ defaults write com.apple.dock springboard-columns Default

# 重启 Dock 生效
➜ killall Dock
```

### 触控优化

MBP 上使用触控板或者台式机外接妙控板进行鼠标指针拖移的话，默认得按压下去才可以拖动窗口，长时候按压难免让手指更加酸痛而且效率不高，这个时候在「辅助功能」-「指针控制」-「鼠标与触控板」开启传说中的「三指拖移」：

[![img](http://image.wangxiaohuan.com/blog/image/202309061801612.png)

这才是 macOS 的触控板真正的精髓呀！效率提升杠杠的。

#### 光标响应

macOS 下的默认光标使用方向键左右移动是非常慢的，在终端下敲命令的时候尤为明显。这个时候得手动调快一些，才可以达到 Linux 下那种顺滑的移动效果，只需要到将「按键重复」调快一些，然后将「重复前延迟」调短一些即可：

[![img](http://image.wangxiaohuan.com/blog/image/202309061801414.png)

#### 关闭 SIP

有些软件需要关闭  SIP 才可以实用，比如 `proxychains-ng` 这种系统级代理软件

#### 白苹果

白苹果得进恢复模式才可以修改，重启 Mac，按住 Option 键进入启动盘选择模式，按 `⌘` + `R` 进入 Recovery 模式。

「菜单栏」 ->「 实用工具（Utilities）」-> 「终端（Terminal）」：

```

# 关闭SIP
➜ csrutil disable

# 查看SIP状态
➜ csrutil status

System Integrity Protection status: disabled.(表明关闭成功)
```

#### 黑苹果

黑苹果的话也可以进恢复模式，但是不如直接修改配置文件更方便高效。以 OC 为例，只要勾选「AllowToggleSip」：

![img](http://image.wangxiaohuan.com/blog/image/202309061801290.png)

然后就可以在开机选择系统的时候手动切换 SIP 开关状态了：![img](http://image.wangxiaohuan.com/blog/image/202309061802381.jpeg)

