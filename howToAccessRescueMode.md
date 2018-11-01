## Redhat(Grub commands)
### Step
1、 restart computer and press the **e** key on a selected menu entry in the boot loader menu.  
2、 Add the following parameter at the end of the **linux** line on 64-Bit IBM Power Series, the **linux16** line on x86-64 BIOS-based systems, or the linuxefi line on UEFI system.  
```shell
systemd.unit=rescue.target
```
> 注意不要换行  

3、Press CTRL+X  
4、input single

#### [参考链接](https://docs.fedoraproject.org/en-US/fedora/f28/system-administrators-guide/kernel-module-driver-configuration/Working_with_the_GRUB_2_Boot_Loader/)
