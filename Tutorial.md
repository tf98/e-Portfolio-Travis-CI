# Tutorial:
### 1. Add .travis.yml 
Add a file called ```.travis.yml``` to the project root. 
In this file you specify all the information about your project Travis needs like your programming language and its version.
You can also specify other scripts and commands that should be run during the build process (more on that later).

Since we are using java you have to specify this:
```
language: java
```
To test against different JDK versions we can also add all the version we want to test:
```
language: java
jdk:
  - oraclejdk8
  - oraclejdk9
```

### 2. Push to your repository
After you have commited and pushed the ```.travis.yml``` file to your repository go to your Travis dashboard. There you can see that your build is beeing carried out.
![Build running](https://github.com/tf98/e-Portfolio-Travis-CI/blob/master/images/build_running.PNG)
After the build is finished you should see that your build has passed.
![Build successful](https://github.com/tf98/e-Portfolio-Travis-CI/blob/master/images/build_success.PNG)

### 3. Alter a method to fail a test (and after the build failed revert it)
To see what happens when your build fails alter a method to fail the tests. For example in the Calculator.multiply() method subtract 1 from the result so its always off by 1.
```java 
public int multiply(int a, int b) {
return (a*b)-1;
}
```
If you push and commit you should see your build failing.
![Build failed](https://github.com/tf98/e-Portfolio-Travis-CI/blob/master/images/build_failed.PNG)

### 4. Add a badge to your readme
You can show your build status with a badge in your readme so that people see that your using a CI tool. Go to your project on Travis-ci.org and click on the badge icon at the top and copy the code from there in to your readme in your git repository.

### 5. Add Coveralls
To automatically trigger a coveralls build on pull requests you have to set up coveralls in your project. Since we are using Maven you need to add the Coveralls Maven Plugin to your pom.xml. You add it in your build section under plugins.
```
<plugin>
        <groupId>org.eluder.coveralls</groupId>
        <artifactId>coveralls-maven-plugin</artifactId>
        <version>4.3.0</version>
        <configuration>
            <!-- Replace with your token -->
            <repoToken>yourcoverallstoken</repoToken>
        </configuration>
</plugin>
```
To get your own token go to Coveralls.io select your repository and copy your token from there.
![Get Coveralls Token](https://github.com/tf98/e-Portfolio-Travis-CI/blob/master/images/coveralls_token.PNG)
We have to then tell Travis that it should run the tests with a Maven command and report the results to Coveralls. We do that we by adding the following to our ```.travis.yml:```. "after-success" tells Travis to run these comments after the build passed. The command lets maven run the tests and then submit the results to Coveralls.
```
after_success:
  - mvn clean test jacoco:report coveralls:report
```

On Coveralls.io go to your repository then to settings and set the option "COVERAGE THRESHOLD FOR FAILURE" to a high value (like 90%). This will prevent pull requests from beeing merged if their code coverage is below the specified value.

### 7. Add a new Method without tests on a new branch
We will now add the functionality to subtract to our Calculator. Create a new branch, name it "subtract" and add a subtract method to the Calculator class.
```java
public void int subtract(int a, int b) {
  return a-b;
}
```
Do not add a test for this method yet!

Commit and push to the new branch. 

### 8. See your build fail
Create a pull request for the subtract branch to merge it into the master branch. On Github you can see that Travis tries to build your changes. The code coverage will be below the specified value so the checks will fail and you won't be able to merge the pull request.

### 9. Add a test to pass the build
Add a test method to the CalculatorTest class.
```java
@Test
public void subtractZeroShouldNotChangeResult() {
Calculator tester = new Calculator();
  assertEquals(10,tester.subtract(10,0));
```
Add this commit to your pull request. Travis will rerun all the checks. This time also the code coverage is high enoug to pass the test.
If all checks are finished you can merge your pull request.
