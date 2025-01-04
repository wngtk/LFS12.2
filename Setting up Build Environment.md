## Setting up Build Environment

这是设置 LFS 的构建环境。假如使用的是虚拟机。

如果 Arch  LiveCD 终端的字体太小，用大号的字体：

```
setfont ter-v28b
setfont ter-v22b
```

准备一个 Arch Linux 的 Live CD。使用 [linutil](https://github.com/ChrisTitusTech/linutil?tab=readme-ov-file) 一键安装 Arch。我选择 ext4 文件系统，我把这个机器的名字叫做 Arch。

我使用 VMware Workstation Pro，直接在 VMware 里面输入命令不太方便，也不能使用剪贴板。安装一个 openssh，通过 Windows 连接到虚拟机：

```
sudo pacman -S openssh
```

> [!TIP]- VMware Workstation 的 NAT 是可以被主机访问的
> 意味着不需要单独设置网络，主机就可以 ssh 访问虚拟机

如果虚拟机网络设置的是 NAT 模式，也可以通过端口转发来让 Windows 可以连接虚拟机。查看 Arch 的 IP 地址：

```
ip add
```

然后打开 VMWare 编辑菜单下的虚拟网络编辑器，点击更改设置，选择 NAT 模式，点击 NAT 设置。添加 TCP 端口转发，主机端口 2200，虚拟机端口 22。

打开 Windows Terminal 连接虚拟机：

```
ssh username@localhost -p 2200
```

Arch Linux 只需要安装 bison 和 binutils，运行 version-check.sh 就已经满足需要了

> [!NOTE]- 什么是 System triplet
> 
> > There are three system names that the build knows about: the machine you are building on (build), the machine that you are building for (host), and the machine that GCC will produce code for (target). When you configure GCC, you specify these with `--build=`, `--host=`, `--target`
> 
> 我的理解是：
> - `--build` 指的是 `GCC` 是在什么平台上被构建的
> - `--host` 指的是 `GCC` 可以在什么平台上运行
> - `--target` 指的是 `GCC` 编译出来的代码是哪个平台的
> 
> 当进行交叉编译的时候，执行 gcc 是 `machine-gcc` or `machine-gcc-version`，这个 `machine` 指的是一个三元组(system triplet)。这个 triplet 是 cpu-vendor-kernel-os，vendor 一般被省略。因此，x86_64 的 Ubuntu 上是 `x86_64-linux-gnu`，这里也说明了普通的桌面 Linux 我们称之为 gnu/linux，至少 gcc 是认为 os 是 gnu。Ubuntu 上的 gcc 是 `x86_64-linux-gnu-gcc`。不过红帽系列好像没有遵守这个规范，他们的 target 是这也的`x86_64-redhat-linux`。
