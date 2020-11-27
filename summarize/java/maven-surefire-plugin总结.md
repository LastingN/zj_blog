# maven-surefire-plugin总结

###  前言

Maven本身并不是一个单元测试框架，Java世界中主流的单元测试框架为JUnit和TestNG。Maven所做的只是在构建执行到特定生命周期阶段的时候，通过插件来执行JUnit或者TestNG的测试用例。这一插件就是maven-surefire-plugin，可以称之为测试运行器（Test Runner），他能很好的兼容JUnit 4、JUnit 5以及TestNG。



### 如何使用

maven依靠maven-surefire-plugin来执行单元测试。在**pom.xml**中基本配置如下

```xml
<plugin>
  <artifactId>maven-surefire-plugin</artifactId>
  <version>2.22.1</version>
  <configuration>
    <skip>${unit.test.skip}</skip>
    <excludes>
    	<exclude>**/AppTest2.java</exclude>
    </excludes>
  </configuration>
</plugin>
```
#### 常用配置

##### 跳过测试阶段

```xml
<unit.test.skip>false</unit.test.skip>
<skip>${unit.test.skip}</skip>
//不但跳过单元测试的运行，也跳过测试代码的编译。

<skipTests>true</skipTests>
//跳过单元测试，但是会继续编译
```

同时可以在执行时修改

```
-Dmaven.test.skip=true
//不但跳过单元测试的运行，也跳过测试代码的编译。

-DskipTests
//跳过单元测试，但是会继续编译
```



##### 忽略测试失败 

```xml
<testFailureIgnore>true</testFailureIgnore> 
```



##### 包含与排除特定的测试类

surefire默认查找的是

+ \*\*/Test*.java
+ \*\*/*Test.java
+ \*\*/*TestCase.java

可自定义包含与排除，支持正则

```xml
<includes>
    <include>Sample.java</include>
    <include>\bTest\b</include>
</includes>
```

```xml
<excludes>
    <exclude>**/*Test.java</exclude>
    <exclude>**/Test*.java</exclude>
</excludes>
```




