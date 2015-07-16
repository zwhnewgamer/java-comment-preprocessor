<font color='red'>Keep in mind that development of the preprocessor started under Windows and in 2002 so that some approaches may look bizarre and non-modern at all</font>


## Preprocessor directives ##
The Preprocessor is two-pass one so that there are directives working only during the first pass and only during the second pass<br>
<ul><li><b>//#local</b>        make local definition which is visilble only in the current file context (2th pass)<br>
<pre><code> //#local VAR="hello"<br>
</code></pre>
</li><li><b>//#ifdefined</b>    check existence of variable in the current context, it starts //#ifdefined..//#else..//#endif control structure (2th pass)<br>
<pre><code> //#ifdefined VAR<br>
  the block printed if VAR is defined<br>
 //#else<br>
  the block printed if VAR is non-defined<br>
 //#endif<br>
</code></pre>
</li><li><b>//#ifndef</b>       works like //#ifdefined but active if variable is not defined (2th pass)<br>
<pre><code> //#ifndef VAR<br>
  the block printed if VAR is non-defined<br>
 //#else<br>
  the block printed if VAR is defined<br>
 //#endif<br>
</code></pre>
</li><li><b>//#ifdef</b>        the short variant of //#ifdefined (2th pass)<br>
<pre><code> //#ifdef VAR<br>
  the block printed if VAR is defined<br>
 //#else<br>
  the block printed if VAR is non-defined<br>
 //#endif<br>
</code></pre>
</li><li><b>//#if</b>           check flag and start //#if..//#else..//#endif construction (2th pass)<br>
<pre><code> //#if VAR=="hello"<br>
  the block printed if VAR equals to string "hello"<br>
 //#else<br>
  the block printed if VAR doesn't contain string "hello"<br>
 //#endif<br>
</code></pre>
</li><li><b>//#else</b>         invert condition result for //#if..//#endif control structure (2th pass)<br>
<pre><code> //#if VAR=="hello"<br>
  the block printed if VAR equals to string "hello"<br>
 //#else<br>
  the block printed if VAR doesn't contain string "hello"<br>
 //#endif<br>
</code></pre>
</li><li><b>//#endif</b>        end of //#if...//#endif control structure (2th pass)<br>
<pre><code> //#if VAR=="hello"<br>
  the block printed if VAR equals to string "hello"<br>
 //#else<br>
  the block printed if VAR doesn't contain string "hello"<br>
 //#endif<br>
</code></pre>
</li><li><b>//#while</b>        start //#while..//#end loop structure (2th pass)<br>
<pre><code> //#while VAR&lt;10<br>
  the block will be printed if VAR less than 10<br>
 //#end<br>
</code></pre>
</li><li><b>//#break</b>        break the current //#while...//#end loop (2th pass)<br>
<pre><code> //#while true<br>
  the block will be printed<br>
 //#break<br>
  the block will not be printed for break<br>
 //#end<br>
  the block will be printed just after break<br>
</code></pre>
</li><li><b>//#continue</b>     jump to current active //#while (2th pass)<br>
<pre><code> //#while VAR&lt;10<br>
  the block will be printed every loop iteration<br>
 //#continue<br>
  the block will not be printed at all because continue makes jump to start<br>
 //#end<br>
</code></pre>
</li><li><b>//#end</b>          end of //#while...//#end loop, do jump to the loop start (2th pass)<br>
<pre><code> //#while VAR&lt;10<br>
  the block will be printed if VAR less than 10<br>
 //#end<br>
</code></pre>
</li><li><b>//#exitif</b>       return to previous one in include stack if flag is true (2th pass)<br>
<pre><code> //#exitif true<br>
  the block will not be printed because exitif has true condition and preprocessing of the current file will be stopped<br>
</code></pre>
</li><li><b>//#exit</b>         return to previous one in include stack (2th pass)<br>
<pre><code> //#exit<br>
  the block will not be printed because preprocessing of the current file will be stopped<br>
</code></pre>
</li><li><b>//#outdir</b>       change result file folder (works like change 'jcp.dst.dir') (2th pass)<br>
<pre><code> //#outdir "/testfolder"<br>
  the output folder for the result file will be changed to /testfolder inside destination folder<br>
</code></pre>
</li><li><b>//#outname</b>      change result file name (works like change 'jcp.dst.name') (2th pass)<br>
<pre><code> //#outname "foo.txt"<br>
  the output name for the result file will be changed to foo.txt<br>
