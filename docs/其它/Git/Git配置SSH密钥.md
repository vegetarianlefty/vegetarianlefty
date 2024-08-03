### Git 配置 SSH

#### 一、查看是否已经生成过/是否应存在了私钥公钥

进入 用户/.ssh 目录

```
$ cd ~/.ssh
```


如果找不到目录或者目录中没有东西，证明以前没有生成过。

或者直接使用命令来查看

```
$ ls -al ~/.ssh
Lists the files in your .ssh directory, if they exist 
```


检查目录列表，看看您是否已经拥有公共SSH密钥。默认情况下，GitHub受支持的公钥的文件名如下之一。

```
id_rsa.pub
id_ecdsa.pub
id_ed25519.pub
```

如果您收到~/.ssh不存在的错误，则在默认位置没有现有的SSH密钥对。您可以在下一步创建新的SSH密钥对。

#### 二、生成密钥对（私钥公钥）

两种方式，一种是生成的时候直接添加好路径；另一种是不带路径，需要第二步添加路径或使用默认路径。

ssh key命名的第一种方式：

```
$ ssh-keygen -t rsa -C "youremail@yourcompany.com" -f ~/.ssh/id_rsa_github
代码参数含义： -t 指定密钥类型，默认是 rsa ，可以省略。 -C 设置注释文字，比如邮箱。 -f 指定密钥文件存储文件名。
```

ssh key命名的第二种方式：

```
$ ssh-keygen -t rsa -C "youremail@xxx.com"
```

以上代码省略了 -f 参数，因此，运行上面那条命令后会让你输入一个文件名，用于保存刚才生成的 SSH key 代码。

按回车后：

```
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/xxx/.ssh/id_rsa): id_rsa_github(取个名字)
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in 
id_rsa_github.
Your public key has been saved in 
id_rsa_github.pub.
```


>最好每次生成时都给SSH Key取个名字，这样后面在管理时自己也一目了然。我这里的格式是id_rsa_项目名_git提供方，我生成的所有key都遵循这个规则命名。建议你也有你自己的一种命名方式，并且保持统一。如果不取名字，默认的是id_rsa，如果后面生成时不命名，会把这个覆盖掉。密码可以不设置，免得每次提交时还要输入一次，安全性自己衡量吧。第一次生成key时，会在~目录下创建一个.ssh目录。

附： 键入ssh-keygen -t，然后是键类型和可选注释。此注释包含在创建的.pub文件中。您可能想使用电子邮件地址进行注释区分。

对于ED25519：

```
ssh-keygen -t ed25519 -C "<comment>"
```


对于2048位RSA：

```
ssh-keygen -t rsa -b 2048 -C "<comment>"
```

#### 三、将SSH密钥添加到ssh代理中

ssh服务器默认是去找id_rsa，现在需要把这个key添加到ssh-agent中，这样ssh服务器才能认识id_rsa_TestSSH_github。

在后台启动ssh-agent。

```
$ eval $(ssh-agent -s)
Agent pid 59566
```


根据您的环境，您可能需要使用其他命令。例如，在启动ssh-agent之前，您可能需要通过运行sudo -s -H来使用root访问，或者您可能需要使用exec ssh-agent bash或exec ssh-agent zsh来运行ssh-agent。

> 如果您使用的是macOS Sierra 10.12.2或更高版本，则需要修改~/.ssh/config文件，以自动将密钥加载到ssh代理中，并将密码存储在钥匙串中。

### 多个Github账号配置不同的 ssh

首先，检查您的~/.ssh/config文件是否存在于默认位置。

```
$ open ~/.ssh/config
> The file /Users/you/.ssh/config does not exist.
```

如果文件不存在，请创建文件。

```
$ touch ~/.ssh/config
```

打开您的~/.ssh/config文件，然后修改文件以包含以下行。如果您的SSH密钥文件的名称或路径与示例代码不同，请修改文件名或路径以匹配当前设置。

```
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_rsa_github
注意：如果您选择不向密钥添加密码，则应省略UseKeychain行。
```

> 注意：如果您看到这样的错误 /Users/USER/.ssh/config: line 16: Bad configuration option: usekeychain Host *部分添加额外的配置行： Host * IgnoreUnknown UseKeychain

将您的SSH私钥添加到ssh代理中，并将密码存储在钥匙串中。如果您使用其他名称创建了密钥，或者如果您正在添加具有不同名称的现有密钥，请将命令中的id_rsa_github替换为私钥文件的名称。

```
$ ssh-add ~/.ssh/id_rsa_github
```

即ssh-add <directory to private SSH key> 如果执行ssh-add时提示"Could not open a connection to your authentication agent"，可以现执行命令：

```
$ ssh-agent bash
```

然后再运行ssh-add命令。

查看添加结果

```
ssh-add -l

# 可以通过 ssh-add -l 来确私钥列表
$ ssh-add -l
# 可以通过 ssh-add -D 来清空私钥列表
$ ssh-add -D
```

### 一、将公钥粘贴到git服务器平台上，如GitHub

当然可以直接打开刚生成的公钥文件id_rsa_github.pub文件，复制全部内容去平台Add SSH Key；也可以使用命令复制公钥文件内容贴到平台 Add SSH Key。

附：使用命令复制 将SSH公钥复制到剪贴板上

```
$ pbcopy < ~/.ssh/id_rsa_github.pub
Copies the contents of the id_rsa_github.pub file to your clipboard
```

提示：如果pbcopy不起作用，您可以找到隐藏的.ssh文件夹，在您最喜欢的文本编辑器中打开文件，并将其复制到剪贴板。

其规则就是：从上至下读取config的内容，在每个Host下寻找对应的私钥。 这里将GitHub SSH仓库地址中的git@github.com替换成新建的Host别名如github2，那么原地址是：git@github.com:username/Mywork.git，替换后应该是：github2:username/Mywork.git。

#### 二、测试连接

测试一下

```
$ ssh -T github2
Hi 0xJoker! You've successfully authenticated, but GitHub does not provide shell
```

1.
2.
输入以下命令测试

```
$ ssh -T git@github.com
Attempts to ssh to GitHub
```

您可能会看到这样的警告：

```
The authenticity of host 'github.com (IP ADDRESS)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)?
```

验证您看到的消息中的指纹是否与GitHub 的RSA公钥指纹匹配。如果是这样，则键入yes：

```
> Hi username! You've successfully authenticated, but GitHub does not
> provide shell access.
```

您可能会看到以下错误信息：

```
...
Agent admitted failure to sign using the key.
debug1: No more authentication methods to try.
Permission denied (publickey).
```

这是某些Linux发行版的已知问题。有关更多信息，请参阅 “错误：代理承认未能签名”。

验证生成的消息是否包含您的用户名。如果您收到“permission denied【权限被拒绝】”消息，请参阅 “Error: Permission denied (publickey)【错误：权限被拒绝（公钥）”】。

###  修改本地仓库地址

列出该仓库的 Git remote，我们可以通过`git remote -v`看看本地仓库对应的远程仓库URL，检查该URL是否与要使用的GitHub主机匹配，否则更新远程原始URL。如果不匹配，可以通过下面这个命令更新：

```
git remote -v
git remote set-url origin git@github.com-worker_user:worker_user/repo_name.git
```

