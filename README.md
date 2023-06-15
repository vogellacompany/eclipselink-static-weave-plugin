# Introduction
Plugin which performs Eclipselink static weaving. Use the weave goal to execute. 

Heavily inspired by https://code.google.com/p/eclipselink-staticweave-maven-plugin. 
This is an updated and enhanced version to be compatible with Java 7 + 8, Maven 3.x and EclipseLink 2.5.1. 

Internally the StaticWeaveProcessor is used, like described in the EclipseLink Wiki https://wiki.eclipse.org/EclipseLink/UserGuide/JPA/Advanced_JPA_Development/Performance/Weaving/Static_Weaving#Use_the_Command_Line. 

Do not forget to add EclipseLink as dependency, otherwise the EclipseLink StaticWeaveProcessor is not found. 

# Common Usage
```xml
 <build>
   	...
   <plugins>
 			 <plugin>
 			 	<groupId>de.empulse.eclipselink</groupId>
 				<artifactId>staticweave-maven-plugin</artifactId>
 				<version>1.0.0</version>
 				<executions>
 					<execution>
 						<phase>process-classes</phase>
 						<goals>
 							<goal>weave</goal>
 						</goals>
 						<configuration>
 							<persistenceXMLLocation>META-INF/persistence.xml</persistenceXMLLocation>
 							<logLevel>FINE</logLevel>
 						</configuration>
 					</execution>
 				</executions>
 				<dependencies>
 					<dependency>
 						<groupId>org.eclipse.persistence</groupId>
 						<artifactId>org.eclipse.persistence.jpa</artifactId>
 						<version>${eclipselink.version}</version>
 					</dependency>
 				</dependencies>
 			</plugin>
   		
   		...
   	</plugins>
   	...
 </build>
```
# Plugin Options

## persistenceXMLLocation
Give here the location of your persistence.xml file. This property is optional. If not set the default location META-INF/persistence.xml is used.

## source
The location of the JPA classes. This property is optional, default value is ${project.build.outputDirectory}.

## target
The location for the weaved classes. This property is optional, default value is ${project.build.outputDirectory}.

## logLevel
The Logging level of the used EclipseLink StaticWeave class. This property is optional, default value is FINE to get informed which JPA classes were woven.
	 
Possible values:
	 
* OFF
* SEVERE
* WARNING
* INFO
* CONFIG
* FINE
* FINER
* FINEST
* ALL

The EclipseLink logging information is always given in Maven INFO loglevel.

Happy weaving!


### Copied wiki page as it may be removed by the Eclipse Foundation:

# Configuring Static Weaving

Use static weaving to weave all applicable class files at build time so that you can deliver pre-woven class files. Consider this option to weave all applicable class files at build time so that you can deliver prewoven class files. By doing so, you can improve application performance by eliminating the runtime weaving step required by dynamic weaving (see [Configuring Dynamic Weaving](EclipseLink/UserGuide/JPA/Advanced_JPA_Development/Performance/Weaving/Dynamic Weaving)).

In addition, consider using static weaving to weave in Java environments where you cannot configure an agent.

## Prerequisite: Organize/Package Files

Prior to weaving, you must package your persistence unit in either of the following configurations: 

* JAR file, as specified in the JPA 2.0 specification.[^1^][1]

* Exploded directory structure

In both cases, the requirements are:

* Classes must be stored at the base in directories based on their package structure 
* There must be a `META-INF` directory that contains the `persistence.xml` file. <br/>Note: You can use the `persistenceinfo` setting on the `weave` Ant task, as described in the [EclipseLink weave Ant Task Attributes](#table-19-31) table, below, to specify a different location.

For example, when using a JAR file, `mypersitenceunit.jar` could contain the following:
* `mypackage/MyEntity1.class`
* `mypackage/MyEntity2.class`
* `mypackage2/MyEntity3.class`
* `META-INF/persistence.xml`

For example, If your base directory is c:/classes, the exploded directory structure would look as follows:

* `c:/classes/mypackage/MyEntity1.class`
* `c:/classes/mypackage/MyEntity2.class`
* `c:/classes/mypackage2/MyEntity3.class`
* `c:/classes/META-INF/persistence.xml`

## Step 1: Execute the Static Weaver

Execute the static weaver in one of the following ways: 

* [Use the weave Ant Task](#use-the-weave-ant-task)
* [Use the Maven plugin](#use-the-maven-plugin)
* [Use the Command Line](#use-the-command-line)

### Use the weave Ant Task

Use the `weave` ant task, as follows:

1. Configure the `weave` Ant task in your build script, as this example shows. The [EclipseLink weave Ant Task Attributes](#table-19-31) table lists the attributes of this task.<br>
**Example 19-37** EclipseLink weave Ant Task
```xml
 <target name="define.task" description="New task definition for EclipseLink static weaving"> 
     <taskdef name="weave" classname="org.eclipse.persistence.tools.weaving.jpa.StaticWeaveAntTask"/>
 </target>
 <target name="weaving" description="perform weaving" depends="define.task">
     <weave  source="c:\myjar.jar"
             target="c:\wovenmyjar.jar"
             persistenceinfo="c:\myjar-containing-persistenceinfo.jar">
         <classpath>
             <pathelement path="c:\myjar-dependent.jar"/>
         </classpath>
 
     </weave>
 </target>
