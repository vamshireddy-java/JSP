# JSP


JSP directives are the elements of a JSP source code that guide the web container on how to translate the JSP page into it’s respective servlet. 
Syntax :
 @

<%@ directive attribute = "value"%>
Directives can have a number of attributes which you can list down as key-value pairs and separated by commas. The blanks between the @ symbol and the directive name, and between the last attribute and the closing %>, are optional. 
Different types of JSP directives : 
There are three different JSP directives available. They are as follows: 

Page Directives :
-----------------
JSP page directive is used to define the properties applying the JSP page, such as the size of the allocated buffer, imported packages and classes/interfaces, defining what type of page it is etc. The syntax of JSP page directive is as follows: 
 
<%@page attribute = "value"%>
Different properties/attributes : 
The following are the different properties that can be defined using page directive :


import:
-------
This tells the container what packages/classes are needed to be imported into the program.
Syntax:
<%@page import = "value"%>
Example : 
<%-- JSP code to demonstrate how to use page
 directive to import a package --%>
 
<%@page import = "java.util.Date"%>
<%Date d = new Date();%>
<%=d%>
Output: 


contentType:
-------------
This defines the format of data that is being exchanged between the client and the server. It does the same thing as the setContentType method in servlet used to.
Syntax:
<%@page contentType="value"%>
Usage Example: 

<%-- JSP code to demonstrate how to use
page directive to set the type of content --%>
 
<%@page contentType = "text/html" %>
<% = "This is sparta" %>
Output : 


info: Defines the string which can be printed using the ‘getServletInfo()’ method.
Syntax:
<%@page info="value"%>
Usage Example: 

<%-- JSP code to demonstrate how to use page
 directive to set the page information --%>
 
<%@page contentType = "text/html" %>
<% = getServletInfo()%>
Output: 


buffer: 
-------
Defines the size of the buffer that is allocated for handling the JSP page. The size is defined in Kilo Bytes.
Syntax: 
<%@page buffer = "size in kb"%>
language: Defines the scripting language used in the page. By default, this attribute contains the value ‘java’.
isELIgnored: This attribute tells if the page supports expression language. By default, it is set to false. If set to true, it will disable expression language.
Syntax:
<%@page isElIgnored = "true/false"%>
Usage Example: 

<%-- JSP code to demonstrate how to use page
directive to ignore expression language --%>
 
<%@page contentType = "text/html" %>
<%@page isELIgnored = "true"%>
<body bgcolor = "blue">
<c:out value = "${'This is sparta}"/>
</body>
Output: 
(blank page) 


errorPage: 
----------
Defines which page to redirect to, in case the current page encounters an exception. 
Syntax:
<%@page errorPage = "true/false"%>
Usage Example: 

//JSP code to divide two numbers
<%@ page errorPage = "error.jsp" %> 
 
<%  
// dividing the numbers
int z = 1/0; 
 
 // result
out.print("division of numbers is: " + z);  
%> 
isErrorPage:
------------
It classifies whether the page is an error page or not. By classifying a page as an error page, it can use the implicit object ‘exception’ which can be used to display exceptions that have occurred.
Syntax:
<%@page isErrorPage="true/false"%>
Usage example: 

//JSP code for error page, which displays the exception
<%@ page isErrorPage = "true" %> 
    
<h1>Exception caught</h1> 
    
The exception is: <% = exception %>
Output:
Output: 


Include directive : 
-------------------
JSP include directive is used to include other files into the current jsp page. These files can be html files, other sp files etc. The advantage of using an include directive is that it allows code re-usability.
The syntax of an include directive is as follows: 
 
<@%include file = "file location"%>
Usage example: 
In the following code, we’re including the contents of an html file into a jsp page.
a.html 

<h1>This is the content of a.html</h1>
index.jsp 

<% = Local content%>
<%@include file = "a.html"%>
<% = local content%>
Output : 


Taglib Directive :
-----------------

The taglib directive is used to mention the library whose custom-defined tags are being used in the JSP page. It’s major application is JSTL(JSP standard tag library). 
Syntax: 
<@%taglib uri = "library url" prefix="the prefix to 
identify the tags of this library with"%>
Usage Example: 

<%-- JSP code to demonstrate
taglib directive--%>
<%@ taglib uri = "http://java.sun.com/jsp/jstl/core"
prefix = "c" %> 
   
<c:out value = "${'This is Sparta'}"/>
In the above code, we’ve used to taglib directive to point to the JSTL library which is a set of some custom-defined tags in JSP that can be used in place of the scirptlet tag (<%..%>). The prefix attribute is used to define the prefix that is used to identify the tags of this library. Here, the prefix c is used in the <c:out> tag to tell the container that this tag belongs to the library mentioned above.

---------------------------------------------------------------------------------------------------------------------------------------------------------------
                                                      Custom Tags in JSP
                                                      --------------------
