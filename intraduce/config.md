### 1 环境配置
> + Operating System：**Windows 10**
> + IDEA:**2020.3.1**
> + JDK：**jdk-15**
> + Mysql： **8.0.26**
> + Tomcat：**7.0.103**
> + Maven：**4.0.0**
### 2 注入maven相关依赖（pom.xml）
```xml
<dependencies>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-api</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-engine</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>jstl</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.4</version>
        </dependency>
        <dependency>
            <groupId>c3p0</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.1.2</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.26</version>
        </dependency>
        <dependency>
            <groupId>taglibs</groupId>
            <artifactId>standard</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>commons-dbutils</groupId>
            <artifactId>commons-dbutils</artifactId>
            <version>1.7</version>
        </dependency>

        <!--邮箱mail依赖、activation依赖-->
        <dependency>
            <groupId>javax.mail</groupId>
            <artifactId>mail</artifactId>
            <version>1.4</version>
        </dependency>
        <dependency>
            <groupId>javax.activation</groupId>
            <artifactId>activation</artifactId>
            <version>1.1.1</version>
        </dependency>
        <dependency>
            <groupId>net.sf.json-lib</groupId>
            <artifactId>json-lib</artifactId>
            <version>2.4</version>
            <classifier>jdk15</classifier>
        </dependency>
    </dependencies>
```
### 3 C3P0连接池的配置
>#### 注入的maven依赖
```xml
        <dependency>
            <groupId>c3p0</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.1.2</version>
        </dependency>
```
>#### C3P0连接池的配置文件（c3p0-config.xml）
```xml
<?xml version="1.0" encoding="UTF-8"?>
<c3p0-config>

    <named-config name="testc3p0">

        <!-- 指定连接数据源的基本属性 -->
        <property name="user">root</property>
        <property name="password">123456</property>
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/library?useUnicode=true&amp;characterEncoding=UTF-8</property>
        <!-- 若数据库中连接数不足时, 一次向数据库服务器申请多少个连接 -->
        <property name="acquireIncrement">5</property>
        <!-- 初始化数据库连接池时连接的数量 -->
        <property name="initialPoolSize">5</property>
        <!-- 数据库连接池中的最小的数据库连接数 -->
        <property name="minPoolSize">5</property>
        <!-- 数据库连接池中的最大的数据库连接数 -->
        <property name="maxPoolSize">10</property>

    </named-config>

</c3p0-config>
```
>#### 基本原理
   + 建立数据库连接池对象（服务器启动）。
   + 按照事先指定的参数创建初始数量的数据库连接（即：初始化空闲连接数）。
   + 对于一个数据库访问请求，直接从连接池中得到一个连接。如果数据库连接池对象中没有空闲的连接，且连接数没有达到最大（即：最大活跃连接数），创建预设定数目的数据库连接。
   + 存取数据。
   + 访问结束，释放对应的数据库连接（将连接放入空闲队列中，如果实际空闲连接数大于连接池最大保留连接数则释放连接）。
   + 释放数据库连接池对象（服务器停止、维护期间，释放数据库连接池对象，并释放所有连接）。
