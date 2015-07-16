# Introduction #
Nowadays Maven has become the main build tool for Java developers thus the JCP preprocessor has an embedded maven plugin to support the maven development cycle.

# Default phase and goal #
The default phase for the preprocessor is "generate-sources" and the goal is "preprocess".

# Configuration parameters #
## `<source>` ##
The parameter allows to set the source directory where the preprocessor will be looking for texts to preprocess, there can be several source roots separated by the ';' char. If the parameter is not set then the preprocessor will be used inside maven project compilation roots.
```
<source>./,./src,/src2</source>
```

## `<destination>` ##
The parameter allows to set the destinaton directory to place the preprocessing results. The default value for the parameter is **${project.build.directory}/generated-sources/preprocessed**
Example:
```
<destination>${project.build.directory}/src</destination>
```

## `<inEncoding>` ##
The parameter allows to set the preprocessing text encoding character set, the default value will be taken from **${project.build.sourceEncoding}**
Example:
```
<inEncoding>UTF-32</inEncoding>
```

## `<outEncoding>` ##
The parameter should be used if the resuld character encoding should be differ from the source encoding, the default value for the parameter is set as **${project.build.sourceEncoding}**
Example:
```
<outEncoding>UTF-16</outEncoding>
```

## `<excluded>` ##
A Comma separated list of file extensions to be excluded from preprocessing process. The default value is **xml**
Example:
```
<excluded>xml,bin,jpg</excluded>
```

## `<processing>` ##
A Comma separated list of file extensions to be preprocessed. The default value is **java,txt,htm,html**
Example:
```
<processing>cpp,c,java</processing>
```

## `<disableOut>` ##
It is a boolean flag to tell the preprocessor that it must not make any writing operations, the default value is **false** and mainly it is used for testing purposes.
Example:
```
<disableOut>true</disableOut>
```

## `<verbose>` ##
It is a boolean flag to make the preprocessor out a bit more information about its inside work and processes. The default value is **false**
Example:
```
<verbose>true</verbose>
```

## `<keepLines>` ##
To keep directives and non-executing lines of code as commented ones to preserve line numeration
```
<keepLines>true</keepLines>
```

## `<clear>` ##
It is a boolean flag shows that the preprocessor must entirely clean the destination directory before preprocessing. The default value is **true**
Example:
```
<clear>false</clear>
```

## `<keepSrcRoot>` ##
When the preprocessing has completed its work it will remove the maven project source roots from the inside maven project (not physically) and replace them by its destination folder. The flag allows to disabled this act. The default value is **false**
Example:
```
<keepSrcRoot>true</keepSrcRoot>
```

## `<removeComments>` ##
It allows to remove all Java-C compatible comments from the result files after their preprocessing (I mean the result files not the sources of course). The default value is **false**
Example:
```
<removeComments>true</removeComments>
```

## `<globalVars>` ##
The tag describes the section where you can place global variable definitions which will be accessible from preprocessing sources. Each variable should be described as **property**
```
<globalVars>
  <property>
    <name>hello_var</name>
    <value>test string</value>
  </property>
  <property>
    <name>some_number</name>
    <value>22</value>
  </property>
</globalVars>
```

## `<cfgFiles>` ##
The tag describes the section where you can set some configuration files which will be loaded by preprocessor, you can place global variables and some directives in those files.
Example:
```
<cfgFiles>
   <file>${basedir}/cfgfile.cfg</file>
   <file>${basedir}/cfgfile2.cfg</file>
</cfgFiles>
```

## `<useTestSources>` ##
By default JCP preprocesses sourc folders of project but sometime needed preprocessing of test source folders, the **useTestSources** flag allows to turn on the mode of test folder preprocessing.
```
<useTestSources>true</useTestSources>
```

# Maven property access #
The preprocessor allows to get access to some maven properties and use them as global variables. All those properties has the "mvn." prefix. You must keep in mind that all those properties are accessible only for reading and represented as String values, they can't be changed, any attempt to write will throw an exception.
The list of allowed properties below
  * mvn.project.name
  * mvn,project.version
  * mvn.project.url
  * mvn,project.packaging
  * mvn.project.modelversion
  * mvn,project.inceptionyear
  * mvn.project.id
  * mvn.project.groupId
  * mvn.project.description

  * mvn.project.artifact.id
  * mvn.project.artifact.artifactId
  * mvn.project.artifact.baseVersion
  * mvn.project.artifact.dependencyConflictId
  * mvn.project.artifact.downloadUrl
  * mvn.project.artifact.groupId
  * mvn.project.artifact.scope
  * mvn.project.artifact.type
  * mvn.project.artifact.version

  * mvn.project.build.directory
  * mvn.project.build.defaultGoal
  * mvn.project.build.outputDirectory
  * mvn.project.build.scriptSourceDirectory
  * mvn.project.build.sourceDirectory
  * mvn.project.build.testOutputDirectory
  * mvn.project.build.testSourceDirectory

  * mvn.project.organization.name
  * mvn.project.organization.url

# Example of usage #
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.igormaznitsa</groupId>
    <artifactId>DummyMavenProjectToTestJCP</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>Dummy Maven Project To Test JCP</name>
    <description>Dummy Maven Project</description>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <prerequisites>
        <maven>3.0</maven>
    </prerequisites>
    <organization>
        <name>Igor Maznitsa</name>
        <url>http://www.igormaznitsa.com</url>
    </organization>

    <build>
        <plugins>
            <plugin>
                <groupId>com.igormaznitsa</groupId>
                <artifactId>jcp</artifactId>
                <version>6.0.0</version>
                <executions>
                    <execution>
                        <phase>generate-sources</phase>
                        <goals>
                            <goal>preprocess</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <source>/</source>
                    <destination>destination_dir</destination>
                    <excluded>xml,html</excluded>
                    <processing>java,txt</processing>
                    <inEncoding>UTF-16</inEncoding>
                    <outEncoding>UTF-32</outEncoding>
                    <keepSrcRoot>true</keepSrcRoot>
                    <removeComments>true</removeComments>
                    <disableOut>true</disableOut>
                    <verbose>true</verbose>
                    <clear>true</clear>
                    <cfgFiles>
                        <file>test1.cfg</file>
                        <file>test2.cfg</file>
                    </cfgFiles>    
                    <globalVars>
                        <property>
                            <name>globalvar1</name>
                            <value>3</value>
                        </property>    
                        <property>
                            <name>globalvar2</name>
                            <value>hello world</value>
                        </property>    
                    </globalVars>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>2.3.2</version>
                <configuration>
                    <source>1.6</source>
                    <target>1.6</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```
# Use for Test phase #
By defaut JCP processes source folders but sometime it is good to preprocess and test source folders, how to do that you can read [here](HowToPreprocessTestSourcesInMaven.md)