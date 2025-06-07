## 继承 :

  1. 创建maven父工程的模块，只留一个pom文件，并且设置打包方式为pom
     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
         <project xmlns="http://maven.apache.org/POM/4.0.0"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>

       <groupId>com.xxxyjade17</groupId>
       <artifactId>spring</artifactId>
       <version>1.0-SNAPSHOT</version>
       <packaging>pom</packaging>    //设置打包方式

       <dependencies>
           ......    //省略依赖
       </dependencies>

     </project>
     ```
  2. 在子工程配置继承关系
     ```xml
     <parent>
             <groupId>com.itheima</groupId>
             <artifactId>tlias-parent</artifactId>
             <version>1.0-SNAPSHOT</version>
             指定父工程pom.xml的相对路径
             <relativePath>../tlias-parent/pom.xml</relativePath>
     </parent>
     ```
  - **注意**:
         在子工程中，配置了继承关系之后，坐标中的 groupId 是可以省略的，因为会自动继承父工程的 。
         relativePath 指定父工程的 pom 文件的相对位置（如果不指定，将从本地仓库 / 远程仓库查找）。
         若父子工程都配置了同一个依赖的不同版本，以子工程的为准。
  3. 统一版本管理
     ```xml
     //自定义属性
     <properties>
             <lombok.version>1.18.30</lombok.version>
             <jjwt.version>0.9.1</jjwt.version>
     </properties>

     //依赖版本管理
     <dependencyManagement>
             <dependencies>
                     <!--JWT令牌-->
                     <dependency>
                             <groupId>io.jsonwebtoken</groupId>
                             <artifactId>jjwt</artifactId>
                             <version>${jjwt.version}</version>
                     </dependency>
             </dependencies>
     </dependencyManagement>
     ```
***
## 聚合
- 核心概念 : 
  将多个模块组织成一个整体，同时进行项目的构建
- 聚合工程 :
  概念 : 一个不具有业务功能的“空”工程（有且仅有一个pom文件）
  作用 : 快速构建项目（无需根据依赖关系手动构建，直接在聚合工程上构建即可
  实现 : 通过 ```<modules>``` 设置当前聚合工程所包含的子模块名称
  ```xml
  <!--聚合-->
  <modules>
  <module>../tlias-pojo</module>
  <module>../tlias-utils</module>
  <module>../tlias-web-management</module>
  </modules>
  ```
  **注意** : 聚合工程中所包含的模块，在构建时，会自动根据模块间的依赖关系设置构建顺序，与聚合工程中模块的配置书写位置无关
- maven中继承与聚合的联系与区别
  1．继承用于简化依赖配置、统一管理依赖版本，是在子工程中配置继承关系
  2．聚合用于快速构建项目，是在父工程(聚合工程)中配置聚合的模块
***
## 私服
- 核心概念 : 在企业内部搭建的 Maven 仓库，加速依赖下载并管理内部构件。
- 配置步骤 : 
  1. 配置访问认证（settings.xml）
     ```xml
     <servers>
         <!-- 发行版仓库认证 -->
         <server>
             <id>maven-releases</id>
             <username>admin</username>
             <password>admin</password>
         </server>
         <!-- 快照版仓库认证 -->
         <server>
             <id>maven-snapshots</id>
             <username>admin</username>
             <password>admin</password>
         </server>
     </servers>
     ```
  2. 配置项目上传地址（pom.xml）
     ```xml
     <distributionManagement>
         <!-- 发行版上传地址 -->
         <repository>
             <id>maven-releases</id>
             <url>http://私服地址/repository/maven-releases/</url>
         </repository>
         <!-- 快照版上传地址 -->
         <snapshotRepository>
             <id>maven-snapshots</id>
             <url>http://私服地址/repository/maven-snapshots/</url>
         </snapshotRepository>
     </distributionManagement>
     ```
  3. 配置镜像（settings.xml）
     ```xml
     <mirrors>
         <mirror>
             <id>nexus</id>
             <mirrorOf>*</mirrorOf>
             <url>http://私服地址/repository/maven-public/</url>
         </mirror>
     </mirrors>
     ```
  4. 配置仓库组（settings.xml）
     ```xml
     <profile>
         <id>allow-snapshots</id>
         <activation>
             <activeByDefault>true</activeByDefault>
         </activation>
         <repositories>
             <repository>
                 <id>maven-public</id>
                 <url>http://192.168.150.101:8081/repository/maven-public/</url>
                 <releases>
                     <enabled>true</enabled>
                 </releases>
                 <snapshots>
                     <enabled>true</enabled>
                 </snapshots>
             </repository>
         </repositories>
     </profile>
     ```