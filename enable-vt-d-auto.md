# 锐角云 Acute Angle PVE下开启VT-D (不需要 BIOS 电池的自动版)

本方法不需要购买 BIOS 电池，也不需要刷入额外的 BIOS，即可实现永久地(实际上是每次开机自动修改)开启 `vt-d`

## 注意

- 首先不保证开启成功
- 其次不保证您的板子不会BOOM或者黑掉
- 再次不负任何责任
- 每次断电重启之后需要**再重启一次**才会生效

## 准备材料

| 名称          | 数量 | 备注                                                                               |
| ------------- | ---- | ---------------------------------------------------------------------------------- |
| 锐角云        | 1    | 最好安装的是 PVE，没有在非 PVE 系统上做过测试                                      |
| setup_var.mod | 1    | 从这里[下载](https://github.com/Cyberpunk2177/AcuteAngle/raw/master/setup_var.mod) |

## 步骤

1. 将 `setup_var.mod` 复制到 `/boot/grub/x86_64-efi/`
2. 修改 `/etc/grub.d/00_header`

``` bash
echo -e 'echo "insmod setup_var"\necho "setup_var_3 0x49 0x01"' >> /etc/grub.d/00_header
```

3. 修改 `/etc/default/grub`

``` bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"
```

4. 生成新的 grub 配置
``` bash
update-grub
```

5. 重启**两次**， 检查是否启用成功

``` bash
root@pve:~# dmesg |grep DMAR
[    0.013559] ACPI: DMAR 0x0000000079AB7820 0000A8 (v01 INTEL  EDK2     00000003 BRXT 0100000D)
[    0.013695] ACPI: Reserving DMAR table memory at [mem 0x79ab7820-0x79ab78c7]
[    0.058291] DMAR: IOMMU enabled
[    0.204229] DMAR: Host address width 39
[    0.204233] DMAR: DRHD base: 0x000000fed64000 flags: 0x0
[    0.204251] DMAR: dmar0: reg_base_addr fed64000 ver 1:0 cap 1c0000c40660462 ecap 7e3ff0505e
[    0.204259] DMAR: DRHD base: 0x000000fed65000 flags: 0x1
[    0.204273] DMAR: dmar1: reg_base_addr fed65000 ver 1:0 cap d2008c40660462 ecap f050da
[    0.204281] DMAR: RMRR base: 0x00000079a32000 end: 0x00000079a51fff
[    0.204286] DMAR: RMRR base: 0x0000007b800000 end: 0x0000007fffffff
[    0.204293] DMAR-IR: IOAPIC id 1 under DRHD base  0xfed65000 IOMMU 1
[    0.204297] DMAR-IR: HPET id 0 under DRHD base 0xfed65000
[    0.204302] DMAR-IR: Queued invalidation will be enabled to support x2apic and Intr-remapping.
[    0.206605] DMAR-IR: Enabled IRQ remapping in x2apic mode
[    2.988101] DMAR: No ATSR found
[    2.988107] DMAR: dmar0: Using Queued invalidation
[    2.988119] DMAR: dmar1: Using Queued invalidation
[    2.994505] DMAR: Intel(R) Virtualization Technology for Directed I/O
```

这样就是成功了，如果只有一行

```
[    0.058291] DMAR: IOMMU enabled
```

说明没有成功，可以再重启一次试试。

## 作者

- @YadominJinta