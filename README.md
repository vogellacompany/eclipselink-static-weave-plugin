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


# Copied wiki page as it may be removed by the Eclipse Foundation:

| `classpath` | The classpath that contains classes referenced by the persistence unit. This can be a nested element that contains one or more `pathelement` elements. | None | Optional |
Kopieren
Execute the weave Ant task. For example:
ant weaving
Kopieren
Use the Maven plugin
Use the EclipseLink Static Weaving Maven plugin, as follows:

Add the following plugin configuration to your project’s pom.xml file:
<plugin>
    <groupId>org.eclipse.persistence</groupId>
    <artifactId>eclipselink-staticweave-maven-plugin</artifactId>
    <version>2.7.9</version>
    <executions>
        <execution>
            <phase>process-classes</phase>
            <goals>
                <goal>weave</goal>
            </goals>
        </execution>
    </executions>
    <configuration>
        <persistenceXMLLocation>META-INF/persistence.xml</persistenceXMLLocation>
        <logLevel>FINE</logLevel>
        <logFile>staticweave.log</logFile>
    </configuration>
</plugin>
Kopieren
Execute the Maven build command. For example:
mvn clean install
Kopieren
For more information about the EclipseLink Static Weaving Maven plugin, see https://github.com/eclipse/eclipselink-staticweave-maven-plugin.

Use the Command Line
Use the StaticWeave command-line utility, as follows:

Set up your environment to include the following JAR files in your classpath:
eclipselink.jar
javax.persistence_2.1.jar
Any other JAR files that your persistence unit depends on
Execute the StaticWeave command with the appropriate options. For example:
java org.eclipse.persistence.tools.weaving.jpa.StaticWeave -persistenceinfo c:\myjar-containing-persistenceinfo.jar -classpath c:\myjar-dependent.jar -loglevel FINE -logfile staticweave.log c:\myjar.jar c:\wovenmyjar.jar
Kopieren
For more information about the StaticWeave command-line utility and its options, see https://www.eclipse.org/eclipselink/documentation/2.7/concepts/app_dev007.htm.



| `classpath` | The classpath that contains classes referenced by the persistence unit. This can be a nested element that contains one or more `pathelement` elements. | None | Optional |
Kopieren
Execute the weave Ant task. For example:
ant weaving
Kopieren
Use the Maven plugin
Use the EclipseLink Static Weaving Maven plugin, as follows:

Add the following plugin configuration to your project’s pom.xml file:
<plugin>
    <groupId>org.eclipse.persistence</groupId>
    <artifactId>eclipselink-staticweave-maven-plugin</artifactId>
    <version>2.7.9</version>
    <executions>
        <execution>
            <phase>process-classes</phase>
            <goals>
                <goal>weave</goal>
            </goals>
        </execution>
    </executions>
    <configuration>
        <persistenceXMLLocation>META-INF/persistence.xml</persistenceXMLLocation>
        <logLevel>FINE</logLevel>
        <logFile>staticweave.log</logFile>
    </configuration>
</plugin>
Kopieren
Execute the Maven build command. For example:
mvn clean install
Kopieren
For more information about the EclipseLink Static Weaving Maven plugin, see https://github.com/eclipse/eclipselink-staticweave-maven-plugin.

Use the Command Line
Use the StaticWeave command-line utility, as follows:

Set up your environment to include the following JAR files in your classpath:
eclipselink.jar
javax.persistence_2.1.jar
Any other JAR files that your persistence unit depends on
Execute the StaticWeave command with the appropriate options. For example:
java org.eclipse.persistence.tools.weaving.jpa.StaticWeave -persistenceinfo c:\myjar-containing-persistenceinfo.jar -classpath c:\myjar-dependent.jar -loglevel FINE -logfile staticweave.log c:\myjar.jar c:\wovenmyjar.jar
Kopieren
For more information about the StaticWeave command-line utility and its options, see https://www.eclipse.org/eclipselink/documentation/2.7/concepts/app_dev007.htm.
