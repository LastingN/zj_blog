# maven-surefire-plugin总结

###  前言

Maven本身并不是一个单元测试框架，Java世界中主流的单元测试框架为JUnit和TestNG。Maven所做的只是在构建执行到特定生命周期阶段的时候，通过插件来执行JUnit或者TestNG的测试用例。这一插件就是maven-surefire-plugin，可以称之为测试运行器（Test Runner），他能很好的兼容JUnit 3、JUnit 4以及TestNG。

### 

