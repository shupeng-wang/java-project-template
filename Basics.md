# Contents #
  1. Downloading the template.
  1. Template organization.
  1. Building the template.
  1. Editing the template.
  1. Customizing the template.
  1. Implementing the commandline main.
  1. Implementing the interactive main.
  1. Implementing the GUI main.
  1. Adding commandline parameters.
  1. Preferences.
  1. Logging.

# Downloading the Template #
The template may be downloaded from the [Java Project Template](http://code.google.com/p/java-project-template/) main project page. The latest and greatest version will always be listed on the main project page. Older versions are available under the "Downloads" tab, but it is highly recommended that you download only the versions featured on the main page.

# Template Organization #
The template has the following important folders:

  * src
  * test
  * bin

The "src" folder contains the project's sources. The "test" folder is where you should put all your JUnit unit tests. The "bin" folder is where the executable JAR is generated.

There are also some additional folders to be aware of:

  * lib
  * build
  * docs

The "lib" folder is there as an alternative output location to "bin", in the event that you choose to generate a library JAR (requires changing the rules in "pom.xml").

The "build" folder is used by Maven2 as a dumping ground for intermediate output files, and the "docs" folder is the destination for generated javadoc documentation.

# Building the Template #
The best way to build the template is from the commandline. Building the project requires that you have Sun Java JDK 5+ and Apache Maven2. GNU Make is also nice to have. For more detailed setup instructions, see [Java Development Setup](http://sites.google.com/site/michaelsafyan/coding/resources/how-to-guides/development-environment-setup/java).

If you have GNU Make, just build the project with "make". You can also use "make clean" to clean the project and "make docs" to build the project's javadoc documentation. If you do not have make, you can build the project using the command "mvn package".

The output is generated in "bin" in the form of a standalone executable JAR, with all of the JAR's thirdparty dependencies wrapped-up into the output JAR file.

**NOTE**: The first time you build the project, especially if it is your first time building with Maven2, the build may take a while, since Maven2 needs to download its own dependencies as well as this project template's dependencies. Do not worry; after you build the project for the first time, building takes a reasonable amount of time.

# Editing the Template #
As of version 6.7 (possibly earlier, as well), the [NetBeans IDE](http://www.netbeans.org/) automatically recognizes Maven2 projects as NetBeans projects. The NetBeans IDE is, IMHO, the best way to edit this project (and, for that matter, any Java project).

# Customizing the Template #
By default, the main package for this template is "com.yourcompanyname.projectname". After all, it is a template. You will want to refactor this (probably using NetBeans's refactoring tool) to a reasonable package name for your product. You will also need to edit the "pom.xml" to reflect this change (you need to update the groupId and artifactId).

The Main class (in what was formerly "com.yourcompanyname.projectname") is the project's main class. It contains functions that return information about your project, including your project's pretty name (e.g. "Java Project Template"), project's codename (e.g. "java-project-template"), your project's version number (e.g. "1.0.0"), and the project's vendor (e.g. "My Company"). You should probably update this information. Be sure that the changes you make are reflected in the "pom.xml" file, if applicable.

# Implementing Commandline Main #
This project is designed so that there are three types of main functions:
  * commandline main
  * interactive main
  * gui main

The commandline main is the main function that is invoked by default. The interactive main is invoked when the commandline option "--interactive" is given. The GUI main is invoked when the commandline option "--gui" is given. Changing the behavior so that only one of these functions is invoked requires a trivial change at the end of the "public static void main(String[.md](.md) args)" function in the Main class, although you will want to update the output of "--help", accordingly, if you do so.

When you implement the commandline main, it should require that all the parameters needed have been provided on the commandline and should print the output to STDOUT/STDERR. This is in contrast to the interactive main, which should prompt for any necessary parameters that have not already been specified in the commandline invocation.

Implementing the commandline main involves:
  1. Capturing any additional parameters in "public static void main(String[.md](.md))" and storing them in a private static variable.

  1. Updating the output of "--help" to reflect the additional commandline options.

  1. Modifying "commandlineMain" to:
    1. Verify that all necessary parameters have been specified and to invoke the usage function and abort if any of the necessary parameters have not been given.
    1. Output the program's logical result, based on the given inputs.

# Implementing Interactive Main #
Implementing interactive main is identical to implementing commandline main, except that you use the "interactiveMain" function, and, instead of displaying the program's usage message and aborting if a needed parameter has not been specified, you should prompt (possibly using the Teletype class) for the required input value.

# Implementing GUI Main #
Implementing GUI main involves rewriting the "graphicalMain" function to construct your application's GUI, taking heed of any parameters that have already been specified, and using reasonable default values for any parameters that have been left unspecified.

# Adding Commandline Parameters #
Adding commandline parameters involves:
  1. Modifying "main" to recognize the added parameters and save them to static variables.
  1. Modifying "printHelpMessage" to list the new options.
  1. Modifying "commandlineMain" to use the parameters.
  1. Modifying "interactiveMain" to use the parameters.
  1. Modifying "graphicalMain" to use the parameters.

# Preferences #
The generated executable JAR accepts the following preference-related options:

  * --prefset _key_=_value_
  * --prefget _key_
  * --prefdel _key_
  * --preflist

These functions use the [Preferences](http://java.sun.com/javase/6/docs/api/java/util/prefs/Preferences.html) class, specifically one resulting from invoking [Preferences](http://java.sun.com/javase/6/docs/api/java/util/prefs/Preferences.html).[userNodeForPackage(](http://java.sun.com/javase/6/docs/api/java/util/prefs/Preferences.html#userNodeForPackage%28java.lang.Class%29)[Main)](http://code.google.com/p/java-project-template/source/browse/trunk/src/com/yourcompanyname/projectname/Main.java), where Main is the main class. This means that these keys belong to and affect the main package (formerly "com.yourcompanyname.projectname") and all of its sub-packages.

The "--prefset" option sets a key in the preferences object. For example: "--prefset x=5", "--prefset y='Hello'", "--prefset window.location.x=0", "--prefset window.location.y=0", etc. As you can see with the examples, you can use dots to create hierarchical information. Keys with dot names will appear in the corresponding sub-package of the main package (i.e. the package that was formerly "com.yourcompanyname.projectname"). For example, if you left the top-level package as "com.yourcompanyname.projectname" and you set a key called "alpha.bravo", the key will appear as "bravo" if you invoke "userNodeForPackage" on a class in "com.yourcompanyname.projectname.alpha".

The "--prefget" option prints the value for a given key, the "--prefdel" option deletes a key, and the "--preflist" option lists all keys that have been set. See the above paragraph on keys.

# Logging #
This template lists the Apache Log4J library as a dependency. You can use log4j, and Maven2 will ensure that it exists wherever you build the project. For added convenience, there is a Loggable class that adds an "entering" and "exiting" function for logging the entering and exiting of functions. However, you can just use log4j directly, and everything will work fine.

This template modifies the default format of the logging output. You can change the line in "public static void main(String[.md](.md) args)" that constructs the PatternLayout, to use your preferred pattern layout.

The template provides several commandline options for controlling logging, including:

  * --loglevel _level_
  * --loglevel _class_=_level_

The first option sets the logging level of the root logger to the given level. The second options sets the logging level of the given class logger to the given level. Valid levels are "all", "trace", "debug", "warn", "error", "fatal", and "off".