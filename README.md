# iStoreOS/OpenWrt一键备份与恢复脚本 <img alt="GitHub release (by tag)" src="https://img.shields.io/github/downloads/wukongdaily/OpenBackRestore/v1.0/total?label=%E4%B8%8B%E8%BD%BD%E6%AC%A1%E6%95%B0&labelColor=%2332CD32&color=black">
## 🤔 这是什么？

该项目可以轻松备份iStoreOS已安装的软件和配置,当系统恢复出厂设置或重置后，可以一键恢复原来的软件和配置。<br>
### 特别说明：对于iStoreOS系统而言，docker的数据分区基本上被用户主动迁移到另一个分区，因此无需备份，因为重置系统并不会删除用户自己新建的分区。若用户没有迁移docker的数据分区，那么我们的备份已经包含！                                                       
## 💡 特色功能

- 💻 支持`一键生成备份档案 包括已安装软件及其配置`
- 💻 支持`一键恢复备份档案 包括已安装软件及其配置`
- 💻 支持`已安装软件及其配置:包含应用商店和第三方安装的ipk/run`
- 🔑 支持`同时支持终端命令行方式和iStore应用商店手动安装方式`
- 支持的OpenWrt系统列表如下
- 1、软路由iStoreOS(x86_64 | ARM64) ✅
- 2、兼容机型：MT3000/2500/6000 ✅
- 3、OpenWrt（squashfs-combined）✅
- 4、OpenWrt（ext4-combined）❌
> 只要是squashfs-combined类型的openwrt固件,理论上都可以兼容的。因为它们都是用了overlayfs文件系统的。

> 特别说明：GL-iNET这三款机型的恢复工作是分两步走。<br>
> 1、执行`sh restore.run `后先恢复到iStoreOS风格,执行完毕后会**提示用户上传你的备份档案。**<br>
> 2、再次执行`sh restore.run `后，提示恢复成功并重启，方可完成✅

## 🚀 方法一 命令行方式

### 1. 生成备份`/tmp/upload/backup.tar.gz`
```bash 
wget -O backup.run https://cafe.cpolar.cn/wkdaily/OpenBackRestore/raw/branch/master/backup/backup.run && sh backup.run
```
> 每次备份都是完整的,可以经常备份,比如每月备份一次
### 🤔 如何自定义备份的路径？方法如下
https://github.com/wukongdaily/OpenBackRestore/wiki

* 下载脚本
```bash
wget -O backup.run https://cafe.cpolar.cn/wkdaily/OpenBackRestore/raw/branch/master/backup/backup.run
```
### 举例说明 假设要备份到 `/mnt/sata1-4`目录
* 执行备份
```bash
sh backup.run /mnt/sata1-4
```

### 2. 恢复备份 

**使用前提** 将备份档案提前上传到 `/tmp/upload/` 目录,如图<br><br>![huifu](https://github.com/wukongdaily/OpenBackRestore/assets/143675923/cd111f10-e6aa-4011-a046-b3004f77c7eb)

> 确定备份文件已经上传了 再执行如下命令即可恢复,恢复完成后会自动重启
### ❤️恢复命令如下

```bash 
wget -O restore.run https://cafe.cpolar.cn/wkdaily/OpenBackRestore/raw/branch/master/backup/restore.run && sh restore.run
```


## 🚀 方法二 手动方式

> 1、在release页面下载backup.run或restore.run<br>
https://github.com/wukongdaily/OpenBackRestore/releases/latest <br>
> 2、打开iStore应用商店,点击手动安装,将run文件拖拽上去即可执行。<br>
![image](https://github.com/wukongdaily/OpenBackRestore/assets/143675923/54fdc034-ed4f-4f81-8aa7-0de556e0c3e2)



# 在1panel的计划任务里做定时备份
> 需要使用我的脚本方式运行1panel docker容器 详见这个项目的第三条命令：https://github.com/wukongdaily/OrangePiShell<br>
> 因为默认情况下istoreos的应用商店里的1panel 并没有映射全部目录，只映射了/mnt，所以无法完成/overlay的备份。因此需要稍微改动一下。详见上述脚本

```
bash -c "$(curl --insecure -fsSL https://cafe.cpolar.cn/wkdaily/OpenBackRestore/raw/branch/master/1panel/backup.sh)" -- /ahost/mnt/mmc1-4/backupSystem
```
- 其中 `/ahost/mnt/mmc1-4/backupSystem` 按需修改为备份文件保存的位置<br>
  
  
<img width="952" alt="1Panel 2024-07-02 14-49-18888" src="https://github.com/wukongdaily/OpenBackRestore/assets/143675923/d022bbc2-425b-426d-9809-0546744a3a95">



# 恢复1panel计划任务产生的备份
```
bash -c "$(curl --insecure -fsSL https://cafe.cpolar.cn/wkdaily/OpenBackRestore/raw/branch/master/1panel/restore.sh)"
```

# 扩展问题:如何备份到NAS？挂cifs即可。方法如图
<img width="919" alt="cifs" src="https://github.com/wukongdaily/OpenBackRestore/assets/143675923/a185f505-76fe-4512-b5fe-35e9c28ee1e7">

