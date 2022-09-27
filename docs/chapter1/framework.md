## AREX composition
  
### Composition Summary
![](../resource/arch.png)
  
#### data storage
* Redis, Storage storage service uses cached data during playback
* MongoDB, storage service to store recorded data and playback results


### AREX module

#### AREX Frontend [AREX](https://github.com/arextest/arex)
AREX front-end is the front-end operation interface of the AREX tool
* Regular tests, Postman-like tests, use case settings, execution, result ASSERT, etc.
* Comparison test, send the same request to different interfaces, and compare the differences in the returned results, support non-MOCK test, but also support AREX real data MOCK test
* Playback test, compare test with real production data
  ![](../resource/c3.4.png)
  
#### Configuration Service [Configuration Service](https://github.com/arextest/arex-config)
##### Agent recording configuration
* Agent recording configuration, including recording time period, time range, frequency, etc.
![](../resource/configservice.record.basic.png)
* Alarm configuration recorded by Agent
![](../resource/configservice.record.advance.png)

##### AREX playback configuration
The time frame (days) that currently includes AREX playback use cases
CASE within one day, set 1 days

##### Compare diff configuration
* Excluded nodes to compare differences (do not align)
* Containing nodes to compare differences (must be aligned)
* Node sorting method of Array type (one-to-one relationship involving comparison)
* Reference node configuration (how to design references between nodes in complex messages, you can configure Reference association comparison)

#### Schedule Service [Schedule Service](https://github.com/arextest/arex-replay-schedule)
The scheduling service is responsible for sending a use case playback request to the service under test, and triggering result comparison and dependency comparison after the service responds
* Service scheduling
* The comparison execution of the comparison SDK

#### Storage Service [Storage Service](https://github.com/arextest/arex-storage)
* The storage service is responsible for receiving the requests captured by the Agent, responding, and storing the real data that depends on it.
* Provided to return the stored data as required by the Agent during playback

#### Report Analysis Service [Report Service](https://github.com/arexest/arex-report)
The report analysis service is responsible for the collection of test results and the display of problems when performing playback tests
