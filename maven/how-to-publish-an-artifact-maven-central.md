# How to publish an artifact maven central

> Prerequisite
>> - GitHub account
>> - Maven project

## 1) Create a project and request access to maven central.

Most importantly, you need an account, a project, and permission to publish to OSSRH.
You will need to create an account on Sonatype JIRA and then request to create your project via a new JIRA ticket.
You can also clone, edit and submit the Jira ticket I created - [OSSRH-83824](https://issues.sonatype.org/browse/OSSRH-83824).

## 2) GPG setup and Signing Artifacts

First thing first, you need to install a gpg tool to sing your artifacts,
you can download this tool from [gnupg.org](https://gnupg.org/download/index.html)
or use my [offline copy](./binaries/gpg4win-4.0.3.exe)

Once installation is complete, open your terminal and follow below commands

### 2.1) Verify you gpg installation by checking gpg version:

💡 while generating key, you need to provide a password, keep it secret and remember it.

```
gpg --version
```

```
C:\Users>gpg --version
gpg (GnuPG) 2.2.28
libgcrypt 1.8.8
Copyright (C) 2021 g10 Code GmbH
License GNU GPL-3.0-or-later <https://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Home: C:/Users/<your-name>/AppData/Roaming/gnupg
Supported algorithms:
Pubkey: RSA, ELG, DSA, ECDH, ECDSA, EDDSA
Cipher: IDEA, 3DES, CAST5, BLOWFISH, AES, AES192, AES256, TWOFISH,
        CAMELLIA128, CAMELLIA192, CAMELLIA256
Hash: SHA1, RIPEMD160, SHA256, SHA384, SHA512, SHA224
Compression: Uncompressed, ZIP, ZLIB, BZIP2
```

### 2.2) Generate a key by using below command:

```
gpg --full-gen-key
```

```
C:\Users>gpg --full-gen-key
gpg (GnuPG) 2.2.28; Copyright (C) 2021 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
  (14) Existing key from card
Your selection? 1
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: John Doe
Email address: john.doe@email.com
Comment:
You selected this USER-ID:
    "John Doe <john.doe@email.com>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: key CE7E2E853D3C5327 marked as ultimately trusted
gpg: revocation certificate stored as 'C:/Users/yourname/AppData/Roaming/gnupg/openpgp-revocs.d\7W1S1S7SDSSS73S4DDFD4D5DCD7D2D8D3DDC5D2D.rev'
public and secret key created and signed.

pub   rsa4096 2022-09-07 [SC]
      7W1S1S7SDSSS73S4DDFD4D5DCD7D2D8D3DDC5D2D
uid                      John Doe <john.doe@email.com>
sub   rsa4096 2022-09-07 [E]
```

### 2.3) Publish key

Once the keys are created, you need to sync the public keys with popular gpg key servers. You can synchronize the keys by retrieving the public key and then sending it to the
key-servers.

```
gpg --list-keys
```

```

C:\Users>gpg --list-keys
gpg: checking the trustdb
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: depth: 0  valid:   3  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 3u
C:/Users/yourname/AppData/Roaming/gnupg/pubring.kbx
-------------------------------------------------
pub   rsa4096 2022-09-07 [SC]
      7W1S1S7SDSSS73S4DDFD4D5DCD7D2D8D3DDC5D2D
uid           [ultimate] John Doe <john.doe@email.com>
sub   rsa4096 2022-09-07 [E]
```

```
C:\Users>gpg --keyserver https://pgp.mit.edu --send-keys 7W1S1S7SDSSS73S4DDFD4D5DCD7D2D8D3DDC5D2D
gpg: sending key CS7E2SS53S3CSS2E to https://pgp.mit.edu
```

**Some popular key servers**  
[https://pgp.key-server.io/](https://pgp.key-server.io/)  
[https://keyserver.ubuntu.com/](https://keyserver.ubuntu.com/)  
[https://pgp.mit.edu/](https://pgp.mit.edu/)  
[http://keys.gnupg.net/](http://keys.gnupg.net/)

You can read more detailed instructions on [the sonatype page on pgp signatures](https://central.sonatype.org/publish/requirements/gpg/).

### 2.3) Export private key

👉 We to add private key to GitHub to work with github-actions, if you're not planning to use gh-action you can skip this step.

```
gpg --export-secret-keys -a 7W1S1S7SDSSS73S4DDFD4D5DCD7D2D8D3DDC5D2D > file-name.txt
```

## 3 Changes in pom.xml

### 3.1) You need to add a few plugins as mention below:

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
                    only use for GitHub action, 
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
<!-- https://mvnrepository.com/artifact/org.sonatype.plugins/nexus-staging-maven-plugin -->
<plugin>
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

### 3.2) You need to some metadata
You can read more here [sonatype requirements](https://central.sonatype.org/publish/requirements/)

```
<project>
   ...
   
    <name>Example Application</name>
    <description>A application used as an example on how to set up pushing its components to the Central Repository.</description>
    <url>http://www.example.com/example-application</url>

    
    <distributionManagement>
        <snapshotRepository>
            <id>ossrh-jaxer-in</id>
            <url>https://s01.oss.sonatype.org/content/repositories/snapshots</url>
        </snapshotRepository>
        <repository>
            <id>ossrh-jaxer-in</id>
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

## 4) Deployment
💡 Before pushing it just make sure your build is generating without fail. 
```
mvn clean package
```

### 4.1) Manual deploy
When maven-gpg-plugin will start its execution, it will ask you password, (one we provided when creating key)
```
mvn clean package deploy -Dmaven.test.skip=true -P<profile> 
```

if nothing goes wrong your build will be created and pushed to maven central  
it may take 8-10 hrs to go live. 
