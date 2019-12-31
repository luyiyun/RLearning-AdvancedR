BaseTypes
================
荣志炜
2019/12/29

  - [1. 介绍](#介绍)
  - [2. Base versus OO objects](#base-versus-oo-objects)
  - [3. Base types](#base-types)
  - [4. Numeric类型](#numeric类型)
  - [5. 问题](#问题)

## 1\. 介绍

R中经常说的一句话是：R中的一切皆是对象。类似的话在Python也给听到，但两者还是有差别的：  
\- Python中是实打实的，所有Python中的对象都是一样的OO（面向对象）对象；  
\- R中的对象不是OOP对象，准确地说，**R中的对象分为两类：Base objects和OO objects**。

R的起源：S语言导致了这样的分歧。

## 2\. Base versus OO objects

查看对象到底是Base还是OO objects，可以使用一下两种方式：

``` r
# is.object来自base R，检验对象是否是OO objects
is.object(1:10)
## [1] FALSE
is.object(mtcars)
## [1] TRUE

# sloop::otype直接返回字符串，表明对象的OOP系统类型
sloop::otype(1:10)
## [1] "base"
sloop::otype(mtcars)
## [1] "S3"
```

Base和OO objects技术上的不同就是OO objects有class的attr：

``` r
attr(1:10, "class")
## NULL
attr(mtcars, "class")
## [1] "data.frame"
```

`class`通常用于返回objects的class。因为OO objects建构与Base
objects之上，而其只返回最后的类型，从而在用于Base和objects时，容易产生歧义。安全的方式是使用`sloop`中的`s3_class`，其返回了更多的信息，包括OO
objects应用的Base objects的类型。？？

``` r
x <- matrix(1:4, nrow = 2)
class(x)
## [1] "matrix"
sloop::s3_class(x)
## [1] "matrix"  "integer" "numeric"

y <- 1:10
class(y)
## [1] "integer"
sloop::s3_class(y)
## [1] "integer" "numeric"
```

## 3\. Base types

因为只有OO objects有class attr，那么分类Base objects使用什么呢？答案是每个Base
objects拥有一个**base type**：

``` r
typeof(1:10)
## [1] "integer"
typeof(mtcars)
## [1] "list"
```

base
types不构成OOP系统，因为其实现的方式是使用的C中的switch语句进行的，即选择语句。这使得只有改变R的核心代码才可能增加base
types，这工作量是巨大的。所以一共就只有25种不同的base types，分别列举在下面：

  - vectors，包括NULL、logical、integer、double、complex、character、list、raw；
  - functions，包括closure（正常的R函数）、special（内置函数，比如`[`）、builtin（原始函数，比如`sum`）；
  - enviroment；
  - S4；
  - 语言成分，包括symbol、language、pairlist、expression；
  - 剩下的比较少见，在C代码中是重要的，包括externalptr、weakref、bytecode、promise、…、any；

不要使用`mode`和`storage.mode`来获得objects的类型，其只返回和S兼容的类型。

## 4\. Numeric类型

“numeric”这个词在R中有3种不同的含义：

1.  许多地方，其等价于double type。比如`as.numeric` == `as.double`，`numeric()` ==
    `double()`（有时候还使用real，比如`NA_real_`）；  
2.  在S3和S4系统中，numeric表示integer和double；

<!-- end list -->

``` r
sloop::s3_class(1)
## [1] "double"  "numeric"
sloop::s3_class(1L)
## [1] "integer" "numeric"
```

3.  `is.numeric`测试的时候，只会通过那些行为像数字的类型。比如factors，其行为更像是strings，所以虽然其base
    type是integer，但`is.numeric`返回`FALSE`；

<!-- end list -->

``` r
typeof(factor("X"))
## [1] "integer"
is.numeric(factor("X"))
## [1] FALSE
```

## 5\. 问题

1.  为什么OO objects也可以返回base type？  
2.  `s3_class`返回的多个类型分别对应的是什么？
