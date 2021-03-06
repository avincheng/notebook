# macOS 安装配置 Homebrew

## 安装 Homebrew

使用 ruby 执行 Homebrew 安装脚本（可能需要网络代理）：

```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## 配置 Homebrew

### 关闭自动更新

可选择关闭 Homebrew 每次执行命令时自动更新：

```shell
echo 'export HOMEBREW_NO_AUTO_UPDATE=true' >> ~/.zshrc
source ~/.zshrc
```

> **Tips:** 若使用 bash（macOS 默认 shell），将 `~/.zhsrc` 换成 `~/.bash_profile` 。

### 加速 Homebrew 更新

可修改 Homebrew 远程仓库地址为中科大开源镜像地址，以加速更新：

```shell
# 替换 brew.git
cd "$(brew --repo)"
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git

# 替换 homebrew-core.git
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

# 替换 homebrew-cask.git
# 需执行过 `brew cask` 命令安装 brew-cask 后才会有
# brew-cask 用于安装 macOS 二进制文件（如带 GUI 的大型应用程序）
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-cask"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git
```

如需恢复至 Github 远程仓库：

```shell
# 恢复 brew.git
cd "$(brew --repo)"
git remote set-url origin https://github.com/Homebrew/brew.git

# 恢复 homebrew-core.git
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://github.com/Homebrew/homebrew-core.git

# 恢复 homebrew-cask.git
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-cask"
git remote set-url origin https://github.com/Homebrew/homebrew-cask.git
```

### 加速软件包下载

使用中科大开源镜像加速软件包下载：

```shell
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc
```

## Homebrew 常用命令

### 更新 Homebrew

```shell
brew update
```

### 安装软件包

```shell
# 安装指定软件包
brew install <formula>

# 跳过依赖直接安装指定软件包
brew install --ignore-dependencies <formula>
```

> **TIps:** `<formula>` 为软件包方案名称，多个用空格隔开。

### 更新软件包

```shell
# 查看所有（指定）软件包是否需要更新
brew upgrade --dry-run [<formula>]

# 更新所有（指定）软件包
brew upgrade [<formula>]
```

### 卸载软件包

```shell
# 卸载指定软件包
brew uninstall <formula>

# 强制卸载软件包，即使它是别的软件包的依赖
brew uninstall --ignore-dependencies <formula>
```

### 查看软件包

使用关键字或正则搜索 Homebrew 支持的软件包：

```shell
brew search <text|/text/>
```

> **TIps:** `<text|/text/>` 为软件名称关键字或者正则。

查看当前已安装的软件包：

```shell
# 查看当前已安装的软件包
brew ls|list

# 查看当前已安装的软件包版本
brew ls|list --versions
```

查看软件包依赖：

```shell
# 查看指定软件包的依赖树
brew deps --tree <formula>

# 查看指定软件包当前已安装的依赖
brew deps --installed <formula>

# 查看当前已安装的所有软件包的依赖树
brew deps --tree --installed
```

查看已安装的所有（指定）软件包缺失依赖：

```shell
brew missing [<formula>]
```

查看指定的软件包详细信息：

```shell
brew info <formula>
```

查看当前已安装的顶级（非依赖）软件包：

```shell
brew leaves
```

### 清除缓存

清除所有缓存文件和残留的旧版文件：

```shell
brew cleanup
```

### 符号链接

建立符号链接：

```shell
# 手动建立符号链接
brew ln|link <formula>

# 手动覆盖建立符号链接
brew ln|link --overwrite <formula>
```

删除符号链接：

```shell
brew unlink <formula>
```

> **Tips:** Homebrew 符号链接存放目录为  `/usr/local/bin` ，软件包实际安装目录为 `/usr/local/Cellar` 。

## 异常处理

### 权限不足

错误信息为：

> Error: Permission denied @ apply2files - /usr/local/share/Library/Caches......

异常可能发生在系统升级之后，解决方案：

```shell
sudo chown -R $(whoami):admin /usr/local/*
sudo chmod -R g+rwx /usr/local/*
```

## 参考文献

- [Homebrew Documentation](https://docs.brew.sh/Manpage)
- [Homebrew 源使用帮助](http://mirrors.ustc.edu.cn/help/brew.git.html)
- [Error Permission denied when running brew cleanup](https://discussions.apple.com/thread/250801501)

