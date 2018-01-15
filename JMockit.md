# JMockit#

## 简介##

基于Java 5 SE 的 java.lang.instrument 开发，内部使用ASM库来修改Java的Bytecode。

![img](http://jmockit.org/toolkit.png)

## mock v.s stub##

1. mock是基于**“行为”**的测试

   > Behavior-oriented(Expectations & Verifications)
   >
   > 主要测试CUT和被依赖者之间的相互调用，对mock目标代码的行为进行模仿，类似黑盒测试。
   >
   > 从被测试代码的使用角度出发，结合数据的输入输出来检验程序运行的正确性。

2. stub是基于**“状态”**的测试

   > State-oriented(MockUp<GenericType>)
   >
   > 主要测试CUT和被依赖着内部数据的状态，是站在目标测试代码内部的角度，对传入的参数进行检查、匹配，才返回某些结果，类似白合测试。

JMockit有两套API，mock API用来进行mock测试；mock-up API用来进行stub测试。

[JMockit GitHub Repository](https://github.com/jmockit/jmockit1)

[JMockit Tutorial](http://jmockit.org/tutorial.html)

```
Lets now see how mocking is done with this API. It is the largest one (three annotations, seven classes, and one interface), but that shouldn't be taken as a suggestion to use it in every test. On the contrary, mocking (as well as "faking", discussed in the next section) is best used in moderation.
```

## mock API##

基于行为验证的单元测试分为三步：

1. 测试类：在这个类中执行测试代码。
2. CUT（Code Under Test）：被测试的类，我希望验证这个类是不是能正确地工作。
3. 依赖类。CUT会调用依赖类的方法。

在JMockit中，测试需要record、replay、verify三个阶段，以测试CUT是否正确调用了依赖类，包括：

- 调用了那些方法
- 通过怎样的参数
- 调用了多少次
- 调用的相对顺序

![img](http://www.51testing.com/attachments/2013/07/14982672_2013071911040416DAo.jpg)

## 调用次数的限制##

在record和verify阶段可以使用times，minTimes，maxTimes来限制。默认为minTimes = 1。



## 移除NonStrictExpectations##

![snipaste_20171116_211659](C:\Users\Michael\Desktop\JMockit\snipaste_20171116_211659.png)

[Changes](http://jmockit.org/changes.html)

## 注意事项##

1. 在pom.xml文件中，jmockit一定要放在Junit之前，不然会出现错误。

   ```When using JUnit 4.5+, verify the jmockit dependency appears before JUnit in the classpath. Alternatively, annotate test classes with @RunWith(JMockit.class).
   When using JUnit 4.5+, verify the jmockit dependency appears before JUnit in the classpath. Alternatively, annotate test classes with @RunWith(JMockit.class).
   ```

   ```
   When using JUnit 5+ or TestNG 6.2+, simply add the jmockit dependency to the classpath.
   ```

   ```
   Note for Eclipse users: use the "Order and Export" tab of the "Java Build Path" window when specifying the order of jars in the classpath. Also, make sure the Eclipse project uses the JRE from a JDK installation instead of a "plain" JRE, since the latter ones lack the "attach" native library.
   ```

   [Getting started with JMockit](http://jmockit.org/gettingStarted.html)

   ```
   java.lang.IllegalStateException: JMockit wasn't properly initialized; check that jmockit.jar precedes junit.jar in the classpath
   ```

2. @Mocked和@Injectable区别

   - @Mocked 针对类型，在同一个测试用例中被修饰的对象都会被mock，对应的类和实例均受影响。
   - @Injectable 针对单个实例，仅mock被修饰的对象。

   ![snipaste_20171116_211457](C:\Users\Michael\Desktop\JMockit\snipaste_20171116_211457.png)

   ```
   @Injectable means that only the instance assigned to the mock field will have mock behavior; otherwise, all instances of the mocked class will be mocked.
   ```

   ​

3. mock parameters


![snipaste_20171116_211253](C:\Users\Michael\Desktop\JMockit\snipaste_20171116_211253.png)

```
Normally, JUnit/TestNG test methods are not allowed to have parameters. When using JMockit, however, such mock parameters are allowed. In general, it's best to use mock fields of the test class only when the mocked types are needed by most or all tests in the class. Otherwise, mock parameters with scope limited to a single test are preferred.
```



















