# MathML
Steps to use MathML with DocBook

## Preamble

From

https://tdg.docbook.org/tdg/5.2/ch02.html

3.7.4. Mathematics
 
DocBook does not define a complete set of elements for representing equations. The Mathematical Markup Language (MathML) [MathML] is a standard that defines a comprehensive grammar for representing equations. MathML markup may be used in any of the equation elements (equation,informalequation, and inlineequation). For simple mathematics equations that do not require extensive markup, the mathphrase element is an alternative.
 
[MathML] David Carlisle, et al., ed. Mathematical Markup Language (MathML) Version 2.0. Second Edition, World Wide Web Consortium, 2003-10-21, http://www.w3.org/TR/MathML/.

## Installing and setting Xalan

This procedure is for OpenBSD.

Install the Xalan package:

```Bash
# pkg_add -iv xalan-j
```

Configure Java:

```
$ vi ~/.profile
# Java
export JAVA_HOME=/usr/local/jdk-1.8.0

export PATH=$JAVA_HOME/bin:$PATH

export CLASSPATH=$CLASSPATH:/usr/local/share/java/classes/xalan-j/serializer.jar:/usr/local/share/java/classes/xalan-j/xalan.jar:/usr/local/share/java/classes/xalan-j/xercesImpl.jar:/usr/local/share/java/classes/xalan-j/xml-apis.jar:~/lib/docbook/extensions/xalan27.jar:~/bin/xslthl-2.1.3/xslthl-2.1.3.jar
```

You can check the contents of the **xalan-j** package with:

```Bash
$ pkg_info -L xalan-j
```
Check if the installation is correct:

```Bash
$ java org.apache.xalan.xslt.EnvironmentCheck
#---- BEGIN writeEnvironmentReport($Revision: 468646 $): Useful stuff found: ----
version.DOM.draftlevel=2.0fd
java.class.path=:/usr/local/share/java/classes/xalan-j/serializer.jar:/usr/local/share/java/classes/xalan-j/xalan.jar:/usr/local/share/java/classes/xalan-j/xercesImpl.jar:/usr/local/share/java/classes/xalan-j/xml-apis.jar:/home/asarch/lib/docbook/extensions/xalan27.jar:/home/asarch/bin/xslthl-2.1.3/xslthl-2.1.3.jar:/usr/local/share/java/classes/xalan-j/serializer.jar:/usr/local/share/java/classes/xalan-j/xalan.jar:/usr/local/share/java/classes/xalan-j/xercesImpl.jar:/usr/local/share/java/classes/xalan-j/xml-apis.jar:/home/asarch/lib/docbook/extensions/xalan27.jar:/home/asarch/bin/xslthl-2.1.3/xslthl-2.1.3.jar
version.JAXP=1.1 or higher
java.ext.dirs=/usr/local/jdk-1.8.0/jre/lib/ext:/usr/java/packages/lib/ext
version.xerces2=Xerces-J 2.11.0
version.xerces1=not-present
version.xalan2_2=Xalan Java 2.7.2
version.xalan1=not-present
version.ant=not-present
java.version=1.8.0_202
version.DOM=2.0
version.crimson=not-present
sun.boot.class.path=/usr/local/jdk-1.8.0/jre/lib/resources.jar:/usr/local/jdk-1.8.0/jre/lib/rt.jar:/usr/local/jdk-1.8.0/jre/lib/sunrsasign.jar:/usr/local/jdk-1.8.0/jre/lib/jsse.jar:/usr/local/jdk-1.8.0/jre/lib/jce.jar:/usr/local/jdk-1.8.0/jre/lib/charsets.jar:/usr/local/jdk-1.8.0/jre/lib/jfr.jar:/usr/local/jdk-1.8.0/jre/classes
#---- BEGIN Listing XML-related jars in: foundclasses.java.class.path ----
serializer.jar-apparent.version=serializer.jar present-unknown-version
serializer.jar-path=/usr/local/share/java/classes/xalan-j/serializer.jar
xalan.jar-path=/usr/local/share/java/classes/xalan-j/xalan.jar
xercesImpl.jar-apparent.version=xercesImpl.jar WARNING.present-unknown-version
xercesImpl.jar-path=/usr/local/share/java/classes/xalan-j/xercesImpl.jar
xml-apis.jar-apparent.version=xml-apis.jar present-unknown-version
xml-apis.jar-path=/usr/local/share/java/classes/xalan-j/xml-apis.jar
serializer.jar-apparent.version=serializer.jar present-unknown-version
serializer.jar-path=/usr/local/share/java/classes/xalan-j/serializer.jar
xalan.jar-path=/usr/local/share/java/classes/xalan-j/xalan.jar
xercesImpl.jar-apparent.version=xercesImpl.jar WARNING.present-unknown-version
xercesImpl.jar-path=/usr/local/share/java/classes/xalan-j/xercesImpl.jar
xml-apis.jar-apparent.version=xml-apis.jar present-unknown-version
xml-apis.jar-path=/usr/local/share/java/classes/xalan-j/xml-apis.jar
#----- END Listing XML-related jars in: foundclasses.java.class.path -----
version.SAX=2.0
version.xalan2x=Xalan Java 2.7.2
#----- END writeEnvironmentReport: Useful properties found: -----
# YAHOO! Your environment seems to be OK.
```

