# Kernel Dev Skills

## kernel 代码格式化 

可以先使用 Astyle 进行代码格式化，然后再使用 `checkpatch` 脚本检查代码是否符合规范要求。

### Astyle 格式化

```C
astyle --style=linux --indent=force-tab=8 --align-pointer=name --max-code-length=90 --break-after-logical -p -H -U drivers/folder/*.c  drivers/folder/*.h
```

### 文件夹格式化检查

需要基于 python3 安装 git python:

```
python3 -m pip install ply
python3 -m pip install gitpython
```

然后可以使用格式化检查：

```C
scripts/checkpatch.pl -f drivers/folder/*.c
```

### 检查后将修改存为 patch

```C
scripts/checkpatch.pl -f drivers/folder/*.c --fix
```

### 检查后在文件上修改

```C
scripts/checkpatch.pl -f drivers/folder/*.c --fix-inplace
```

### 将修改整理成 patch 后检查

```C
git format-patch -1
scripts/checkpatch.pl 0001-kernel-xxx-init.patch
```

或者直接使用如下命令，生成 patch 后直接将 patch 名称传递给 checkpatch 检查：

```
git format-patch --stdout -1 | scripts/checkpatch.pl --no-signoff --no-changeid -
```

### 重定位内核 log 到 printk

```
su; echo 7777 > /proc/sys/kernel/printk
```

## 存储 deconfig 文件

- make xxx_linux_virt_defconfig 选择一个预设的
- make menuconfig 进行配置
- make savedefconfig
- cp defconfig 存储到指定目录

