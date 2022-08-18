# AREX录制回放测试

## 录制回放的首页面
![](../resource/c5.0.png)
* 菜单区选择Replay按钮
* 选择Workspace区的AREX录制应用(应用配置AREX Agent后,自动显示在此处)
* 选择了具体应用后,工作区显示此应用的报告信息

### 执行录制回放
* 点击"Start Replay"执行回放  
![](../resource/c5.1.png)

### 回放记录和细节
![](../resource/c5.2.png)
* 在列表中选择回放记录(ReportName中包含回放的具体时间)
* 最新的回放记录显示在最前面,可以通过翻页来查看更多的历史
* 在选择回放记录后,进入Report看具体的回放结果

### 回放报告
![](../resource/c5.3.png)
* 报告的最上方,"Rerun"可以在此重新运行测试
* 显示本次执行的测试统计
* 显示本次执行的每个API测试结果,时间,案例,通过情况
* 当发现Failed非0时(比对发现差异),点击Analysis查看报文差异

### 报文差异

![](../resource/c5.4.png)  
1. 左上方,选择差异节点所在的API调用关系  
![](../resource/c5.5.png)  
被测试API:/Owners,及其接口内部调用的第三方依赖,比如数据库  
![](../resource/c5.6.png)  
2. 操作1后,显示场景数,用例数,点击右边"Scenes"  
3. Diff Detail显示差异节点,差异信息如下图  
![](../resource/c5.7.png)  