## Installing and configuring FOP

Get the FOP formatter object from:

https://xmlgraphics.apache.org/fop/

and the jEuclid plugin from:

http://jeuclid.sourceforge.net/

Uncompress the package to:

```Bash
$ tar vxzf fop-2.3-bin.tar.gz -C ~/bin
```

Uncompress the package of the jEuclid plugin and copy the jar's files to the fop's **/lib** dir:

```
$ unzip jeuclid-3.1.9-distribution.zip
Archive:  ../jeuclid-3.1.9-distribution.zip
   creating: jeuclid-3.1.9/
   creating: jeuclid-3.1.9/bin/
  inflating: jeuclid-3.1.9/bin/mml2xxx
  inflating: jeuclid-3.1.9/bin/foprep
  inflating: jeuclid-3.1.9/bin/mathviewer
  inflating: jeuclid-3.1.9/bin/listfonts
   creating: jeuclid-3.1.9/repo/             
  inflating: jeuclid-3.1.9/repo/jeuclid-core-3.1.9.jar
  inflating: jeuclid-3.1.9/repo/commons-logging-1.1.1.jar
  ...

$ cp jeuclid-3.1.9/repo/* ~/bin/fop-2.3/fop/lib/
```
Enable the plugin:

```Bash
$ vi ~/.foprc
# From http://malatsblog.blogspot.com/2010/01/generating-pdf-from-docbook-on-linux.html
find_jars jeuclid-core jeuclid-fop
```

## Transformations

There are two ways to do it:

Using Xalan:

```Bash
$ java org.apache.xalan.xslt.Process -in equations.xml -out equations.fo -xsl ~/lib/docbook/fo/docbook.xsl -param use.extensions 1
```

or with xsltproc:

```Bash
$ xsltproc ~/lib/docbook/fo/docbook.xsl equations.xml > equations.fo
```
## Getting the PDF file

And now render the file into PDF:

```Bash
$ ~/bin/fop-2.3/fop/fop -fo equations.fo -pdf equations.pdf
```

## Resources

For more information

- http://www.w3.org/TR/MathML/
- https://developer.mozilla.org/en-US/docs/Web/MathML
- https://developer.mozilla.org/en-US/docs/Mozilla/MathML_Project

# Extra

Enabling syntax hightlighting in DocBook document

From:

```Bash
$ cat ~/lib/docbook/highlighting/README                                                                                           
To use the syntax higlighting extension with DocBook-XSL 1.74.3+, you must:
1. Use a processor that works with the extension: Saxon 6 or Xalan-J.
2. Add the latest version of xslthl-2.X.X.jar to your classpath.
3. Set the highlight.source parameter to 1.
4. Import into your customization one of the following stylesheet module:
  * html/highlight.xsl
  * xhtml/highlight.xsl
  * xhtml-1_1/highlight.xsl
  * fo/highlight.xsl
5. Use that customiztion layer.

Note: Saxon 8.5 or later is also supported, but since it is an XSLT 2.0
processor it is not guaranteed to work with DocBook-XSL in all
circumstances. 
```

Get the jar file from:

https://sourceforge.net/projects/xslthl/

and uncompress it:

```Bash
$ cd ~/bin

$ unzip ~/Downloads/xslthl-2.1.3-dist.zip
```

add its path to your **CLASSPATH**:

```Bash
$ export CLASSPATH=$CLASSPATH:~/bin/xslthl-2.1.3/xslthl-2.1.3.jar
```
