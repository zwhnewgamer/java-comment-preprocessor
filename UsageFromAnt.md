# Introduction #

The Apache ANT is very widely used today thus I supporting it in the preprocessor through adding a special Ant task.

# Ant Task #

The common example of the preprocessor usage from the Apache Ant you can see below
```
<?xml version="1.0" encoding="UTF-8"?>
<project name="test" default="preprocess">
  
<taskdef resource="com/igormaznitsa/jcp/ant/tasks.properties"/>
<target name="preprocess">
  <preprocess 
	    source="${basedir}/Source" 
	    destination="${basedir}/Result" 
	    verbose="true">
      <global name="hello" value="world"/>
  </preprocess>
  <echo>Preprocessing complete!</echo>
</target>
</project>
```
As you can see it is very easy.
The ant task descriptors are placed in the **com/igormaznitsa/jcp/ant** folder and there are both the task.properties and the antlib.xml.
## Ant task parameters ##
### source ###
The task attribute allows to set the preprocessing source roots, you can use several semi separated roots. The default value is "./"
```
source="./src1;./src2"
```
### destination ###
The task attribute allows to set the destination directory where the result will be placed. The default value is "./preprocessed"
```
destination="./preprocessed"
```
### inCharset ###
The task attribute allows to set the input text character encoding, the default value is **UTF8**
```
inCharset="UTF-16"
```
### outCharset ###
The task attribute allows to set the result text character encoding, the default value is **UTF8**
```
outCharset="UTF-32"
```
### excluded ###
The task attribute keeps the file extensions excluded from preprocessing. The default value is **xml**
```
excluded="xml,bin,jpg"
```
### processing ###
The task attribute keeps the file extensions which should be preprocessed. The default value is **java,txt,htm,html**
```
processing="cpp,py,java,pl"
```
### clear ###
The task attribute allows to set the clear destination directory flag, if it is true then the destination directory will be entirely cleaned before preprocessing start. The default value is **true**
```
clear="false"
```
### removeComments ###
The task attribute allows to make the preprocessor to remove all Java-C compatible comments from the result files, the default value is **false**
```
removeComments="true"
```
### verbose ###
It make the preprocessor more informative in its messages, the default value is **false**
```
verbose="true"
```
### disableOut ###
It disables all writing operations for the preprocessor and mainly is being used for test purposes. The default value is **false**
```
disableOut="true"
```
### keepLines ###
To keep directives and non-executing lines of code as commented ones to preserve the line numeration
```
keepLines="true"
```
### cfgFile ###
**It is an inside task tag** (!) and you can use it to define a preprocessor configuration file to be loaded and interpeted by the preprocessor before start.
```
<cfgFile file="./cfgfile.cfg"/>
```
### global ###
**It is an inside task tag** (!), it allows to define a global variable.
```
<global name="var1" value="test"/>
```
# ANT properties access #
You can access to any ANT property during preprocessing as to a global variable with the **"ant."** prefix. Remember please that all those properties are accessible only for reading and any writing attempt will throw an exception. **In some cases the prefix will be duplicated (!).**
```
//#local base_dir = ant.basedir
//#local project_name = ant.ant.project.name
```