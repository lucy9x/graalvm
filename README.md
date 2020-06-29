# Installation
- Download: [https://www.graalvm.org/downloads/](https://www.graalvm.org/downloads/)
- Install native-image: `graalvm-ce-java8-20.1.0\bin\gu install native-image`
- Requirement
  - Ubuntu: `sudo apt install build-essential zlib1g zlib1g-dev`
  - Windows: Install SDK 7.1 [GRMSDKX_EN_DVD.iso](https://www.microsoft.com/en-us/download/details.aspx?id=8442) & Install Compiler x64 [VC-Compiler-KB2519277](https://download.microsoft.com/download/7/5/0/75040801-126C-4591-BCE4-4CD1FD1499AA/VC-Compiler-KB2519277.exe)
  
# Build Native Spring boot
- `call "C:\Program Files\Microsoft SDKs\Windows\v7.1\Bin\SetEnv.cmd` if windows platform
- `mvn -Pnative clean package`
- pom.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.3.1.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>demo</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>demo</name>
	<description>Demo project for Spring Boot</description>

	<properties>
        <start-class>
            com.example.demo.DemoApplication
        </start-class>
        <java.version>1.8</java.version>
    </properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.springframework.experimental</groupId>
			<artifactId>spring-graalvm-native</artifactId>
			<version>0.7.1</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context-indexer</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
			<exclusions>
				<exclusion>
					<groupId>org.junit.vintage</groupId>
					<artifactId>junit-vintage-engine</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
	</dependencies>
	<repositories>
		<repository>
			<id>spring-milestones</id>
			<name>Spring Milestones</name>
			<url>https://repo.spring.io/milestone</url>
		</repository>
	</repositories>
	<pluginRepositories>
		<pluginRepository>
			<id>spring-milestones</id>
			<name>Spring Milestones</name>
			<url>https://repo.spring.io/milestone</url>
		</pluginRepository>
	</pluginRepositories>
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
	<profiles>
		<profile>
			<id>native</id>
			<build>
				<plugins>
					<plugin>
						<groupId>org.graalvm.nativeimage</groupId>
						<artifactId>native-image-maven-plugin</artifactId>
						<version>20.1.0</version>
						<configuration>
							<buildArgs>-H:+TraceClassInitialization -H:+ReportExceptionStackTraces -Dspring.native.remove-unused-autoconfig=true -Dspring.native.remove-yaml-support=true</buildArgs>
							<imageName>${project.artifactId}</imageName>
						</configuration>
						<executions>
							<execution>
								<goals>
									<goal>native-image</goal>
								</goals>
								<phase>package</phase>
							</execution>
						</executions>
					</plugin>
					<plugin>
						<groupId>org.springframework.boot</groupId>
						<artifactId>spring-boot-maven-plugin</artifactId>
					</plugin>
				</plugins>
			</build>
		</profile>
	</profiles>
</project>

```
