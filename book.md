
[__SOURCE](./README.md)
# ${cont_model} Controller Function Manual - App Installer 

[__SOURCE](0-about-this-manual/precautions.md)
# Precautions

{% include file="en/precautions.md" %}

[__SOURCE](1-intro/README.md)
# 1. Overview

To fully understand this manual, you should be familiar with the following:

- [${cont_model} Controller Operation Manual - TP630](https://hrbook-hrc.web.app/#/view/doc-hi6-operation/en-tp630/README?cont_model=${cont_model})
- [${cont_model} Controller Function Manual - Teach Pendant App](https://hrbook-hrc.web.app/#/view/doc-hi6-tp-app/en/README?cont_model=${cont_model})
- [${cont_model} Controller Function Manual - SDK](https://hrbook-hrc.web.app/#/view/doc-hi6-sdk/en/README?cont_model=${cont_model})
- [${cont_model} Controller Function Manual - Open API](https://hrbook-hrc.web.app/#/view/doc-hi6-open-api/en/README?cont_model=${cont_model})

This manual provides instructions on how to install user-developed apps using the `hrc_installer`.

[__SOURCE](2-preparation/README.md)
# 2. Preparation

This chapter explains the preparation steps for installation.

2.1. [Download `App Installer`](./1-download.md)  
2.2. [Write `App Installer` configuration file](./2-config.md)

[__SOURCE](2-preparation/1-download.md)
## 2.1 Download Installer

### 2.1.1. App Installer Download Process

1. Download the `hrc_installer.zip` file from the [HD Hyundai Robotics Download Center](https://hd-hyundairobotics.com/download-center/list).  
  <img src="../_assets/0_download_center.png" style="max-height:250px;">
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

[__SOURCE](2-preparation/2-config.md)
## 2.2 Writing Configuration File

Once the App Installer is ready on the USB, proceed with the following steps.

1. Place the app to install inside the `apps` folder.
2. Modify the App Installer configuration file (`hrc_installer.cfg`).

### 2.2.1. Placing the App to Install

Copy the target App folder or file you wish to install on the COM (controller) or TP (Teach Pendant) under the `apps` folder.

<div style="max-width:fit-content;">

```text
📁 USB_ROOT (USB Root)
 ┗ 📁 hi6
    ┗ 📁 apps
       ┗ 📁 Install_Apps
          ┣ 📄 hrc_installer
          ┣ 📄 hrc_installer.cfg
          ┣ 📄 hrc_installer.png
          ┣ 📄 info.json
          ┗ 📁 mastering           <-- (App to install)
```

</div>


### 2.2.2. Writing the App Installer Configuration File (hrc_installer.cfg)

The configuration file (hrc_installer.cfg) serves as a type of task specification.  
For proper operation, edit it with a text editor according to the format below.


#### App Types

The commands used in the App Installer configuration file vary depending on the type of app you intend to install.
Please refer to the table below.

<div style="max-width:fit-content;">


| Category | <a href="https://hrbook-hrc.web.app/#/view/doc-hi6-tp-app/en/1-intro/README?cont_model=${cont_model}" style="color:#222222">TP App</a> | <a href="https://hrbook-hrc.web.app/#/view/doc-hi6-sdk/en/1-intro/2-plugin-app-concept?cont_model=${cont_model}" style="color:#222222">Plug-in App</a> |
| :--- | :--- | :--- |
| **Description** | App running directly on the TP screen<br>(e.g., [Cimon Xpanel](https://hrbook-hrc.web.app/#/view/doc-hi6-tp-app/en/2-installation/2-install?cont_model=${cont_model})) | App running via the internal server on the COM<br>(e.g., [pickit](https://hrbook-hrc.web.app/#/view/doc-hi6-pickit/en/README)) |
| **Installation Location** | TP | COM |
| **Installer Command** | **`xcopy`** | **`rcopy`** |

</div>


#### Command Types

a. `xcopy`
- **Usage**: Used to install a TP-dedicated app inside the TP.
- **Format**: `xcopy` \[Source Path\] \[Target Path\]
- **Behavior**:
  - The copying method changes depending on whether there is a slash (/) at the end of the target path.
  - When there is no / at the end (Copy by specifying a name):
    - The source folder/file is copied and overwrites the target path with the specified name.  
    - Ex) `xcopy` $(AppDir)/Xpanel /usr/share/hyundai/hi6/apps/Xpanel2  
          ➔ Copied under the name Xpanel2.
  - When there is a / at the end (Copy to subfolder):
    - The source folder/file is copied into the target path with its original name.  
    - Ex) `xcopy` $(AppDir)/Xpanel /usr/share/hyundai/hi6/apps/ 
           ➔ The entire Xpanel folder is copied into the apps folder.

b. `rcopy` 
- **Usage:** Used to install a plug-in app inside the COM.
- **Format:** `rcopy` \[Source Path\] \[Target Path\]
- **Behavior**:
  - The source is uploaded with its original folder name inside the folder specified in the target path.
  - If the target path does not exist on the server, it automatically creates the hierarchical folders (`mkdir`).
  - Ex) `rcopy $(AppDir)/pickit $(RemoteAppsDir)/temp`  
        ➔ If the `temp` folder does not exist in `$(RemoteAppsDir)` on the COM, it creates the folder and then uploads the `pickit` folder under it.


#### Path Macros

<div style="max-width:fit-content;">


| Macro | Description |
| :--- | :--- |
| **`$(AppDir)`** | The location of the `hrc_installer` executable file on the inserted USB<br>(Corresponds to `Install_Apps` in section 2.2.1) |
| **`$(RemoteAppsDir)`** | The default app installation path on the COM |
| **`$(RemoteReleaseDir)`** | The built-in plug-in app installation path on the COM |


{% hint style="warning" %}
General users should use `$(RemoteAppsDir)` as the target path when using the `rcopy` command.
{% endhint %}

</div>


#### Writing Format

The format for the App Installer configuration file is as follows:

<div style="max-width:fit-content;">

```bash
# App Description

# Comment
Command Source_Path Target_Path
```

</div>

- The `App Description` must be added as a comment on the very first line.  
  The content of this field will be displayed on the `title bar` when the `hrc_installer` program is executed.

- A `Comment` is used to add supplementary explanations for the configuration commands and will not appear on the installer's log screen.

- The `Command line` must be written according to the `Format` described in the `Command Types` section above.

Ex) xcopy

<div style="max-width:fit-content;">

```bash
# XPanel 

# Install Xpanel and XpanelFiles under TP apps
xcopy Xpanel /usr/share/hyundai/hi6/apps/
xcopy XpanelFiles /usr/share/hyundai/hi6/apps/Xpanel/
```
</div>

Ex) rcopy

```bash
# App_Description 

rcopy $(AppDir)/App_Name $(RemoteAppsDir)
```

</div>

[__SOURCE](3-execution/README.md)
# 3. Execution

Using the prepared App Installer USB, proceed with the actual app installation and check the results.

[__SOURCE](3-execution/1-run.md)
## 3.1 Run Installer

Once the USB is ready, follow the steps below to run the installer.

#### Step 1 

Insert the USB memory stick prepared with the App Installer into the USB port of the TP.

<figure>
  <img src="../_assets/0_usb_ready.png" style="max-height:200px;">
  <figcaption>Fig 1. Prepared USB folder structure and configuration file contents</figcaption>
</figure>

Check the USB connection status on the TP Home screen.

<figure>
  <img src="../_assets/0_usb_ready_tp_home.png" style="max-height:350px;">
  <figcaption>Fig 2. Checking the USB connection status on the taskbar</figcaption>
</figure>



#### Step 2

Verify the App Installer.  
- TP Home > Service > 10: App > Click Location > Click the App Installer you want to install

<figure>
  <img src="../_assets/1_app_installer_list.png" style="max-height:350px;">
  <figcaption>Fig 3. Verifying the Installer</figcaption>
</figure>


#### Step 3 

Click the [F4: Run] button at the bottom of the app screen to execute the installer.

<figure>
  <img src="../_assets/2_app_installer_executed.png" style="max-height:350px;">
  <figcaption>Fig 4. Installer execution screen</figcaption>
</figure>

#### Step 4

Click [START] to proceed with the installation.

<figure>
  <img src="../_assets/3_app_installer_start.png" style="max-height:350px;">
  <figcaption>Fig 5-1. Installer start screen</figcaption>
</figure>

<figure>
  <img src="../_assets/4_app_installer_finish.png" style="max-height:350px;">
  <figcaption>Fig 5-2. Installer finish screen</figcaption>
</figure>



#### Step 5

Once the installation is complete, click the [Exit] button to close the installer, and then `reboot the controller.`

<div style="max-width:fit-content;">

{% hint style="warning" %}
The installed APP will operate normally only after the controller is rebooted.
{% endhint %}

</div>

[__SOURCE](3-execution/2-result.md)
## 3.2 Check Execution Logs

Installation progress and final results can be checked in real-time through the log window in the center of the installer screen.  
You can intuitively understand the status through the text colors.

### 3.2.1 Log Color Guide

- 🟦 Blue: Currently executing command (e.g., xcopy, rcopy, etc.)
- ⬛ Black: Copy progress status and general details
- 🟩 Green: Individual command successfully processed (Pass)
- 🟥 Red: Individual command failed and the reason for the error (Fail)

### 3.2.2 Determining Final Installation Results

- Once all tasks are completed, the final summary result is printed on the very last line of the log window.
- TotalLines refers to the number of command lines entered in `hrc_installer.cfg`.
- Installation Success: 🟩 PASS: TotalLines=[Total number of commands], NG=0  
  -> This means all app installations were completed normally without any errors.

<figure>
  <img src="../_assets/4_app_installer_finish.png" style="max-height:350px;">
  <figcaption>Fig 6. Installer normal completion screen</figcaption>
</figure>

- Installation Failure: 🟥 FAIL: TotalLines=[Total number of commands], NG=[Number of failures]  
  -> This means an error occurred in some or all of the commands. Check the red error message above and take action.

<figure>
  <img src="../_assets/5_app_installer_finish.png" style="max-height:350px;">
  <figcaption>Fig 7. Installer execution failure screen</figcaption>
</figure>

[__SOURCE](4-troubleshooting/README.md)
# 4. Troubleshooting & Error Codes

If a `FAIL` occurs during installation, check the red error message printed in the log window and refer to the table below to identify the cause and take action.

<div style="max-width:fit-content;">


| Error Message (Log Output) | Cause and Action |
| :--- | :--- |
| **Unsupported command** | Unsupported command. Check for typos in the commands (`xcopy`, `rcopy`, etc.) in the `hrc_installer.cfg` file. |
| **Failed in deleting previous folder.** | Failed to delete an existing folder at the target path. It may be an internal permission issue on the controller, or the file might be in use. |
| **Failed in creating destination folder.** | Failed to create the destination folder. Check the controller's storage capacity or permission settings. |
| **Failed in copying file.** | Failed during the actual file copying process. Check if the source file is damaged or verify the USB connection status. |
| **Fail: Invalid destination. Only '$(RemoteAppsDir)' or '$(RemoteReleaseDir)' are allowed.** | Invalid destination path specified when using `rcopy`. Make sure you used the permitted macros exactly in the target path. |
| **Fail: Local source path does not exist.** | The local source path (inside the USB) specified in the command cannot be found. Check for typos in the path or verify that the actual file exists on the USB. |
| **Fail: Network connection failed.** | Remote communication failed when executing `rcopy`. Check the network connection status between the TP and the controller (COM). |
| **Fail: API 'isExist' / 'mkdir' / 'upload' / 'rdelete' call failed.** | Failed to call the file control API (inquiry/create/upload/delete) on the remote server. Check the controller's system status. |
| **Fail: Source disappeared during scan. (Check USB connection)** | The source target disappeared during the file scan. Check if the USB connection was disconnected during installation. |
| **Fail: Cannot open local file. (USB connection / File Permission issue)** | Failed to read the local file. Check the file's read permissions or for USB connection issues. |
| **Fail: Process line crashed. / cannot be started. / failed.** | The external system command (`>`) process specified by the user did not execute normally or terminated abnormally during execution. |

</div>
