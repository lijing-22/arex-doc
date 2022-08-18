## Pre-script
arex.env.set("foo", "bar");

### Getting Started
A special arex object contains various methods for creating tests and is made available globally. It can be referenced by name to access methods such as arex.env.set().
```
arex.env.set("foo", "bar");
```
arex.env.set() can be used directly for quick and convenient environment variables definition. Or, if preferred, arex.env.set() can be used to better organize requet code.
```
arex.env.set("baseURL", "https://httpbin.org");
```
Accessing environment data.
It's desirable to write environment variables against a request. This is done by accessing the <<variable_name>> object.
```
<<baseURL>>>
```
Use environment variables enclosed in double angular brackets (<<>>) anywhere in request section.

Example: Environment variables with pre-request scripts
