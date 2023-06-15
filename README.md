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



{{EclipseLink_UserGuide
|info=y
|toc=n
|eclipselink=y
|eclipselinktype=JPA}}

=Configuring Static Weaving=

Use static weaving to weave all applicable class files at build time so that you can deliver pre-woven class files. Consider this option to weave all applicable class files at build time so that you can deliver prewoven class files. By doing so, you can improve application performance by eliminating the runtime weaving step required by dynamic weaving (see [[EclipseLink/UserGuide/JPA/Advanced_JPA_Development/Performance/Weaving/Dynamic Weaving|Configuring Dynamic Weaving]]).

In addition, consider using static weaving to weave in Java environments where you cannot configure an agent.

==Prerequisite: Organize/Package Files==
Prior to weaving, you must package your persistence unit in either of the following configurations: 

* JAR file, as specified in the JPA 2.0 specification.{{EclipseLink_Spec
|link=http://jcp.org/en/jsr/detail?id=220
|section=Section 11.1.18 "Persistence Unit Packaging"}}

* Exploded directory structure

In both cases, the requirements are:

* Classes must be stored at the base in directories based on their package structure 
* There must be a <tt>META-INF</tt> directory that contains the <tt>persistence.xml</tt> file. <br/>Note: You can use the <tt>persistenceinfo</tt> setting on the <tt>weave</tt> Ant task, as described in the [[#Table 19-31|EclipseLink weave Ant Task Attributes]] table, below, to specify a different location.

For example, when using a JAR file, <tt>mypersitenceunit.jar</tt> could contain the following:
* <tt>mypackage/MyEntity1.class</tt>
* <tt>mypackage/MyEntity2.class</tt>
* <tt>mypackage2/MyEntity3.class</tt>
* <tt>META-INF/persistence.xml </tt>

For example, If your base directory is c:/classes, the exploded directory structure would look as follows:

* <tt>c:/classes/mypackage/MyEntity1.class</tt>
* <tt>c:/classes/mypackage/MyEntity2.class</tt>
* <tt>c:/classes/mypackage2/MyEntity3.class</tt>
* <tt>c:/classes/META-INF/persistence.xml </tt>

==Step 1: Execute the Static Weaver==

Execute the static weaver in one of the following ways: 

* [[#Use the weave Ant Task|Use the weave Ant Task]]
* [[#Use the Maven plugin|Use the Maven plugin]]
* [[#Use the Command Line|Use the Command Line]]

===Use the weave Ant Task===
Use the <tt>weave</tt> ant task, as follows:

<ol>
<li> Configure the <tt>weave</tt> Ant task in your build script, as this example shows. The [[#Table 19-31|EclipseLink weave Ant Task Attributes]] table lists the attributes of this task.<br>
<span id="Example 19-37"></span>
''''' EclipseLink weave Ant Task''''' 
<div class="pre">
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
</div>
<br>
<span id="Table 19-31"></span>
''''' EclipseLink <tt>weave</tt> Ant Task Attributes'''''
{| class="RuleFormalMax" dir="ltr" title="EclipseLink weave Ant Task Attributes" summary="This table lists the attributes of the EclipseLink JPA weave Ant task." width="100%" border="1" frame="border" rules="all" cellpadding="3" frame="border" rules="all"
|- align="left" valign="top"
! id="r1c1-t47" align="left" valign="bottom" | '''Attribute'''
! id="r1c2-t47" align="left" valign="bottom" | '''Description'''
! id="r1c3-t47" align="left" valign="bottom" | '''Default'''
! id="r1c4-t47" align="left" valign="bottom" | '''Required or Optional'''
|- align="left" valign="top"
| id="r2c1-t47" headers="r1c1-t47" align="left" |
<tt>source</tt>
| headers="r2c1-t47 r1c2-t47" align="left" |
Specifies the location of the Java source files to weave: either a directory or a JAR file.

If the <tt>persistence.xml</tt> file is not in a META-INF directory at this location, you must specify the location of the <tt>persistence.xml</tt> using the <tt>persistenceinfo</tt> attribute.
| headers="r2c1-t47 r1c3-t47" align="left" | <br>
| headers="r2c1-t47 r1c4-t47" align="left" |
Required
|- align="left" valign="top"
| id="r3c1-t47" headers="r1c1-t47" align="left" |
<tt>target</tt>
| headers="r3c1-t47 r1c2-t47" align="left" |
Specifies the output location: either a directory or a JAR file.
| headers="r3c1-t47 r1c3-t47" align="left" | <br>
| headers="r3c1-t47 r1c4-t47" align="left" |
Required
|- align="left" valign="top"
| id="r4c1-t47" headers="r1c1-t47" align="left" |
<tt>persistenceinfo</tt>
| headers="r4c1-t47 r1c2-t47" align="left" |
Specifies the location of the <tt>persistence.xml</tt> file if it is not in the same location as the source.  Note: persistence.xml should be put in a directory called META-INF at this location
| headers="r4c1-t47 r1c3-t47" align="left" | <br>
| headers="r4c1-t47 r1c4-t47" align="left" |
Optional
|- align="left" valign="top"
| id="r5c1-t47" headers="r1c1-t47" align="left" |
<tt>log</tt>
| headers="r5c1-t47 r1c2-t47" align="left" |
Specifies a logging file.
| headers="r5c1-t47 r1c3-t47" align="left" |

| headers="r5c1-t47 r1c4-t47" align="left" |
Optional
|- align="left" valign="top"
| id="r6c1-t47" headers="r1c1-t47" align="left" |
<tt>loglevel</tt>
| headers="r6c1-t47 r1c2-t47" align="left" |
Specifies the amount and detail of log output.

Valid <tt>java.util.logging.Level</tt> values are the following:
* <tt>OFF</tt>
* <tt>SEVERE</tt>
* <tt>WARNING</tt>
* <tt>INFO</tt>
* <tt>CONFIG</tt>
* <tt>FINE</tt>
* <tt>FINER</tt>
* <tt>FINEST</tt>
* <tt>ALL</tt>

For more information, see [[Introduction%20to%20EclipseLink%20Sessions%20(ELUG)#Logging|Logging]].
| headers="r6c1-t47 r1c3-t47" align="left" |
<tt>Level.OFF</tt>
| headers="r6c1-t47 r1c4-t47" align="left" |
Optional
|}
<br><br>
{| class="Note oac_no_warn" width="80%" border="1" frame="hsides" rules="groups" cellpadding="3" frame="hsides" rules="groups"
| align="left" |
'''Note:''' If <tt>source</tt> and <tt>target</tt> point to the same location and, if the <tt>source</tt> is a directory (not a JAR file), EclipseLink will weave in place. If <tt>source</tt> and <tt>target</tt> point to different locations, or if the <tt>source</tt> is a JAR file (as the [[#Example 19-37|EclipseLink weave Ant Task]] example shows), EclipseLink cannot weave in place.
|}
<br></li>
<li> Configure the <tt>weave</tt> task with an appropriate <tt><classpath></tt> element, as the [[#Example 19-37|EclipseLink weave Ant Task]] example shows, so that EclipseLink can load all required source classes.</li>
<li> Execute the Ant task using the command line that this example shows.<br>In this example, the <tt>weave</tt> Ant task is in the <tt>build.xml</tt> file:<br>

<span id="Example 19-38"></span>
''''' EclipseLink weave Ant Task Command Line''''' 
<div class="pre">
 ant -lib .\eclipselink.jar -lib .\persistence.jar -f build.xml weave
</div>

{| class="Note oac_no_warn" width="80%" border="1" frame="hsides" rules="groups" cellpadding="3" frame="hsides" rules="groups"
| align="left" |
'''Note:''' You must specify the <tt>eclipselink.jar</tt> and <tt>persistence.jar</tt> files (the JAR that contains the EclipseLink <tt>weave</tt> Ant task) using the Ant command line <tt>-lib</tt> option instead of using the <tt>taskdef</tt> attribute <tt>classpath</tt>.
|}

<br>
</li></ol>

===Use the Maven plugin===
If you are using Maven as build service there are two Maven plugins available.

For EclipseLink < 2.5.1 use [https://code.google.com/p/eclipselink-staticweave-maven-plugin/ original plugin.]

If you are using EclipseLink 2.5.1 or newer or you want to use Java 7 or newer there is an updated alternative available in [https://github.com/empulse-gmbh/eclipselink-static-weave-plugin GitHub]. Compiled versions are provided in Maven central.

Just add the plugin to your pom.xml.
<div class="pre">
 <build>
  ...
   <plugins>
     ...
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
</div>

Replace 
<div class="pre>
 ${eclipselink.version}
</div>
with the EclipseLink version you use in your project.

If you are using Eclipse as your IDE ensure m2e executes the plugin, otherwise weaving is not available in JUnit Tests.

Please follow the usage instructions given in the Git Repository for further configuration options.

===Use the Command Line===
Use the command line as follows: 
<div class="pre">
<tt><nowiki> java org.eclipse.persistence.tools.weaving.jpa.StaticWeave [arguments] <source> <target></nowiki></tt>
 </div>
<br>The following example shows how to use the <tt>StaticWeave</tt> class on Windows systems. The [[#Table 19-32|EclipseLink StaticWeave Class Command Line Arguments]] table lists the arguments of this class.<br>
<span id="Example 19-39"></span>
''''' Executing StaticWeave on the Command Line''''' 
<div class="pre">
 java org.eclipse.persistence.tools.weaving.jpa.StaticWeave  -persistenceinfo c:\myjar-containing-persistencexml.jar 
 -classpath c:\classpath1;c:\classpath2 c:\myjar-source.jar c:\myjar-target.jar
</div>
<br>

<span id="Table 19-32"></span>
''''' EclipseLink StaticWeave Class Command Line Arguments'''''

{| class="RuleFormalMax" dir="ltr" title="EclipseLink StaticWeave Class Command Line Arguments" summary="This table lists the arguments of the EclipseLink JPA StaticWeave class." width="100%" border="1" frame="border" rules="all" cellpadding="3" frame="border" rules="all"
|- align="left" valign="top"
! id="r1c1-t50" align="left" valign="bottom" | '''Argument'''
! id="r1c2-t50" align="left" valign="bottom" | '''Description'''
! id="r1c3-t50" align="left" valign="bottom" | '''Default'''
! id="r1c4-t50" align="left" valign="bottom" | '''Required or Optional'''
|- align="left" valign="top"
| id="r2c1-t50" headers="r1c1-t50" align="left" |
<tt>-persistenceinfo</tt>
| headers="r2c1-t50 r1c2-t50" align="left" |
Specifies the location of the <tt>persistence.xml</tt> file if it is not at the same location as the source (see <tt>-classpath</tt>) Note: EclipseLink will look in a META-INF directory at that location for persistence.xml.
| headers="r2c1-t50 r1c3-t50" align="left" | <br>
| headers="r2c1-t50 r1c4-t50" align="left" |
Optional
|- align="left" valign="top"
| id="r3c1-t50" headers="r1c1-t50" align="left" |
<tt>-classpath</tt>
| headers="r3c1-t50 r1c2-t50" align="left" |
Specifies the location of the Java source files to weave: either a directory or a JAR file. For Windows systems, use delimiter <tt>" ; "</tt>, and for Unix, use delimiter <tt>" : "</tt>.

If the <tt>persistence.xml</tt> file is not in this location, you must specify the location of the <tt>persistence.xml</tt> using the <tt>-persistenceinfo</tt> attribute.
| headers="r3c1-t50 r1c3-t50" align="left" | <br>
| headers="r3c1-t50 r1c4-t50" align="left" |
Required
|- align="left" valign="top"
| id="r4c1-t50" headers="r1c1-t50" align="left" |
<tt>-log</tt>
| headers="r4c1-t50 r1c2-t50" align="left" |
Specifies a logging file.
| headers="r4c1-t50 r1c3-t50" align="left" |

| headers="r4c1-t50 r1c4-t50" align="left" |
Optional
|- align="left" valign="top"
| id="r5c1-t50" headers="r1c1-t50" align="left" |
<tt>-loglevel</tt>
| headers="r5c1-t50 r1c2-t50" align="left" |
Specifies the amount and detail of log output.

Valid <tt>java.util.logging.Level</tt> values are as follows:
* <tt>OFF</tt>
* <tt>SEVERE</tt>
* <tt>WARNING</tt>
* <tt>INFO</tt>
* <tt>CONFIG</tt>
* <tt>FINE</tt>
* <tt>FINER</tt>
* <tt>FINEST</tt>

For more information, see [[Introduction%20to%20EclipseLink%20Sessions%20(ELUG)#Logging|Logging]].
| headers="r5c1-t50 r1c3-t50" align="left" |
<tt>Level.OFF</tt>
| headers="r5c1-t50 r1c4-t50" align="left" |
Optional
|- align="left" valign="top"
| id="r6c1-t50" headers="r1c1-t50" align="left" |
<tt><nowiki><source></nowiki></tt>
| headers="r6c1-t50 r1c2-t50" align="left" |
Specifies the location of the Java source files to weave: either a directory or a JAR file.

If the <tt>persistence.xml</tt> file is not in this location, you must specify the location of the <tt>persistence.xml</tt> using the [[#-persistenceinfo|<tt>-persistenceinfo</tt>]] attribute.
| headers="r6c1-t50 r1c3-t50" align="left" | <br>
| headers="r6c1-t50 r1c4-t50" align="left" |
Required
|- align="left" valign="top"
| id="r7c1-t50" headers="r1c1-t50" align="left" |
<tt><target></tt>
| headers="r7c1-t50 r1c2-t50" align="left" |
Specifies the output location: either a directory or a JAR file.
| headers="r7c1-t50 r1c3-t50" align="left" | <br>
| headers="r7c1-t50 r1c4-t50" align="left" |
Required
|}
<br><br>
{| class="Note oac_no_warn" width="80%" border="1" frame="hsides" rules="groups" cellpadding="3" frame="hsides" rules="groups"
| align="left" |
'''Note:''' If <tt><nowiki><source></nowiki></tt> and <tt><target></tt> point to the same location and if the <tt><nowiki><source></nowiki></tt> is a directory (not a JAR file), EclipseLink will weave in place. If <tt><nowiki><source></nowiki></tt> and <tt><target></tt> point to different locations, or if the <tt>source</tt> is a JAR file (as the [[#Example 19-39| Executing StaticWeave on the Command Line]] example shows), EclipseLink cannot weave in place.
|}
<br>

===Step 2: Configure persistence.xml===
Configure your <tt>persistence.xml</tt> file with a <tt>[[#eclipselink.weaving|eclipselink.weaving]]</tt> extension set to <tt>static</tt>, as this example shows:<br>

<span id="Example 19-40"></span>

''''' Setting eclipselink.weaving in the persistence.xml File''''' 
<div class="pre">
 <persistence>
     <persistence-unit name="HumanResources">
         <class>com.acme.Employee</class>
         ...
         <properties>
 
             <property
                 name="eclipselink.weaving"
                 value="static"
             />
         </properties>
     </persistence-unit>
 </persistence>
</div>
<br>
For more information, see the [[#Table 19-16|EclipseLink JPA Persistence Unit Properties for Customization and Validation]] table.

===Step 3: Package and Deploy===

Package and deploy your application.




{{EclipseLink_JPA
|previous=[[EclipseLink/UserGuide/JPA/Advanced_JPA_Development/Performance/Weaving/Dynamic Weaving|Configuring Dynamic Weaving]]
|next    =[[EclipseLink/UserGuide/JPA/Advanced_JPA_Development/Performance/Weaving/Weaving_POJO_Classes|Weaving POJO Classes]]
|up      =[[EclipseLink/UserGuide/JPA/Advanced_JPA_Development/Performance/Weaving|Weaving]]
|version=2.2.0 DRAFT}}


