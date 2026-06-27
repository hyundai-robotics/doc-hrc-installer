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