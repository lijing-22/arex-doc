
## script(Post Request TESTS)

You can write test scripts (Javascript) in the tab "Tests" to verify if your API is working correctly.

These tests are executed after the response has been returned, hence the name "post-request tests". The results appear in the same tab under "Test reports".

Getting Started  
The global pw object contains methods test and expect.

Use pw.expect directly for quick and convenient testing. Every pw.expect statement will generate a line on the test report.
```
// This test will pass
pw.expect(1).toBe(1);

// This test will fail
pw.expect(2).not.toBe(2);
```
Wrap expect statements with pw.test to group and describe related statements.
```
// This will return 4 lines on the test report, grouped under "Arithmetic operations"
pw.test("Arithmetic operations", () => {
  const size = 500 + 500;
  pw.expect(size).toBe(1000);
  pw.expect(size - 500).toBe(500);
  pw.expect(size * 4).toBe(4000);
  pw.expect(size / 4).toBe(250);
});
```
If neither a pw.expect nor pw.test statement is present, no test reports will be generated.
```
// This will not generate any test reports
(99 + 1).toBe(100);
```

#### pw API
.expect(value)
.response
.not
.toBe(value)
.toBeLevel2xx(value)
.toBeLevel3xx(value)
.toBeLevel4xx(value)
.toBeLevel5xx(value)
.toBeType(type)
.toHaveLength(number)
.test(name, fn)
.expect(value)
expect returns an expectation object, on which you can call matcher functions. The example below calls the matcher function toBe on the expectation object that is returned by calling pw.expect with the response id (pw.response.body.id) as an argument.
```
pw.test("Id of the response is equal to 1", () => {
  pw.expect(pw.response.body.id).toBe(1);
});
```
Note that you can pass any valid JavaScript as an argument for expect.
```
pw.test("Price is between $100 and $150", () => {
  const price = pw.response.body.price;
  pw.expect(price > 100 && price < 150).toBe(true);
  pw.expect(pw.response.body.currency).toBe("USD");
});
```

.response
Assert response data by accessing the pw.response object.
```
// This test will pass
pw.test("Response is ok", () => {
  pw.expect(pw.response.status).toBe(200);
  pw.expect(pw.response.body).not.toHaveProperty("errors");
});
```
Currently supported response values
* status: -number- The status code as an integer.
* headers: -object- The response headers.
* body: -object- the data in the response. In many requests, this is the JSON sent by the server.

.not
Test for the inverse by adding .not before calling the matcher function.
```
// These tests will pass
pw.expect(true).not.toBe(false);
pw.expect(200).not.toBeLevel3xx();
```
.toBe(value)
Test for exact equality using toBe.
```
pw.expect(pw.response.body.category).toBe("Sneakers");
```
toBe uses strict equality and is recommended for primitive data types.
```
// These tests will fail
pw.expect("hello").toBe("Hello");
pw.expect(5).toBe("5");
pw.expect([]).toBe([]);
```
.toBeLevelxxx()
There are four different matcher functions for quick and convenient testing of the http status code that is returned:
```
toBeLevel2xx()
toBeLevel3xx()
toBeLevel4xx()
toBeLevel5xx()
```
For example, an argument passed to expect must be within 200 and 299 inclusive to pass toBeLevel2xx().
```
// These tests will pass
pw.expect(204).toBeLevel2xx();
pw.expect(308).toBeLevel3xx();
pw.expect(404).toBeLevel4xx();
pw.expect(503).toBeLevel5xx();
```
If the argument passed to expect is a non-numeric value, it is first parsed with parseInt().
```
// This test will pass
pw.expect("404").toBeLevel4xx();
```

.toBeType(type)
Use .toBeType(type) for type checking. The argument for this method should be "string", "boolean", "number", "object", "undefined", "bigint", "symbol" or "function".
```
// These tests will pass
pw.expect(5).toBeType("number");
pw.expect("Hello, world!").toBeType("string");

pw.expect(5).not.toBeType("string");
pw.expect("Hello, world!").not.toBeType("number");
```

.toHaveLength(number)
Use .toHaveLength(number) to check that an object has a .length property and it is set to a certain numeric value.
```
// These expectations will pass
pw.expect("hoppscotch").toHaveLength(10);
pw.expect("hoppscotch").not.toHaveLength(9);

pw.expect(["apple", "banana", "coconut"]).toHaveLength(3);
pw.expect(["apple", "banana", "coconut"]).not.toHaveLength(4);
```

.test(name, fn)
Create a group of tests, with the name as a string and fn as a callback function to write tests associated with the group. The test results will include the given name for better organization.

You cannot nest .test within a .test callback function.
```
pw.test("a group of tests", () => {
  pw.expect(10).toBe(10);
  // more tests here
});
```
Examples
Test whether the response status code is 200.
```
pw.test("Response status is 200", () => {
  pw.expect(pw.response.status).toBe(200);
});
Parse the data as JSON and assert properties from the response body.
pw.test("Project name is Hoppscotch", () => {
  const project = pw.response.body.json();
  pw.expect(project.name).toBe("Hoppscotch");
  pw.expect(project.name).toBeType("string");
});
```