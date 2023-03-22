# Some useful maven commands

#### How to create effective-pom?

```
mvn help:effective-pom -Doutput=effective-pom.xml -Dverbose=true
```

#### How to skip test?

```
mvn clean install -Dmaven.test.skip=true
```

#### How to release without scm commit?

```
mvn release:prepare -DpushChanges=false -f pom.xml
```
