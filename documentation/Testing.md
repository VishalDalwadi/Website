---
layout: plain
title: Testing
description: >
How to use and extend the test pipelines.
hide_description: true
---

Polypheny-DB deploys has three independent test suites for testing different aspects:
- Matrix   
- Docker CI
- Adapter Matrix CI


## Matrix  
This is the main test suite, executing all (unit) tests, as long as not specified otherwise. The tests or executed on different operating systems and using different Java versions.


## Docker CI
This test suite only handles the Docker specific testing. This test suite should not be used for testing adapters or other aspects of the Polypheny-DB. To add tests to this test suite, the class has to be annotated with the `DockerManager` category:
```java
@Category(DockerManagerTest.class)
public class TestClass {
```


## Adapter Matrix CI
This test suite allows to execute tests for all adapters and therefore serves as an integration testing. This is also extremely helpful when working on new adapters or improving existing ones.

To include a new adapter in the testing suite, one has to do following three steps:
- Add the adapter-specific deployment settings in ```org.polypheny.db.catalog.Adapter```.
- Create a new class in ```core/src/test/java/org/polypheny/db/excluded/``` using the following naming schema ```[NameOfAdapter]Excluded```.
- Add the name of the adapter to the matrix list in ```.github/workflows/integration.yml```.

### Exclude Tests from Integration Testing Pipeline
To exclude specific tests not yet supported by an adapter, these tests can be annotated with the adapter specific ```[NameoFAdapter]Excluded``` class:
```java
@Category([NameOfAdapter]Excluded.class)
public void testingMethodYourAdapterDoesNotSupport(...
```

The same annotation can also be used to exclude whole classes:
```java
@Category([NameOfAdapter]Excluded.class)
public class TestClassWhichYourAdapterNotSupports {
```

  
  

