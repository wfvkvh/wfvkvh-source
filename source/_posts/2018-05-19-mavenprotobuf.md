---
layout: post_layout
title: 「序列化」用 Maven 实现一个 protobuf 的 Java 例子
date: 2018-05-19
tags: 
- 序列化
---
### 1. 介绍Protocol Buffers

Protocal Buffers(简称protobuf)是谷歌的一项技术，用于结构化的数据序列化、反序列化，常用于RPC 系统（Remote Procedure Call Protocol System）和持续数据存储系统。


其类似于XML生成和解析，但protobuf的效率高于XML，不过protobuf生成的是字节码，可读性比XML差，类似的还有json、Java的Serializable等。


很适合做数据存储或 RPC 数据交换格式。可用于通讯协议、数据存储等领域的语言无关、平台无关、可扩展的序列化结构数据格式。

<!--more-->

    
### 2. Idea 安装protobuf插件

安装插件protobuf Support，之后重启


### 3. 配置依赖
pom.xml添加

```xml
 <dependencies>
         <dependency>
             <groupId>com.google.protobuf</groupId>
             <artifactId>protobuf-java</artifactId>
             <version>3.4.0</version>
         </dependency>
 </dependencies>

 <build>
         <extensions>
             <extension>
                 <groupId>kr.motd.maven</groupId>
                 <artifactId>os-maven-plugin</artifactId>
                 <version>1.4.1.Final</version>
             </extension>
         </extensions>
         <plugins>
             <plugin>
                 <groupId>org.xolstice.maven.plugins</groupId>
                 <artifactId>protobuf-maven-plugin</artifactId>
                 <version>0.5.0</version>
                 <configuration>
                     <protocArtifact>
                         com.google.protobuf:protoc:3.1.0:exe:${os.detected.classifier}
                     </protocArtifact>
                     <pluginId>grpc-java</pluginId>
                 </configuration>
                 <executions>
                     <execution>
                         <goals>
                             <goal>compile</goal>
                             <goal>compile-custom</goal>
                         </goals>
                     </execution>
                 </executions>
             </plugin>
         </plugins>
 </build>
```

### 4. 书写proto文件

```java
 syntax = "proto3";
 option java_package = "com.jihite";
 option java_outer_classname = "PersonModel";

 message Person {
      int32 id = 1;
      string name = 2;
      string email = 3;
 }
```

### 5. 转化成Java文件


### 6. 测试

把生成的类考的代码路径下，用下面测试用例测试

```java
package com.jihite;

import com.google.protobuf.InvalidProtocolBufferException;
import org.junit.Test;
public class protobufTest {
    @Test
    public void testN() throws InvalidProtocolBufferException {
        PersonModel.Person.Builder builder = PersonModel.Person.newBuilder();
        builder.setId(1);
        builder.setName("jihite");
        builder.setEmail("jihite@jihite.com");

        PersonModel.Person person = builder.build();
        System.out.println("before:" + person);

        System.out.println("===Person Byte:");
        for (byte b : person.toByteArray()) {
            System.out.print(b);
        }
        System.out.println("================");

        byte[] byteArray = person.toByteArray();
        PersonModel.Person p2 = PersonModel.Person.parseFrom(byteArray);
        System.out.println("after id:" + p2.getId());
        System.out.println("after name:" + p2.getName());
        System.out.println("after email:" + p2.getEmail());

    }
}
```

### 结果