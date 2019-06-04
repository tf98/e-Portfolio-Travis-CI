# Tutorial:
### 1. Add .travis.yml 
Add a file called .travis.yml to project source. Since we are using java you have to specify this:
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
You can then see on your Travis Dashboard that a build is carried out. 

### 3. Alter a method to fail a test (and after the build failed revert it)
### 4. Add a badge to your readme
Go to your project on Travis and click on the badge icon at the top and copy the code from there in to your readme in your git repository.

### 5. Add Coveralls
To automatically trigger a coveralls build on pull requests you have to set up coveralls in your project. Since we are using Maven you need to add the Coveralls MAven Plugin to your pom.xml:
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
To get your own token go to Coveralls.io select your repository and copy yor token from there.
We have to then tell Travis that it should run the tests with a Maven command and report the results to Coveralls. We do that we by adding the following to our .travis.yml:
```
after_success:
  - mvn clean test jacoco:report coveralls:report
```

On Coveralls go to your repository then to settings and the option "COVERAGE THRESHOLD FOR FAILURE" to a high value (like 90%). This will prevent pull requests from beeing merged if ther code coverage is below the specified value.

### 7. Add a new Method without tests on a new branch
### 8. See your build fail
### 9. Add a test to pass the build
