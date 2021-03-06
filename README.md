<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

	<modelVersion>4.0.0</modelVersion>

	<groupId>aptasuite</groupId>

	<artifactId>aptasuite</artifactId>

	<version>0.0.1-SNAPSHOT</version>

	<name>AptaSUITE</name>

	<description>A full-featured software collection for the comprehensive analysis of HT-SELEX experiments with both graphical and command line based user interfaces.</description>

	<profiles>

    <profile>

        	<id>windows</id>

        	<build>

            	<directory>c:/Users/hoinkaj/temp/aptasuite</directory>

        	</build>

    	</profile>

	</profiles>

	<build>

		<plugins>

			<!-- The build scheme was adopted from https://ath3nd.wordpress.com/2013/12/25/packaging-a-multimodule-maven-spring-app-in-a-standalone-jar/ -->

			<plugin>

				<!-- This plugin is used to just copy all the dependency jars 

				of the main project into a single folder 

				(default is ${project.location}/target/dependency).  -->

				<groupId>org.apache.maven.plugins</groupId>

				<artifactId>maven-dependency-plugin</artifactId>

				<version>2.8</version>

				<executions>

					<execution>

						<id>copy-dependencies</id>

						<phase>package</phase>

						<goals>

							<goal>copy-dependencies</goal>

						</goals>

						<configuration>

							<overWriteReleases>true</overWriteReleases>

							<overWriteSnapshots>true</overWriteSnapshots>

							<overWriteIfNewer>true</overWriteIfNewer> 

							<prependGroupId>true</prependGroupId>

						</configuration>

					</execution>

				</executions>

			</plugin>

			<plugin>

				<!-- This plugin is used to add stuff to the manifest of your standalone jar. 

				The important thing is to add the classpath of each of your dependency jars 

				into the manifest. For this you’d have to use a custom classpath layout -->

				<groupId>org.apache.maven.plugins</groupId>

				<artifactId>maven-jar-plugin</artifactId>

				<version>3.0.2</version>

				<configuration>

					<archive>

						<manifest>

							<addClasspath>true</addClasspath>

							<mainClass>aptasuite.Aptasuite</mainClass>

							<classpathLayoutType>custom</classpathLayoutType>

							<customClasspathLayout>lib${file.separator}${artifact.groupId}.${artifact.artifactId}-${artifact.version}${dashClassifier?}.${artifact.extension}</customClasspathLayout>

						</manifest>

					</archive>

				</configuration>

			</plugin>

			<plugin>

				<!-- And finally, the assembly plugin is used to generate the jar, but this 

				time it is not going to be using a predefined assembly descriptor but it would 

				have to create one on its own. -->

				<groupId>org.apache.maven.plugins</groupId>

				<artifactId>maven-assembly-plugin</artifactId>

				<version>2.6</version>

				<executions>

					<execution>

						<id>assembly:package</id>

						<phase>package</phase>

						<goals>

							<goal>single</goal>

						</goals>

						<configuration>

							<appendAssemblyId>false</appendAssemblyId>

							<descriptors>

								<descriptor>src${file.separator}main${file.separator}resources${file.separator}assembly.xml</descriptor>

							</descriptors>

							<resources>

								<resource>

									<directory>src${file.separator}main${file.separator}gui${file.separator}core</directory>

									<includes>

										<include>**/*.fxml</include>

										<include>**/*.css</include>

									</includes>

								</resource>

   							</resources>							

						</configuration>

					</execution>

				</executions>

			</plugin>			

			<!--  <plugin>

				<groupId>org.apache.maven.plugins</groupId>

				<artifactId>maven-jar-plugin</artifactId>

				<version>3.0.2</version>

				<executions>

					<execution>

						<id>AptaSuiteMainInstall</id>

						<phase>install</phase>

						<configuration>

							<archive>

								<manifest>

									<mainClass>aptasuite.Aptasuite</mainClass>

								</manifest>

							</archive>

						</configuration>

					</execution>

				</executions>

			</plugin> -->

			<!-- <plugin>

				<groupId>org.apache.maven.plugins</groupId>

				<artifactId>maven-shade-plugin</artifactId>

				<version>3.1.0</version>

				<configuration>

					<shadedArtifactAttached>true</shadedArtifactAttached>

					<shadedClassifierName>${shadedClassifier}</shadedClassifierName>

					<createDependencyReducedPom>true</createDependencyReducedPom>

					<filters>

						<filter>

							<artifact>*:*</artifact>

							<excludes>

								<exclude>org/datanucleus/**</exclude>

								<exclude>META-INF/*.SF</exclude>

								<exclude>META-INF/*.DSA</exclude>

								<exclude>META-INF/*.RSA</exclude>

							</excludes>

						</filter>

					</filters>

				</configuration>

				<executions>

					<execution>

						<phase>package</phase>

						<goals>

							<goal>shade</goal>

						</goals>

						<configuration>

							<transformers>

								<transformer

									implementation="org.apache.maven.plugins.shade.resource.IncludeResourceTransformer" />

								<transformer

									implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer">

									<resource>reference.conf</resource>

								</transformer>

								<transformer

									implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />

								<transformer

									implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">

									<mainClass>aptasuite.Aptasuite</mainClass>

								</transformer>

							</transformers>

						</configuration>

					</execution>

				</executions>

			</plugin> -->

			<!-- Executable Jars BEGIN -->

			<!-- <plugin>

				<artifactId>maven-assembly-plugin</artifactId>

				<version>2.6</version>

				<executions>

					<execution>

						<id>AptaSuiteMain</id>

						<configuration>

							<appendAssemblyId>false</appendAssemblyId>

							<finalName>aptasuite</finalName>

							<attach>false</attach>

							<descriptorRefs>

								<descriptorRef>jar-with-dependencies</descriptorRef>

							</descriptorRefs>

							<archive>

								<manifest>

									<mainClass>aptasuite.Aptasuite</mainClass>

								</manifest>

							</archive>

						</configuration>

						<phase>package</phase>

						<goals>

							<goal>single</goal>

						</goals>

					</execution>

					<execution>

						<id>RNAFold4J</id>

						<configuration>

							<appendAssemblyId>false</appendAssemblyId>

							<finalName>rnafold4j</finalName>

							<attach>false</attach>

							<descriptorRefs>

								<descriptorRef>jar-with-dependencies</descriptorRef>

							</descriptorRefs>

							<archive>

								<manifest>

									<mainClass>lib.structure.rnafold.RNAFold</mainClass>

								</manifest>

							</archive>

						</configuration>

						<phase>package</phase>

						<goals>

							<goal>single</goal>

						</goals>

					</execution>

					<execution>

						<id>MapDBBenchmark</id>

						<configuration>

							<finalName>MapDBBenchmark</finalName>

							<descriptorRefs>

								<descriptorRef>jar-with-dependencies</descriptorRef>

							</descriptorRefs>

							<archive>

								<manifest>

									<mainClass>benchmarks.MapDB</mainClass>

								</manifest>

							</archive>

						</configuration>

						<phase>package</phase>

						<goals>

							<goal>single</goal>

						</goals>

					</execution>

				</executions>

			</plugin> -->

			<!-- Executable Jars END -->

		</plugins>

	</build>



	<!-- Choose in Properties depending on the Platform -->

	<dependencyManagement>

		<dependencies>

			<dependency>

				<groupId>org.nd4j</groupId>

				<artifactId>nd4j-native-platform</artifactId>

				<version>${nd4j.version}</version>

			</dependency>

			<dependency>

				<groupId>org.nd4j</groupId>

				<artifactId>nd4j-cuda-7.5-platform</artifactId>

				<version>${nd4j.version}</version>

			</dependency>

			<dependency>

				<groupId>org.nd4j</groupId>

				<artifactId>nd4j-cuda-8.0-platform</artifactId>

				<version>${nd4j.version}</version>

			</dependency>

		</dependencies>

	</dependencyManagement>



	<dependencies>

		<dependency>

			<groupId>org.mapdb</groupId>

			<artifactId>mapdb</artifactId>

			<version>3.0.5</version>

		</dependency>

		<dependency>

			<groupId>com.googlecode.concurrent-trees</groupId>

			<artifactId>concurrent-trees</artifactId>

			<version>2.6.0</version>

		</dependency>

		<dependency>

			<groupId>com.baqend</groupId>

			<artifactId>bloom-filter</artifactId>

			<version>1.0.7</version>

		</dependency>

		<dependency>

			<groupId>org.apache.commons</groupId>

			<artifactId>commons-configuration2</artifactId>

			<version>2.1</version>

		</dependency>

		<dependency>

			<groupId>commons-beanutils</groupId>

			<artifactId>commons-beanutils</artifactId>

			<version>1.9.3</version>

		</dependency>

		<dependency>

			<groupId>net.sf.trove4j</groupId>

			<artifactId>trove4j</artifactId>

			<version>3.0.3</version>

		</dependency>

		<dependency>

			<groupId>com.milaboratory</groupId>

			<artifactId>milib</artifactId>

			<version>1.4</version>

		</dependency>

		<dependency>

			<groupId>net.byteseek</groupId>

			<artifactId>byteseek</artifactId>

			<version>2.0.3</version>

		</dependency>

		<dependency>

			<groupId>commons-cli</groupId>

			<artifactId>commons-cli</artifactId>

			<version>1.4</version>

		</dependency>

		<dependency>

			<groupId>jfree</groupId>

			<artifactId>jfreechart</artifactId>

			<version>1.0.13</version>

		</dependency>

		<dependency>

			<groupId>com.itextpdf</groupId>

			<artifactId>itextpdf</artifactId>

			<version>5.5.10</version>

		</dependency>

		<dependency>

			<groupId>it.unimi.dsi</groupId>

			<artifactId>fastutil</artifactId>

			<version>6.5.4</version>

		</dependency>

		<dependency>

			<groupId>com.koloboke</groupId>

			<artifactId>koloboke-compile</artifactId>

			<version>0.5.1</version>

			<scope>provided</scope>

		</dependency>

		<dependency>

			<groupId>com.koloboke</groupId>

			<artifactId>koloboke-impl-common-jdk8</artifactId>

			<version>1.0.0</version>

		</dependency>

		<dependency>

			<groupId>org.eclipse.collections</groupId>

			<artifactId>eclipse-collections-api</artifactId>

			<version>8.1.0</version>

		</dependency>

		<dependency>

			<groupId>org.eclipse.collections</groupId>

			<artifactId>eclipse-collections</artifactId>

			<version>8.1.0</version>

		</dependency>

		<dependency>

			<groupId>commons-lang</groupId>

			<artifactId>commons-lang</artifactId>

			<version>2.6</version>

		</dependency>

		<dependency>

			<groupId>org.datavec</groupId>

			<artifactId>datavec-api</artifactId>

			<version>${nd4j.version}</version>

		</dependency>

		<!-- cuDNN Option. This requires additional external dependencies, disable 

			for production or check for support in code -->

		<!-- <dependency> <groupId>org.deeplearning4j</groupId> <artifactId>deeplearning4j-cuda-8.0</artifactId> 

			<version>${nd4j.version}</version> </dependency> -->

		<dependency>

			<groupId>org.nd4j</groupId>

			<artifactId>${nd4j.backend}</artifactId>  <!-- WE NEED TO CHANGE THIS IN PROPERTIES FOR CPU/CUDA SUPPORT -->

		</dependency>

		<!-- Dependency for parallel wrapper (for multi-GPU parameter averaging -->

		<dependency>

			<groupId>org.deeplearning4j</groupId>

			<artifactId>deeplearning4j-parallel-wrapper</artifactId>

			<version>0.7.2</version>

		</dependency>

		<!-- Core DL4J functionality -->

		<dependency>

			<groupId>org.deeplearning4j</groupId>

			<artifactId>deeplearning4j-core</artifactId>

			<version>${nd4j.version}</version>

		</dependency>

		<!-- Visualizing Network training functionality -->

		<dependency>

			<groupId>org.deeplearning4j</groupId>

			<artifactId>deeplearning4j-ui_2.11</artifactId>

			<version>${nd4j.version}</version>

		</dependency>

		<dependency>

			<groupId>org.slf4j</groupId>

			<artifactId>slf4j-simple</artifactId>

			<version>1.7.12</version>

		</dependency>

	</dependencies>

	<repositories>

		<repository>

			<snapshots>

				<enabled>false</enabled>

			</snapshots>

			<id>central</id>

			<name>bintray</name>

			<url>http://jcenter.bintray.com</url>

		</repository>

	</repositories>

	<properties>

		<nd4j.backend>nd4j-cuda-8.0-platform</nd4j.backend>

		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>

		<maven.compiler.source>1.8</maven.compiler.source>

		<maven.compiler.target>1.8</maven.compiler.target>

		<nd4j.version>0.9.1</nd4j.version>

		<deeplearning4j-ui.version>0.6.0</deeplearning4j-ui.version>

	</properties>

</project>
