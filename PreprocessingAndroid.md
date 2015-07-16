# Introduction #

Usually Java doesn't need any preprocessing (especially Java EE) and even it is a bad practice to add such things into projects but since 2000th we have also mobile Java and Java for different embedded devices, TV and mobile phones. The Areas have zoo of devices and sometime differences between devices of different vendors are very small and you need minimal changes in your code to support them, for such cases the preprocessor is an ideal tool. As I wrote, the preprocessor was developed to adapt bunch of Java2ME projects for different devices. Today we have very popular Android platfor where the preprocessor can be useful, how to use the preprocessor together the Android SDK, you can read below.

# How to use JCP for Android development #
As you know, Android SDK build process is based on [the ANT tool](http://ant.apache.org/) so that we need make some manipulations inside the project ANT script to include the preprocessor in the build process.
## Download the Preprocessor JAR ##
You should dowload the latest preprocessor JAR version, usually I place them in [the Google driver folder](https://d7c3446b46fe99cb8536c21d42a822f85d2812dc.googledrive.com/host/0BxHnNp97IgMRVHFzLUdMYjRJczQ/). Download a preprocessor jar file and create new **jcplib** folder in your android file, then place the downloaded jar into the folder.
## Add the JCP ANT script ##
To preprocess files we need special script, I have prepared special ANT script [jcpandroid.xml](https://52fe6e2da99ac0e464b584ef2ffdccb5808dce44.googledrive.com/host/0BxHnNp97IgMRQzlYcEZ6TlRRUjg/jcpandroid.xml), download that and place in the root project folder, where the project build.xml situated.
```
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Author: Igor Maznitsa (http://www.igormaznitsa.com)
  Preprocessor page is https://code.google.com/p/java-comment-preprocessor/
  The ANT script makes preprocessing of SRC,ASSETS and RES folders during -pre-build phase and
  delete them during -post-build phase. You have to replace the standard folders in the ant.properties
  for instance as below:
    source.dir=.prep_src
    asset.dir=.prep_assets
    resource.dir=.prep_res
-->
<project name="jcp_android">
  <!-- The Standard source folder for Android ANT projects-->
  <property name="std.src.folder" value="src"/>
  <!-- The Standard asset folder for Android ANT projects-->
  <property name="std.asset.folder" value="assets"/>
  <!-- The Standard resource folder for Android ANT projects-->
  <property name="std.res.folder" value="res"/>

  <!-- We need path to the preprocessor jar-->
  <taskdef resource="com/igormaznitsa/jcp/ant/tasks.properties" classpath="jcplib/jcp-6.0.0.jar"/>

  <target name="-pre-build" description="Preprocessing files">
    <fail message="[Preprocessor] DETECTED NON-REPLACED STANDARD PROJECT FOLDERS! IT'S POSSIBLE THAT YOU HAVE NOT REDEFINED THEM!">
      <condition>
        <or>
          <equals arg1="${source.dir}" arg2="${std.src.folder}"/>
          <equals arg1="${asset.dir}" arg2="${std.asset.folder}"/>
          <equals arg1="${resource.dir}" arg2="${std.res.folder}"/>
        </or>
      </condition>
    </fail>
    <!-- Preprocess all main folders in places visible for compiler -->
    <echo>[Preprocessor] Preprocessing sources from ${std.src.folder} to ${source.dir}</echo>
    <preprocess clear="true" source="${std.src.folder}"  destination="${source.dir}" excluded="" processing="java,xml"/>
    <echo>[Preprocessor] Preprocessing assets from ${std.asset.folder} to ${asset.dir}</echo>
    <preprocess clear="true" source="${std.asset.folder}"  destination="${asset.dir}" excluded="" processing="java,xml"/>
    <echo>[Preprocessor] Preprocessing resources from ${std.res.folder} to ${resource.dir}</echo>
    <preprocess clear="true" source="${std.res.folder}"  destination="${resource.dir}" excluded="" processing="java,xml"/>
  </target>

  <target name="-post-build" description="Clean preprocessing folders">
    <fail message="[Preprocessor] DETECTED STANDARD FOLDERS TO DELETE! IT'S POSSIBLE THAT YOU HAVE NOT REDEFINED THEM!">
    <condition>
        <or>
          <equals arg1="${source.dir}" arg2="${std.src.folder}"/>
          <equals arg1="${asset.dir}" arg2="${std.asset.folder}"/>
          <equals arg1="${resource.dir}" arg2="${std.res.folder}"/>
        </or>
    </condition>
    </fail>
    <echo>[Preprocessor] Delete preprocessor folders</echo>
    <delete dir="${source.dir}" failonerror="true"/>
    <delete dir="${resource.dir}" failonerror="true"/>
    <delete dir="${asset.dir}" failonerror="true"/>
  </target>
</project>
```
## Corrections in build.xml ##
Open your build.xml and find the line **`<import file="custom_rules.xml" optional="true" />`**, then add just under found line the string **`<import file="jcpandroid.xml" optional="false"/>`**. Now your android build process will call preprocessor every ant build phase.
## Tune folders in ant.properties ##
By default, Android SDK uses **src**,**assets** and **res** folders as the sources but we need replace them by preprocessed ones to make Android SDK working with our preprocessed data. Open **ant.properties** and add below properties
```
source.dir=.prep_src
asset.dir=.prep_assets
resource.dir=.prep_res
```
now Android SDK uses the defined folders as the sources for the project and we will place our preprocessed stuff into them.
## Check build ##
Start build process with **ant debug** and check that you see **`[Preprocessor]`** inside the build log.
## Usage with Idea and NetBeans ##
I have checked the approach with Intellij IDEA and NetBeans, all works well, but NBAndroid plugin for NetBeans works with it automatically and IDEA looks like using its own building for Android and you should use **Ant Build** panel to start tasks manually.
# P.S. #
If you want to define some global properties to be used in your preprocessed projects then it is very easy, just add such properties into the ant.properties file
```
hello_world=Hello World!
```
and preprocessed code in ANT to show the property will be
```
   System.out.println("/*$ant.hello_world$*/");
```
just don't forget about **ant** prefix for ant properties which are visible under preprocessing, if a property already has **ant** prefix (like ant.some) then in preprocessing it will have name **ant.ant.some**.