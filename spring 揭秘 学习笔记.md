---
title: spring 揭秘 学习笔记 
tags: 笔记
grammar_cjkRuby: true
---

### IOC容器

ioc容器包括两个阶段

1.容器的启动阶段
2.bean实例化阶段

#### 容器的启动阶段

容器的启动阶段工作内容将Configuration MetaData 通过BeanDefinitionReader的分析后的信息编组为相应的BeanDefinition，注册到相应的BeanDefinitionRegistry




#### Bean的实例化阶段