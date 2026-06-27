## 2.1 下载安装程序

### 2.1.1. 应用程序安装程序下载流程

1. 从 [HD Hyundai Robotics 下载中心](https://hd-hyundairobotics.com/download-center/list) 下载 `hrc_installer.zip` 文件。  
  <img src="../_assets/0_download_center.png" style="max-height:250px;">
2. 将 zip 文件解压到 USB 驱动器的根目录下（推荐使用 FAT32 格式）。
3. 检查以下目录结构。

### 2.1.2. 检查目录结构

- 确保其与下面的目录结构完全匹配。
- 验证所有 4 个文件是否存在。

<div style="max-width:fit-content;">

{% hint style="warning" %}
此结构必须严格保持，以便在 TP 上正常识别应用程序安装程序。
{% endhint %}

```text
📁 USB_ROOT (USB 根目录)
 ┗ 📁 hi6
    ┗ 📁 apps
       ┗ 📁 Install_Apps
          ┣ 📄 hrc_installer       (安装程序可执行文件)
          ┣ 📄 hrc_installer.cfg   (安装配置文件)
          ┣ 📄 hrc_installer.png   (安装程序图标图片)
          ┗ 📄 info.json           (应用程序信息文件)
```

</div>