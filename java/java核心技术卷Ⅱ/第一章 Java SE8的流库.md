# 第一章 Java SE8的流库

第一张主要介绍了流的创建、转换和终止流。

## 创建流

在本章中，创建流的方式依据流的来源可分为：array-based源的流、collection-based源的流。依据流自身的特点可分为：创建无限流、创建空流。

#### 从array-based数组中创建

* Stream.of()
* Arrays.stream()

#### 从collection-based数组中创建

* Collection.stream()

#### 创建空流

* Stream.empty()

#### 创建无限流

* Stream.generate()
* Stream.iterate()

## 处理流的转换



