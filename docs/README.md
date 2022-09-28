# <img src="https://avatars.githubusercontent.com/u/103105168?s=200&v=4" alt="Arex Icon" width="27" height=""/>AREX
**Real automated API testing with real data.**

## Solve pain points
1. Reduce the cost of interface testing
2. Improve interface test coverage

**Interface testing** is a type of software testing, which includes two types of testing: in a narrow sense, it refers to the test directly aimed at the function of the application program interface (abbreviated as API test); in a broad sense, it refers to the integration test, through calling The overall functional completion, reliability, security and performance indicators of API testing. The activities of interface testing include, test design, test case writing, test script writing, test data construction, test execution, and further, in continuous development During the maintenance process, continuous inspection and maintenance of the four active outputs should be carried out.
  
**AREX** can minimize the implementation cost of testing activities, improve test coverage, improve quality, and free up more development engineers and test engineers' time.
  
## new version update

##### v0.2.3 version 2022/09/28
* Fix bugs
   * Chrome plugin supports user-defined cookies, please download the latest plugin ([link](https://github.com/arextest/arex-chrome-extension/releases))
   * Fix the bug that playback Rerun does not take effect
   * After the page is refreshed, it will be relocated to the current page according to the route, and the initial blank page will no longer be displayed
   * Fixed an occasional error when clicking to refresh the page.
   * Fixed the bug that the entire tab disappears when clicking on the right-most quick menu bar in the settings page.
   * Fixed the problem that Get request parameters are spliced ​​twice
* New features
   * Added plugin version upgrade reminder (e.g. Chrome Extension version is incorrect, please install1.0.4)
   * Supports the use of Environment variables in request parameters
   * Support Environment clone function
   * Added dynamic class configuration function.
   * Added the paging function of playing back the Case list page.
   * Optimize the text description of the comparison details card.
* Main goals of the planned 10/17 version (0.2.4)
   * Fix the bug of 0.2.3
   * Added AREX recording and playback test case extraction (the interface has been completed/Report, front-end development)
   * Optimized the result display interface of the comparison test (postponed)
   * config module and report module are merged into arex-api module
   * Add a single comparison use case error display page in the front end
* Known Issues
    * When the interface is tested, the page with too much data cannot be loaded
    * "Start Replay" playback range uses "Case Range" in the configuration item to default to 1. When the recording time is inconsistent with the configuration, the playback fails (prompt that there is no use case), and the "Playback Time Range" configuration needs to be added to this interface.
    * There is a bug in the script ASSERT function

##### v0.2.2 version 2022/09/09
* The version in Docker-compose in the Deployment warehouse is set to latest, and the specified version number is no longer written.  
   1.1 The docker version generated this time is 0.2.2, the Latest Tag has been updated, and Docker HUB has been uploaded   
   1.2 demo.arextest.com has been updated to the latest version (arex-aws-demo can perform playback)
* Fix bugs in 0.2.1
   2.1 Front-end interface optimization  
   2.2 Arex Agent version has been upgraded, users need to update AREX-Agent  
* Add function
   3.1 Front-end Record configuration, Replay configuration and other functions
   3.2 Add Sidecar configuration demo (sidecar directory)
   3.3 Helm configuration (arex-chart directory)
   Users need to modify storageClass by themselves
   Installation command helm install your-demo-name ./arex-chart --namespace yourNamespace
* Main goals of the planned 9/26 version (0.2.3)
    * Fix the bug of 0.2.2
    * Add AREX recording and playback test case extraction (the interface has been completed/Report, front-end development)
    * Optimize the result display interface of the comparison test (postponed)
    * Basic functions of template management
* Known Issues
    * In the AREX playback test, the single use case Rerun failed
    * Custom cookie settings in conventional test cases do not take effect (browser plug-in)

##### v0.2.1 2022/08/29
1. Go to MySQL: Config service, schedule service go to mysql, change to read and write mongodb
2. The generated docker version tag is unified to 0.2.1
3. Arex-agent upgrade, version upgrade to 0.1.0
4. The mailbox registration code function is upgraded when logging in (the user does not need to perceive and configure the smtp server, and arex provides services uniformly)
5. Release branch: The branch released by the arex project is rel/1.0.1, and the others are the main branch
6. Major bug fixes
    * Playback prompts the problem that the recording action is empty
    * Failed to record and playback under tomcat
7. In the 9/2 plan, the function of delayed release
    * Complete the Helm configuration (debugging is not complete)
    * Optimize the result display interface of the comparison test (postponed)
    * Add AREX recording and playback test case extraction (the interface has been completed/Report, front-end development)
8. The version originally planned on 9/2 will no longer be released, and a new version 0.2.2 is planned to be released on 9/12 (two weeks apart).
9. 9/12 Version 0.2.2 Main Goals
    * The front-end recording configuration, playback configuration and other functions are released
    * Fix bugs

##### v0.2 2022/08/19
* New UI revision
* Add routine test, comparison test
* Modify the parameter reading method of each service, support the user to configure the interface address
* The front-end image name is changed from replay-front-end to arex, and the service name is also changed to arex
* Mirror version upgraded to 0.2
* Known issue: Version incompatibility (AREX to MYSQL is not completed, when subsequent upgrades (without mysql), the historical "recording and playback records" will be lost)

##### v0.1 2022/03/16 Initial version