# How to publish an artifact maven central

> Prerequisite
> - GitHub account
> - Maven project

## 1) Create a project and request access to maven central.

Most importantly, you need an account, a project, and permission to publish to OSSRH.
You will need to create an account on Sonatype JIRA and then request to create your project via a new JIRA ticket.
You can also clone, edit and submit the Jira ticket I created - [OSSRH-83824](https://issues.sonatype.org/browse/OSSRH-83824).

## 2) GPG setup and Signing Artifacts

Checkout this [article](../gpg/index.md)

## 3 Changes in pom.xml

### 3.1) Plugins

You need to add a few plugins as mention below:

```
<!--maven-gpg-plugin-->
<plugin>
    <!-- https://mvnrepository.com/artifact/org.apache.maven.plugins/maven-gpg-plugin -->
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-gpg-plugin</artifactId>
    <version>1.6</version>
    <executions>
        <execution>
            <id>sign-artifacts</id>
            <phase>verify</phase>
            <goals>
                <goal>sign</goal>
            </goals>
            <configuration>
                <!-- 
                    Prevent `gpg` from using pinentry programs,
                    for non interactive mode such as CICD ie. GitHub action, 
                    remove <gpgArguments> tag for manual deploy
                 -->
                <gpgArguments>
                    <arg>--pinentry-mode</arg>
                    <arg>loopback</arg>
                </gpgArguments>
            </configuration>
        </execution>
    </executions>
</plugin>
```

```
<!--maven-source-plugin-->
<plugin>
    <!-- https://mvnrepository.com/artifact/org.apache.maven.plugins/maven-source-plugin -->
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-source-plugin</artifactId>
    <version>3.2.0</version>
    <executions>
        <execution>
            <id>attach-sources</id>
            <goals>
                <goal>jar</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

```
<!--maven-javadoc-plugin-->
<plugin>
    <!-- https://mvnrepository.com/artifact/org.apache.maven.plugins/maven-javadoc-plugin -->
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-javadoc-plugin</artifactId>
    <version>3.2.0</version>
    <executions>
        <execution>
            <id>attach-javadocs</id>
            <goals>
                <goal>jar</goal>
            </goals>
            <configuration>
                <!-- add this to disable checking -->
                <doclint>none</doclint>
            </configuration>
        </execution>
    </executions>
</plugin>
```

```
<!-- nexus-staging-maven-plugin -->
<plugin>
    <!-- https://mvnrepository.com/artifact/org.sonatype.plugins/nexus-staging-maven-plugin -->
    <groupId>org.sonatype.plugins</groupId>
    <artifactId>nexus-staging-maven-plugin</artifactId>
    <version>1.6.7</version>
    <extensions>true</extensions>
    <configuration>
        <serverId>ossrh</serverId>
        <!-- below url will be provided by sonaty in you JIRA issue -->
        <nexusUrl>s01.oss.sonatype.org</nexusUrl>
        <autoReleaseAfterClose>true</autoReleaseAfterClose>
    </configuration>
</plugin>
```

### 3.2) Metadata

You need to some metadata. You can read more here [sonatype requirements](https://central.sonatype.org/publish/requirements/)

```
<project>
   ...
   
    <name>Example Application</name>
    <description>A application used as an example on how to set up pushing its components to the Central Repository.</description>
    <url>http://www.example.com/example-application</url>

    
    <distributionManagement>
        <snapshotRepository>
            <id>ossrh</id>
            <url>https://s01.oss.sonatype.org/content/repositories/snapshots</url>
        </snapshotRepository>
        <repository>
            <id>ossrh</id>
            <url>https://s01.oss.sonatype.org/service/local/staging/deploy/maven2/</url>
        </repository>
    </distributionManagement>
        
    <scm>
        <connection>scm:git:git://github.com:username/repository.git</connection>
        <developerConnection>scm:git:ssh://github.com:username/repository.git</developerConnection>
        <url>https://github.com/username/repository</url>
        <tag>v${project.version}</tag>
    </scm>
    
    <licenses>
        <license>
            <name>Apache 2.0</name>
            <url>https://www.apache.org/licenses/LICENSE-2.0</url>
        </license>
    </licenses>
    
    <developers>
        <developer>
            <id>dev-id</id>
            <name>John Doe</name>
            <email>john.doe@email.com</email>
            <roles>
                <role>Project Lead</role>
            </roles>
            <organization>Open Source</organization>
        </developer>
    </developers>

    ...
</project>
```

### 3.3) Maven settings

Now we need to add a server in settings.xml file

> 💡 file will be available in ${maven-directory}/conf/settings.xml

```
...
    <servers>
        ...
        <server>
            <id>ossrh</id>
            <username>JIRA-USERNAME</username>
            <password>JIRA-PASSWORD</password>
        </server>
        ...
    </servers>
...
```

## 4) Deployment

> 💡 Before pushing it just make sure your build is generating without fail.

```
mvn clean package
```

### 4.1) Manual deploy

When maven-gpg-plugin will start its execution, it will ask you password, (one we provided when creating key)

```
mvn clean package deploy -Dmaven.test.skip=true -P<profile> 
```

After you successfully release, your component will be available to the public on [Central](https://repo1.maven.org/maven2/), typically within 30 minutes, though updates
to [maven-search](https://search.maven.org) can take up to four hours.

### 4.2) Auto deploy via GitHub action
