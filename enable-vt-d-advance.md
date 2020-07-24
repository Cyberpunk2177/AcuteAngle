## 锐角云 Acute Angle PVE下开启VT-D (不占用U盘版)

### 警告

- 首先不保证开启成功
- 其次不保证您的板子不会BOOM或者黑掉
- 再次不负任何责任
- 原帖链接 https://tieba.baidu.com/p/4934345324

### 注意

**因为此次修改不是直接修改BIOS因此断电以后设置会丢失，你可能需要重新设置，可以买个BIOS电池什么的，不嫌麻烦就直接重新再弄一次。。。**

### 准备材料

| 名称        | 数量 | 备注                                                         |
| ----------- | ---- | ------------------------------------------------------------ |
| 锐角云      | 1    | 没改过BIOS                                                   |
| U盘         | 1    | 格式无所谓，能不能启动也无所谓，只要是好的U盘就行，建议fat32 |
| grub2启动器 | 1    | BOOTX64.EFI 链接在[这里](https://github.com/Cyberpunk2177/AcuteAngle/raw/master/bootx64.efi) |
|             |      |                                                              |

### 我们开始吧！

1. 还原你的BIOS设置

2. 把BOOTX64.EFI 丢到U盘的根目录下，记住这个名字以后要用到

3. 重启锐角云，在出现红色LOGO的时候按一下F7 ，在弹出的启动项选择菜单中选择【uefi built in efi shell】

4. 耐心等待到出现

   ```shell
   SHELL> _
   ```

   字样以后，输入 

   ```shell
   fs0:
   ```

   然后回车（ **不要忘记英文冒号** ），再输入

   ```shell
   ls
   ```

   看看是否会出现文件列表，是否会显示你U盘中的文件以及 BOOTX64.EFI 文件

   如果显示了，跳到5，如果没显示，继续

   ```shell
   fs1:
   然后回车，输入
   ls
   如果还是没显示，输入fs2: 然后
   ls 依此类推，直到显示BOOTX64.EFI
   ```

   如果输入到fs9的时候还没显示，那么换一个U盘继续。。。

5. 显示bootx64.efi以后输入

   ```shell
   \BOOTX64.EFI
   ```

   然后回车，此时如果不出意外会进入grub2界面。显示器的表现是刷新一段代码然后停留在以下界面

   ```shell
   grub2>: _
   ```

6. 此时在命令行输入

   ```shell
   setup_var_3 0x49 0x01 
   ```

   然后回车，此时如果正常会显示

   ```shell
   GNU GRUB version 2.03
   
   Minimal BASH-like line editing is supported. For the first word, TAB 1ists possible command complet ions. Anywhere else TAB lists possible device or 11le complet ions.
   
   grub> setup_var Ox49 Ox1 Looking for Setup variable...
   
   var name: Setup, var size: 12, var guid: ec87d643-eba4-4bb5 a1-e5-3f-3e-36-b2-0d-a9
   
   --> GUID does not match expected GUID, taking it nevertheless... expected a different size of the Setup variable (got 1453 (0x5ad) bytes).
   Continue with care. 
   successfully obtained "Setup" variable from ss (got 1453 (0x5ad) bytes).  offset Ox49 is: Ox00
   setting offset Ox49 to Ox01 var name: Setup, var size: 12, var guid: 80e1202e-2697-4264 - 9c-c9-80-
   
   ```

7. 重启电脑，进入PVE，sudo 或者 su 提权到root权限，打开终端

   ```shell
   vi /etc/default/grub
   或者
   vim /etc/default/grub
   或者 
   nano /etc/default/grub
   
   将
   GRUB_CMDLINE_LINUX_DEFAULT="quiet"
   改为
   GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"
   
   ```

   重启机器，享受生活。 Enjoy It





### 感谢

- @LongSoft  的BIOS工具提供支持
- @codedad1983 的grub2引导器（以前的不能用.jpg
- @Oscarice_Moe 的测试和确认
