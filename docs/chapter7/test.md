
## Script (Post Request TESTS)

You can write test scripts (Javascript) in the tab "Tests" to verify if your API is working correctly.

These tests are executed after the response has been returned, hence the name "post-request tests". The results appear in the same tab under "Test reports".

### Getting Started  
The global arex object contains methods test and expect.

Use arex.expect directly for quick and convenient testing. Every arex.expect statement will generate a line on the test report.
```
// This test will pass
arex.expect(1).toBe(1);

// This test will fail
arex.expect(2).not.toBe(2);
```
Wrap expect statements with arex.test to group and describe related statements.
```
// This will return 4 lines on the test report, grouped under "Arithmetic operations"
arex.test("Arithmetic operations", () => {
  const size = 500 + 500;
  arex.expect(size).toBe(1000);
  arex.expect(size - 500).toBe(500);
  arex.expect(size * 4).toBe(4000);
  arex.expect(size / 4).toBe(250);
});
```
If neither a arex.expect nor arex.test statement is present, no test reports will be generated.
```
// This will not generate any test reports
(99 + 1).toBe(100);
```
