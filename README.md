# Example showing class-shadowing problem


## Background

This project contains a JAR, `libs/javac.jar`, on its compile classpath.

The JAR contains a POJO `Foo` with no methods and a default constructor.

This project then _shadows_ Foo by in `src/main/java/Foo.java` by adding a method `public void doIt()`.

The source of POGO `wut.Bar` references `Foo#doIt()`.

## Steps to reproduce

`$ ./gradlew clean jar`

## Expected behavior
Build succeeds. A jar is produced in `build/libs` which contains `wut.Bar`'s bytecode referencing `Foo#doIt()`

## Actual behavior 
Build fails - the method-less version of `Foo` from the classpath "wins".

```
> Task :compileGosu 
Dumping javac classpath for :compileGosu:
~/dev/tmp/javac/lib/javac.jar
~/.gradle/caches/modules-2/files-2.1/org.slf4j/slf4j-api/1.7.25/da76ca59f6a57ee3102f8f9bd9cee742973efa8a/slf4j-api-1.7.25.jar
~/.m2/repository/org/gosu-lang/gosu/gosu-core/1-manifold-SNAPSHOT/gosu-core-1-manifold-SNAPSHOT.jar
~/.m2/repository/org/gosu-lang/gosu/gosu-core-api/1-manifold-SNAPSHOT/gosu-core-api-1-manifold-SNAPSHOT.jar
~/.m2/repository/org/gosu-lang/gosu/managed/gw-asm-all/5.0.4/gw-asm-all-5.0.4.jar
~/.m2/repository/org/gosu-lang/gosu/managed/gw-jcommander/1.48/gw-jcommander-1.48.jar
~/.m2/repository/systems/manifold/manifold-json/0.13-gosu-SNAPSHOT/manifold-json-0.13-gosu-SNAPSHOT.jar
~/.m2/repository/systems/manifold/manifold-ext/0.13-gosu-SNAPSHOT/manifold-ext-0.13-gosu-SNAPSHOT.jar
~/.m2/repository/systems/manifold/manifold-js/0.13-gosu-SNAPSHOT/manifold-js-0.13-gosu-SNAPSHOT.jar
~/.m2/repository/systems/manifold/manifold/0.13-gosu-SNAPSHOT/manifold-0.13-gosu-SNAPSHOT.jar
~/.m2/repository/systems/manifold/manifold-bootstrap-plugin/0.13-gosu-SNAPSHOT/manifold-bootstrap-plugin-0.13-gosu-SNAPSHOT.jar
~/.m2/repository/systems/manifold/manifold-util/0.13-gosu-SNAPSHOT/manifold-util-0.13-gosu-SNAPSHOT.jar
~/dev/tmp/javac/build/classes/java/main
End javac classpath for :compileGosu:
[parsing started RegularFileObject[~/dev/tmp/javac/src/main/java/Foo.java]]
[parsing completed 14ms]
gosuc: about to compile file: ~/dev/tmp/javac/src/main/gosu/wut/Bar.gs
~/dev/tmp/javac/src/main/gosu/wut/Bar.gs:6: error: No function descriptor found for function, doIt, on class, Foo [line:6 col:5] in
    super.doIt()
    ^
  line 5:   construct() {
  line 6:     super.doIt()
  line 7:   }

```
