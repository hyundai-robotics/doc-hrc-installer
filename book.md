
[__SOURCE](./README.md)
# ${cont_model} 控制器功能手册 - 应用程序安装程序
[__SOURCE](0-about-this-manual/README.md)
# 关于手册
[__SOURCE](0-about-this-manual/precautions.md)
# 注意事项

{% include file="zh/precautions.md" %}
[__SOURCE](0-about-this-manual/safety-notice.md)
# 安全注意事项

{% include file="zh/safety-notice.md" %}
[__SOURCE](1-intro/README.md)
# 1. 概述

要完全理解本手册，您应该熟悉以下内容：

- [${cont_model} 控制器操作手册 - TP630](https://hrbook-hrc.web.app/#/view/doc-hi6-operation/zh-tp630/README?cont_model=${cont_model})
- [${cont_model} 控制器功能手册 - 教学挂件应用](https://hrbook-hrc.web.app/#/view/doc-hi6-tp-app/zh/README?cont_model=${cont_model})
- [${cont_model} 控制器功能手册 - SDK](https://hrbook-hrc.web.app/#/view/doc-hi6-sdk/zh/README?cont_model=${cont_model})
- [${cont_model} 控制器功能手册 - 开放 API](https://hrbook-hrc.web.app/#/view/doc-hi6-open-api/zh/README?cont_model=${cont_model})

本手册提供有关如何使用 `hrc_installer` 安装用户开发的应用程序的说明。
[__SOURCE](2-preparation/README.md)
# 2. 准备

本章节解释安装的准备步骤。

2.1. [下载 `App Installer`](./1-download.md)  
2.2. [编写 `App Installer` 配置文件](./2-config.md)
[__SOURCE](2-preparation/1-download.md)
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
[__SOURCE](2-preparation/2-config.md)
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
[__SOURCE](3-execution/README.md)
# 3. 执行

使用准备好的应用程序安装 USB，进行实际的应用程序安装并检查结果。
[__SOURCE](3-execution/1-run.md)
## 3.1 运行安装程序

一旦 USB 准备好，请按照以下步骤运行安装程序。

#### 第一步

将准备好的应用程序安装程序的 USB 内存棒插入 TP 的 USB 端口。

<figure>
  <img src="../_assets/0_usb_ready.png" style="max-height:200px;">
  <figcaption>图 1. 准备好的 USB 文件夹结构和配置文件内容</figcaption>
</figure>

检查 TP 主屏幕上的 USB 连接状态。

<figure>
  <img src="../_assets/0_usb_ready_tp_home.png" style="max-height:350px;">
  <figcaption>图 2. 在任务栏上检查 USB 连接状态</figcaption>
</figure>

#### 第二步

验证应用程序安装程序。  
- TP 主屏幕 > 服务 > 10: 应用 > 点击位置 > 点击您想要安装的应用程序安装程序

<figure>
  <img src="../_assets/1_app_installer_list.png" style="max-height:350px;">
  <figcaption>图 3. 验证安装程序</figcaption>
</figure>

#### 第三步

点击应用程序屏幕底部的 [F4: 运行] 按钮以执行安装程序。

<figure>
  <img src="../_assets/2_app_installer_executed.png" style="max-height:350px;">
  <figcaption>图 4. 安装程序执行屏幕</figcaption>
</figure>

#### 第四步

点击 [开始] 以继续安装。

<figure>
  <img src="../_assets/3_app_installer_start.png" style="max-height:350px;">
  <figcaption>图 5-1. 安装程序开始屏幕</figcaption>
</figure>

<figure>
  <img src="../_assets/4_app_installer_finish.png" style="max-height:350px;">
  <figcaption>图 5-2. 安装程序完成屏幕</figcaption>
</figure>

#### 第五步

安装完成后，点击 [退出] 按钮以关闭安装程序，然后 `重启控制器。`

<div style="max-width:fit-content;">

{% hint style="warning" %}
安装的应用程序只有在控制器重启后才能正常运行。
{% endhint %}

</div>
[__SOURCE](3-execution/2-result.md)
## 3.2 检查执行日志

安装进度和最终结果可以通过安装程序界面中心的日志窗口实时查看。  
您可以通过文本颜色直观地了解状态。

### 3.2.1 日志颜色指南

- 🟦 蓝色：当前正在执行的命令（例如，xcopy、rcopy等）
- ⬛ 黑色：复制进度状态和一般细节
- 🟩 绿色：单个命令成功处理（通过）
- 🟥 红色：单个命令失败及错误原因（失败）

### 3.2.2 确定最终安装结果

- 一旦所有任务完成，最终摘要结果将在日志窗口的最后一行打印出来。
- TotalLines指的是在`hrc_installer.cfg`中输入的命令行数量。
- 安装成功： 🟩 PASS: TotalLines=[Total number of commands], NG=0  
  -> 这意味着所有应用程序安装均已正常完成，没有任何错误。

<figure>
  <img src="../_assets/4_app_installer_finish.png" style="max-height:350px;">
  <figcaption>图6. 安装程序正常完成界面</figcaption>
</figure>

- 安装失败： 🟥 FAIL: TotalLines=[Total number of commands], NG=[Number of failures]  
  -> 这意味着某些或所有命令发生了错误。请检查上面的红色错误信息并采取措施。

<figure>
  <img src="../_assets/5_app_installer_finish.png" style="max-height:350px;">
  <figcaption>图7. 安装程序执行失败界面</figcaption>
</figure>
[__SOURCE](4-troubleshooting/README.md)
# 4. 故障排除与错误代码

如果在安装过程中发生 `FAIL`，请检查日志窗口中打印的红色错误消息，并参考下表以识别原因并采取行动。

<div style="max-width:fit-content;">


| 错误消息 (日志输出) | 原因和措施 |
| :--- | :--- |
| **不支持的命令** | 不支持的命令。检查 `hrc_installer.cfg` 文件中的命令（`xcopy`，`rcopy` 等）是否有打字错误。 |
| **删除先前文件夹失败。** | 在目标路径删除现有文件夹失败。这可能是控制器的内部权限问题，或文件可能正在使用中。 |
| **创建目标文件夹失败。** | 创建目标文件夹失败。检查控制器的存储容量或权限设置。 |
| **复制文件失败。** | 在实际文件复制过程中失败。检查源文件是否损坏或验证USB连接状态。 |
| **失败：无效的目的地。只允许 '$(RemoteAppsDir)' 或 '$(RemoteReleaseDir)'。** | 使用 `rcopy` 时指定了无效的目标路径。确保在目标路径中准确使用了允许的宏。 |
| **失败：本地源路径不存在。** | 命令中指定的本地源路径（USB内部）无法找到。检查路径中的打字错误或验证实际文件在USB上是否存在。 |
| **失败：网络连接失败。** | 执行 `rcopy` 时远程通信失败。检查TP与控制器（COM）之间的网络连接状态。 |
| **失败：API 'isExist' / 'mkdir' / 'upload' / 'rdelete' 调用失败。** | 在远程服务器上调用文件控制API（查询/创建/上传/删除）失败。检查控制器的系统状态。 |
| **失败：扫描期间源消失。（检查USB连接）** | 在文件扫描期间源目标消失。检查在安装过程中USB连接是否断开。 |
| **失败：无法打开本地文件。（USB连接/文件权限问题）** | 读取本地文件失败。检查文件的读取权限或USB连接问题。 |
| **失败：进程行崩溃。/ 无法启动。/ 失败。** | 用户指定的外部系统命令（`>`）进程未正常执行或在执行期间异常终止。 |

</div>