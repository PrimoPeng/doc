---
title: spring-boot揭秘笔记
tags: spring-boot
grammar_cjkRuby: true
---


### @SpringBootApplication

SpringBootApplication 是一个三体结构，实际上它是一个复合Annotation:
其中包括 @Configuration @EnableAutoConfiguration @ComponentScan

#### @Configuration

它就是JavaConfig形式的SpringIoc容器的配置类使用的那个@Configuration
这里启动类标注了@Configuration之后，本身其实也是一个IOC容器配置类！


#### @EnableAutoConfiguration的功效

@EnableAutoConfiguration 也是借助 @Import帮助，将所有符合自动配置条件的bean定义加载到IOC容器，仅此而已。
最关键的要属@Import（EnableAutoConfigurationImportSelector.class）借助EanbleAutoConfigurationImportSelector，@EnableAutoConfiguration可以帮助SpringBoot应用将所有符合条件的@Configuration配置都加载到当前SpringBoot创建并使用的IOC容器。

- [ ] 自动配置的幕后英雄：SpringFactoriesLoader详解

SpringFactoriesLoader属于Spring框架私有的一种扩展方案，其主要功能就是从制定的配置文件/MATA-INF/spring.factories加载配置，spring.factories是一个典型的java properties文件，配置的格式key=value，只不过key和value都是java类型的完整类名。

所以，@EnableAutoConfiguration自动配置的魔法其实就变成了:  从classpath中搜寻所有META—INF/spring.factories配置文件，并将其中org.springframework.boot.autoconfigure.EnableAutoConfiguration对应的配置项通过反射实例化为对应的标注了@Configuration的JavaConfig形式的IoC容器配置类，然后汇总为一个并加载到IoC容器。

* spring.provide *   被使用STS IDE工具
