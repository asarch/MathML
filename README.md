# MathML
Steps to use MathML with DocBook

# Preamble

From

https://tdg.docbook.org/tdg/5.2/ch02.html

3.7.4. Mathematics

DocBook does not define a complete set of elements for representing equations. The Mathematical Markup Language (MathML) [MathML] is a standard that defines a comprehensive grammar for representing equations. MathML markup may be used in any of the equation elements (equation,informalequation, and inlineequation). For simple mathematics equations that do not require extensive markup, the mathphrase element is an alternative.

[MathML] David Carlisle, et al., ed. Mathematical Markup Language (MathML) Version 2.0. Second Edition, World Wide Web Consortium, 2003-10-21, http://www.w3.org/TR/MathML/.

# Installing and setting Xalan

This procedure is for OpenBSD

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

You can check the contents of the xalan-j package with:

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
# Installing and configuring FOP

Get the FOP formatter object from:

https://xmlgraphics.apache.org/fop/

and the jEuclide plugin from:

http://jeuclid.sourceforge.net/
