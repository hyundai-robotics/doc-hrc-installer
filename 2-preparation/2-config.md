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
