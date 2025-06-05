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

     注意:
         在子工程中，配置了继承关系之后，坐标中的 groupId 是可以省略的，因为会自动继承父工程的 。
         relativePath 指定父工程的 pom 文件的相对位置（如果不指定，将从本地仓库 / 远程仓库查找）。
         若父子工程都配置了同一个依赖的不同版本，以子工程的为准。
     ```
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