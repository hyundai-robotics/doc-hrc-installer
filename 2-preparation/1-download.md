## 2.1 Download Installer

### 2.1.1. App Installer Download Process

1. Download the `hrc_installer.zip` file from the [HD Hyundai Robotics Download Center](https://hd-hyundairobotics.com/download-center/list).
2. Extract the zip file to the root directory of a USB drive (FAT32 format is recommended).
3. Check the directory structure below.

### 2.1.2. Checking the Directory Structure

- Ensure it exactly matches the directory structure below.
- Verify that all 4 files exist.

<div style="max-width:fit-content;">

{% hint style="warning" %}
This exact structure must be strictly maintained for the App Installer to be recognized normally on the TP.
{% endhint %}

```text
📁 USB_ROOT (USB Root)
 ┗ 📁 hi6
    ┗ 📁 apps
       ┗ 📁 Install_Apps
          ┣ 📄 hrc_installer       (Installer executable file)
          ┣ 📄 hrc_installer.cfg   (Installation config file)
          ┣ 📄 hrc_installer.png   (Installer icon image)
          ┗ 📄 info.json           (App info file)
```

</div>