In this example, we are going to create a custom tag that prints the current date and time. We are performing action at the start of tag.

For creating any custom tag, we need to follow following steps:

-> Create the Tag handler class and perform action at the start or at the end of the tag.
-> Create the Tag Library Descriptor (TLD) file and define tags
-> Create the JSP file that uses the Custom tag defined in the TLD file     

Understanding flow of custom tag in jsp:
----------------------------------------
![customtagflow](https://user-images.githubusercontent.com/112934893/188632876-8c6d9a48-8271-498e-b189-89d6ef21c623.jpg)


1) Create the Tag handler class

To create the Tag Handler, we are inheriting the TagSupport class and overriding its method doStartTag().To write data for the jsp, we need to use the JspWriter class.

The PageContext class provides getOut() method that returns the instance of JspWriter class. TagSupport class provides instance of pageContext bydefault.


package com.javatpoint.sonoo;  
import java.util.Calendar;  
import javax.servlet.jsp.JspException;  
import javax.servlet.jsp.JspWriter;  
import javax.servlet.jsp.tagext.TagSupport;  
public class MyTagHandler extends TagSupport{  
  
public int doStartTag() throws JspException {  
    JspWriter out=pageContext.getOut();//returns the instance of JspWriter  
    try{  
     out.print(Calendar.getInstance().getTime());//printing date and time using JspWriter  
    }catch(Exception e){System.out.println(e);}  
    return SKIP_BODY;//will not evaluate the body content of the tag  
}  
}  

2) Create the TLD file

Tag Library Descriptor (TLD) file contains information of tag and Tag Hander classes. It must be contained inside the WEB-INF directory.

File: mytags.tld
<?xml version="1.0" encoding="ISO-8859-1" ?>  
<!DOCTYPE taglib  
        PUBLIC "-//Sun Microsystems, Inc.//DTD JSP Tag Library 1.2//EN"  
    "http://java.sun.com/j2ee/dtd/web-jsptaglibrary_1_2.dtd">  
  
<taglib>  
  
  <tlib-version>1.0</tlib-version>  
  <jsp-version>1.2</jsp-version>  
  <short-name>simple</short-name>  
  <uri>http://tomcat.apache.org/example-taglib</uri>  
  
<tag>  
<name>today</name>  
<tag-class>com.javatpoint.sonoo.MyTagHandler</tag-class>  
</tag>  
</taglib>  



3) Create the JSP file

Let's use the tag in our jsp file. Here, we are specifying the path of tld file directly. But it is recommended to use the uri name instead of full path of tld file. We will learn about uri later.

It uses taglib directive to use the tags defined in the tld file.

File: index.jsp
<%@ taglib uri="WEB-INF/mytags.tld" prefix="m" %>  
Current Date and Time is: <m:today/>  

![customtagoutput](https://user-images.githubusercontent.com/112934893/188634095-014bd702-e269-48a8-90b1-c5f106efd39d.jpg)



Attributes in JSP Custom Tag:  https://www.javatpoint.com/attributes-in-jsp-custom-tag
-----------------------------

There can be defined too many attributes for any custom tag. To define the attribute, you need to perform two tasks:

1) Define the property in the TagHandler class with the attribute name and define the setter method
2) define the attribute element inside the tag element in the TLD file

Let's understand the attribute by the tag given below:
-----------------------------------------------------

<m:cube number="4"></m:cube>  
Here m is the prefix, cube is the tag name and number is the attribute.

Simple example of attribute in JSP Custom Tag
---------------------------------------

In this example, we are going to use the cube tag which return the cube of any given number. Here, we are defining the number attribute for the cube tag. We are using the three file here:


-> index.jsp
-> CubeNumber.java
-> mytags.tld

index.jsp
---------
<%@ taglib uri="WEB-INF/mytags.tld" prefix="m" %>  
Cube of 4 is: <m:cube number="4"></m:cube>  

CubeNumber.java
-------------------
package com.javatpoint.taghandler;  
import javax.servlet.jsp.JspException;  
import javax.servlet.jsp.JspWriter;  
import javax.servlet.jsp.tagext.TagSupport;  
  
public class CubeNumber extends TagSupport{  
private int number;  
      
public void setNumber(int number) {  
    this.number = number;  
}  
  
public int doStartTag() throws JspException {  
    JspWriter out=pageContext.getOut();  
    try{  
        out.print(number*number*number);  
    }catch(Exception e){e.printStackTrace();}  
      
    return SKIP_BODY;  
}  
}  


mytags.tld
---------

 <tag>  
    <name>cube</name>  
    <tag-class>com.javatpoint.taghandler.CubeNumber</tag-class>  
    <attribute>  
    <name>number</name>  
    <required>true</required>  
    </attribute>  
  </tag>  
  
  





