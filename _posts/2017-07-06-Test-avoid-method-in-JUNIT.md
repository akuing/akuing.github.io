---
layout: post
title: "Junit下测试没有返回值的类方法"
---

项目中，特别是面对遗留代码的时候，难免会遇到需要针对没有返回值的类方法添加单元测试，由于没有返回值常用的assert大法就不管用了，这个时候Mockito的verify方法可以起到很好的检测被测试类方法预期行为的作用。

## 场景一 检测预期的行为发生


参见 Mockito的帮助文档：1. Let's verify some behaviour!

>https://static.javadoc.io/org.mockito/mockito-core/2.8.47/org/mockito/Mockito.html#verification

## 场景二 检测预期的行为发生，并捕获发生时的入参，以便后续使用assert大法

这个而功能很强大，参考下面的代码。

    ArgumentCaptor<Person> argument = ArgumentCaptor.forClass(Person.class);
    verify(mock).doSomething(argument.capture());
    assertEquals("John", argument.getValue().getName());
 

需要注意的是，不建议ArgumentCaptor和stub一起使用，这个功能的主要设计意图是用于捕获参数之后进行验证。


从一定意义上说，ArgumentCaptor和ArgumentMatcher有共通之处，这两种技术都可以用于保证某些参数被传递给mock对象。

但是，ArgumentCaptor适合你只需要检测Mock对象的入参的值来完成验证的情形。

而通过ArgumentMatcher来进行自定义擦书匹配（Custom argument matchers）通常更适合与和stub一起使用的场景。


## 参考资料

Mockito的官方文档真的是每一个希望做好单元测试的开发人员都应该仔细阅读的最佳单元测试实践指南。

>https://stackoverflow.com/questions/1142837/verify-object-attribute-value-with-mockito

>https://static.javadoc.io/org.mockito/mockito-core/2.8.47/org/mockito/Mockito.html


