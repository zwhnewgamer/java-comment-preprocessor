# Introduction #

[Javassist](http://www.javassist.org) is a great Java framework, it allows to change bytecode on fly and make dynamic compilation of sources written in Java like language. it works great and in use in many great tools like [JRebel](http://zeroturnaround.com/software/jrebel/) and [XRebel](http://zeroturnaround.com/software/xrebel/). Lets take a look at some example of [Javassist usage](http://theholyjava.wordpress.com/2011/09/07/practical-introduction-into-code-injection-with-aspectj-javassist-and-java-proxy/)
```
public void insertTimingIntoMethod(String targetClass, String targetMethod) throws NotFoundException, CannotCompileException, IOException {
      Logger logger = Logger.getLogger("Javassist");
      final String targetFolder = "./target/javassist";
 
      try {
         final ClassPool pool = ClassPool.getDefault();
         pool.appendClassPath(new LoaderClassPath(getClass().getClassLoader()));
         final CtClass compiledClass = pool.get(targetClass);
         final CtMethod method = compiledClass.getDeclaredMethod(targetMethod);
         method.addLocalVariable("startMs", CtClass.longType);
         method.insertBefore("startMs = System.currentTimeMillis();");
         method.insertAfter("{final long endMs = System.currentTimeMillis();" +
            "iterate.jz2011.codeinjection.javassist.PerformanceMonitor.logPerformance(\"" +
            targetMethod + "\",(endMs-startMs));}");
 
         compiledClass.writeFile(targetFolder);
         logger.info(targetClass + "." + targetMethod +
               " has been modified and saved under " + targetFolder);
      } catch (NotFoundException e) {
         logger.warning("Failed to find the target class to modify, " +
               targetClass + ", verify that it ClassPool has been configured to look " +
               "into the right location");
      }
   }
```
and it is very easy case of javassist usage but as you can see it is very easy to make error or typo in such "string-programming" and imagine that you need write 500-1000 lines of such "string" code, it can make you crazy especially if take in count that the error will be detected mainly in run-time.

# How JCP can help #

Since 6.0 version in JCP has been added pair new functions which allows to decrease complexity of "string" programming for Javassist, those functions are evalfile() and str2java().
  * **str evalfile(str)** allows to preprocess some external file defined by string name and return the preprocessing result as a string
  * **str str2java(str,bool)** allows to escape a string into java compatible format and also to cut the string to well formed lines.

## Problem ##
Let's imagine that we also have a need to insert a prefix into a class method to catch execution and return another data and the method looks like below
```
public static Class<?> prepare() throws Exception {
      final ClassPool cp = ClassPool.getDefault();
      cp.appendClassPath(new LoaderClassPath(Main.class.getClassLoader()));
      final CtClass ctClazz = cp.get(Main.class.getPackage().getName()+".ExtProcessor");
      final CtMethod method1 = ctClazz.getDeclaredMethod("extractExtension");
      method1.insertBefore(
      +"if (!$2) {return $1;} else { final int index = $1.lastIndexOf('.');"
      +"String result;"
      +"if (index < 0) {result = \"\";} else { result = $1.substring(index + 1);}"
      +"return result;}"
      );
      ctClazz.writeFile();
      return ctClazz.toClass();
  }
```
looks good but it is no so easy to understand logic and editing can make pain and typos, so let's take a look how JCP can help us even in the easy case.
## Form method body as external class ##
Let's create one more class in the same package and name it _**extractExtensionMethod.java** and place inside the file all we need
```
//#excludeif true
//#-
public class _extractExtensionMethod {
  public String extractExtension(final String ____arg1, final boolean ____arg2) {
    final String fileName = ____arg1;
    final boolean allowDynamicCode = ____arg2;
//#+
    //$String fileName=$1;
    //$boolean allowDynamicCode=$2;
    if (!allowDynamicCode) {
      return /*$"$1;"$*//*-*/ "";
    }
    else {
      final int index = fileName.lastIndexOf('.');
      final String result;
      if (index < 0) {
        result = "";
      }
      else {
        result = fileName.substring(index + 1);
      }
      return result;
    }
//#-
  }
}
//+
```
looks a bit strange of course if you don't have experience in work with JCP but there is very important profit, as minimum 80% of your code in the class can be refactored and controlled by IDE and chance to get typo much less.
The Class has active **//#excludeif** so that it will not be included into result of preprocessing and different not-needed parts of the class included in **//#-..//#+** and they will be cutted from preprocessing result. The Trick with **//-//**allows to get after preprocessing $1 as the result._

## Adapt our main class to use the external one ##
We need just to remove all our "string-programming" code and replace it by result of preprocessing for the class _extractExtensionMethod.java, true as the second parameter for str2java() shows that the result will be not only escaped but also formed as a concatenation of strings prepared to be used as argument in Java sources
```
public static Class<?> prepare() throws Exception {
      final ClassPool cp = ClassPool.getDefault();
      cp.appendClassPath(new LoaderClassPath(Main.class.getClassLoader()));
      final CtClass ctClazz = cp.get(Main.class.getPackage().getName()+".ExtProcessor");
      final CtMethod method1 = ctClazz.getDeclaredMethod("extractExtension");
      method1.insertBefore(
/*$str2java(evalfile("_extractExtensionMethod.java"),true)$*//*-*/""
      );
      ctClazz.writeFile();
      return ctClazz.toClass();
  }
```
now looks much better_

## Result ##
Let start preprocessing of the example and check the result. The Generated file will have such prepare() method
```
  public static Class<?> prepare() throws Exception {
      final ClassPool cp = ClassPool.getDefault();
      cp.appendClassPath(new LoaderClassPath(Main.class.getClassLoader()));
      final CtClass ctClazz = cp.get(Main.class.getPackage().getName()+".ExtProcessor");
      final CtMethod method1 = ctClazz.getDeclaredMethod("extractExtension");
      method1.insertBefore(
"    String fileName=$1;\n"
+"    boolean allowDynamicCode=$2;\n"
+"    if (!allowDynamicCode) {\n"
+"      return $1;\n"
+"    }\n"
+"    else {\n"
+"      final int index = fileName.lastIndexOf('.');\n"
+"      final String result;\n"
+"      if (index < 0) {\n"
+"        result = \"\";\n"
+"      }\n"
+"      else {\n"
+"        result = fileName.substring(index + 1);\n"
+"      }\n"
+"      return result;\n"
+"    }\n"
      );
      ctClazz.writeFile();
      return ctClazz.toClass();
  }
```
looks much better and less manual work, reading and editing will make much less pain and dramatically decreased chance of typos.
## Sources of the example ##
The Demo project sources can be downloaded from [google drive](https://googledrive.com/host/0BxHnNp97IgMRckZhaG5tT0RTSGM/JCPJavassistDemo.zip). It is a maven project and after unzipping it can be started with maven command
```
 mvn clean install exec:java
```