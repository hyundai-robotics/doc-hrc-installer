# 4. Troubleshooting & Error Codes

If a `FAIL` occurs during installation, check the red error message printed in the log window and refer to the table below to identify the cause and take action.


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
