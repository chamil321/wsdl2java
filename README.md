### Steps to follow
- Open a new java project using maven
- Copy following to the pom file replacing {{}} with your values
```xml
<name>{{suitetalk-chamil-proxy-v2019_2}}</name>
    <build>
        <plugins>
            <!-- Generates JAVA source files from the WSDL -->
            <plugin>
                <groupId>org.apache.axis2</groupId>
                <artifactId>axis2-wsdl2code-maven-plugin</artifactId>
                <version>1.7.7</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>wsdl2code</goal>
                        </goals>
                        <configuration>
                            <packageName>{{com.chamil.wsdl.java}}</packageName>
                            <!-- Location of the WSDL -->
                            <wsdlFile>{{https://webservices.netsuite.com/wsdl/v2019_2_0/netsuite.wsdl}}</wsdlFile> 
                            <databindingName>xmlbeans</databindingName>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <artifactId>maven-source-plugin</artifactId>
                <executions>
                    <execution>
                        <id>attach-sources</id>
                        <goals>
                            <goal>jar</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <artifactId>maven-assembly-plugin</artifactId>
                <configuration>
                    <appendAssemblyId>false</appendAssemblyId>
                    <descriptorRefs>
                        <descriptorRef>src</descriptorRef>
                    </descriptorRefs>
                </configuration>
                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <!-- this is used for inheritance merges -->
                        <phase>package</phase>
                        <!-- bind to the packaging phase -->
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>copy</id>
                        <phase>process-resources</phase>
                        <goals>
                            <goal>copy</goal>
                        </goals>
                        <configuration>
                            <artifactItems>
                                <artifactItem>
                                    <groupId>org.apache.openejb</groupId>
                                    <artifactId>openejb-javaagent</artifactId>
                                    <version>3.0-beta-2</version>
                                    <outputDirectory>${project.build.directory}</outputDirectory>
                                </artifactItem>
                            </artifactItems>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
        <resources>
            <resource>
                <directory>target/generated-sources/axis2/wsdl2code/resources</directory>
            </resource>
            <resource>
                <directory>target/generated-sources/xmlbeans/resources</directory>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
            </resource>
        </resources>
    </build>
    <dependencies>
        <dependency>
            <groupId>org.apache.axis2</groupId>
            <artifactId>axis2-wsdl2code-maven-plugin</artifactId>
            <version>1.7.7</version>
        </dependency>
    </dependencies>
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
```
- Run - mvn clean install
