<?xml version="1.0" encoding="UTF-8"?>

<!--
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at
    http://www.apache.org/licenses/LICENSE-2.0
Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <parent>
    <groupId>org.apache.maven</groupId>
    <artifactId>maven</artifactId>
    <version>4.0.0-alpha-1-SNAPSHOT</version>
  </parent>

  <artifactId>apache-maven</artifactId>
  <packaging>pom</packaging>

  <name>Apache Maven Distribution</name>
  <description>The Apache Maven distribution, source and binary, in zip and tar.gz formats.</description>

  <properties>
    <distributionFileName>${distributionId}-${project.version}</distributionFileName>
  </properties>

  <dependencies>
    <dependency>
      <groupId>org.apache.maven</groupId>
      <artifactId>maven-embedder</artifactId>
    </dependency>
    <dependency>
      <groupId>org.apache.maven</groupId>
      <artifactId>maven-core</artifactId>
    </dependency>
    <dependency>
      <groupId>org.apache.maven</groupId>
      <artifactId>maven-compat</artifactId>
    </dependency>
    <dependency>
      <groupId>org.eclipse.sisu</groupId>
      <artifactId>org.eclipse.sisu.plexus</artifactId>
    </dependency>
    <!-- CLI -->
    <dependency>
      <groupId>commons-cli</groupId>
      <artifactId>commons-cli</artifactId>
    </dependency>
    <dependency>
      <groupId>org.apache.maven.wagon</groupId>
      <artifactId>wagon-http</artifactId>
      <classifier>shaded</classifier>
      <exclusions>
        <exclusion>
          <groupId>org.apache.httpcomponents</groupId>
          <artifactId>httpclient</artifactId>
        </exclusion>
        <exclusion>
          <groupId>org.apache.httpcomponents</groupId>
          <artifactId>httpcore</artifactId>
        </exclusion>
        <exclusion>
          <groupId>org.apache.maven.wagon</groupId>
          <artifactId>wagon-http-shared</artifactId>
        </exclusion>
      </exclusions>
    </dependency>
    <!--  This is included in Wagon HTTP.
          We are just making the dependency explicit
          in order to ease license attribution -->
    <dependency>
      <groupId>org.jsoup</groupId>
      <artifactId>jsoup</artifactId>
      <scope>runtime</scope>
    </dependency>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>jcl-over-slf4j</artifactId>
      <version>${slf4jVersion}</version>
      <scope>runtime</scope>
    </dependency>
    <dependency>
      <groupId>org.apache.maven.wagon</groupId>
      <artifactId>wagon-file</artifactId>
    </dependency>
    <dependency>
      <groupId>org.apache.maven.resolver</groupId>
      <artifactId>maven-resolver-connector-basic</artifactId>
    </dependency>
    <dependency>
      <groupId>org.apache.maven.resolver</groupId>
      <artifactId>maven-resolver-transport-wagon</artifactId>
    </dependency>
    <dependency>
      <groupId>org.apache.maven</groupId>
      <artifactId>maven-slf4j-provider</artifactId>
    </dependency>
    <dependency>
      <groupId>org.fusesource.jansi</groupId>
      <artifactId>jansi</artifactId>
    </dependency>
    <dependency>
      <groupId>org.codehaus.plexus</groupId>
      <artifactId>plexus-cipher</artifactId>
    </dependency>
  </dependencies>

  <build>
    <finalName>${distributionFileName}</finalName>
    <pluginManagement>
      <plugins>
        <plugin>
          <groupId>org.apache.rat</groupId>
          <artifactId>apache-rat-plugin</artifactId>
          <configuration>
            <excludes combine.children="append">
              <exclude>src/assembly/maven/bin/m2.conf</exclude>
              <!-- these are partial scripts, resulting in mvn/mwnw scripts -->
              <exclude>src/assembly/shared/init</exclude>
              <exclude>src/assembly/shared/init.cmd</exclude>
              <exclude>src/assembly/shared/mvnlauncher</exclude>
              <exclude>src/assembly/shared/mvnlauncher.cmd</exclude>
              <exclude>src/assembly/shared/mvnvalidate</exclude>
              <exclude>src/assembly/shared/mvnvalidate.cmd</exclude>
              <exclude>src/assembly/shared/mvnwlauncher</exclude>
              <exclude>src/assembly/shared/mvnwlauncher.cmd</exclude>
              <exclude>src/assembly/shared/run</exclude>
              <exclude>src/assembly/shared/run.cmd</exclude>
              <exclude>src/assembly/wrapper/*/download</exclude>
              <exclude>src/assembly/wrapper/*/download.cmd</exclude>
            </excludes>
          </configuration>
        </plugin>
      </plugins>
    </pluginManagement>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <executions>
          <execution>
            <id>test-compile</id>
            <goals>
              <goal>testCompile</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <configuration>
          <systemProperties>
            <property>
              <name>basedir</name>
              <value>${basedir}</value>
            </property>
          </systemProperties>
        </configuration>
        <executions>
          <execution>
            <id>test</id>
            <goals>
              <goal>test</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <id>create-distro-packages</id>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
            <configuration>
              <descriptors>
                <descriptor>src/assembly/maven/bin.xml</descriptor>
              </descriptors>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>

  <pluginRepositories>
    <pluginRepository>
      <id>apache.snapshots</id>
      <url>https://repository.apache.org/snapshots/</url>
      <snapshots>
        <enabled>true</enabled>
      </snapshots>
      <releases>
        <enabled>false</enabled>
      </releases>
    </pluginRepository>
  </pluginRepositories>

  <profiles>
    <profile>
      <id>create-distribution-in-dir</id>
      <activation>
        <property>
          <name>distributionTargetDir</name>
        </property>
      </activation>
      <build>
        <plugins>
          <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-clean-plugin</artifactId>
            <executions>
              <execution>
                <goals>
                  <goal>clean</goal>
                </goals>
                <id>clean-target-dir</id>
                <phase>prepare-package</phase>
                <configuration>
                  <excludeDefaultDirectories>true</excludeDefaultDirectories>
                  <filesets>
                    <fileset>
                      <directory>${distributionTargetDir}</directory>
                    </fileset>
                  </filesets>
                </configuration>
              </execution>
            </executions>
          </plugin>
          <plugin>
            <artifactId>maven-assembly-plugin</artifactId>
            <executions>
              <execution>
                <id>create-distribution-dir</id>
                <phase>package</phase>
                <goals>
                  <goal>single</goal>
                </goals>
                <configuration>
                  <finalName>./</finalName>
                  <appendAssemblyId>false</appendAssemblyId>
                  <attach>false</attach>
                  <outputDirectory>${distributionTargetDir}</outputDirectory>
                  <descriptors>
                    <descriptor>src/assembly/maven/dir.xml</descriptor>
                  </descriptors>
                </configuration>
              </execution>
            </executions>
          </plugin>
        </plugins>
      </build>
    </profile>
    <profile>
      <id>apache-release</id>
      <build>
        <plugins>
          <plugin>
            <artifactId>maven-assembly-plugin</artifactId>
            <executions>
              <execution>
                <id>make-src-assembly</id>
                <phase>package</phase>
                <goals>
                  <goal>single</goal>
                </goals>
                <configuration>
                  <descriptors>
                    <descriptor>src/assembly/maven/src.xml</descriptor>
                  </descriptors>
                  <tarLongFileMode>gnu</tarLongFileMode>
                </configuration>
              </execution>
            </executions>
          </plugin>
          <!-- calculate checksums of source release for Apache dist area -->
          <plugin>
            <groupId>net.nicoulaj.maven.plugins</groupId>
            <artifactId>checksum-maven-plugin</artifactId>
            <executions>
              <execution>
                <id>source-release-checksum</id>
                <goals>
                  <goal>files</goal>
                </goals>
              </execution>
            </executions>
            <configuration>
              <fileSets>
                <fileSet>
                  <directory>${project.build.directory}</directory>
                  <includes>
                    <include>${project.artifactId}-${project.version}-src.zip</include>
                    <include>${project.artifactId}-${project.version}-src.tar.gz</include>
                    <include>${project.artifactId}-${project.version}-bin.zip</include>
                    <include>${project.artifactId}-${project.version}-bin.tar.gz</include>
                  </includes>
                </fileSet>
              </fileSets>
              <failIfNoFiles>true</failIfNoFiles>
            </configuration>
          </plugin>
        </plugins>
      </build>
    </profile>
    <profile>
      <id>versionlessMavenDist</id>
      <build>
        <finalName>${project.artifactId}</finalName>
      </build>
    </profile>
  </profiles>
</project>
