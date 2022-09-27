# AREX recording and playback test

## The first page of the recording and playback
![](../resource/c5.0.png)
* Select the Replay button in the menu area
* Select the AREX recording application in the Workspace area (after the application is configured with AREX Agent, it will be automatically displayed here)
* After selecting a specific application, the work area displays the report information of this application

### Perform recording playback
* Click "Start Replay" to perform playback
![](../resource/c5.1.png)

### Playback records and details
![](../resource/c5.2.png)
* Select the playback record in the list (ReportName contains the specific time of playback)
* The latest playback record is displayed at the front, you can view more history by turning the page
* After selecting the playback record, enter Report to see the specific playback results

### Playback report
![](../resource/c5.3.png)
* At the top of the report, "Rerun" can rerun the test here
* Display the test statistics of this execution
* Display each API test result, time, case and pass status of this execution
* When it is found that Failed is not 0 (differences are found by comparison), click Analysis to view the packet differences

### Packet difference

![](../resource/c5.4.png)
* The upper left area is the main interface and its external calls
![](../resource/c5.5.png)
* Tested API:/Owners, and third-party dependencies called internally by its interface, such as databases
![](../resource/c5.6.png)
* Display the number of scenes, the number of use cases, click "Scenes" on the right
* Diff Detail shows the difference node, the difference information is as follows
![](../resource/c5.7.png)