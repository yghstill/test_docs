(installation)=

# Installation

AngleSlim支持如下安装方式：

- [pip安装（推荐）](#pip安装（推荐）)
- [编译安装](#编译安装)
- [指定环境变量](#指定环境变量)

## pip安装（推荐）

通过pip安装最新AngleSlim稳定发布版：

```shell
pip install angleslim
```

## 编译安装

如果对工具代码做过改动，或者想使用master分支最新功能，推荐使用编译安装方式：

```shell
python3 setup.py install
```

## 指定环境变量

如果对源码做了修改，更简易的方式是指定PYTHONPATH环境变量，例如：
```shell
export PYTHONPATH=Your/Path/to/AngleSlim/:$PYTHONPATH
```

:::{note}
指定环境变量后，需要和执行压缩算法的脚本在同一终端执行，比如放在同一个shell脚本内，先export PYTHONPATH环境变量，然后运行压缩程序代码。
:::