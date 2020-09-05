# Use Maven to Arrange Java Project

## Install and Setup Maven

1. Install the JDK(Use JDK 1.8 here) and setup the **$JAVA_HOME** variable.
   In Mac OSX 10.5 or later, set the **$JAVA_HOME** to `/var/libexec/java_home` and export the **$JAVA_HOME** in `~/.bash_profile` file.

   ```bash
   # Add JAVA_HOME as env variable
   $ vim .bash_profile
   export JAVA_HOME=$(/usr/libexec/java_home)
   
   # Confirm the JAVA_HOME variable
   $ source .bash_profile
   $ echo $JAVA_HOME
   /Library/Java/JavaVirtualMachines/jdk1.8.0_191.jdk/Contents/Home
   ```

2. Download the maven from https://maven.apache.org/download.cgi and extract the distribution archive in any directory.

   ```bash
   $ unzip apache-maven-3.6.3-bin.zip
   ```

   The directory show as below example after the extraction:

   ```bash
   drwxr-xr-x   9 demo  staff    288 12 21  2019 .
   drwxr-xr-x   5 root   wheel    160  3 30 00:18 ..
   -rw-r--r--@  1 demo  staff  17504 11  7  2019 LICENSE
   -rw-r--r--@  1 demo  staff   5141 11  7  2019 NOTICE
   -rw-r--r--@  1 demo  staff   2612 11  7  2019 README.txt
   drwxr-xr-x   8 demo  staff    256 12 21  2019 bin
   drwxr-xr-x@  4 demo  staff    128 11  7  2019 boot
   drwxr-xr-x@  5 demo  staff    160 11  7  2019 conf
   drwxr-xr-x@ 65 demo  staff   2080 11  7  2019 lib
   ```

3. Add bin directory of the `apache-maven-3.6.3` directory to the `PATH` environment variable.

   ```bash
   # Add MAVEN_HOME as env variable and append the bin directory to PATH variable.
   $ vim .bash_profile
   export MAVEN_HOME=/opt/apache-maven-3.6.3
   export PATH=$MAVEN_HOME/bin:$PATH
   
   # Confirm the MAVEN_HOME variable.
   $ source .bash_profile
   $ echo $MAVEN_HOME
   /opt/apache-maven-3.6.3
   
   # Confirm the maven version and further information(for example in MACOS).
   $ mvn -version
   Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
   Maven home: /opt/apache-maven-3.6.3
   Java version: 1.8.0_191, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk1.8.0_191.jdk/Contents/Home/jre
   Default locale: zh_TW, platform encoding: UTF-8
   OS name: "mac os x", version: "10.15.6", arch: "x86_64", family: "mac"
   ```

   

## Maven Repository

A repository in Maven  build artifacts and dependencies of varying types such as **local** and **remote**.

### Local Repository

Local repository is a directory on the local computer running Maven. It caches the remote downloads and dependencies. The default location varies by platforms, Linux & MacOS as  `~/.m2` and Windows  as `C:\Users\{username}\.m2`.  The path of the <localRepository> tag in the `{MAVEN_HOME}\conf\settings.xml` is the directory that cashes the downloads and dependencies to build the artifacts.

```bash
  <!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ${user.home}/.m2/repository
  <localRepository>/path/to/local/repo</localRepository>
  -->
```

### Remote Repository

The remote repository provides the replacement instead of local one in case of inability to find the downloads. The default remote repository is https://repo.maven.apache.org/maven2/. The 3rd party also built their own remote repository to provide the artifact for downloading and make a change to add a <repository> tag as below:

```xml
<repositories>
	<repository>
    <id>java.net</id>
    <url>https://maven.java.net/content/repositories/public</url>
  </repository>
  <repository>
  	<id>another.repository</id>
    <url>https://maven.another.repository/public</url>
  </repository>
</repositories>
```

## Run Maven Command to Create Project

Maven supports to create the different projects as below:

```bash
$ mvn archetype:generate -DgroupId=package根目錄 -DartifactId=專案名稱 -DarchetypeArtifactId=tempalte名稱 -DinteractiveMode=false
```

### Create Java SE Project by Template

Run the below command to create the `MyMavenSETest` project. Maven may take a while on the first run. It downloads the latest artifacts to the lcoal repository. 

```bash
$ mvn archetype:generate -DgroupId=com.demo -DartifactId=MyMavenSETest -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

After the download is complete, the project structure lists as below:

```bash
MyMavenSETest
|-- pom.xml
`-- src
    |-- main
    |   `-- java
    |       `-- com
    |           `-- demo
    |               `-- App.java
    `-- test
        `-- java
            `-- com
                `-- demo
                    `-- AppTest.java
```

The `pom.xml` is the core configuration file of the maven project. It contains the majority of information to build the project.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.demo</groupId>
  <artifactId>MyMavenSETest</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>MyMavenSETest</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

#### Build the Project

Run the command to build the project. The command will print the various actions.

```bash
$ cd MyMavenSETest & mvn package
```

The command execution is complate and end with the below:

```
[INFO] Building jar: /Users/demo/MyMavenSETest/target/MyMavenSETest-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  01:05 min
[INFO] Finished at: 2020-09-06T03:34:27+08:00
[INFO] ------------------------------------------------------------------------
```

#### Show the Result

Run the below command to print out the message:

```bash
$ java -cp target/MyMavenSETest-1.0-SNAPSHOT.jar com.demo.App
```

The console shows as below message:

```bash
Hello world!
```

### Create Java Web Project by Template

Run the command to create the `MyMavenWebTest` web project.

```bash
$ mvn archetype:generate -DgroupId=com.demo -DartifactId=MyMavenWebTest -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false
```

After the download is complete, the project structure lists as below:

```bash
MyMavenSETest
|-- pom.xml
`-- src
    `-- main
        `-- resources
        `-- webapp
            `-- WEB-INF
                `-- web.xml
            `-- index.jsp

```

The `pom.xml` is the core configuration file of the maven project. It contains the majority of information to build the project.

```bash
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.goden</groupId>
  <artifactId>MyMavenWebTest</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>MyMavenWebTest Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
  <build>
    <finalName>MyMavenWebTest</finalName>
  </build>
</project>
```