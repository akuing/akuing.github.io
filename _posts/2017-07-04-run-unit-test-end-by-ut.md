---
layout: post
title: "运行所有以UT结尾的Junit单元测试类"
---

运行所有以"UT"结尾的Junit单元测试案例类
==================================

项目中所有的Junit编写的单元测试（UT）和集成测试（IT）都分布在test目录下的各个package里面，如何运行指定类型的Junit测试案例是个问题。

其实很简单，将所有的单元测试案例都以UT结尾，所有的集成测试都以IT结尾。使用junit-toolbox中的WildcardPatternSuite类可以很好的解决这个问题。

## 第一步 在pom中增加依赖

    <dependency>
       <groupId>com.googlecode.junit-toolbox</groupId>
       <artifactId>junit-toolbox</artifactId>
       <version>1.8</version>
    </dependency> 

注意：junit-toolbox在2.0以后的版本只支持Java 8 。我现在用的是Java 7 所以选了1.8版本。

## 第二步 在test目录的package中增加一个RunUTs类（也可以用其他名字）

    package com.xxx.xxx.xxx;
    
    import com.googlecode.junittoolbox.SuiteClasses;
    import com.googlecode.junittoolbox.WildcardPatternSuite;
    import org.junit.runner.RunWith;
    
    /**
    * Created by akui on 2017/7/4.
    */
    @RunWith(WildcardPatternSuite.class)
    @SuiteClasses("**/*UT.class")
    public class RunUTs {
    }

## 第三步 运行RunUTs测试类

看看效果，这里就不上图了。

## 参考资料

junit-toolbox帮助文档的 WildcardPatternSuite 章节。

    https://github.com/michaeltamm/junit-toolbox