</code></pre>
</li><li><b>//#+</b>            turn on text output (2th pass)<br>
<pre><code> //#-<br>
  the block will be hidden in output<br>
 //#+<br>
  the block will be printed<br>
</code></pre>
</li><li><b>//#-</b>            turn off text output (2th pass)<br>
<pre><code> //#-<br>
  the block will be hidden in output<br>
 //#+<br>
  the block will be printed<br>
</code></pre>
</li><li><b>//#//</b>           comment the next line (just after the directive) (2th pass)<br>
<pre><code> //#//<br>
 the line will be commented in output<br>
 the line will not be commented<br>
</code></pre>
</li><li><b>//#definel</b>      define local(!) variable as true (by default) or initialize it by expression result (if presented) (2th pass)<br>
<pre><code> //#definel LOCALVAR<br>
 a local variable LOCALVAR with TRUE content is available here<br>
 //#definel LOCALVAR 10+23<br>
 a local variable LOCALVAR with number 33 as content is available here<br>
</code></pre>
</li><li><b>//#define</b>       define global(!) variable as true (by default) or initialize it by expression result (if presented) (2th pass)<br>
<pre><code> //#define GLOBALVAR<br>
 a global variable GLOBALVAR with TRUE content is available here<br>
 //#define GLOBALVAR 10+23<br>
 a global variable GLOBALVAR with number 33 as content is available here<br>
</code></pre>
</li><li><b>//#undef</b>        undefine either local or global variable if it is defined (2th pass)<br>
<pre><code> //#define GLOBALVAR<br>
 the GLOBALVAR variable is presented here<br>
 //#undef GLOBALVAR<br>
 there is no more any GLOBALVAR variable in context<br>
</code></pre>
</li><li><b>//#flush</b>        flush text buffers to disk and clear the buffers (2th pass)<br>
<pre><code> //#flush<br>
 all inside buffered preprocessing text data has been saved as a file and all buffers are clear<br>
</code></pre>
</li><li><b>//#include</b>      include file and preprocess in the current file context (2th pass)<br>
<pre><code> //#definel index 1<br>
 //#include "somefile"+index+".txt"<br>
 the line above will be replaced by preprocessed text from file somefile1.txt <br>
</code></pre>
</li><li><b>//#action</b>       call preprocessor user extension, arguments are comma separated (2th pass)<br>
<pre><code> //#action 1,2,3<br>
 the line above will send action notification to user preprocessor extension with arguments 1,2,3<br>
</code></pre>
</li><li><b>//#postfix</b>      turn on(+) or turn off(-) writing into result file postfix (2th pass)<br>
<pre><code> //#postfix+<br>
 the text will be printed into the bottom section of result text file<br>
 //#postfix-<br>
 the text will be printed in the normal section of result text file, over of the bottom section<br>
</code></pre>
</li><li><b>//#prefix</b>       turn on(+) or turn off(-) writing into result file prefix (2th pass)<br>
<pre><code> //#prefix+<br>
 the text will be printed into the top section of result text file<br>
 //#prefix-<br>
 the text will be printed in the normal section of result text file, under of the top section<br>
</code></pre>
</li><li><b>//#global</b>       define global variable (1st pass)<br>
<pre><code> //#global VAR=10+33<br>
 the VAR will be defined during the first (!) pass and will be visible in all files on start of the second pass <br>
</code></pre>
</li><li><b>//#<code>_</code>if</b>          start //#<code>_</code>if..//#<code>_</code>endif control construction (1st pass)<br>
<pre><code> //#global FLAG=true<br>
 //#_if FLAG<br>
  //#global SOMEVALUE=10<br>
 //#else<br>
  //#global SOMEVALUE="test"<br>
 //#_endif<br>
 SOMEVALUE will be defined during the first pass(!)<br>
</code></pre>
</li><li><b>//#<code>_</code>else</b>        invert condition result for //#<code>_</code>if..//#<code>_</code>endif control construction (1st pass)<br>
<pre><code> //#global FLAG=true<br>
 //#_if FLAG<br>
  //#global SOMEVALUE=10<br>
 //#else<br>
  //#global SOMEVALUE="test"<br>
 //#_endif<br>
 SOMEVALUE will be defined during the first pass(!)<br>
</code></pre>
</li><li><b>//#<code>_</code>endif</b>       end //#<code>_</code>if..//#<code>_</code>endif control construction (1st pass)<br>
<pre><code> //#global FLAG=true<br>
 //#_if FLAG<br>
  //#global SOMEVALUE=10<br>
 //#else<br>
  //#global SOMEVALUE="test"<br>
 //#_endif<br>
 SOMEVALUE will be defined during the first pass(!)<br>
