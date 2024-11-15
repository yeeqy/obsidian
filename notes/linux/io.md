相关命令

如果你的系统没有安装 `iotop`，你可以通过以下方法安装它，或者使用其他工具来监控磁盘I/O。

### 1. **安装 iotop**
在 Linux 上，你可以根据你所使用的发行版安装 `iotop`。

#### **Debian/Ubuntu**
```bash
sudo apt-get update
sudo apt-get install iotop
```

#### **CentOS/RHEL**
```bash
sudo yum install epel-release
sudo yum install iotop
```

#### **Fedora**
```bash
sudo dnf install iotop
```

安装完成后，可以使用 `sudo iotop` 来运行它（需要管理员权限）。

### 2. **使用其他工具监控 I/O 操作**
如果不想使用 `iotop`，也有其他工具可以用来监控 I/O 操作：

#### a. **使用 `iostat`**
`iostat` 是一个系统性能监控工具，可以用来查看磁盘的输入/输出情况。

```bash
iostat -x 1
```

这将每秒输出一次磁盘的扩展统计信息，包括每个磁盘的 I/O 使用情况。

#### b. **使用 `dstat`**
`dstat` 是一个非常强大的实时系统资源监控工具，它可以提供多种系统资源（包括磁盘、网络、CPU）的使用情况。

```bash
sudo apt-get install dstat  # 安装
dstat -d  # 显示磁盘 I/O 使用情况
```

#### c. **使用 `vmstat`**
`vmstat` 用于报告虚拟内存、进程、I/O、系统活动等信息，可以帮助你查看磁盘I/O的活动。

```bash
vmstat 1
```

#### d. **使用 `sar`**
`sysstat` 包含了 `sar` 工具，它提供了系统性能的详细历史数据，特别适用于I/O的长时间监控。

```bash
sudo apt-get install sysstat  # 安装
sar -d 1 1  # 查看磁盘I/O
```

这些工具都可以帮助你监控系统资源，特别是磁盘I/O。如果你只是想快速了解系统I/O使用情况，`iostat` 和 `vmstat` 都是很好的选择。