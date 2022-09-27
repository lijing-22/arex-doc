# Compare Testing

## Comparison test
The comparison test is to send the same message to two API addresses, and then compare the difference between the returned messages  
Scenarios used include  
* Compare the interface of the new and old versions, and do the verification and acceptance test
* Verification test after refactoring the system, the return results of the old and new systems after refactoring should theoretically remain unchanged
Wait 

![](../resource/comparetest1.png)
* Comparison test results, if there is a comparison difference, it will be displayed with a colored background (the yellow background in the above picture)
* Note: This interface is being optimized, and the interface display is unreasonable when there are multiple differences.