</code></pre>
</li><li><b>//#excludeif</b>    exclude file from preprocessing if flag is true (1st pass)<br>
<pre><code> //#_if FLAG<br>
  //#excludeif SOMEVALUE=="test"<br>
 //#_endif<br>
 the current file will be excluded from the second pass and output if FLAG is true and SOMEVALUE equals "test"<br>
</code></pre>
</li><li><b>//#error</b>        throw fatal preprocessor exception with message and stop work (2th pass)<br>
<pre><code> //#error "Some terrible error in "+VALUE<br>
 the expression result will be printed as message and stack-trace will be shown, preprocessing will be canceled  <br>
</code></pre>
</li><li><b>//#warning</b>      log warning message (2th pass)<br>
<pre><code> //#warning "Warning! variable VAR="+VAR<br>
 the expression result will be shown as message with stack trace but preprocessing will not be stopped<br>
</code></pre>
</li><li><b>//#echo</b>    (former //#assert)     macroses will be replaced in the text tail and the result will be out as info (2th pass)<br>
<pre><code> //#echo Value of VAR is /*$VAR$*/<br>
 the macroses will be replaced and the text tail will be printed as info message<br>
</code></pre>
</li><li><b>//#msg</b>          string tail macroses will be replaced and message will be printed as info (2th pass)<br>
<pre><code> //#msg "Hello"+" world"<br>
 the result of the expression will be printed as info<br>
</code></pre>
</li><li><b>//#noautoflush</b>  disable autoflush for text buffers in the end of file processing (2th pass)<br>
<pre><code> //#noautoflush<br>
 saving of result text buffers after file preprocessing completion has been disabled, the result will not be saved automatically<br>
</code></pre>
</li><li><b>//#abort</b>        abort preprocessing and show the line tail as message (allows macroses) (2th pass)<br>
<pre><code> //#abort Abort the mission because we have terrible state of /*$CPU$*/<br>
 preprocessing will be stopped with message but it will not be recognized as error situation<br>
</code></pre></li></ul>

<h3>Special directives for string manipulations</h3>
<ul><li><b>/<code>*</code>$...$<code>*</code>/</b>  macros which will be replaced in preprocessing text by the result of excpression processing<br>
<pre><code> //#local VALUE="Hello World"<br>
 System.out.println("/*$VALUE$*/");// result will be System.out.println("Hello World");<br>
</code></pre>
</li><li><b>//$</b>           replace macroses in following text and out result<br>
<pre><code> //#local VAR="hello"<br>
 //$ var contains /*$VAR$*/<br>
 the output file fill have only " var contains hello"<br>
</code></pre>
</li><li><b>//$$</b>          works like //$ but without macros replacement<br>
<pre><code> //#local VAR="hello"<br>
 //$$ var contains /*$VAR$*/<br>
 the output file fill have only " var contains /*$VAR$*/"<br>
</code></pre>
</li><li><b>/<code>*</code>-<code>*</code>/</b>         discard the following text tail<br>
<pre><code> we need the part of text/*-*/and do not need the part<br>
 only "we need the part of text" will be printed in result<br>
