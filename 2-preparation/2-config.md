## 2.2 编写配置文件

一旦 USB 上的应用程序安装程序准备就绪，请按照以下步骤进行操作。

1. 将要安装的应用程序放入 `apps` 文件夹中。
2. 修改应用程序安装程序配置文件 (`hrc_installer.cfg`)。

### 2.2.1. 放置要安装的应用程序

将要在 COM（控制器）或 TP（教学挂件）上安装的目标应用程序文件夹或文件复制到 `apps` 文件夹下。

<div style="max-width:fit-content;">

```text
📁 USB_ROOT (USB 根目录)
 ┗ 📁 hi6
    ┗ 📁 apps
       ┗ 📁 Install_Apps
          ┣ 📄 hrc_installer
          ┣ 📄 hrc_installer.cfg
          ┣ 📄 hrc_installer.png
          ┣ 📄 info.json
          ┗ 📁 mastering           <-- (要安装的应用程序)
```

</div>

### 2.2.2. 编写应用程序安装程序配置文件 (hrc_installer.cfg)

配置文件 (hrc_installer.cfg) 作为一种任务规范。  
为了确保正确操作，请根据以下格式使用文本编辑器进行编辑。

#### 应用程序类型

所使用的应用程序安装程序配置文件中的命令取决于您打算安装的应用程序类型。  
请参阅下表。

<div style="max-width:fit-content;">

| 类别 | <a href="https://hrbook-hrc.web.app/#/view/doc-hi6-tp-app/zh/1-intro/README?cont_model=${cont_model}" style="color:#222222">TP 应用</a> | <a href="https://hrbook-hrc.web.app/#/view/doc-hi6-sdk/zh/1-intro/2-plugin-app-concept?cont_model=${cont_model}" style="color:#222222">插件应用</a> |
| :--- | :--- | :--- |
| **描述** | 直接在 TP 屏幕上运行的应用程序<br>(例如，[Cimon Xpanel](https://hrbook-hrc.web.app/#/view/doc-hi6-tp-app/zh/2-installation/2-install?cont_model=${cont_model})) | 通过 COM 上的内部服务器运行的应用程序<br>(例如，[pickit](https://hrbook-hrc.web.app/#/view/doc-hi6-pickit/zh/README)) |
| **安装位置** | TP | COM |
| **安装命令** | **`xcopy`** | **`rcopy`** |

</div>

#### 命令类型

a. `xcopy`
- **用法**：用于在 TP 内部安装专用 TP 应用程序。
- **格式**：`xcopy` \[源路径\] \[目标路径\]
- **行为**：
  - 复制方法取决于目标路径末尾是否有斜杠（/）。
  - 当末尾没有 / 时（通过指定名称进行复制）：
    - 源文件夹/文件被复制并用指定名称覆盖目标路径。  
    - 例) `xcopy` $(AppDir)/Xpanel /usr/share/hyundai/hi6/apps/Xpanel2  
          ➔ 以名称 Xpanel2 复制。
  - 当末尾有 / 时（复制到子文件夹）：
    - 源文件夹/文件以其原始名称复制到目标路径。  
    - 例) `xcopy` $(AppDir)/Xpanel /usr/share/hyundai/hi6/apps/ 
           ➔ 整个 Xpanel 文件夹被复制到 apps 文件夹中。

b. `rcopy` 
- **用法**：用于在 COM 内部安装插件应用程序。
- **格式**：`rcopy` \[源路径\] \[目标路径\]
- **行为**：
  - 源与其原始文件夹名称被上传到目标路径中指定的文件夹内。
  - 如果目标路径在服务器上不存在，它会自动创建层级文件夹 (`mkdir`)。
  - 例) `rcopy $(AppDir)/pickit $(RemoteAppsDir)/temp`  
        ➔ 如果 `$(RemoteAppsDir)` 上的 `temp` 文件夹不存在，它会创建该文件夹，然后将 `pickit` 文件夹上传到其中。

#### 路径宏

<div style="max-width:fit-content;">

| 宏 | 描述 |
| :--- | :--- |
| **`$(AppDir)`** | 插入的 USB 上 `hrc_installer` 可执行文件的位置<br>(对应于第 2.2.1 节中的 `Install_Apps`) |
| **`$(RemoteAppsDir)`** | COM 上默认的应用程序安装路径 |
| **`$(RemoteReleaseDir)`** | COM 上内置插件应用程序的安装路径 |

{% hint style="warning" %}
普通用户在使用 `rcopy` 命令时，应使用 `$(RemoteAppsDir)` 作为目标路径。
{% endhint %}

</div>

#### 编写格式

应用程序安装程序配置文件的格式如下：

<div style="max-width:fit-content;">

```bash
# 应用程序描述

# 注释
命令 源路径 目标路径
```

</div>

- `应用程序描述` 必须作为注释添加到第一行。  
  此字段的内容将在执行 `hrc_installer` 程序时显示在 `标题栏` 上。

- `注释` 用于添加对配置命令的补充说明，并且不会出现在安装程序的日志屏幕上。

- `命令行` 必须按照上述 `命令类型` 部分中描述的 `格式` 编写。

例) xcopy

<div style="max-width:fit-content;">

```bash
# XPanel 

# 在 TP 应用程序下安装 Xpanel 和 XpanelFiles
xcopy Xpanel /usr/share/hyundai/hi6/apps/
xcopy XpanelFiles /usr/share/hyundai/hi6/apps/Xpanel/
```
</div>

例) rcopy

```bash
# 应用程序描述 

rcopy $(AppDir)/App_Name $(RemoteAppsDir)
```

</div>