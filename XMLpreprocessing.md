## Introduction ##

Sometime users want to preprocess XML files and they are surprised that preprocessor just ignoring the files.

## Details ##

Because XML can be used as data sources during preprocessing, by default .xml extension is listed in excluded extension list for preprocessing and preprocessor doesn't preprocess xml files and even doesn't copy them.

## How to just copy XML ##

if preprocessor doesn't see an extension in both ignoring extension list and in preprocessing extension list then it just make copy of the file into the destination folder, so that we just need to exclude xml from the ignoring extension list, lets just define new list with extension 'none' and provide it through /EF: CLI key
**/EF:none**
in the case, the ignoring list has only 'none' extension and xml file will be copied.

## How to include XML in preprocessing ##
As the first one you should exclude xml from ignoring list as described above and then add the extension into preprocessing list with /F: CLI key
**/F:xml**
of course you should also add another file extensions to avoid their skipping, let say that you develop java project where you have .java and .xml files, then you should define **/F:java,xml**