</code></pre>
<h3>Operators</h3>
</li><li><b>==</b>            equal to<br>
</li><li><b>></b>             greater than<br>
</li><li><b>>=</b>            greater than or equal to<br>
</li><li><b><</b>             less than<br>
</li><li><b><=</b>            less than or equal to<br>
</li><li><b>!=</b>            not equal to<br>
</li><li><b>+</b>             additive operator (also used for string concatenation)<br>
</li><li><b>-</b>             subtraction operator<br>
</li><li><b><code>*</code></b>             multiplication operator<br>
</li><li><b>/</b>             division operator<br>
</li><li><b>%</b>             remainder operator<br>
</li><li><b>!</b>             logical complement operator and unary bitwise complement<br>
</li><li><b>&&</b>            conditional-AND and bitwise-AND<br>
</li><li><b>||</b>            conditional-OR and bitwise inclusive OR<br>
</li><li><b><code>^</code></b>             conditional-XOR and bitwise exclusive OR<br>
<h3>Functions</h3>
</li><li><b>abs</b>                     get absolute value of numeric value  <a href='ANY.md'>abs (INT) | ANY abs (FLOAT)</a><br>
</li><li><b>round</b>                   round float value to nearest integer  <a href='INT.md'>round (FLOAT) | INT round (INT)</a><br>
</li><li><b>str2int</b>                 convert string into integet number  <a href='INT.md'>str2int (STR)</a><br>
</li><li><b>str2web</b>                 escape string for HTML3  <a href='STR.md'>str2web (STR)</a><br>
</li><li><b>str2csv</b>                 escape string for Comma Separated Values  <a href='STR.md'>str2csv (STR)</a><br>
</li><li><b>str2js</b>                  escape string for EcmaScript/JavaScript  <a href='STR.md'>str2js (STR)</a><br>
</li><li><b>str2json</b>                escape string for JSON  <a href='STR.md'>str2json (STR)</a><br>
</li><li><b>str2xml</b>                 escape string for XML 1.0  <a href='STR.md'>str2xml (STR)</a><br>
</li><li><b>str2java</b>                escapes a string to be compatible with java  <a href='STR.md'>str2java (STR,BOOL)</a><br>
</li><li><b>strlen</b>                  get string length  <a href='INT.md'>strlen (STR)</a><br>
</li><li><b>issubstr</b>                check that substring presented in string (case insensitive)  <a href='BOOL.md'>issubstr (STR,STR)</a><br>
</li><li><b>evalfile</b>                load and preprocess file and return text result as string  <a href='STR.md'>evalfile (STR)</a><br>
</li><li><b>xml<code>_</code>get</b>                 get element from element list by its index (first 0)  <a href='STR.md'>xml`_`get (STR,INT)</a><br>
</li><li><b>xml<code>_</code>size</b>                get element list size  <a href='INT.md'>xml`_`size (STR)</a><br>
</li><li><b>xml<code>_</code>attr</b>                get named attribute from element, nonexisting attribute returns empty string  <a href='STR.md'>xml`_`attr (STR,STR)</a><br>
</li><li><b>xml<code>_</code>root</b>                get the document root element  <a href='STR.md'>xml`_`root (STR)</a><br>
</li><li><b>xml<code>_</code>name</b>                get element name (tag)  <a href='STR.md'>xml`_`name (STR)</a><br>
</li><li><b>xml<code>_</code>list</b>                get element list by element tag  <a href='STR.md'>xml`_`list (STR,STR)</a><br>
</li><li><b>xml<code>_</code>text</b>                get element text content, text of children also included  <a href='STR.md'>xml`_`text (STR)</a><br>
</li><li><b>xml<code>_</code>open</b>                open XML file and parse as DOM  <a href='STR.md'>xml`_`open (STR)</a><br>
</li><li><b>xml<code>_</code>xlist</b>               get element list for XPath  <a href='STR.md'>xml`_`xlist (STR,STR)</a><br>
</li><li><b>xml<code>_</code>xelement</b>            get element for XPath  <a href='STR.md'>xml`_`xelement (STR,STR)</a><br></li></ul>

<h3>Data types</h3>
<ul><li><b>BOOLEAN</b>: true,false<br>
</li><li><b>INTEGER</b>: 2374,0x56FE (signed 64 bit)<br>
</li><li><b>STRING</b> : "Hello World!" (or $Hello World!$ for the command string)<br>
</li><li><b>FLOAT</b>  : 0.745 (signed 32 bit)<br></li></ul>

<h3>Special predefined variables</h3>
<ul><li><b>jcp.version</b>             The Preprocessor version<br>
</li><li><b>jcp.src.path</b>            Full path to the current preprocessing file, read only<br>
</li><li><b><code>__</code>file<code>__</code></b>                The Synonym for 'jcp.dst.path', read only<br>
</li><li><b>jcp.src.dir</b>             The Current preprocessing file folder, read only<br>
</li><li><b><code>__</code>filefolder<code>__</code></b>          The Synonym for 'jcp.src.dir', read only<br>
</li><li><b>jcp.src.name</b>            The Current preprocessing file name, read only<br>
</li><li><b><code>__</code>filename<code>__</code></b>            The Synonym for 'jcp.src.name', read only<br>
</li><li><b><code>__</code>line<code>__</code></b>                The Current preprocessing line number in the current source file, read only<br>
</li><li><b>jcp.dst.path</b>            The Full Destination File path for the preprocessing file, read only<br>
</li><li><b>jcp.dst.dir</b>             The Destination File path for the preprocessing file, read only<br>
</li><li><b>jcp.dst.name</b>            The Destination File name for the preprocessing file, allowed for reading and writing<br>
</li><li><b><code>__</code>time<code>__</code></b>                The Current time<br>
</li><li><b><code>__</code>date<code>__</code></b>                The Current date<br>
</li><li><b><code>__</code>timestamp<code>__</code></b>           The Timestamp of the current source file<br>